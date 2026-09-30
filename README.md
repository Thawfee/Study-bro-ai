# StudyBro AI — Your AI Study Guardian

StudyBro AI is a local-first study companion that combines **real in-browser computer vision** with an explainable **Focus Fusion Engine**. It does more than detect a face: it fuses live behavior signals into a study state, decides when an intervention is warranted, and measures recovery.

This package includes the **Expo Accuracy** pipeline: a short personal calibration step, adaptive eye thresholds, relative head-pose baselines, detector-priority arbitration, multi-frame phone evidence, and anti-flicker face tracking.

## Quick start

```bash
npm install
npm run dev
```

Open `http://localhost:3000` in Chrome or Edge and allow camera access when prompted.

Production commands:

```bash
npm run build
npm run start
npm run lint
```

No environment variables are required for the local prototype.

## Offline mode (recommended for the expo)

The live AI needs three model files. By default the browser downloads them from public CDNs when the camera starts, so a bad venue Wi-Fi would stop the AI. To make StudyBro fully offline, run this **once** on a machine with internet:

```bash
npm run setup-offline
```

It copies the MediaPipe runtime into `public/mediapipe/wasm` (this also happens automatically on `npm install`) and downloads the face model and phone detector into `public/models/`. The app uses these local files when they exist and falls back to the CDNs when they don't. The browser console shows which source was used (`[StudyBro] face model: …`).

Fonts are bundled in `src/app/fonts/`, so `npm run build` also works without internet.

## Deploying

StudyBro is a static Next.js app with no backend, so it can be deployed on Vercel, Netlify or any Node host:

1. Push the project to a GitHub repository.
2. On [vercel.com](https://vercel.com), choose **Add New → Project**, import the repository, and keep the default Next.js settings.
3. Deploy. The camera works on the deployed site because Vercel serves it over HTTPS.

The camera requires HTTPS (or `localhost`). Opening the app on another device over plain `http://192.168.x.x` will block camera access.

## Main product flow

**Camera → Vision Analysis → Behavior Signals → Focus Fusion Engine → Study State → Intervention Engine → Recovery → Analytics**

StudyBro currently combines:

- face presence and confidence
- eye openness and drowsiness timing
- head pose / gaze direction
- phone object detection
- study-position / posture cues
- student presence / away state
- event duration and recent session history

The Focus Monitor turns those signals into explainable states such as **Deep Focus**, **Focused**, **Distracted**, **Fatigued**, **Intervention Required**, and **Away**.

## Real AI behavior

| Behavior | Implementation | Confirmation behavior |
|---|---|---|
| Phone | TensorFlow.js COCO-SSD + multi-frame evidence | configurable, ~2s confirmed evidence after latch |
| Face / eyes | MediaPipe Face Landmarker | 3.2s personal calibration + continuous signal |
| Drowsiness | adaptive Eye Aspect Ratio + median smoothing + pose gate | configurable, accuracy floor ~5.2s closed eyes |
| Distraction | calibrated relative head pose | configurable, accuracy floor ~3.2s sustained look-away |
| Presence | face-presence history + short tracking hold | configurable, accuracy floor ~6s away |
| Posture | calibrated face-position / distance cues | configurable, accuracy floor ~4.5s sustained issue |

The posture feature is a **study-position cue**, not a medical posture diagnosis.

The first camera start downloads the MediaPipe and TensorFlow.js models from their CDN sources. Inference then runs in the browser. Camera recordings are not stored by default.

## Intervention behavior

The interface exposes three intervention levels:

1. **Level 1 — Soft warning** for early warning signals.
2. **Level 2 — Strong warning** for confirmed problems when a video is not active.
3. **Level 3 — Video intervention** when the alert gate permits it.

For live camera interventions, the video is condition-controlled. Examples:

- phone present → video remains active → phone removed → short debounce → video stops automatically
- eyes closed beyond threshold → wake-up video remains active → eyes reopen → video stops automatically

Manual dismissal is disabled for live recovery-managed alerts so recovery time is measured from the actual live state instead of a button click.

## Sessions

A session starts when you start live camera monitoring (or run a demo), not when a page loads. Study time only counts while the camera is running, and **Monitoring Paused** stops all detections, score changes and alerts. Only live camera sessions feed the Personal Focus Baseline. Demo sessions never do.

## Recovery + session intelligence

StudyBro records live recovery durations when an intervention ends and uses those records for:

- average recovery time
- Focus Health breakdown
- Focus Timeline
- deterministic session summary from actual session metrics
- Personal Focus Baseline built from prior local sessions

No accuracy percentages or experiment results are invented.

## AI Test Lab

Open `/lab` to enter your own prototype evaluation trials for:

- drowsiness
- phone
- distraction
- posture
- presence

The lab calculates detection rate, false-positive rate, accuracy, and average detection time only from the trials you enter. It is labeled **Student-collected prototype evaluation**.

## Expo Mode + Judge Demo

Open `/expo` for the exhibition layout. It emphasizes:

- live AI monitor
- current Focus State
- live behavior signals
- intervention status
- recovery time
- timeline
- AI explanation

The **Judge Demo** is explicitly labeled Demo Mode and is disabled while live camera AI is active. This prevents a controlled simulation from being presented as live detection.

## Pages

`/` landing · `/login` · `/register` · `/dashboard` · `/focus` · `/timer` · `/tasks` · `/goals` · `/notes` · `/analytics` · `/lab` · `/expo` · `/achievements` · `/profile` · `/settings` · `/help` · `/privacy` · `/terms`

## Video alerts

Files in `public/videos/`:

- `sleep-1.mp4`
- `sleep-2.mp4`
- `general-alert.mp4`

Drowsiness uses one of the sleep clips. Phone, distraction, posture, and away use the general alert clip. Missing video files degrade to a standard notification rather than crashing the app.

## Adaptive settings

`/settings` lets you configure the live confirmation timing for:

- drowsiness
- distraction
- phone
- away
- posture

It also controls video alerts, cooldown, volume, autoplay, looping, and the minimal face-landmark overlay.

## Tech

Next.js 16 · React 19 · TypeScript · Tailwind CSS v4 · Lucide React · Recharts · TensorFlow.js / COCO-SSD · MediaPipe Tasks Vision.
