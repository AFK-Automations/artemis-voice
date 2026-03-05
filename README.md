# Artemis Voice

**Voice dictation fix for [Artemis Android](https://github.com/ClassicOldSong/moonlight-android)** — a feature-rich fork of [Moonlight](https://github.com/moonlight-stream/moonlight-android).

## What This Fixes

Stock Moonlight/Artemis drops all text from Android IME `commitText()` calls. This means **voice dictation, swipe typing, and autocomplete** through Gboard (or any soft keyboard) silently fail — the text never reaches the Windows host.

This fork adds a custom `InputConnection` that intercepts those IME events and routes them to the remote host via the existing `NvConnection.sendUtf8Text()` protocol path.

| Input Method | Stock Artemis | Artemis Voice |
|---|---|---|
| Hardware keyboard | Works | Works |
| Gboard tap typing | Works | Works |
| Gboard voice dictation | **Broken** | **Fixed** |
| Gboard swipe typing | **Broken** | **Fixed** |
| Autocomplete suggestions | **Broken** | **Fixed** |

## Downloads

* [APK (Debug)](https://github.com/AFK-Automations/artemis-voice/raw/master/builds/artemis-voice.apk) — installs alongside stock Moonlight and Artemis

## Changes from Upstream

Only **3 files** changed (117 lines added):

1. **`StreamInputConnection.java`** (new) — Custom `BaseInputConnection` that routes `commitText()` → `NvConnection.sendUtf8Text()`
2. **`StreamView.java`** — Override `onCreateInputConnection()` to return `StreamInputConnection`
3. **`Game.java`** — Pass `NvConnection` reference to `StreamView`

Application ID changed to `com.mncompute.artemis.voice` so it installs alongside stock apps.

Internal package names remain `com.limelight.*` to minimize diff and simplify upstream merges.

## Building

```bash
# Prerequisites: JDK 17, Android SDK (compileSdk 34), NDK 27
git clone --recurse-submodules https://github.com/AFK-Automations/artemis-voice.git
cd artemis-voice
# Create local.properties with sdk.dir and ndk.dir paths
./gradlew assembleNonRootDebug
# Output: app/build/outputs/apk/nonRoot/debug/app-nonRoot-debug.apk
```

## Related

* **[moonlight-voice](https://github.com/AFK-Automations/moonlight-voice)** — Same voice fix applied to stock [Moonlight Android](https://github.com/moonlight-stream/moonlight-android)
* **Upstream issue:** [moonlight-stream/moonlight-android#1496](https://github.com/moonlight-stream/moonlight-android/issues/1496)
* **Upstream PR:** [moonlight-stream/moonlight-android#1556](https://github.com/moonlight-stream/moonlight-android/pull/1556)

## Credits

* [Artemis Android](https://github.com/ClassicOldSong/moonlight-android) by ClassicOldSong
* [Moonlight](https://github.com/moonlight-stream/moonlight-android) by Cameron Gutman et al.
* Voice dictation fix by [Jon Gutierrez / MN Compute](https://github.com/AFK-Automations)

## License

GPL-3.0-or-later (same as upstream)
