# Final Verification

Verification of the fix release (30 September 2026). Everything below was run
on a clean install, not just read from the source.

## Automated checks

| Check | Result |
|---|---|
| `npm ci` | ✅ installs cleanly (postinstall copies the MediaPipe runtime) |
| `npm run typecheck` (`tsc --noEmit`) | ✅ 0 errors |
| `npm run lint` | ✅ 0 errors, 0 warnings (was 15 errors, 7 warnings) |
| `npm run build` | ✅ all 22 routes built, **with no internet access** (fonts are now bundled) |

## Browser tests (Chromium, both `next dev` and `next start`)

| Test | Result |
|---|---|
| AI Test Lab: saved trial survives opening `/lab` in dev mode | ✅ kept (previously erased on every visit) |
| AI Test Lab: record a trial, reload | ✅ both trials kept and shown |
| Personal Baseline: 5 saved sessions, 65 s idle, open Analytics | ✅ all 5 kept (previously replaced by 1 idle "session") |
| Dashboard study time after 65 s idle with camera off | ✅ 0s (previously counted as focused study) |
| Demo alert still starts a demo session and counts time | ✅ |
| Theme choice survives reload | ✅ |
| All 18 pages load with zero console errors | ✅ |
| Camera starts (fake test camera) and loads the local MediaPipe runtime | ✅ `wasm=local` |
| No models reachable: clear message pointing to `npm run setup-offline` | ✅ |

## What could not be tested here

The test machine had no access to Google's model servers, so live face and
phone detection (the models themselves) could not run end to end. Test the
following on your own laptop with a real webcam before the expo:

1. Run `npm run setup-offline` once with internet, then turn Wi-Fi off and
   confirm the monitor starts (console shows `face model: wasm=local, model=local`).
2. Calibrate, then close your eyes and let your head drop forward: expect a
   **drowsiness** alert (not distraction).
3. Turn your head far to one side for about 4 seconds: expect a **distraction**
   alert, not "away".
4. Press **Monitoring Paused** and try a phone or eyes-closed test: expect no
   alert and no score change.
5. Show a phone for about 1 second, then hide it: the red box should disappear.

Record the results in the AI Test Lab (`/lab`).
