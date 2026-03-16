# Android Build Warning Report

> Note: This report was generated from the conversation content available to the assistant. If your pasted build log in the chat was truncated or contained additional details not captured here, upload the full `build_output.log` and I'll regenerate a complete `.md` immediately.

## Summary
- Source: build output provided in conversation.
- Status: Best-effort parsing; some sections may be placeholders where the original log was incomplete or truncated.

---

## Module: app
### Manifest Warnings
- duplicated permission: QUERY_ALL_PACKAGES
- duplicated permission: READ_TV_LISTINGS
- duplicated permission: ACCESS_NETWORK_STATE

### Native Warnings
- Unable to strip libraries: libabr.so, libc++_shared.so
- Native library mismatch warnings (example entries)

---

## Module: ott:core
### Kotlin Warnings
- Parcelize annotations from package `kotlinx.android.parcel` are deprecated
- Nullable receiver unsafe call (possible NPE)

### Java Warnings
- Deprecated API usage

---

## Module: ott:uielements
### Kotlin Warnings
- Deprecated BottomNavigationView listener
- Parcelize deprecated usage

---

## Module: searchdomain
### Kotlin Warnings
- PositionalDataSource deprecated (Paging 3 migration recommended)

---

## Top action items (suggested)
1. Migrate deprecated `kotlinx.android.parcel` usages to `kotlin-parcelize`.
2. Replace PositionalDataSource with Paging 3 `PagingSource`.
3. Resolve duplicated permissions by consolidating manifest entries.
4. Investigate native library strip failures and ensure correct NDK/toolchain configuration.
5. Add a CI job to automatically generate this markdown report on build failures.

---

## How to regenerate a complete report (recommended)
1. Save full build output:
   ```bash
   ./gradlew :app:assemblePlProd --warning-mode=all > build_output.log 2>&1
   ```
2. Upload `build_output.log` here.
3. I'll parse it and produce a precise `.md` including warning counts and file/line references.

---

## Notes / Missing details
- The original pasted log appeared truncated in the conversation. If you confirm the full log is already in the chat and I'm mistaken, tell me and I'll re-run the parser. Otherwise, upload the file for a guaranteed complete report.

---
