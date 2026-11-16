# Trazo — Product Specification

## 1. Product

Trazo is a private Android application. Package name: `app.trazo`.

This document specifies the scaffold the project starts from. Product behaviour beyond the
baseline below is not defined yet — write it here before implementing it.

## 2. Technology

- Kotlin, Jetpack Compose, Material 3.
- compileSdk/targetSdk 37, minSdk 24, JDK 17 bytecode.
- Navigation 3 (`androidx.navigation3`), lifecycle/ViewModel Compose integration.
- Fully offline: no `INTERNET` permission, no accounts, analytics, ads or remote assets.

## 3. Current behaviour

- Launch goes straight to the single screen; there is no onboarding.
- The centred top app bar shows the app name, `trazo`.
- Data flows from `DataRepository` through `MainScreenViewModel`, which exposes three states:
  `Loading` (centred progress indicator), `Success` (greeting rows) and `Error` (message).
- `DefaultDataRepository` emits one entry, `Android`, so a launched build renders the greeting
  `Hello Android !` and an OK button that currently does nothing.
- Edge-to-edge layout; the Material 3 theme follows the system light/dark setting, with dynamic
  color disabled.

## 4. Non-goals

- Accessibility work: screen-reader semantics, contrast ratios, touch-target minimums and
  font-scaling support are out of scope unless explicitly requested.
- App-store publishing, accounts, analytics, ads and any network access.
- Third-party runtime libraries without a stated reason; the Compose BOM and AndroidX set the bar.

## 5. Versioning

`versionCode` and `versionName` live in `app/build.gradle.kts` (`1` / `0.1.0` today). Release tags
are annotated `v<versionName>`. CI compares the tag with the `versionName` reported by the built
APK, so the build file and the tag must agree.
