# AGENTS.md

Guidance for coding agents working in this repository.

## Project

Trazo is a private, fully offline Android app built with Kotlin and Jetpack Compose. Package
`app.trazo`; minimum Android 7.0 (API 24). It starts from the Android CLI `empty-activity`
scaffold and has no product behaviour beyond that baseline yet.

- [docs/SPEC.md](docs/SPEC.md) — behaviour specification.
- [README.md](README.md) — overview, build and signing.

## Toolchain

- JDK 17+ (CI: Temurin 21), Gradle 9.1.0 through the wrapper, AGP 9.0.1, Kotlin 2.3.20.
- compileSdk/targetSdk 37, build-tools 36.0.0, minSdk 24. These are the scaffold's choice:
  don't re-pin or hand-replace the generated build files.
- The SDK path comes from `local.properties` (`sdk.dir`) or `ANDROID_HOME`; neither is tracked.
  `tools/install-android-sdk.sh` installs the platform and build-tools on a fresh machine.

## Device

A physical phone is attached over USB debugging. Check with `adb devices`; pass `-s SERIAL`
when more than one is listed. Use an emulator only if no device is available and the user
approved it.

Keep adb interaction non-destructive and limited to `app.trazo`: never factory-reset, wipe
storage or touch other apps. Instrumented runs need the app in the foreground — if a Compose
wait times out, check whether another app took focus before suspecting the app.

## Commands

```sh
./tools/verify                                                   # full gate
./gradlew testDebugUnitTest --tests 'app.trazo.*'               # focused JVM tests
./gradlew installDebug                                           # build and install
./gradlew assembleDebugAndroidTest
adb shell am instrument -w app.trazo.test/androidx.test.runner.AndroidJUnitRunner
```

## Verification policy

Run the focused test (or `tools/verify` for shared infrastructure) while iterating; run the full
`./tools/verify` before a release. Docs-only changes need no tests.

## Release

- `versionCode`/`versionName` live in `app/build.gradle.kts`; bump both before tagging and update
  the release badge in `README.md` when one exists.
- Tags are annotated `v<versionName>`. CI asserts the tag against the `versionName` the built APK
  reports; don't predict either by hand.
- Release signing material lives outside the tree at `~/.android/keys/trazo/trazo-release.jks`,
  with `trazo.storeFile`/`storePassword`/`keyAlias`/`keyPassword` in `~/.gradle/gradle.properties`
  (CI: the `TRAZO_*` secrets). Never commit keys or passwords. Losing the key means published
  installs can never be upgraded in place.

## Conventions

- Dependency bar: Compose BOM and AndroidX first; add a third-party runtime library only with a
  stated reason and a licence note in `docs/`.
- JVM tests go in `app/src/test`, instrumented tests in `app/src/androidTest`.
- Formatting follows `.editorconfig` (two-space Kotlin). No separate formatter is configured.
- Launcher assets and other images are scaffold-generated or user-supplied; record third-party
  provenance in `docs/` and keep licences in a notices file when adding assets.

## Out of scope

- App-store publishing, accounts, analytics, ads, crash-reporting SDKs and any network access.
- Accessibility work (semantics, contrast ratios, touch-target minimums, font scaling) unless
  explicitly requested.

## Autonomy

Pick a reasonable default, state the decision in the reply, and continue. Don't block on asset,
copy or naming questions.
