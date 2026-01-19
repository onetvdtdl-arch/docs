# Analytics Utils Async Refactor

## Context
Analytics logging currently crosses multiple async layers:

1) `CompositeAnalyticsUtil` uses Rx (`Observable.fromIterable`) to fan out log calls.
2) `MqttAnalyticsHelper` uses coroutines to send MQTT events.
3) Several delegates used to add Rx/IO scheduling before calling analytics (e.g., `AnalyticsActivityDelegate`, `AnalyticsApplicationDelegate`).
4) `AnalyticsManager` used to provide helper wrappers that introduced additional Rx/IO scheduling.

This multiplies scheduling, adds allocation overhead per event, and spreads error handling across layers. It also makes it harder to reason about performance when analytics fire frequently (player, settings, detail, etc.).

Additional call-site async layers existed in analytics delegates and helper utilities, which introduced extra thread hops on top of the core analytics pipeline.

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

AnalyticsManager.analyticsWorkCoroutine (direct call)
  -> AnalyticsUtilImpl (CoroutineScope, Dispatchers.IO)
    -> MqttAnalyticsUtil (sync)
```

## Current State Snapshot
- Delegates (`AnalyticsActivityDelegate`, `AnalyticsApplicationDelegate`) log synchronously and rely on `AnalyticsUtilImpl` for async dispatch.
- Call sites that were previously using `compositeDisposable.add(...)` now call analytics delegates directly.
- `AnalyticsManager` is reduced to a single synchronous helper: `analyticsWorkCoroutine(work: () -> Unit)`; other helper variants were removed.
- `AnalyticsUtilImpl` now builds common attributes via `AnalyticsCommonDelegate.getAttributes()` and merges event-specific parameters in one place.
- `AnalyticsCommonDelegate.getCommonAttributes()` is now a no-op placeholder; call sites should avoid building common attributes locally.

## Key Changes
- `AnalyticsUtilImpl`:
  - Holds a list of `IAnalyticsUtils` backends.
  - Uses a single coroutine scope to dispatch all log calls off the caller thread.
  - Wraps each backend call in try/catch to avoid failure fan-out.
  - Centralizes common attribute assembly using `AnalyticsCommonDelegate.getAttributes()`.

- `MqttAnalyticsUtil`:
  - Owns MQTT settings retrieval (topic/qos), tracking checks, and actual send.
  - No internal coroutines or Rx.
  - No helper class required.
- Call-site cleanup:
  - Removed Rx scheduling from analytics delegates where `AnalyticsUtilImpl` already dispatches.
  - Switched to direct calls to `analyticsUtils.logEvent(...)` without extra wrappers.
  - Simplified `AnalyticsManager` helpers to avoid redundant thread hops.
  - Removed per-call common attribute fetching in delegates; `AnalyticsUtilImpl` now handles it.

- Dagger wiring:
  - Remove `MqttAnalyticsHelper` provider.
  - Provide `MqttAnalyticsUtil` directly.
  - Provide `AnalyticsUtilImpl` as `IAnalyticsUtils`.

## Example Code Changes (Phased)

### Phase 1: Rename/merge utils + move coroutine fan-out
Renamed `CompositeAnalyticsUtil` to `AnalyticsUtilImpl`, merged `MqttAnalyticsHelper` into `MqttAnalyticsUtil`, and moved coroutine + mutex fan-out into `AnalyticsUtilImpl`.

### AnalyticsUtilImpl (current implementation)
```kotlin
class AnalyticsUtilImpl(
    private val analyticsUtils: List<IAnalyticsUtils>,
    private val isLoggingEnabled: Boolean = false,
    private val analyticsCommonDelegate: Lazy<AnalyticsCommonDelegate>,
    private val environment: Lazy<IEnvironment>
) : IAnalyticsUtils {

    private val scopeName = "Analytics-Scope"
    private val mqttExceptionHandler =
        CoroutineExceptionHandler { _, throwable ->
            Timber.e(throwable, "Exception in AnalyticsUtilImpl: ${throwable.message}")
        }
    private val mutex = Mutex()
    private val scope: CoroutineScope =
        CoroutineScope(SupervisorJob() + Dispatchers.IO + CoroutineName(scopeName) + mqttExceptionHandler)

    private inline fun dispatch(crossinline call: (IAnalyticsUtils) -> Unit) {
        if (!isLoggingEnabled) return
        scope.launch {
            mutex.withLock {
                analyticsUtils.forEach { backend ->
                    try {
                        call(backend)
                    } catch (throwable: Throwable) {
                        Timber.e(
                            throwable,
                            "Analytics backend failed: %s",
                            backend.javaClass.simpleName
                        )
                    }
                }
            }
        }
    }

    override fun logEvent(
        category: String?,
        action: String,
        eventParameter: EventParameter?
    ) {
        dispatch {
            val param = EventParameter.create(
                analyticsCommonDelegate.get().getAttributes().apply {
                    putAll(eventParameter?.parameters ?: emptyMap())
                }
            )
            logDebug(
                "Mqtt - streaming event :: logging - category=%s, action=%s, eventParameters=%s",
                category,
                action,
                param
            )
            it.logEvent(
                category, action, param
            )
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

### Phase 2: Remove Rx from delegates

### AnalyticsActivityDelegate (current direction)
```kotlin
// Before: Single.fromCallable(...).subscribeOn(...)
fun onScreenViewed() {
    val attributes = hashMapOf<String, Any?>().apply {
        put(OneTvEvents.Fields.ATTR_STRING, CATEGORY_NETWORK_ACTIVITY)
    }
    analyticsUtils.get().logEvent(
        CATEGORY_NETWORK_ACTIVITY,
        VIEWED_SCREEN,
        EventParameter.create(attributes)
    )
}
```

### Phase 3: Collapse AnalyticsManager async variants

### AnalyticsManager (current direction)
```kotlin
// Before: analyticsWorkCoroutine launched Dispatchers.IO
fun analyticsWorkCoroutine(work: () -> Unit) {
    work()
}
```

### Phase 4: Remove Rx from other delegates and modules

Refactors landed across modules to strip Rx scheduling wrappers:
- analytics - Core analytics module (refactored AnalyticsCommonDelegate, removed RxJava dependencies)
- playerui - Player UI module (updated AnalyticsPlayerDelegate)
- channelui - Channel UI module (updated AnalyticsChannelsDelegate)
- detailui - Detail UI module (updated AnalyticsDetailDelegate)
- epgui - EPG UI module (updated AnalyticsEpgDelegate)
- loginui - Login UI module (updated AnalyticsAccountManagementDelegate)
- homeui - Home UI module (updated SettingsAnalyticsDelegate)
- settingsui - Settings UI module (updated AnalyticsPinAuthenticationDelegate)
- tvsubpageui - TV Subpage UI module (updated SubpageAnalyticsDelegate)
- notification - Notification module (updated AnalyticsNotificationDelegate)
- digitalnpsdomain - Digital NPS domain module (updated NpsAnalyticsDelegate)

### Example (loginui/AnalyticsAccountManagementDelegate)
```kotlin
// Before: Rx wrapper
fun onLoginInitiated(loginType: LoginEvent.LoginType): Disposable =
    Single
        .fromCallable {
            val attributes = commonDelegate.get().getCommonAttributes()
            Timber.tag(TAG).d("onLoginInitiated login type = ${loginType.value}")
            attributes.apply {
                put(com.atvretail.analytics.OneTvEvents.Fields.ATTR_STRING, ACCOUNT_MANAGEMENT)
                put(LOGIN_TYPE, loginType.value)
            }
            attributes
        }
        .subscribeOn(Schedulers.io())
        .subscribe({
            analyticsUtils.get().logEvent(
                ACCOUNT_MANAGEMENT,
                ACCOUNT_MANAGEMENT_LOGIN_INITIATED,
                EventParameter.create(it)
            )
        }, {
            Timber.tag(TAG).d("onLoginInitiated error")
            Timber.e(it, "onLoginInitiated error")
        })

// After: direct call
fun onLoginInitiated(loginType: LoginEvent.LoginType) {
    try {
        val attributes = commonDelegate.get().getCommonAttributes()
        Timber.tag(TAG).d("onLoginInitiated login type = ${loginType.value}")
        attributes.apply {
            put(com.atvretail.analytics.OneTvEvents.Fields.ATTR_STRING, ACCOUNT_MANAGEMENT)
            put(LOGIN_TYPE, loginType.value)
        }

        analyticsUtils.get().logEvent(
            ACCOUNT_MANAGEMENT,
            ACCOUNT_MANAGEMENT_LOGIN_INITIATED,
            EventParameter.create(attributes)
        )
    } catch (throwable: Throwable) {
        Timber.tag(TAG).d("onLoginInitiated error")
        Timber.e(throwable, "onLoginInitiated error")
    }
}
```

### Phase 5: Centralize common attribute fetching
Common attributes now come from `AnalyticsUtilImpl`, which always merges `AnalyticsCommonDelegate.getAttributes()` before dispatching. Delegates no longer build or enrich common attributes per call.

### AnalyticsUtilImpl (common attribute merge)
```kotlin
val param = EventParameter.create(
    analyticsCommonDelegate.get().getAttributes().apply {
        putAll(eventParameter?.parameters ?: emptyMap())
    }
)
```

### AnalyticsCommonDelegate (current implementation)
```kotlin
fun getAttributes(): HashMap<String, Any?> {
    return hashMapOf<String, Any?>().apply {
        putAll(staticCommonAttributes)
        put(KEYS.COMMON_CONNECTION_TYPE, analyticsConstants.getConnectionType())
        put(KEYS.COMMON_USER_ID, analyticsConstants.userId())
        put(KEYS.COMMON_SESSION_ID, environment.get().userSessionId)
        put(KEYS.COMMON_NETWORK_PROVIDER, connectivityUtil.get().networkTypeInfo)
        put(KEYS.COMMON_TIMESTAMP, DateTimeUtils.currentTimeMillis())
        put(KEYS.COMMON_IP_ADDRESS, PublicIpAddressHolder.getPublicIpAddress())
        put(KEYS.COMMON_LOG_TIME, DateTimeManager.getCurrentDateTimeInUtc(true).toString())
        put(KEYS.PERSONA_ID, authPreferences.personaId)
        put(KEYS.PERSONALISED_CONTENT_OPT_OUT, analyticsConstants.isOptedOut())
        put(KEYS.COMMON_ACCOUNT_ID, analyticsConstants.accountId())
        put(KEYS.COMMON_ACCOUNT_TYPE, analyticsConstants.accountType())
        if (environment.get().isManagedDevice) {
            if (DeviceUtils.cpuUsage != null && cmsDataManager.cmsConfig.isCPULoggingEnabled) {
                put(KEYS.COMMON_CPU_USAGE, DeviceUtils.cpuUsage)
            }
            if (cmsDataManager.cmsConfig.isMemoryLoggingEnabled) {
                if (DeviceUtils.availableMemoryInMB != null) {
                    put(KEYS.COMMON_AVAILABLE_MEMORY, DeviceUtils.availableMemoryInMB)
                }
                if (DeviceUtils.totalMemoryInMB != null) {
                    put(KEYS.COMMON_TOTAL_MEMORY, DeviceUtils.totalMemoryInMB)
                }
            }
        }
    }
}
```

### AnalyticsCommonDelegate (no-op placeholder)
```kotlin
fun getCommonAttributes(): HashMap<String, Any?> {
    return hashMapOf()
}
```

## Risks / Considerations
- Ensure coroutine scope lifecycle is acceptable (app-level singleton).
- Validate behavior of filter expressions (UTIL flags) remains unchanged.
- Preserve MQTT topic/qos and debug logging behavior.
- Delegate-level try/catch is defensive; if you want analytics failures to surface, remove those wrappers.
- Delegates now log directly and rely on `AnalyticsUtilImpl` for async dispatch; remaining wrappers should stay synchronous to avoid double hops.
- Mutex location matters: placing serialization in `AnalyticsUtilImpl` assumes all analytics calls route through it. If any backend is called directly, it bypasses the mutex and can run concurrently.
