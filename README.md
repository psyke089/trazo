# Trazo

A private, offline Android app in Kotlin and Jetpack Compose, scaffolded from the Android CLI's
`empty-activity` template.

- Package `app.trazo`, minimum Android 7.0 (API 24), compile/target SDK 37.
- No `INTERNET` permission: no accounts, analytics, ads or remote assets.
- Behaviour is specified in [docs/SPEC.md](docs/SPEC.md).

## Build

```sh
./tools/verify                    # unit tests, lint, debug APK
./gradlew :app:assembleRelease     # release APK, signed when a keystore is configured
```

Requirements: JDK 17+ (CI uses Temurin 21), Android SDK platform 37 and build-tools 36.0.0,
Gradle 9.1.0 through the included wrapper. The SDK path comes from `local.properties`
(untracked) or `ANDROID_HOME`; on a fresh machine `tools/install-android-sdk.sh` installs the
SDK packages.

Debug APK: `app/build/outputs/apk/debug/app-debug.apk`.

## Signing

Release builds stay unsigned unless the keystore is configured locally, outside the tree:

```properties
# ~/.gradle/gradle.properties
trazo.storeFile=/home/psyke/.android/keys/trazo/trazo-release.jks
trazo.storePassword=…
trazo.keyAlias=trazo
trazo.keyPassword=…
```

The `TRAZO_*` environment variables are the CI equivalent. Never commit the keystore or its
passwords; the key is the update identity, so back it up.

## Version

`versionCode`/`versionName` live in `app/build.gradle.kts` (`1`/`0.1.0` today). Release tags are
`v<versionName>`; CI asserts the tag against the version the built APK reports.
