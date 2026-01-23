# Build Performance Notes - merge_ott

## What We Did So Far
- Tuned local build settings in `gradle.properties` for a 32 GB M2 Pro:
  - `org.gradle.parallel=true`, `org.gradle.workers.max=8`
  - `android.r8.maxWorkers=3`
  - `kotlin.daemon.jvmargs=-Xmx2g ...`
  - `android.r8.jvmArgs=-Xmx6g ...`
  - `org.gradle.jvmargs=-Xmx8g -XX:MaxMetaspaceSize=1g ...`
- Added CI-only Gradle overrides in `.gitlab-ci.yml` by writing to `$GRADLE_USER_HOME/gradle.properties`:
  - `org.gradle.jvmargs=-Xmx12g -XX:MaxMetaspaceSize=3g ...`
  - `kotlin.daemon.jvmargs=-Xmx2g ...`
  - `android.r8.jvmArgs=-Xmx6g ...`
  - `org.gradle.daemon=false`, `org.gradle.parallel=false`, `org.gradle.workers.max=1`
  - `android.r8.maxWorkers=1`
- Added CI memory introspection logs in `.gitlab-ci.yml` to read `/proc/meminfo` and cgroup limits.

## Metrics Achieved (Local)
- First run: `:app:assembleStandaloneAtProdDebug --profile`
  - Total build time: 13m36.99s
  - 2143 actionable tasks: 549 executed, 1594 up-to-date
  - Configuration cache: entry stored
- Second run: `:app:assembleStandaloneAtProdDebug --profile`
  - Total build time: 5m46.20s
  - 2143 actionable tasks: 12 executed, 2131 up-to-date
  - Artifact transforms: 0s (cache hit)
  - Configuration cache: reused (confirmed via `--info`)

## Findings (processors / kapt)
- `kotlin-kapt` is applied to all modules via `buildTask.gradle`, so kapt processors are configured broadly.
- Kapt processors in `dependencies.gradle` include:
  - Dagger compiler and Dagger Android processor
  - Airbnb DeepLinkDispatch processor
  - Glide compiler
  - Room compiler (in `app`)
- Dagger version is `2.25.2` (older; likely non-incremental).
- KSP plugin is applied in `app/build.gradle` but there are no `ksp(...)` dependencies.

## Concrete Improvements (Low Risk)
- Phase 1:
  1) Scope `kotlin-kapt` to only modules that require annotation processing.
  2) Remove the unused KSP plugin if no `ksp(...)` dependencies are present.
- Optional next steps (post Phase 1):
  - Upgrade Dagger to an incremental version after validating compatibility.
  - Investigate dependency graph size for `standaloneAtProdDebugRuntimeClasspath`.

## Phase 1 Plan (Implement #1 and #2)
1) Update `buildTask.gradle` to avoid applying `kotlin-kapt` globally.
2) Apply `kotlin-kapt` only in modules that use kapt dependencies.
3) Remove the unused `com.google.devtools.ksp` plugin from `app/build.gradle`.
4) Re-run `:app:assembleStandaloneAtProdDebug --profile` twice and compare.
