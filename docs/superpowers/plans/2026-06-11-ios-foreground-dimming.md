# iOS Foreground Dimming Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Bring the iOS app closer to Android by combining in-app dimming with temporary foreground brightness reduction and reliable state restoration.

**Architecture:** Keep Android-aligned business rules in the existing view model and persistence layer, add a focused brightness-session service for temporary `UIScreen.brightness` control, and rehydrate the session from scene lifecycle changes instead of promising unsupported system-wide overlay behavior.

**Tech Stack:** SwiftUI, UserDefaults, App Intents, ActivityKit, XCTest

---

### Task 1: Lock the new behavior in tests

**Files:**
- Modify: `NightVeil-iOS/NightVeilIOSTests/MaskHomeStateTests.swift`
- Create: `NightVeil-iOS/NightVeilIOSTests/MaskBrightnessServiceTests.swift`

- [ ] Add tests proving stopped iOS state still exposes the start action even when notification permission is undetermined.
- [ ] Add tests proving brightness engagement captures the original brightness once, updates from the stored baseline, and restores on release.

### Task 2: Implement foreground brightness session management

**Files:**
- Modify: `NightVeil-iOS/NightVeilIOS/Persistence/MaskPreferencesStore.swift`
- Create: `NightVeil-iOS/NightVeilIOS/Services/Display/MaskBrightnessService.swift`
- Modify: `NightVeil-iOS/NightVeilIOS.xcodeproj/project.pbxproj`

- [ ] Persist the managed baseline brightness in `MaskPreferencesStore`.
- [ ] Add a focused brightness service that can engage, update, suspend, and restore the foreground dimming session.
- [ ] Register the new source file in the Xcode project.

### Task 3: Wire lifecycle and UI behavior

**Files:**
- Modify: `NightVeil-iOS/NightVeilIOS/App/AppEnvironment.swift`
- Modify: `NightVeil-iOS/NightVeilIOS/Features/Home/HomeView.swift`
- Modify: `NightVeil-iOS/NightVeilIOS/Features/Home/HomeViewModel.swift`
- Modify: `NightVeil-iOS/NightVeilIOS/Services/Activity/MaskLiveActivityService.swift`
- Modify: `NightVeil-iOS/NightVeilIOS/Services/Capability/OverlayCapabilityService.swift`
- Modify: `NightVeil-iOS/NightVeilIOS/Resources/Localization/en.lproj/Localizable.strings`
- Modify: `NightVeil-iOS/NightVeilIOS/Resources/Localization/zh-Hans.lproj/Localizable.strings`

- [ ] Inject the brightness service through the environment and apply it when Night Veil starts or opacity changes.
- [ ] Suspend the brightness effect when the app leaves the foreground, and resume it when the app becomes active again if the user still has Night Veil marked as running.
- [ ] Keep Live Activity updates idempotent and update user-facing copy to explain the foreground-only enhancement clearly.
