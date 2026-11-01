# X-VOID AI — Android Voice Assistant

Starter source project for an Android voice assistant with realtime, interruptible speech.

## Included
- Android app (Kotlin + WebView)
- WebRTC voice client in the WebView
- Node.js backend that creates short-lived Realtime client secrets
- Indonesian X-VOID persona and server VAD interruption (`interrupt_response: true`)

## Important
This is source code, not a prebuilt APK. It has not been compiled or tested on a physical Android device here. Build it with Android Studio. OpenAI API usage is billed separately; a ChatGPT subscription does not include API credits.

Voice preset: `shimmer`. Voice presets do not guarantee a particular perceived gender/accent. Change `REALTIME_VOICE` in `.env` if desired.

## Requirements
- Android Studio stable, Android SDK 35, JDK 17
- Node.js 20+ and npm
- OpenAI API key with Realtime API access and billing enabled

## 1. Backend
```bash
cd backend
cp .env.example .env
```
Put your key in `.env`, then:
```bash
npm install
npm start
```
Backend listens on port 3000.

Backend URL in app:
- Android emulator: `http://10.0.2.2:3000`
- Physical phone: computer LAN IP, e.g. `http://192.168.1.10:3000`. Same Wi-Fi; allow port 3000 through firewall.
- Public use: deploy behind HTTPS, authentication, rate limits and restricted CORS. Do not expose this dev server publicly.

## 2. Build APK
1. Open the `android/` folder in Android Studio.
2. Sync Gradle; install SDK 35 if prompted.
3. Build > Build Bundle(s) / APK(s) > Build APK(s).
4. Output: `android/app/build/outputs/apk/debug/app-debug.apk`.
5. Install and grant microphone permission.

## 3. Use
Open app, set Backend URL, tap CONNECT, grant microphone, then talk. Server VAD detects speech and interrupts the assistant response when you begin speaking. Tap DISCONNECT to end.

## Troubleshooting
- 401: check backend `.env` API key.
- 429/quota: check API billing, project limits and Realtime access.
- Backend unreachable: check URL, Wi-Fi, firewall and port 3000.
- No mic: Android Settings > Apps > X-VOID AI > Permissions > Microphone.
- No sound: check media volume and Bluetooth routing.
- Model/voice unavailable: set `REALTIME_MODEL` and `REALTIME_VOICE` to values enabled for your API project.

## Security
Permanent API key stays on backend, never in APK. Android receives a short-lived client secret. Do not log/share it. Add authentication, rate limiting and HTTPS before production.

## Build using Android phone only (GitHub Actions)
1. Create a GitHub account and a new repository.
2. Upload all files and folders from this project to the repository (keep their folder structure).
3. Open the repository's Actions tab and run **Build X-VOID Android APK** (or push to `main`).
4. Open the completed workflow run, scroll to Artifacts, and download `x-void-ai-debug-apk`.
5. Extract the artifact ZIP on Android and install `app-debug.apk`.

No local Android Studio is needed. GitHub builds the APK on a cloud runner.

## Host backend using Render from phone
1. Push this project to GitHub.
2. In Render, create a new Blueprint and select this repository; Render reads `render.yaml`.
3. Set the secret `OPENAI_API_KEY` in the Render service environment.
4. Deploy and copy the service HTTPS URL.
5. In the app, enter `https://YOUR-SERVICE.onrender.com` as Backend URL.

Do not put your permanent OpenAI key in GitHub, the APK, or the app's Backend URL field.
