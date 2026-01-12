# Analytics Utils Async Refactor

## Context
Analytics logging currently crosses two async layers:

1) `CompositeAnalyticsUtil` uses Rx (`Observable.fromIterable`) to fan out log calls.
2) `MqttAnalyticsHelper` uses coroutines to send MQTT events.

This doubles scheduling, adds allocation overhead per event, and spreads error handling across layers. It also makes it harder to reason about performance when analytics fire frequently (player, settings, detail, etc.).

## Goal
- Use **one async boundary** for analytics delivery.
- Centralize thread management and error handling in a single place.
- Keep an extensible entry point for future analytics backends.

## Proposed Architecture
- Rename `CompositeAnalyticsUtil` -> `AnalyticsUtilImpl`.
- Remove Rx from the analytics fan-out path.
- Merge `MqttAnalyticsHelper` logic into `MqttAnalyticsUtil`.
- Put **coroutine scope + dispatch** inside `AnalyticsUtilImpl`.
- `MqttAnalyticsUtil` becomes a synchronous backend implementation that builds payloads and sends via MQTT client.

```
AnalyticsUtilImpl (coroutines, single async layer)
  -> MqttAnalyticsUtil (sync backend)
```

## Current Flow (Before)
```
AnalyticsActivityDelegate
  -> CompositeAnalyticsUtil (Rx Observable + Schedulers.single())
    -> MqttAnalyticsUtil
      -> MqttAnalyticsHelper (CoroutineScope, Dispatchers.IO)
        -> IAnalytics.logEvent(...)
```

## Target Flow (After)
```
AnalyticsActivityDelegate
  -> AnalyticsUtilImpl (CoroutineScope, Dispatchers.IO)
    -> MqttAnalyticsUtil (sync)
      -> IAnalytics.logEvent(...)
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

- Dagger wiring:
  - Remove `MqttAnalyticsHelper` provider.
  - Provide `MqttAnalyticsUtil` directly.
  - Provide `AnalyticsUtilImpl` as `IAnalyticsUtils`.

## Example Code Changes

### AnalyticsUtilImpl (new, sync fan-out + coroutine boundary)
```kotlin
class AnalyticsUtilImpl(
    private val backends: List<IAnalyticsUtils>,
    private val isLoggingEnabled: Boolean
) : IAnalyticsUtils {

    private val scope = CoroutineScope(SupervisorJob() + Dispatchers.IO)

    override fun logEvent(category: String?, action: String, eventParameter: EventParameter?) {
        if (!isLoggingEnabled) return
        scope.launch {
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
```

### MqttAnalyticsUtil (merged helper, sync backend)
```kotlin
class MqttAnalyticsUtil(
    private val analytics: Lazy<IAnalytics>,
    private val mqttSettingsProvider: Lazy<IMqttSettingsProvider>,
    private val eventTrackingSettingsProvider: Lazy<IEventTrackingSettingsProvider>,
    private val environment: Lazy<IEnvironment>
) : IAnalyticsUtils {

    override fun logEvent(category: String?, action: String, eventParameter: EventParameter?) {
        if (!eventTrackingSettingsProvider.get().eventTrackingSettings.isEventTrackingEnabled) return
        val settings = mqttSettingsProvider.get().mqttSettings ?: return

        val params = HashMap<String, Any?>().apply {
            put("event_action", action)
            put("category", category)
            eventParameter?.parameters?.forEach { (k, v) -> put(k, v) }
        }

        val topic = eventTrackingSettingsProvider.get().eventTrackingSettings.eventTrackingTopicName() ?: "OAAnalytics"
        val qos = eventTrackingSettingsProvider.get().eventTrackingSettings.eventTrackingQos()
        val publishOptions = PublishOptions(topic, qos)

        analytics.get().logEvent(params, publishOptions, null)
    }
}
```

## Benefits
- Removes Rx allocations and scheduler hops per event.
- Single async boundary, simpler error handling.
- Easier to add future backends without duplicating thread management.

## Risks / Considerations
- Ensure coroutine scope lifecycle is acceptable (app-level singleton).
- Validate behavior of filter expressions (UTIL flags) remains unchanged.
- Preserve MQTT topic/qos and debug logging behavior.

## Test / Validation Plan
- Trigger analytics on key screens (Home, Player, Settings, Detail) and confirm MQTT events are emitted.
- Verify analytics disable flag still short-circuits logging.
- Check logcat for new backend error logs (no crash).

## Rollout Notes
- This is an internal refactor; event payloads should remain unchanged.
- If future backends are added, they should be synchronous and rely on `AnalyticsUtilImpl` for threading.
