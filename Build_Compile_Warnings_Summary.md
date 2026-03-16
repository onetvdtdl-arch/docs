# Build Compile-Time Warnings Summary

**Project:** AT-atv-retail-app  
**Build:** `:app:assemblePlProd`  
**Date:** March 2025  
**Purpose:** Shareable reference for team to track and fix 1000+ compile-time warnings.

---

## 1. Gradle / AGP (Android Gradle Plugin)

| Warning | Location | Action |
|--------|----------|--------|
| **`android.defaults.buildfeatures.buildconfig=true` is deprecated** (removed in AGP 10.0) | Root/gradle or app | In each module that needs BuildConfig, add in `build.gradle`: `android { buildFeatures { buildConfig = true } }`. Or use Android Studio: **Refactor → Migrate BuildConfig to Gradle Build Files**. |
| **`publishNonDefault` is deprecated** | `:castlabplayer` | Remove; all variants are published by default. |
| **`dexOptions` is obsolete** (removed in AGP 8.0) | `:castlabplayer` | Remove `dexOptions { }` block; AGP handles dexing. |
| **Configuration resolved during configuration time** | `:castlabplayer` (plProdCompileClasspath, plProdRuntimeClasspath, plProdProtobuf, etc.) | Avoid resolving configurations in configuration phase. Defer to task execution. See [Gradle #2298](https://github.com/gradle/gradle/issues/2298). Run with `--info` for stacktrace. |

---

## 2. AndroidManifest

| Issue | Action |
|-------|--------|
| **Duplicate `uses-permission` and `permission` elements** (e.g. QUERY_ALL_PACKAGES, EPG, READ_TV_LISTINGS, ACCESS_NETWORK_STATE, RECEIVE_BOOT_COMPLETED, REBOOT_APP, SETTINGS.FINISH, LOG_OUT, etc.) | Merge manifest sources: keep a single declaration per permission. Check app manifest and all library manifests (manifest merger). |
| **`manifest@android:allowBackup`** tagged to replace but no other declaration | Remove the `tools:replace` if nothing is being replaced, or fix the merge. |
| **`application@android:networkSecurityConfig`** same | Same as above for `networkSecurityConfig`. |

---

## 3. Kotlin: Parcelize

**Message:** *Parcelize annotations from package 'kotlinx.android.parcel' are deprecated. Change package to 'kotlinx.parcelize'.*

**Affected (examples):**  
`ott/coreinterface` (AccountDetails, AccountInfo), `ott/core` (CustomSnackBar, AppStatus, CampaignDetail, Cta, Onboarding), `ott/uielements` (ActionsItem, ChannelSubscriptionData), `ott/appcommons` (NavContext, MetadataItem, TagRailItemData, WatchlistEntity, BookmarksApiResponse, etc.), `ott/onetvsettingdomain` (ContentLock, ContentSessionType), `subscriptiondomain` (SubscriptionFlow), `detaildomain`, `ott/detaildomain`, `ott/playerdomain`, `ott/transactiondomain`, `ott/epgdomain`, `ott/searchdomain`, `ott/homedomain`, and others.

**Action:**  
Replace  
`import kotlinx.android.parcel.Parcelize`  
with  
`import kotlinx.parcelize.Parcelize`  
(and add `kotlin-parcelize` plugin if not already applied).

---

## 4. Kotlin: Annotation target (KT-73255)

**Message:** *This annotation is currently applied to the value parameter only, but in the future it will also be applied to field.*

**Action (choose one):**  
- Add compiler arg: `-Xannotation-default-target=param-property`  
- Or use explicit target: `@param:MyAnnotation` to keep parameter-only.

**Affected areas:** NotificationConfiguration, TagRailItemData, ItemBookmarks, BookmarksApiResponse, RailComponentItemInfo, BottomBarData, DpadGridSpacingDecoration, DpadLinearSpacingDecoration, PublicIpResponse, and many ViewModels/entities across appcommons, settingsui, playercommon, loginui, etc.

---

## 5. Kotlin: Parcel / IgnoredOnParcel

**Message:** *Property will not be serialized into a 'Parcel'. Add '@IgnoredOnParcel' annotation to remove the warning.*

**Affected (examples):**  
- `subscriptiondomain/SubscriptionFlow.kt` (4 properties)  
- `ott/appcommons/entity/widgets/rail/RailComponentItemInfo.kt` (4 properties)  
- `detaildomain/PurchaseOptionSelectionItem.kt` (2 properties)  
- `ott/epgdomain/entity/EpgData.kt` (multiple properties)

**Action:** Add `@IgnoredOnParcel` on properties that must not be parceled (e.g. callbacks, non-Parcelable types).

---

## 6. Deprecated Java/Android API usage

| Deprecated API | Replacement / action |
|----------------|----------------------|
| **`onBackPressed()`** | Use `OnBackPressedDispatcher` / `OnBackCallback`. |
| **`getParcelableExtra(String)` / `getParcelable(String)`** | Use `getParcelableExtra(key, Class)` / `getParcelable(key, Class)` (API 33+). |
| **`Handler()`** | Prefer `Handler(Looper.getMainLooper())` or structured concurrency (e.g. coroutines). |
| **`ViewModelProviders.of()`** | Use `ViewModelProvider(activity/fragment, factory)`. |
| **`onActivityCreated()`** | Move logic to `onViewCreated()` / `onCreate()`. |
| **`startActivityForResult()` / `onActivityResult()`** | Use Activity Result API (`registerForActivityResult`). |
| **`NetworkInfo` / `ConnectivityManager.getActiveNetworkInfo()`** | Use `NetworkCapabilities` / `ConnectivityManager.getNetworkCapabilities()`. |
| **`Context.getDisplay()` / `Display.getMetrics()`** | Use `DisplayManager` / modern display APIs where applicable. |
| **`BluetoothAdapter.getDefaultAdapter()`** | Use `BluetoothManager.getAdapter()` with context. |
| **`String.capitalize()`** | Use `replaceFirstChar { it.uppercase() }`. |
| **`Html.fromHtml(String)`** | Use `Html.fromHtml(String, flag)` or `HtmlCompat`. |
| **`WorkManager.getInstance(Context)`** | Use injection or `WorkManager.getInstance()` with proper context. |
| **`fallbackToDestructiveMigration()`** | Use overload that specifies whether to drop all tables (e.g. `fallbackToDestructiveMigration(dropAllTables = true/false)`). |
| **`@OnLifecycleEvent`** | Use `LifecycleEventObserver` or `DefaultLifecycleObserver`. |
| **`PositionalDataSource` / `ItemKeyedDataSource`** | Migrate to `PagingSource` (Paging 3). |
| **`PagedList`** | Migrate to `PagingData` / Flow (Paging 3). |
| **`adapterPosition`** (RecyclerView) | Prefer `bindingAdapterPosition` where appropriate. |
| **`versionCode`** | Use `longVersionCode` or `versionCode` via `BuildConfig`/namespace. |

---

## 7. Annotation processors not incremental

**Message:** *The following annotation processors are not incremental: deeplinkdispatch-processor (com.airbnb:deeplinkdispatch-processor:4.1.0), auto-service (com.google.auto.service:1.0-rc3).*

**Impact:** Slower incremental builds.

**Action:**  
- Prefer incremental-friendly versions of these processors if available.  
- Isolate modules that use them so other modules benefit from incremental compilation.  
- Consider replacing or wrapping usage to reduce recompilation.

---

## 8. Room

| Warning | Action |
|--------|--------|
| **Schema export directory not provided** (ATVDatabase, HomeDb, ChannelLogoDb, SubpageTagDb, CmsKeymapDb, RetailDb, NpsDatabase, etc.) | Either apply Room Gradle plugin and set `room.schemaLocation`, or set `exportSchema = false` in `@Database`. |
| **`fallbackToDestructiveMigration()` deprecated** | Use the overload with parameter (e.g. drop all tables or not). |
| **Foreign key columns not in index** (e.g. `categoryId`, `componentId`) | Add `@Index` on FK columns (e.g. in `Component`, `RailComponentItemInfo`, `Highlight`) to avoid full table scans. |
| **Component `highlights` type mismatch** (List<?> vs List<Object>) | Align type in entity with Room’s expected type (e.g. use a concrete type or converter). |

---

## 9. Data binding / Layout

| Location | Issue | Action |
|----------|--------|--------|
| **homeuicompoent: highlight_view_item.xml (Line 91)** | View field `categoryTitle` collides with a variable or import. | Rename the view or the variable so they don’t conflict. |
| **loginui: login_username_password_fragmet.xml (151, 177)** | Method reference using `.` deprecated (e.g. `viewModel.onUserNameChange`). | Use `viewModel::onUserNameChange`, `viewModel::onPasswordChange`. |
| **loginui: login_native_user_pass_activity.xml (80, 139)** | Same. | Use `viewModel::onUserNameChange`, `viewModel::onPasswordChange`. |

---

## 10. Resources

| Message | Action |
|--------|--------|
| **Removing resource without required default** (e.g. `detail__detail_page__rent_sub_label_hours`, `player__player__live_content` for `pl`) | Add default value in `values/strings.xml` (or base locale) so the key exists when locale-specific value is missing. |

---

## 11. Native / strip

**Message:** *Unable to strip the following libraries, packaging them as they are: libabr.so, libandroidx.graphics.path.so, libc++_shared.so, libcrashlytics-*.so, libeit.so, libkeys.so, libsdk.so, libsubtitlebaseplugin.so, libtoolChecker.so.*

**Action:** Optional: run with `--info` to see why strip was skipped. Often acceptable for debug; for release, ensure strip is enabled where possible and third-party libs are built with symbols in a separate bundle.

---

## 12. DeepLink

**Message:** *Output path not specified, DeepLink doc is not going to be generated.* (and *Output file is null, DeepLink doc not generated*)

**Action:** If DeepLink docs are desired, configure the annotation processor with an output path for the doc. Otherwise this can be ignored.

---

## 13. Other recurring themes

- **Unchecked cast in ViewModelFactory `create()`:** Common and often acceptable; can be suppressed or replaced with a type-safe factory pattern if desired.  
- **“Condition is always true/false”:** Remove dead code or fix logic.  
- **“Corresponding parameter in supertype named X”:** Align parameter names with interface/superclass to avoid confusion with named arguments.  
- **“Only safe (?.) or non-null asserted (!!.) calls allowed on nullable receiver”:** Add null checks or use safe calls.  
- **“Expected performance impact from inlining is insignificant”:** Remove `inline` from functions that don’t take function parameters, or leave as-is if readability is preferred.  
- **“Modifier 'open' is redundant for interface”:** Remove redundant `open` in Kotlin interfaces.  
- **`analyticsWorkCoroutine` deprecated:** Use the recommended coroutine-based API from AnalyticsUtilsImpl.

---

## Suggested priority

1. **High:** Manifest duplicates, Room schema/index and type mismatch, layout/data binding errors (categoryTitle, method reference).  
2. **Medium:** BuildConfig/dexOptions/publishNonDefault (AGP), Parcelize migration, `onBackPressed`/Activity Result API.  
3. **Lower:** Annotation target (KT-73255), incremental processors, unchecked casts, inlining, and doc generation.

---

## How to use this doc

- **Ownership:** Assign each section or module to a team/owner.  
- **Tracking:** Copy rows into your issue tracker (e.g. Jira) and link to this file.  
- **Build:** Re-run `./gradlew :app:assemblePlProd` (or your variant) and grep for `warning:` / `AGPBI` to verify fixes.  
- **Baseline:** Consider enabling `-Werror` for a subset of warnings (e.g. in CI) after fixing the most critical ones.

*Generated from build log for :app:assemblePlProd.*
