# Global Voice AI Translator — Starter Scaffold

A real, runnable starting point for the app described in the spec: Flutter
mobile client + Node.js/Express backend with WebSocket streaming, wired for
Whisper / NLLB-200 / SeamlessM4T / GPT / Coqui XTTS integration.

This is a **scaffold**, not the finished 150-language, call/meeting-translating,
voice-cloning product — it gives you working auth, a working translate flow
end-to-end (stubbed AI providers), a WebSocket gateway for live conversation
mode, and a Postgres schema to build on. Every AI integration point is clearly
marked so you (or an AI coding assistant) can drop in real API calls.

## Structure

```
backend/     Node.js + Express + WebSocket API
mobile/      Flutter app (Riverpod, Material 3, clean architecture)
database/    PostgreSQL starter schema
```

## Backend quickstart

```bash
cd backend
cp .env.example .env      # fill in API keys as you get them
npm install
npm run dev                # starts on http://localhost:4000
```

Or with Docker (spins up Postgres, Mongo, Redis too):

```bash
cd backend/docker
docker compose up --build
```

Health check: `GET http://localhost:4000/api/v1/health`

### What's real vs. stubbed

| Area | Status |
|---|---|
| Express app, routing, middleware, error handling | ✅ Fully working |
| JWT auth (register/login/guest) | ✅ Working (in-memory store — swap for Postgres) |
| Rate limiting, helmet, CORS | ✅ Working |
| WebSocket gateway (rooms, live translation streaming) | ✅ Working plumbing |
| Whisper / NLLB-200 / SeamlessM4T / Coqui XTTS / GPT calls | 🔧 Stubbed — clearly marked integration points |
| Postgres schema | ✅ Core tables (users, subscriptions, history, AI usage, devices, errors) |
| Firebase Admin (push, OAuth verification) | 🔧 Stubbed — needs service account credentials |
| Stripe / Apple Pay / Google Pay | 🔧 Not yet wired (recommend RevenueCat to unify all three) |

## Mobile quickstart

```bash
cd mobile
flutter pub get
flutter run --dart-define=API_BASE_URL=http://localhost:4000/api/v1 \
            --dart-define=WS_URL=ws://localhost:4000/ws
```

### What's real vs. stubbed

| Area | Status |
|---|---|
| App shell, Material 3 theme (light/dark), routing | ✅ Working |
| Riverpod state management structure | ✅ Working |
| Text translation screen wired to backend | ✅ Working end-to-end (against stub responses) |
| Login screen (Google/Apple/Facebook/Email/Guest) | 🔧 UI built, provider SDK calls not wired |
| Voice, camera OCR, conversation mode, call/meeting translation | 🔧 Not built yet — natural next screens to add |
| Offline packs, favorites/history persistence (Hive) | 🔧 Dependencies included, not implemented |

## Suggested next steps

1. Stand up a self-hosted NLLB-200 + Coqui XTTS inference server (or start
   with DeepL/Google Translate + ElevenLabs while you validate the product).
2. Replace the in-memory user store with real Postgres queries.
3. Wire Firebase Auth in the Flutter app and exchange ID tokens via `/auth/oauth`.
4. Build out voice recording (`record` package) → `/translate/voice` flow.
5. Add camera OCR screen using `google_mlkit_text_recognition`.
6. Layer RevenueCat for premium entitlements (cleanest way to unify Stripe/Apple Pay/Google Pay).
