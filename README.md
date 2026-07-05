# Global Voice AI Translator — Flutter Skeleton

A working, functional starting point for the app described in the spec.
This covers the **Volume 1 (System Architecture)** and the mobile-facing
slice of Volume 4 (AI Engine), using only free/on-device APIs — no keys
required to run it.

## What actually works right now

| Feature | Implementation | Cost |
|---|---|---|
| Text translation | [LibreTranslate](https://libretranslate.com) public API | Free |
| Speech-to-text | `speech_to_text` (on-device OS engine) | Free |
| Text-to-speech | `flutter_tts` (on-device OS engine) | Free |
| Camera OCR | Google ML Kit Text Recognition (on-device) | Free |
| Live Conversation Mode | Two-pane UI built on the same STT/TTS/translate pipeline | Free |
| History & Favorites | Local persistence via `shared_preferences` | Free |
| Onboarding, theming, navigation | Material 3 + `go_router` | — |

Things marked "Premium" in the spec (offline packs, voice cloning, call/
meeting translation, Stripe/Apple Pay/Google Pay checkout) are stubbed as
UI only — they need paid infrastructure (server, billing account) that
can't be wired up without your credentials.

## Architecture

```
lib/
  core/
    constants/     # API endpoints, storage keys, fallback language list
    router/         # go_router route table
    theme/          # Material 3 theme
  data/
    models/         # LanguageModel, TranslationEntry
    services/       # TranslationService, SpeechService, StorageService
  providers/         # Riverpod state (languages, history, translate flow)
  presentation/
    screens/         # One file per screen
    widgets/         # Shared widgets (language picker, mic button)
```

This is intentionally provider-agnostic: `TranslationService` is an
abstract interface. `LibreTranslateService` implements it today. Swapping
in Google Translate, DeepL, or an OpenAI-based translator later means
writing one new class and changing one line in `providers/app_providers.dart`
— nothing in the UI changes.

## Running it

```bash
flutter pub get
flutter run
```

You'll need a device/emulator with microphone and camera for the voice and
camera features to do anything (they degrade gracefully with a permission
message if denied).

## Known limitations of this skeleton

- **LibreTranslate's public instance is rate-limited and sometimes slow.**
  For real use, self-host it (Docker image on the LibreTranslate GitHub) or
  swap in a paid provider once you have API keys.
- **No backend yet.** History/favorites are local-only; there's no login,
  no cloud sync, no admin dashboard. That's Volumes 2, 3, and 5 of the
  original spec — separate builds.
- **No voice cloning, call translation, or meeting translation.** These
  need either paid APIs (ElevenLabs, Twilio, Zoom/Meet SDKs) or your own
  infrastructure.
- **Not yet tested against a live Flutter SDK in this environment** (no
  network/Flutter toolchain available here) — review `pubspec.yaml`
  versions against the latest stable Flutter before running.

## Suggested next build

Given the full spec is 9 volumes, the next highest-leverage piece is
usually the **backend** (Volume 2): auth, a real translation-history API,
and a place for the admin dashboard to read from. Happy to build that
next if useful.
