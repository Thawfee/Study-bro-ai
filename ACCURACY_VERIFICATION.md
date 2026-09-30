# Expo Accuracy Verification

See `FINAL_VERIFICATION.md` for the checks run on this release and the
real-webcam tests to do before the expo.

## Detection changes in this release

- **Frames:** only new camera frames are analysed. The FPS badge now shows the
  real analysis rate, and calibration and eye smoothing no longer count the
  same frame several times.
- **Dozing off:** if the eyes were confirmed closed and the head then drops
  forward, the drowsiness timer keeps running instead of resetting. Looking
  down at a book with open eyes still cannot trigger drowsiness.
- **Far head turns:** if the face is lost while already looking sideways,
  StudyBro keeps treating it as distraction and waits twice as long before
  calling it "away".
- **Phone boxes:** a single false phone detection no longer leaves a red box
  on screen.
- **Pause:** Monitoring Paused now stops all detections, alerts and score changes.
- **Metrics:** live problem time stops counting the moment the student
  recovers (no extra 7 seconds per episode).
