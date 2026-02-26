# Jenkins → GitLab Automated Build Flow for MediaKind SDK Testing

## Overview

This document explains the purpose, working architecture, and data flow
of the Jenkins-driven automated build system created for MediaKind SDK
validation within the OneTV platform.

The solution enables teams to dynamically generate application builds
without requiring manual code changes or developer intervention.

------------------------------------------------------------------------

## Purpose

Previously, MediaKind SDK testing required developers to:

-   Create temporary branch-level changes
-   Update MK Player version or replace AAR manually
-   Trigger GitLab pipelines
-   Share generated APK builds with MediaKind team

This resulted in: - Developer dependency - Increased turnaround time -
Manual coordination - Repeated build efforts

The new Jenkins orchestration flow eliminates these challenges by
providing a centralized and parameter-driven build mechanism.

------------------------------------------------------------------------

## System Architecture Flow

``` mermaid
flowchart LR
    User[User / MediaKind Team]
    Jenkins[Jenkins Job]
    GitLab[GitLab Pipeline]
    S3[(S3 Artifact Storage)]
    Artifact[Jenkins Artifacts]
    Download[APK Download]

    User -->|Build Parameters| Jenkins
    Jenkins -->|Trigger Pipeline| GitLab
    GitLab -->|Upload Build| S3
    Jenkins -->|Fetch Artifacts| S3
    Jenkins --> Artifact
    Artifact --> Download
```

------------------------------------------------------------------------

## Sequential Build Execution Flow

``` mermaid
flowchart TD
    Start([User Starts Jenkins Job])
    Split[Split Selected Build Types]

    Build1[Trigger GitLab Build - Type 1]
    Wait1[Wait for Completion]
    Download1[Download APK + Archive]

    Build2[Trigger GitLab Build - Type 2]
    Wait2[Wait for Completion]
    Download2[Download APK + Archive]

    BuildN[Trigger Next Build Type]
    WaitN[Wait for Completion]
    DownloadN[Download APK + Archive]

    Success([All Builds Completed])
    Fail([Abort Flow])

    Start --> Split
    Split --> Build1
    Build1 --> Wait1

    Wait1 -->|Success| Download1
    Wait1 -->|Failure| Fail

    Download1 --> Build2
    Build2 --> Wait2

    Wait2 -->|Success| Download2
    Wait2 -->|Failure| Fail

    Download2 --> BuildN
    BuildN --> WaitN

    WaitN -->|Success| DownloadN
    WaitN -->|Failure| Fail

    DownloadN --> Success
```

------------------------------------------------------------------------

## Data Flow

``` mermaid
sequenceDiagram
    participant User
    participant Jenkins
    participant GitLab
    participant S3
    participant Artifact

    User->>Jenkins: Start Build with Parameters
    Jenkins->>S3: Upload Custom AAR (Optional)
    Jenkins->>GitLab: Trigger Pipeline
    GitLab->>GitLab: Build Application
    GitLab->>S3: Upload APK
    Jenkins->>GitLab: Poll Pipeline Status
    Jenkins->>S3: Download APK
    Jenkins->>Artifact: Archive APK
    Artifact->>User: Download Build
```

------------------------------------------------------------------------

## Workflow Summary

### Step 1: User Input (Jenkins)

User triggers **Build with Parameters** and provides:

-   Source Branch
-   Build Types (Prod / Release / Debug etc.)
-   MK Version
-   Optional Custom MediaKind AAR

------------------------------------------------------------------------

### Step 2: Jenkins Orchestration

Jenkins performs the following:

1.  Cleans workspace
2.  Uploads custom AAR to S3 (if provided)
3.  Generates Jenkins Pipeline ID
4.  Triggers GitLab pipeline sequentially per build type
5.  Passes runtime variables to GitLab

------------------------------------------------------------------------

### Step 3: GitLab Execution

GitLab pipeline:

-   Checks out requested branch
-   Downloads custom AAR (if provided)
-   Applies MK Version configuration
-   Builds Android application
-   Uploads generated artifacts to S3

------------------------------------------------------------------------

### Step 4: Artifact Handling

After successful build:

-   Jenkins waits for pipeline completion
-   Downloads APK artifacts from S3
-   Stores APKs as Jenkins build artifacts

------------------------------------------------------------------------

### Step 5: Build Download

Users can download builds directly from:

Jenkins → Build History → Build Number → Artifacts

No direct S3 access required.

------------------------------------------------------------------------

## Execution Behavior

-   Builds execute sequentially
-   Failure stops entire flow
-   Jenkins abort cancels GitLab pipeline
-   Multiple APK outputs supported
-   Optional SDK override supported

------------------------------------------------------------------------

## Benefits

-   Removes developer dependency
-   Faster MediaKind SDK validation
-   Standardized build process
-   Reduced manual operations
-   Centralized build access
-   Improved traceability

------------------------------------------------------------------------

## Typical Use Cases

-   MediaKind Player validation
-   Custom multicast SDK testing
-   QA verification builds
-   Branch-specific validation
-   Release candidate generation

------------------------------------------------------------------------

## Ownership

Maintained by: OneTV Android & DevOps Team

------------------------------------------------------------------------

## Future Enhancements

-   Parallel controlled builds
-   Automated reporting
-   Artifact retention policies
-   Build analytics dashboard
