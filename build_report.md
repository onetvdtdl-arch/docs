# Build Warnings by Module (from build log)

Plain module-by-module list with compiler warning text and suggestions as in the log.

---

## :app (Configure project)

- **Warning:** The option setting 'android.defaults.buildfeatures.buildconfig=true' is deprecated. The current default is 'false'. It will be removed in version 10.0 of the Android Gradle plugin.
- **Suggestion:** To keep using this feature, add the following to your module-level build.gradle files: `android.buildFeatures.buildConfig = true` or from Android Studio, click: `Refactor` > `Migrate BuildConfig to Gradle Build Files`.

---

## :app (Task :app:stripPlProdDebugSymbols)

- **Warning:** Unable to strip the following libraries, packaging them as they are: libabr.so, libandroidx.graphics.path.so, libc++_shared.so, libcrashlytics-common.so, libcrashlytics-handler.so, libcrashlytics-trampoline.so, libcrashlytics.so, libeit.so, libkeys.so, libsdk.so, libsubtitlebaseplugin.so, libtoolChecker.so.
- **Suggestion:** Run with --info option to learn more.

---

## :app (Task :app:processPlProdMainManifest)

- **Warning:** Element uses-permission#android.permission.QUERY_ALL_PACKAGES at AndroidManifest.xml:27:5-28:53 duplicated with element declared at AndroidManifest.xml:20:5-77
- **Warning:** Element uses-permission#com.android.providers.tv.permission.WRITE_EPG_DATA at AndroidManifest.xml:35:5-90 duplicated with element declared at AndroidManifest.xml:18:5-90
- **Warning:** Element uses-permission#com.android.providers.tv.permission.READ_EPG_DATA at AndroidManifest.xml:36:5-89 duplicated with element declared at AndroidManifest.xml:16:5-89
- **Warning:** Element uses-permission#com.android.providers.tv.permission.ACCESS_ALL_EPG_DATA at AndroidManifest.xml:37:5-95 duplicated with element declared at AndroidManifest.xml:17:5-95
- **Warning:** Element uses-permission#android.permission.READ_TV_LISTINGS at AndroidManifest.xml:51:5-75 duplicated with element declared at AndroidManifest.xml:15:5-74
- **Warning:** Element uses-permission#android.permission.ACCESS_NETWORK_STATE at AndroidManifest.xml:71:5-79 duplicated with element declared at AndroidManifest.xml:39:5-79
- **Warning:** Element uses-permission#android.permission.READ_PRIVILEGED_PHONE_STATE at AndroidManifest.xml:119:5-121:47 duplicated with element declared at AndroidManifest.xml:32:5-33:47
- **Warning:** Element uses-permission#android.permission.RECEIVE_BOOT_COMPLETED at AndroidManifest.xml:122:5-80 duplicated with element declared at AndroidManifest.xml:19:5-81
- **Warning:** Element permission#com.telekom.permission.REBOOT_APP at AndroidManifest.xml:124:5-128:44 duplicated with element declared at AndroidManifest.xml:78:5-82:44
- **Warning:** Element uses-permission#com.telekom.permission.REBOOT_APP at AndroidManifest.xml:130:5-73 duplicated with element declared at AndroidManifest.xml:25:5-73
- **Warning:** Element uses-permission#com.telekom.permission.SETTINGS.FINISH at AndroidManifest.xml:131:5-78 duplicated with element declared at AndroidManifest.xml:69:5-78
- **Warning:** Element permission#com.telekom.permission.SETTINGS.FINISH at AndroidManifest.xml:139:5-143:44 duplicated with element declared at AndroidManifest.xml:62:5-66:44
- **Warning:** Element permission#com.telekom.permission.LISTEN_TAG_RAIL_UPDATE at AndroidManifest.xml:145:5-149:44 duplicated with element declared at AndroidManifest.xml:96:5-100:44
- **Warning:** Element permission#com.telekom.permission.LOG_OUT at AndroidManifest.xml:153:5-157:44 duplicated with element declared at AndroidManifest.xml:84:5-88:44
- **Warning:** Element permission#com.telekom.permission.SESSION_VALUE_CHANGE at AndroidManifest.xml:158:5-162:44 duplicated with element declared at AndroidManifest.xml:90:5-94:44
- **Warning:** Element uses-permission#com.telekom.permission.LOG_OUT at AndroidManifest.xml:164:5-70 duplicated with element declared at AndroidManifest.xml:76:5-70
- **Warning:** Element permission#com.telekom.permission.FCM_SYNC_UPDATE at AndroidManifest.xml:167:5-171:44 duplicated with element declared at AndroidManifest.xml:102:5-105:43
- **Warning:** Element permission#com.telekom.permission.provider.READ_RETAIL_DATA at AndroidManifest.xml:175:5-179:44 duplicated with element declared at AndroidManifest.xml:107:5-111:44
- **Warning:** Element permission#com.telekom.permission.provider.WRITE_RETAIL_DATA at AndroidManifest.xml:180:5-184:44 duplicated with element declared at AndroidManifest.xml:112:5-116:44
- **Warning:** Element uses-permission#android.permission.READ_PRIVILEGED_PHONE_STATE at AndroidManifest.xml:185:5-186:47 duplicated with element declared at AndroidManifest.xml:119:5-121:47
- **Warning:** Element uses-permission#android.permission.ACCESS_NOTIFICATIONS at AndroidManifest.xml:188:5-190:47 duplicated with element declared at AndroidManifest.xml:42:5-44:47
- **Warning:** Element uses-permission#android.permission.MODIFY_PARENTAL_CONTROLS at AndroidManifest.xml:191:5-193:47 duplicated with element declared at AndroidManifest.xml:45:5-47:47
- **Warning:** Element uses-permission#android.permission.READ_CONTENT_RATING_SYSTEMS at AndroidManifest.xml:194:5-196:47 duplicated with element declared at AndroidManifest.xml:48:5-50:47
- **Warning:** Element uses-permission#android.permission.READ_TV_LISTINGS at AndroidManifest.xml:197:5-75 duplicated with element declared at AndroidManifest.xml:51:5-75
- **Warning:** Element uses-permission#com.android.providers.tv.permission.ACCESS_WATCHED_PROGRAMS at AndroidManifest.xml:198:5-200:47 duplicated with element declared at AndroidManifest.xml:52:5-54:47
- **Warning:** Element permission#com.telekom.permission.AUDIO.STOP at AndroidManifest.xml:202:5-206:44 duplicated with element declared at AndroidManifest.xml:56:5-60:44
- **Warning:** Element permission#com.telekom.permission.SETTINGS.FINISH at AndroidManifest.xml:208:5-212:44 duplicated with element declared at AndroidManifest.xml:139:5-143:44
- **Warning:** Element uses-permission#com.telekom.permission.AUDIO.STOP at AndroidManifest.xml:214:5-73 duplicated with element declared at AndroidManifest.xml:68:5-73
- **Warning:** Element uses-permission#com.telekom.permission.SETTINGS.FINISH at AndroidManifest.xml:215:5-78 duplicated with element declared at AndroidManifest.xml:131:5-78
- **Warning:** Element permission#com.telekom.permission.REBOOT_APP at AndroidManifest.xml:217:5-221:44 duplicated with element declared at AndroidManifest.xml:124:5-128:44
- **Warning:** Element permission#com.telekom.permission.LOG_OUT at AndroidManifest.xml:223:5-227:44 duplicated with element declared at AndroidManifest.xml:153:5-157:44
- **Warning:** Element permission#com.telekom.permission.SESSION_VALUE_CHANGE at AndroidManifest.xml:229:5-233:44 duplicated with element declared at AndroidManifest.xml:158:5-162:44
- **Warning:** Element permission#com.telekom.permission.LISTEN_TAG_RAIL_UPDATE at AndroidManifest.xml:235:5-239:44 duplicated with element declared at AndroidManifest.xml:145:5-149:44
- **Warning:** Element permission#com.telekom.permission.FCM_SYNC_UPDATE at AndroidManifest.xml:241:5-244:43 duplicated with element declared at AndroidManifest.xml:167:5-171:44
- **Warning:** Element permission#com.telekom.permission.provider.READ_RETAIL_DATA at AndroidManifest.xml:246:5-250:44 duplicated with element declared at AndroidManifest.xml:175:5-179:44
- **Warning:** Element permission#com.telekom.permission.provider.WRITE_RETAIL_DATA at AndroidManifest.xml:251:5-255:44 duplicated with element declared at AndroidManifest.xml:180:5-184:44
- **Warning:** Element uses-permission#android.permission.ACCESS_NETWORK_STATE at AndroidManifest.xml:258:5-79 duplicated with element declared at AndroidManifest.xml:71:5-79
- **Warning:** Element uses-permission#android.permission.INTERNET at AndroidManifest.xml:259:5-67 duplicated with element declared at AndroidManifest.xml:38:5-67
- **Warning:** Element uses-permission#android.permission.ACCESS_NETWORK_STATE at AndroidManifest.xml:264:5-78 duplicated with element declared at AndroidManifest.xml:258:5-79
- **Warning:** Element uses-permission#android.permission.READ_PHONE_STATE at AndroidManifest.xml:267:5-75 duplicated with element declared at AndroidManifest.xml:31:5-74
- **Warning:** Element uses-feature#android.software.leanback at AndroidManifest.xml:275:5-277:35 duplicated with element declared at AndroidManifest.xml:7:5-9:35
- **Warning:** Element uses-feature#android.hardware.touchscreen at AndroidManifest.xml:278:5-280:36 duplicated with element declared at AndroidManifest.xml:10:5-12:36
- **Warning:** Element uses-permission#android.permission.RECEIVE_BOOT_COMPLETED at AndroidManifest.xml:281:5-80 duplicated with element declared at AndroidManifest.xml:122:5-80
- **Warning:** manifest@android:allowBackup was tagged at AndroidManifest.xml:2 to replace other declarations but no other declaration present
- **Warning:** application@android:networkSecurityConfig was tagged at AndroidManifest.xml:284 to replace other declarations but no other declaration present

---

## :castlabplayer (Configure project)

- **Warning:** publishNonDefault is deprecated and has no effect anymore. All variants are now published.
- **Warning:** DSL element 'dexOptions' is obsolete and should be removed. It will be removed in version 8.0 of the Android Gradle plugin. Using it has no effect, and the AndroidGradle plugin optimizes dexing automatically.
- **Warning:** Configuration 'plProdCompileClasspath' was resolved during configuration time. This is a build performance and scalability issue. See https://github.com/gradle/gradle/issues/2298. Run with --info for a stacktrace.
- **Warning:** Configuration 'plProdRuntimeClasspath' was resolved during configuration time. This is a build performance and scalability issue. See https://github.com/gradle/gradle/issues/2298. Run with --info for a stacktrace.
- **Warning:** Configuration 'plProdProtobuf' was resolved during configuration time. This is a build performance and scalability issue. See https://github.com/gradle/gradle/issues/2298. Run with --info for a stacktrace.
- **Warning:** Configuration 'plProtobuf' was resolved during configuration time. This is a build performance and scalability issue. See https://github.com/gradle/gradle/issues/2298. Run with --info for a stacktrace.
- **Warning:** Configuration 'prodProtobuf' was resolved during configuration time. This is a build performance and scalability issue. See https://github.com/gradle/gradle/issues/2298. Run with --info for a stacktrace.
- **Warning:** Configuration 'protobuf' was resolved during configuration time. This is a build performance and scalability issue. See https://github.com/gradle/gradle/issues/2298. Run with --info for a stacktrace.

---

## :ott:autotranslator

- **Note:** Output path not specified, DeepLink doc is not going to be generated.
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar (com.airbnb:deeplinkdispatch-processor:4.1.0), auto-service-1.0-rc3.jar (com.google.auto.service:auto-service:1.0-rc3). Make sure all annotation processors are incremental to improve your build speed.
- **Note:** Some input files use unchecked or unsafe operations. Recompile with -Xlint:unchecked for details.

---

## :ott:coreinterface

- **Warning:** Parcelize annotations from package 'kotlinx.android.parcel' are deprecated. Change package to 'kotlinx.parcelize'. (AccountDetails.kt:7:1, AccountDetails.kt:34:1, AccountInfo.kt:7:1)
- **Note:** Output path not specified, DeepLink doc is not going to be generated.
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.

---

## :ott:core

- **Warning:** Parcelize annotations from package 'kotlinx.android.parcel' are deprecated. Change package to 'kotlinx.parcelize'. (CustomSnackBar.kt, AppStatus.kt, CampaignDetail.kt, Cta.kt, Onboarding.kt, etc.)
- **Warning:** Only safe (?.) or non-null asserted (!!.) calls are allowed on a nullable receiver of type 'String?'. (AnalyticsCommonDelegate.kt:80:34)
- **Warning:** This annotation is deprecated in Kotlin. Use '@kotlin.annotation.Retention' instead. (Cachable.kt:9:1)
- **Warning:** 'fun onBackPressed(): Unit' is deprecated. Deprecated in Java. (BaseToolBarFragment.kt:47:35)
- **Warning:** 'class NetworkInfo : Any, Parcelable' is deprecated. Deprecated in Java. (BaseActivity.kt:7:8)
- **Warning:** 'fun <T : Parcelable!> getParcelable(p0: String?): T?' is deprecated. Deprecated in Java. (BaseActivity.kt:55:29)
- **Warning:** 'val isConnected: Boolean' is deprecated. Deprecated in Java. (BaseActivity.kt:64:60)
- **Warning:** Condition is always 'true'. (BaseFragment.kt:52:13)
- **Warning:** Condition is always 'false'. (BaseFragment.kt:94:26)
- **Warning:** Condition is always 'true'. (BaseFragment.kt:95:13)
- **Warning:** Override 'fun timeout(): Timeout?' has incorrect nullability in its signature compared to the overridden declaration 'fun timeout(): Timeout'. (NetworkResultCall.kt:34:5)
- **Warning:** 'static fun obtain(): AccessibilityEvent!' is deprecated. Deprecated in Java. (ContextExtensions.kt:20:36)
- **Warning:** Expected performance impact from inlining is insignificant. Inlining works best for functions with parameters of function types. (DateTimeUtils.kt:32:1)
- **Warning:** 'val defaultDisplay: Display!' is deprecated. Deprecated in Java. (DeviceUtils.kt, ExtensionsKt.kt)
- **Warning:** 'fun getMetrics(p0: DisplayMetrics!): Unit' is deprecated. Deprecated in Java.
- **Warning:** 'static fun getDefaultAdapter(): BluetoothAdapter!' is deprecated. Deprecated in Java. (DeviceUtils.kt:121:64, 143:51)
- **Warning:** 'val activeNetworkInfo: NetworkInfo?' is deprecated. Deprecated in Java. (ExtensionsKt.kt:48:49)
- **Warning:** 'val isConnected: Boolean' is deprecated. Deprecated in Java. (ExtensionsKt.kt:49:59)
- **Warning:** Only safe (?.) or non-null asserted (!!.) calls are allowed on a nullable receiver of type 'String?'. (ExtensionsKt.kt:56:49, 56:83)
- **Warning:** 'fun setTextAppearance(p0: Context!, p1: Int): Unit' is deprecated. Deprecated in Java. (ExtensionsKt.kt:115:14)
- **Warning:** 'class CookieSyncManager : Any, Runnable' is deprecated. Deprecated in Java. (ExtensionsKt.kt:174:29)
- **Warning:** 'static fun createInstance(p0: Context!): CookieSyncManager!' is deprecated. Deprecated in Java. (ExtensionsKt.kt:174:47)
- **Warning:** Identity equality for arguments of types 'Int' and 'Int' is deprecated. (GmsUtil.kt:12:23)
- **Warning:** This annotation is currently applied to the value parameter only, but in the future it will also be applied to field. To opt in to applying to both value parameter and field, add '-Xannotation-default-target=param-property' to your compiler arguments. To keep applying to the value parameter only, use the '@param:' annotation target. See https://youtrack.jetbrains.com/issue/KT-73255 (NotificationConfiguration.kt:12, 14, 16)
- **Warning:** 'annotation class OnLifecycleEvent : Any, Annotation' is deprecated. Deprecated in Java. (AppLifecycleObserver.kt:5:8, 15:6, 23:6)
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.
- **Note:** Some input files use or override a deprecated API. Recompile with -Xlint:deprecation for details.
- **Note:** Some input files use unchecked or unsafe operations. Recompile with -Xlint:unchecked for details.

---

## :sharedresources

- **Note:** Output path not specified, DeepLink doc is not going to be generated.
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.

---

## :zappingdomain

- **Note:** Output path not specified, DeepLink doc is not going to be generated.

---

## :logger

- **Note:** Output path not specified, DeepLink doc is not going to be generated.
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.

---

## :logindomain

- **Note:** Output path not specified, DeepLink doc is not going to be generated.
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.

---

## :settingsdomainhome

- **Warning:** Condition is always 'true'. (RetailSettingsManager.kt:940:13)
- **Note:** Output path not specified, DeepLink doc is not going to be generated.
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.

---

## :subscriptiondomain

- **Warning:** Property will not be serialized into a 'Parcel'. Add '@IgnoredOnParcel' annotation to remove the warning. (SubscriptionFlow.kt:22, 23, 28, 29)
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.

---

## :ott:uielements

- **Warning:** This annotation is currently applied to the value parameter only, but in the future it will also be applied to field. To opt in to applying to both value parameter and field, add '-Xannotation-default-target=param-property' to your compiler arguments. To keep applying to the value parameter only, use the '@param:' annotation target. See https://youtrack.jetbrains.com/issue/KT-73255 (BottomBarData.kt:21:70)
- **Warning:** 'constructor(): Handler' is deprecated. Deprecated in Java. (CustomNavigationBottomBar.kt:90:9)
- **Warning:** 'fun setOnNavigationItemSelectedListener(p0: BottomNavigationView.OnNavigationItemSelectedListener?): Unit' is deprecated. Deprecated in Java. (CustomNavigationBottomBar.kt:96:9)
- **Warning:** 'interface OnNavigationItemSelectedListener : Any, NavigationBarView.OnItemSelectedListener' is deprecated. Deprecated in Java. (CustomNavigationBottomBar.kt:96:75)
- **Warning:** Parcelize annotations from package 'kotlinx.android.parcel' are deprecated. Change package to 'kotlinx.parcelize'. (ActionsItem.kt, ChannelSubscriptionData.kt)
- **Warning:** 'var view: View?' is deprecated. Deprecated in Java. (AppCustomToast.kt:25:15, 58:15)
- **Warning:** '@JvmOverloads' annotation has no effect for methods without default arguments. (AppImageView.kt:31:5, 36:5; GlobalAttributesMarkupView.kt:11:5)
- **Warning:** 'val adapterPosition: Int' is deprecated. Deprecated in Java. (CastNdCrewAdapter.kt:51:62)
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.
- **Note:** Some input files use or override a deprecated API. Recompile with -Xlint:deprecation for details.

---

## :ott:cmsdomain

- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.

---

## :ott:onetvsettingdomain

- **Warning:** Parcelize annotations from package 'kotlinx.android.parcel' are deprecated. Change package to 'kotlinx.parcelize'. (ContentLock.kt, ContentSessionType.kt)
- **Warning:** The corresponding parameter in the supertype 'ISettingGateWay' is named 'parentalPref'. This may cause problems when calling this function with named arguments. (SettingsGateway.kt:38:40)
- **Warning:** Condition is always 'false'. (SecuritySettingsManger.kt:87:13, 125:13)
- **Warning:** Condition is always 'true'. (SettingsService.kt:64:29)
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.

---

## :cms

- **Warning:** Only safe (?.) or non-null asserted (!!.) calls are allowed on a nullable receiver of type 'Array<(out) File!>?'. (CmsStoreGatewayImpl.kt:265:26)
- **Warning:** The corresponding parameter in the supertype 'Any' is named 'other'. This may cause problems when calling this function with named arguments. (CmsConfig.kt:226:25)
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.

---

## :ott:appcommons

- **Warning:** Parcelize annotations from package 'kotlinx.android.parcel' are deprecated. Change package to 'kotlinx.parcelize'. (NavContext.kt, MetadataItem.kt, TagRailItemData.kt, WatchlistEntity.kt, ContinueRail.kt, BookmarksApiResponse.kt, ProtectedRecording.kt, RecordinContentDetailDTO.kt, ChannelModel.kt, RailComponentItemInfo.kt, FindabilityView.kt)
- **Warning:** Expected performance impact from inlining is insignificant. Inlining works best for functions with parameters of function types. (NewExtensions.kt:6:1, 14:1)
- **Warning:** The corresponding parameter in the supertype 'WatchlistLocalGateway' is named 'watchlistEntity'. This may cause problems when calling this function with named arguments. (WatchlistLocalService.kt:20:30)
- **Warning:** The corresponding parameter in the supertype 'WatchlistLocalGateway' is named 'id'. This may cause problems when calling this function with named arguments. (WatchlistLocalService.kt:80:52)
- **Warning:** The corresponding parameter in the supertype 'IBookmarksGateway' is named 'bookmarks'. This may cause problems when calling this function with named arguments. (BookmarksGateway.kt:38:33)
- **Warning:** This annotation is currently applied to the value parameter only, but in the future it will also be applied to field. To opt in to applying to both value parameter and field, add '-Xannotation-default-target=param-property' to your compiler arguments. To keep applying to the value parameter only, use the '@param:' annotation target. See https://youtrack.jetbrains.com/issue/KT-73255 (TagRailItemData.kt:130, 159; ItemBookmarks.kt:17; BookmarksApiResponse.kt:68, 87; RailComponentItemInfo.kt:113)
- **Warning:** Property will not be serialized into a 'Parcel'. Add '@IgnoredOnParcel' annotation to remove the warning. (RailComponentItemInfo.kt:168, 169, 170, 171)
- **Warning:** This declaration needs opt-in. Its usage should be marked with '@kotlinx.coroutines.ExperimentalCoroutinesApi' or '@OptIn(kotlinx.coroutines.ExperimentalCoroutinesApi::class)'. (NetworkRegainHelper.kt:99:13, 108:36)
- **Warning:** 'class NetworkInfo : Any, Parcelable' is deprecated. Deprecated in Java. (NetworkRegainHelper.kt:121:28)
- **Warning:** 'val activeNetworkInfo: NetworkInfo?' is deprecated. Deprecated in Java. (NetworkRegainHelper.kt:121:64)
- **Warning:** 'val isConnected: Boolean' is deprecated. Deprecated in Java. (NetworkRegainHelper.kt:122:55)
- **Warning:** Only safe (?.) or non-null asserted (!!.) calls are allowed on a nullable receiver of type 'CoreComponent?'. This will become an error in language version 2.3. See https://youtrack.jetbrains.com/issue/KT-71718. (FindabilityView.kt:126:35)
- **Warning:** 'static fun getInstance(): WorkManager' is deprecated. Deprecated in Java. (SyncRecordingWorker.kt:50:18)
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.
- **Note:** RecordState.java uses or overrides a deprecated API. Recompile with -Xlint:deprecation for details.
- **Note:** RetrySubjectContainer.java uses unchecked or unsafe operations. Recompile with -Xlint:unchecked for details.

---

## :ott:searchdomain

- **Warning:** Unchecked cast of 'MovieResultsDataSource' to 'DataSource<Int, MovieTvEntity>'. (MovieResultsSourceFactory.kt:24:128)
- **Warning:** 'class PositionalDataSource<T : Any> : DataSource<Int, T>' is deprecated. PositionalDataSource is deprecated and has been replaced by PagingSource. (PersonDetailDataSource.kt:4:8, 19:5; SearchResultsDataSource.kt:4:8, 18:5)
- **Warning:** Unchecked cast of 'PersonResultsDataSource' to 'DataSource<Int, Person>'. (PersonResultsSourceFactory.kt:21:73)
- **Warning:** Java type mismatch: inferred type is 'String?', but 'String' was expected. (SearchResultsDataSource.kt:33:55)
- **Warning:** Unchecked cast of 'TvResultsDataSource' to 'DataSource<Int, MovieTvEntity>'. (TvResultsSourceFactory.kt:24:128)
- **Warning:** Parcelize annotations from package 'kotlinx.android.parcel' are deprecated. Change package to 'kotlinx.parcelize'. (SearchResults.kt:24:1)
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.

---

## :ott:widgets

- **Warning:** 'fun thumbnail(p0: Float): GlideRequest<Drawable!>' is deprecated. Deprecated in Java. (GlideExt.kt:47:10, 138:10)
- **Warning:** 'class ItemKeyedDataSource<Key : Any, Value : Any> : DataSource<Key, Value>' is deprecated. ItemKeyedDataSource is deprecated and has been replaced by PagingSource. (RelatedDataSource.kt:5:8, 18:263)
- **Warning:** Unchecked cast of 'MutableList<RailComponentItemInfo?>' to 'MutableList<RailComponentItemInfo>'. (RelatedDataSource.kt:46:58, 72:58)
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.
- **Note:** HighlightsCarousel.java uses or overrides a deprecated API. Recompile with -Xlint:deprecation for details.
- **Note:** Some input files use unchecked or unsafe operations. Recompile with -Xlint:unchecked for details.

---

## :ott:epgdomain

- **Warning:** The corresponding parameter in the supertype 'EpgContentGateway' is named 'selfCareHeader'. This may cause problems when calling this function with named arguments. (EpgApiClient.kt:32:104)
- **Warning:** Parcelize annotations from package 'kotlinx.android.parcel' are deprecated. Change package to 'kotlinx.parcelize'. (Entitlements.kt, EpgData.kt multiple classes)
- **Warning:** Property will not be serialized into a 'Parcel'. Add '@IgnoredOnParcel' annotation to remove the warning. (EpgData.kt:119, 246, 321, 326, 332)
- **Warning:** 'fun readSerializable(): Serializable?' is deprecated. Deprecated in Java. (EpgSelectorData.kt:8:47)
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.

---

## :ott:tvauth

- **warn:** removing resource com.telekom.onetv.tv.pl:string/detail__detail_page__rent_sub_label_hours without required default value.
- **warn:** removing resource com.telekom.onetv.tv.pl:string/player__player__live_content without required default value.

---

## :appcommon

- **Warning:** 'val defaultDisplay: Display!' is deprecated. Deprecated in Java. (DeviceUtils.kt:17:35, 28:12)
- **Warning:** 'fun getMetrics(p0: DisplayMetrics!): Unit' is deprecated. Deprecated in Java. (DeviceUtils.kt:19:17, 28:27)
- **Warning:** Java type mismatch: inferred type is 'String?', but 'String' was expected. (WarningLabelMap.kt:35:35)
- **Warning:** 'static fun obtain(p0: Int): AccessibilityEvent!' is deprecated. Deprecated in Java. (LayoutAccessibilityHelper.kt:190:40)
- **Warning:** This annotation is currently applied to the value parameter only, but in the future it will also be applied to field. To opt in to applying to both value parameter and field, add '-Xannotation-default-target=param-property' to your compiler arguments. To keep applying to the value parameter only, use the '@param:' annotation target. See https://youtrack.jetbrains.com/issue/KT-73255 (DpadGridSpacingDecoration.kt:38, 39, 40; DpadLinearSpacingDecoration.kt:40, 41, 42; PublicIpResponse.kt:41, 46)
- **Warning:** 'static fun obtain(): AccessibilityEvent!' is deprecated. Deprecated in Java. (ContextExtensions.kt:34:36)
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.

---

## :homeuicompoent

- **warning:** ERROR: View field categoryTitle collides with a variable or import file://homeuicompoent/src/main/res/layout/highlight_view_item.xml Line:91
- **Warning:** 'constructor(): Handler' is deprecated. Deprecated in Java. (HighlightsCarousel.kt:96:46; HighLightWidget.kt:39:37)
- **Warning:** Java type mismatch: inferred type is 'MutableList<HighlightAdapter.ViewHolder>?', but '(Mutable)List<*>' was expected. (HighlightsCarousel.kt:1121:50)
- **Warning:** Modifier 'open' is redundant for 'interface'. (ISourceRepository.kt:15:1)
- **Warning:** Unchecked cast of 'HighLightViewModel' to 'T (of fun <T : ViewModel> create)'. (HomeUiComponentViewModelFactory.kt:20:57)
- **Warning:** Unchecked cast of 'TestActvityViewModel' to 'T (of fun <T : ViewModel> create)'. (HomeUiComponentViewModelFactory.kt:22:92)
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.

---

## :cmshome

- **warning:** com.telekom.onetv.homedata.model.Component's highlights property has type java.util.List<?> but its getter returns java.util.List<java.lang.Object>. This mismatch might cause unexpected highlights values in the database when com.telekom.onetv.homedata.model.Component is inserted into database. - highlights in com.telekom.onetv.homedata.model.Component
- **warning:** categoryId column references a foreign key but it is not part of an index. This may trigger full table scans whenever parent table is modified so you are highly advised to create an index that covers this column. - com.telekom.onetv.homedata.model.Component
- **warning:** componentId column references a foreign key but it is not part of an index. This may trigger full table scans whenever parent table is modified so you are highly advised to create an index that covers this column. - com.telekom.onetv.homedata.model.RailComponentItemInfo
- **warning:** componentId column references a foreign key but it is not part of an index. This may trigger full table scans whenever parent table is modified so you are highly advised to create an index that covers this column. - com.telekom.onetv.homedata.model.Highlight
- **warning:** Schema export directory was not provided to the annotation processor so Room cannot export the schema. You can either provide `room.schemaLocation` annotation processor argument by applying the Room Gradle plugin (id 'androidx.room') OR set exportSchema to false. (ATVDatabase.java:6)
- **Warning:** The corresponding parameter in the supertype 'Migration' is named 'db'. This may cause problems when calling this function with named arguments. (ATVDatabase.kt:58:34)
- **Warning:** 'fun fallbackToDestructiveMigration(): RoomDatabase.Builder<ATVDatabase>' is deprecated. Replace by overloaded version with parameter to indicate if all tables should be dropped or not. (ATVDatabase.kt:66:18)
- **Warning:** The corresponding parameter in the supertype 'InstalledApp' is named 'incluseSystemApps'. This may cause problems when calling this function with named arguments. (InstalledAppDataHelper.kt:44:43, 70:62)
- **Warning:** Identity equality for arguments of types 'Int' and 'Int' is deprecated. (InstalledAppDataHelper.kt:184:17, 184:72)
- **Warning:** Condition is always 'true'. (TIFProviderDataHelper.kt:637:29)
- **Warning:** This annotation is currently applied to the value parameter only, but in the future it will also be applied to field. To opt in to applying to both value parameter and field, add '-Xannotation-default-target=param-property' to your compiler arguments. To keep applying to the value parameter only, use the '@param:' annotation target. See https://youtrack.jetbrains.com/issue/KT-73255 (CmsKeyMappingResponse.kt:17, 19)
- **Warning:** 'static field FLAG_IS_GAME: Int' is deprecated. Deprecated in Java. (Utils.kt:108:47, 108:80)
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.

---

## :homeretaildomain

- **warning:** com.telekom.onetv.homedata.model.Component's highlights property has type java.util.List<?> but its getter returns java.util.List<java.lang.Object>. This mismatch might cause unexpected highlights values in the database when com.telekom.onetv.homedata.model.Component is inserted into database. - highlights in com.telekom.onetv.homedata.model.Component
- **warning:** categoryId column references a foreign key but it is not part of an index. This may trigger full table scans whenever parent table is modified so you are highly advised to create an index that covers this column. - com.telekom.onetv.homedata.model.Component
- **warning:** componentId column references a foreign key but it is not part of an index. This may trigger full table scans whenever parent table is modified so you are highly advised to create an index that covers this column. - com.telekom.onetv.homedata.model.RailComponentItemInfo
- **warning:** componentId column references a foreign key but it is not part of an index. This may trigger full table scans whenever parent table is modified so you are highly advised to create an index that covers this column. - com.telekom.onetv.homedata.model.Highlight
- **warning:** Schema export directory was not provided to the annotation processor so Room cannot export the schema. You can either provide `room.schemaLocation` annotation processor argument by applying the Room Gradle plugin (id 'androidx.room') OR set exportSchema to false. (HomeDb.java:6)
- **Warning:** 'fun fallbackToDestructiveMigration(): RoomDatabase.Builder<HomeDb>' is deprecated. Replace by overloaded version with parameter to indicate if all tables should be dropped or not. (HomeDb.kt:62:23)
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.

---

## :epgdomain

- **warning:** Schema export directory was not provided to the annotation processor so Room cannot export the schema. You can either provide `room.schemaLocation` annotation processor argument by applying the Room Gradle plugin (id 'androidx.room') OR set exportSchema to false. (ChannelLogoDb.java:5)
- **Warning:** This annotation is currently applied to the value parameter only, but in the future it will also be applied to property. To opt in to applying to both value parameter and property, add '-Xannotation-default-target=param-property' to your compiler arguments. To keep applying to the value parameter only, use the '@param:' annotation target. See https://youtrack.jetbrains.com/issue/KT-73255 (EpgAllChannelsManager.kt:42:5; LiveEpgManager.kt:140:5)
- **Warning:** Unchecked cast of 'List<Unit>' to 'ArrayList<ProgramModel>'. (EpgAllChannelsManager.kt:465:100)
- **Warning:** Condition is always 'true'. (LiveEpgManager.kt:715:33, 1973:92, 1991:92, 2720:20, 4138:59, 4141:51, 4941:33, 5155:26)
- **Warning:** The corresponding parameter in the supertype 'ProgramSlotResponseInterface' is named 'slotRequestInfo'. This may cause problems when calling this function with named arguments. (LiveEpgManager.kt:3877:52)
- **Warning:** Unchecked cast of 'ArrayList<ProgramModel?>?' to 'ArrayList<ProgramModel>'. (LiveEpgManager.kt:4132:51, 4143:44, 4608:27, 4616:31)
- **Warning:** Condition is always 'false'. (LiveEpgManager.kt:4138:59)
- **Warning:** Java type mismatch: inferred type is 'List<ChannelModel?>?', but '(Mutable)Collection<out ChannelModel?>' was expected. (LiveEpgManager.kt:5155:26)
- **Warning:** 'fun fallbackToDestructiveMigration(): RoomDatabase.Builder<ChannelLogoDb>' is deprecated. Replace by overloaded version with parameter to indicate if all tables should be dropped or not. (ChannelLogoDb.kt:28:23)
- **Warning:** Only safe (?.) or non-null asserted (!!.) calls are allowed on a nullable receiver of type 'Date?'. (EpgUtils.kt:94:9)
- **Warning:** Only safe (?.) or non-null asserted (!!.) calls are allowed on a nullable receiver of type 'SimpleDateFormat?'. (EpgUtils.kt:98:13, 100:13, 103:16)
- **Warning:** Java type mismatch: inferred type is 'Date?', but 'Date' was expected. (EpgUtils.kt:103:33, 184:47, 192:47, 192:86)
- **Warning:** Condition is always 'false'. (EpgUtils.kt:139:17)
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.

---

## :retailbase

- **warning:** Schema export directory was not provided to the annotation processor so Room cannot export the schema. You can either provide `room.schemaLocation` annotation processor argument by applying the Room Gradle plugin (id 'androidx.room') OR set exportSchema to false. (CmsKeymapDb.java:6)
- **Warning:** Condition is always 'false'. (PosterDataUtils.kt:784:43)
- **Warning:** The corresponding parameter in the supertype 'IRetailSettingsManager' is named 'queryType'. This may cause problems when calling this function with named arguments. (RetailSettingsManagerImpl.kt:55:42)
- **Warning:** Unchecked cast of 'AuthErrorTransparentViewModel' to 'T (of fun <T : ViewModel> create)'. (AuthErrorActivityViewModelFactory.kt:19:15)
- **Warning:** 'fun <T : Parcelable!> getParcelable(p0: String?): T?' is deprecated. Deprecated in Java. (QRCodeFragment.kt:60:33)
- **Warning:** 'static fun fromHtml(p0: String!): Spanned!' is deprecated. Deprecated in Java. (QRCodeFragment.kt:81:22)
- **Warning:** 'fun onBackPressed(): Unit' is deprecated. Deprecated in Java. (QRCodeFragment.kt:116:23)
- **Warning:** 'fun startActivityForResult(p0: Intent, p1: Int): Unit' is deprecated. Deprecated in Java. (RetailBaseActivity.kt:604:13, 606:13)
- **Warning:** This declaration overrides a deprecated member but is not marked as deprecated itself. Add the '@Deprecated' annotation or suppress the diagnostic. (RetailBaseFragment.kt:75:18)
- **Warning:** 'fun onActivityCreated(p0: Bundle?): Unit' is deprecated. Deprecated in Java. (RetailBaseFragment.kt:77:15)
- **Warning:** 'fun getRunningTasks(p0: Int): (Mutable)List<ActivityManager.RunningTaskInfo!>!' is deprecated. Deprecated in Java. (copy/RetailBaseActivity.kt:520:82)
- **Warning:** 'fun startActivityForResult(p0: Intent, p1: Int): Unit' is deprecated. Deprecated in Java. (copy/RetailBaseActivity.kt:638:13, 640:13)
- **Warning:** 'val connectionInfo: WifiInfo!' is deprecated. Deprecated in Java. (copy/RetailBaseActivity.kt:867:22)
- **Warning:** This declaration overrides a deprecated member but is not marked as deprecated itself. Add the '@Deprecated' annotation or suppress the diagnostic. (copy/RetailBaseFragment.kt:75:18)
- **Warning:** 'fun onActivityCreated(p0: Bundle?): Unit' is deprecated. Deprecated in Java. (copy/RetailBaseFragment.kt:77:15)
- **Warning:** This annotation is currently applied to the value parameter only, but in the future it will also be applied to field. To opt in to applying to both value parameter and field, add '-Xannotation-default-target=param-property' to your compiler arguments. To keep applying to the value parameter only, use the '@param:' annotation target. See https://youtrack.jetbrains.com/issue/KT-73255 (CmsKeyMappingResponse.kt:16, 23)
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.

---

## :loginui

- **warning:** ERROR: Method references using '.' is deprecated. Instead of 'viewModel.onUserNameChange', use 'viewModel::onUserNameChange' file://loginui/src/main/res/layout/login_username_password_fragmet.xml Line:151
- **warning:** ERROR: Method references using '.' is deprecated. Instead of 'viewModel.onPasswordChange', use 'viewModel::onPasswordChange' file://loginui/src/main/res/layout/login_username_password_fragmet.xml Line:177
- **warning:** ERROR: Method references using '.' is deprecated. Instead of 'viewModel.onUserNameChange', use 'viewModel::onUserNameChange' file://loginui/src/main/res/layout/login_native_user_pass_activity.xml Line:80
- **warning:** ERROR: Method references using '.' is deprecated. Instead of 'viewModel.onPasswordChange', use 'viewModel::onPasswordChange' file://loginui/src/main/res/layout/login_native_user_pass_activity.xml Line:139
- **Warning:** Only safe (?.) or non-null asserted (!!.) calls are allowed on a nullable receiver of type 'AccountSettings?'. This will become an error in language version 2.3. See https://youtrack.jetbrains.com/issue/KT-71718. (BaseLoginViewModel.kt:196:25; DeviceLimitExceededViewModel.kt:223:33; DeviceListViewModel.kt:257:33; LoginWebviewViewModel.kt:350:29)
- **Warning:** Java type mismatch: inferred type is 'AgeSlot?', but 'AgeSlot' was expected. This will become an error in language version 2.3. See https://youtrack.jetbrains.com/issue/KT-71718. (BaseLoginViewModel.kt:216:25; DeviceLimitExceededViewModel.kt:240:81; DeviceListViewModel.kt:274:81; LoginWebviewViewModel.kt:370:29)
- **Warning:** 'fun get(p0: String!): Any?' is deprecated. Deprecated in Java. (ErrorDialog.kt:59, 60, 61, 63)
- **Warning:** This declaration overrides a deprecated member but is not marked as deprecated itself. Add the '@Deprecated' annotation or suppress the diagnostic. (LoginActivity.kt:122:18)
- **Warning:** Check for instance is always 'true'. (LoginActivity.kt:130:48, 130:76, 130:106)
- **Warning:** 'fun onBackPressed(): Unit' is deprecated. Deprecated in Java. (multiple files in loginui)
- **Warning:** 'constructor(): Handler' is deprecated. Deprecated in Java. (DeviceListFragment.kt:161:17; LoginWebviewViewModel.kt:143:25)
- **Warning:** 'when' is exhaustive so 'else' is redundant here. (DeviceLimitExceededFragment.kt:241:13; DeviceListFragment.kt:391:13; OnBoardingFragment.kt:127:13)
- **Warning:** Unchecked cast in ViewModelFactory create() (DeviceListModelFactory, LoginViewModelFactory, ForgetPasswordQRViewModelFactory, LoginPageViewModelFactory, OnBoardingViewModelFactory, RegisterWithQrViewModelFactory)
- **Warning:** The corresponding parameter in the supertype 'Observer' is named 'value'. This may cause problems when calling this function with named arguments. (LoginWebviewFragment.kt:412:28)
- **Warning:** 'static field SOFT_INPUT_ADJUST_RESIZE: Int' is deprecated. Deprecated in Java. (LoginWebviewFragment.kt:580:71)
- **Warning:** 'annotation class OnLifecycleEvent : Any, Annotation' is deprecated. Deprecated in Java. (LoginWebviewViewModel.kt:10:8, 218:6)
- **Warning:** This annotation is currently applied to the value parameter only, but in the future it will also be applied to property. To opt in to applying to both value parameter and property, add '-Xannotation-default-target=param-property' to your compiler arguments. To keep applying to the value parameter only, use the '@param:' annotation target. See https://youtrack.jetbrains.com/issue/KT-73255 (LoginOption.kt:5:24; LoginWebViewModelFactory.kt:35:9)
- **Warning:** 'fun toggleSoftInput(p0: Int, p1: Int): Unit' is deprecated. Deprecated in Java. (OTPView.kt:252:33)
- **Warning:** 'static field SHOW_FORCED: Int' is deprecated. Deprecated in Java. (OTPView.kt:253:36)
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.

---

## :app (Task :app:compilePlProdKotlin / compilePlProdJavaWithJavac)

- **Warning:** Only safe (?.) or non-null asserted (!!.) calls are allowed on a nullable receiver of type 'Thread.UncaughtExceptionHandler?'. (TVApplication.kt:1163:21)
- **Warning:** 'field versionCode: Int' is deprecated. Deprecated in Java. (TVApplication.kt:1382:27)
- **Warning:** 'fun fallbackToDestructiveMigration(): RoomDatabase.Builder<OneTvDb>' is deprecated. Replace by overloaded version with parameter to indicate if all tables should be dropped or not. (AppModule.kt:126:14)
- **Warning:** Type argument for reified type parameter 'T' was inferred to the intersection of ['Comparable<*>' & 'Serializable']. Reification of an intersection type results in the common supertype being used. This may lead to subtle issues and an explicit type argument is encouraged. This will become an error in a future release. (RetailAppProvider.kt:822:29)
- **Warning:** Only safe (?.) or non-null asserted (!!.) calls are allowed on a nullable receiver of type 'Array<(out) File!>?'. (DeleteMkLogsWorker.kt:45:27)
- **Warning:** 'annotation class OnLifecycleEvent : Any, Annotation' is deprecated. Deprecated in Java. (FlavoredTVApplication.kt:5:8, 18:10)
- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar, auto-service-1.0-rc3.jar. Make sure all annotation processors are incremental to improve your build speed.
- **Note:** ConfigurationProvider.java uses unchecked or unsafe operations. Recompile with -Xlint:unchecked for details.

---

## Annotation processors (repeated across many modules)

- **Note:** The following annotation processors are not incremental: deeplinkdispatch-processor-4.1.0.jar (com.airbnb:deeplinkdispatch-processor:4.1.0), auto-service-1.0-rc3.jar (com.google.auto.service:auto-service:1.0-rc3). Make sure all annotation processors are incremental to improve your build speed.

---

## Compiler suggestion (repeated for Parcelize)

- **Suggestion:** Parcelize annotations from package 'kotlinx.android.parcel' are deprecated. Change package to 'kotlinx.parcelize'.

---

## Compiler suggestion (repeated for annotation target KT-73255)

- **Suggestion:** This annotation is currently applied to the value parameter only, but in the future it will also be applied to field. To opt in to applying to both value parameter and field, add '-Xannotation-default-target=param-property' to your compiler arguments. To keep applying to the value parameter only, use the '@param:' annotation target. See https://youtrack.jetbrains.com/issue/KT-73255

---

## Compiler suggestion (Room schema)

- **Suggestion:** Schema export directory was not provided to the annotation processor so Room cannot export the schema. You can either provide `room.schemaLocation` annotation processor argument by applying the Room Gradle plugin (id 'androidx.room') OR set exportSchema to false.

---

## Compiler suggestion (Room fallbackToDestructiveMigration)

- **Suggestion:** 'fun fallbackToDestructiveMigration(): RoomDatabase.Builder<T>' is deprecated. Replace by overloaded version with parameter to indicate if all tables should be dropped or not.

---

## Compiler suggestion (Data binding method reference)

- **Suggestion:** Method references using '.' is deprecated. Instead of 'viewModel.onUserNameChange', use 'viewModel::onUserNameChange'. Instead of 'viewModel.onPasswordChange', use 'viewModel::onPasswordChange'.

---

## Compiler suggestion (Parcel @IgnoredOnParcel)

- **Suggestion:** Property will not be serialized into a 'Parcel'. Add '@IgnoredOnParcel' annotation to remove the warning.
