# NightVeil Context Bootstrap

## Scope

This document captures the currently verified root-level project context for the
`NightVeil` workspace. It summarizes what can be confirmed from committed files,
submodule structure, build scripts, architecture documents, and local read-only
inspection. It does not infer product intent or platform capability beyond what
the repository already states.

## Project Shape

- Root repository role: coordination workspace
- Main business code location:
  - `NightVeil-android/`
  - `NightVeil-iOS/`
- AI engineering workflow location:
  - `.ai/ai-engineering-skills/`
- Root-level documentation buckets:
  - `docs/ai-sessions/`
  - `docs/decisions/`
  - `docs/incident-history/`

## Repository Composition

Verified from `.gitmodules`:

- `.ai/ai-engineering-skills` is a Git submodule
- `NightVeil-android` is a Git submodule
- `NightVeil-iOS` is a Git submodule

This means the root repository primarily coordinates documentation, AI workflow
rules, and the Android/iOS project entry points instead of holding all product
source directly.

## Technology Stack

### Root repository

- Git-based multi-repository workspace using submodules
- Project-level AI workflow entry files:
  - `AGENTS.md`
  - `CLAUDE.md`

### Android project

Verified from `NightVeil-android/README.md`, Gradle files, manifest, and source:

- Native Android app
- Gradle Groovy DSL
- Android Gradle Plugin `8.13.2`
- Kotlin Android plugin `2.3.21`
- Gradle Wrapper `8.13`
- Single app module: `app`
- Namespace / application id: `com.nightveil.app`
- `minSdk 21`
- `compileSdk 36`
- `targetSdk 36`
- UI stack: Android Views/XML with ViewBinding
- Persistence: `SharedPreferences`
- Foreground service + overlay window model

### iOS project

Verified from `NightVeil-iOS/README.md`, Xcode project settings, and source:

- Native iOS app
- SwiftUI app shell
- Swift `5.0`
- iOS deployment target `17.0`
- Persistence: `UserDefaults`
- Capability abstraction for platform differences
- Live Activity and App Intents integration stubs

No committed evidence was found for CocoaPods, Swift Package Manager manifests,
or other external package management at the root of the iOS project.

## Module Structure

### Root level

- `.ai/`: AI engineering skills submodule
- `docs/`: root-level documentation container
- `NightVeil-android/`: Android implementation
- `NightVeil-iOS/`: iOS implementation

### Android modules and packages

Verified from `NightVeil-android/docs/architecture.md` and source layout:

- `MainActivity`: home screen orchestration
- `FloatingService`: foreground service and overlay lifecycle
- `ShortcutProxyActivity`: launcher shortcut entry
- `model/`: pure business and UI-state rules
- `data/`: `MaskPreferences`
- `overlay/`: overlay rendering and animation control
- `notification/`: foreground notification construction
- `utils/`: window and inset helpers

### iOS modules and folders

Verified from `NightVeil-iOS/docs/technical-design.md` and source layout:

- `App/`: app bootstrap and dependency wiring
- `Features/Home/`: home view and interaction flow
- `Domain/`: shared rules and state
- `Persistence/`: local storage
- `Services/Permission/`: notification permission logic
- `Services/Capability/`: capability decisions and messaging
- `Services/Activity/`: Live Activity stub
- `Services/Shortcuts/`: App Intents shortcut stub
- `NightVeilIOSTests/`: rule-level tests

## Cross-Platform Positioning

The repository already documents an explicit platform boundary:

- Android is implemented as a system-wide dimming overlay app
- iOS currently starts from an in-app dimming path
- System-wide overlay parity on iOS is documented as unverified

This boundary is confirmed in `NightVeil-iOS/README.md` and
`NightVeil-iOS/docs/technical-design.md` and should be treated as an active
product/technical constraint unless future evidence updates it.

## Build and Test Entry Points

### Android

Documented commands:

```powershell
.\gradlew.bat --offline testDebugUnitTest
.\gradlew.bat --offline assembleDebug
```

Verified test layout:

- Unit tests under `NightVeil-android/app/src/test/java/`
- Instrumentation tests under `NightVeil-android/app/src/androidTest/java/`

Verified coverage areas from committed tests:

- home UI state rules
- overlay opacity rules
- notification permission rules
- foreground service constants
- overlay animation math
- overlay state signal contract
- shortcut actions
- notification factory behavior
- window helper and inset helper logic
- preference key constants

### iOS

Verified test target:

- `NightVeilIOSTests`

Verified committed tests cover:

- home state resolution
- opacity rules
- overlay capability service

### CI

No committed CI configuration was found at the root workspace during this review:

- no `.github/workflows/`
- no `Jenkinsfile`
- no `bitrise.yml`
- no `fastlane/`
- no `.circleci/`

This does not prove CI does not exist elsewhere, only that it was not found in
the committed files reviewed in this workspace.

## Applicable AI Skills

Based on current repository shape, these project skills are the most relevant:

- `context-bootstrap`
- `requirement-analysis`
- `technical-design`
- `android-development`
- `ios-development`
- `android-to-ios-porting`
- `test-case-generation`
- `build-ci-fix`
- `code-review`
- `security-review`
- `performance-review`
- `session-retrospective`

Recommended default flow for cross-platform work:

1. `context-bootstrap`
2. `requirement-analysis`
3. `technical-design`
4. platform implementation skill
5. `test-case-generation`
6. `code-review`
7. `session-retrospective`

## Project Constraints

Verified from `AGENTS.md`, `CLAUDE.md`, and `skills/common/`:

- Read project entry rules and relevant skills before starting work
- Do not guess when code or docs do not confirm a fact
- Do not modify unrelated files
- Do not perform unrelated refactoring
- Do not introduce new dependencies without explicit confirmation
- Do not commit secrets, tokens, certificates, or private data
- High-risk modules must be designed first before implementation
- Outputs should clearly state verification method, verification result, and
  unverified items
- Only long-term, verified, reusable information should be added to durable docs

## Missing Context

The following root-level context is still missing or incomplete:

- A root-level architecture overview describing how the root repo, Android repo,
  iOS repo, and AI workflow submodule should be used together
- A root-level onboarding guide for cloning, submodule initialization, and
  per-platform local setup
- A root-level build and test guide spanning both Android and iOS
- A committed CI/CD configuration or a document that explains where CI lives
- A cross-platform capability matrix that records confirmed parity and known
  non-parity between Android and iOS
- A verified iOS build/run record from macOS/Xcode

## Verification Notes

### Verification method

- Read `AGENTS.md`, `CLAUDE.md`, common skill rules, and project skill docs
- Read root and subproject README files
- Read Android and iOS architecture/design docs
- Inspected root directory structure and key source folders
- Read Android Gradle files, manifest, and core runtime classes
- Read iOS Xcode project settings and core runtime classes
- Inspected committed tests and CI-related file locations
- Attempted documented Android Gradle commands in the local workspace

### Verification result

- Root project structure and submodule layout were confirmed
- Android and iOS stack judgments were confirmed from committed code/config
- Test locations were confirmed for both platforms
- No committed CI configuration was found in the reviewed workspace

### Not verified

- Android build success in the current machine environment
- Android instrumentation execution on device or emulator
- iOS Xcode build success
- iOS simulator or device behavior
- Live Activity runtime behavior
- App Intents registration behavior

### Blocking factor for local Android verification

The attempted Android Gradle commands failed in the current environment because
the Android SDK location is not configured. The repository requested either:

- `ANDROID_HOME`, or
- `local.properties` with `sdk.dir`

This is an environment issue, not source-level proof that the project cannot
build.
