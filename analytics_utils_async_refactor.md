# Analytics Utils Async Refactor

## Context
Analytics logging currently crosses multiple async layers:

1) `CompositeAnalyticsUtil` uses Rx (`Observable.fromIterable`) to fan out log calls.
2) `MqttAnalyticsHelper` uses coroutines to send MQTT events.
3) Several delegates add Rx/IO scheduling before calling analytics (e.g., `AnalyticsActivityDelegate`, `AnalyticsApplicationDelegate`).
4) `AnalyticsManager` provides helper wrappers that introduce additional Rx/IO scheduling.

This multiplies scheduling, adds allocation overhead per event, and spreads error handling across layers. It also makes it harder to reason about performance when analytics fire frequently (player, settings, detail, etc.).

Additional call-site async layers exist in analytics delegates and helper utilities, which can introduce extra thread hops on top of the core analytics pipeline.

## Proposed Architecture
Analytics calls should flow through `AnalyticsUtilImpl`, which owns coroutine dispatch and fan-out. Backend implementations (like MQTT) are synchronous and do not manage threads.

```
AnalyticsUtilImpl (coroutines, single async layer)
  -> MqttAnalyticsUtil (sync backend)
```

## Current Flow (Before)
```
AnalyticsActivityDelegate (Rx Single)
  -> CompositeAnalyticsUtil (Rx Observable + Schedulers.single())
    -> MqttAnalyticsUtil
      -> MqttAnalyticsHelper (CoroutineScope, Dispatchers.IO)
        -> IAnalytics.logEvent(...)

AnalyticsManager.analyticsWork (Rx Single)
  -> CompositeAnalyticsUtil (Rx Observable + Schedulers.single())
    -> MqttAnalyticsHelper (CoroutineScope)
```

Some call sites also schedule analytics before calling the util:
```
AnalyticsApplicationDelegate (Rx Single)
  -> CompositeAnalyticsUtil (Rx)
    -> MqttAnalyticsHelper (CoroutineScope)
```

## Target Flow (After)
```
AnalyticsActivityDelegate (direct call)
  -> AnalyticsUtilImpl (CoroutineScope, Dispatchers.IO)
    -> MqttAnalyticsUtil (sync)
      -> IAnalytics.logEvent(...)

AnalyticsManager.analyticsWork (direct call)
  -> AnalyticsUtilImpl (CoroutineScope, Dispatchers.IO)
    -> MqttAnalyticsUtil (sync)
```

## Key Changes
- `AnalyticsUtilImpl`:
  - Holds a list of `IAnalyticsUtils` backends.
  - Uses a single coroutine scope to dispatch all log calls off the caller thread.
  - Wraps each backend call in try/catch to avoid failure fan-out.

- `MqttAnalyticsUtil`:
  - Owns MQTT settings retrieval (topic/qos), tracking checks, and actual send.
  - No internal coroutines or Rx.
  - No helper class required.
- Call-site cleanup (follow-up):
  - Remove Rx scheduling from analytics delegates where `AnalyticsUtilImpl` already dispatches.
  - Prefer direct calls to `analyticsUtils.logEvent(...)` without extra wrappers.
  - Simplify `AnalyticsManager` helpers to avoid redundant thread hops.

- Dagger wiring:
  - Remove `MqttAnalyticsHelper` provider.
  - Provide `MqttAnalyticsUtil` directly.
  - Provide `AnalyticsUtilImpl` as `IAnalyticsUtils`.

## Example Code Changes

### AnalyticsUtilImpl (current implementation)
```kotlin
class AnalyticsUtilImpl(
    private val backends: List<IAnalyticsUtils>,
    private val isLoggingEnabled: Boolean,
    private val environment: Lazy<IEnvironment>
) : IAnalyticsUtils {

    private val scope = CoroutineScope(SupervisorJob() + Dispatchers.IO)
    private val fanOutMutex = Mutex()

    override fun logEvent(category: String?, action: String, eventParameter: EventParameter?) {
        if (!isLoggingEnabled) return
        scope.launch {
            fanOutMutex.withLock {
                backends.forEach { backend ->
                    try {
                        backend.logEvent(category, action, eventParameter)
                    } catch (t: Throwable) {
                        Timber.e(t, "Analytics backend failed: %s", backend.javaClass.simpleName)
                    }
                }
            }
        }
    }
}
```

### MqttAnalyticsUtil (current implementation)
```kotlin
class MqttAnalyticsUtil(
    private val analytics: Lazy<IAnalytics>,
    private val mqttSettingsProvider: Lazy<IMqttSettingsProvider>,
    private val eventTrackingSettingsProvider: Lazy<IEventTrackingSettingsProvider>
) : IAnalyticsUtils {

    override fun logEvent(category: String?, action: String, eventParameter: EventParameter?) {
        val settings = eventTrackingSettingsProvider.get().eventTrackingSettings ?: return
        if (!settings.isEventTrackingEnabled) return
        if (mqttSettingsProvider.get().mqttSettings == null) return

        val params = HashMap<String, Any?>().apply {
            put("event_action", action)
            put("category", category)
            eventParameter?.parameters?.forEach { (k, v) -> put(k, v) }
        }

        val topic = settings.eventTrackingTopicName() ?: "OAAnalytics"
        val qos = settings.eventTrackingQos()
        val publishOptions = PublishOptions(topic, qos)

        analytics.get().logEvent(params, publishOptions, null)
    }
}
```

### AnalyticsActivityDelegate (current direction)
```kotlin
// Before: Single.fromCallable(...).subscribeOn(...)
fun onScreenViewed() {
    val attributes = analyticsCommonDelegate.get().getCommonAttributes().apply {
        put(OneTvEvents.Fields.ATTR_STRING, CATEGORY_NETWORK_ACTIVITY)
    }
    analyticsUtils.get().logEvent(
        CATEGORY_NETWORK_ACTIVITY,
        VIEWED_SCREEN,
        EventParameter.create(attributes)
    )
}
```

### AnalyticsManager (current direction)
```kotlin
// Before: analyticsWork uses Rx, analyticsWorkCoroutine launches Dispatchers.IO
fun analyticsWork(work: () -> Unit) {
    work.invoke()
}
```

## Risks / Considerations
- Ensure coroutine scope lifecycle is acceptable (app-level singleton).
- Validate behavior of filter expressions (UTIL flags) remains unchanged.
- Preserve MQTT topic/qos and debug logging behavior.
- Some delegates still schedule analytics work; refactoring them is a follow-up to reach a single async hop end-to-end.
- Mutex location matters: placing serialization in `AnalyticsUtilImpl` assumes all analytics calls route through it. If any backend is called directly, it bypasses the mutex and can run concurrently.
