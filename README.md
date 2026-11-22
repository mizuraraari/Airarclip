# AI AUTO EDITOR — Android project v1.0.0

Native Android source for short-form TikTok / YouTube Shorts editing, with an optional FastAPI + FFmpeg cloud backend.

## v1.0.0 — Smart Face Follow

- Cloud finalization optionally runs OpenCV Haar face detection on the rendered 9:16 clip.
- Samples at up to 2 frames/second and tracks the largest detected face center over time.
- FFmpeg applies a restrained 1.10x dynamic crop with piecewise-linear face-center motion, then scales back to 1080x1920.
- If no face is detected (or detection fails), the video still renders normally; no face identity is recognized.
- Backend response includes `X-Face-Tracking-Detections` and `X-Face-Tracking-Applied` headers.

## Existing editing pipeline

1. Select and preview a video in Android.
2. Choose duration, subtitle style, music mood, and creative preset.
3. When HTTPS cloud is configured: transcribe speech, analyze sampled frames, choose a highlight, and generate an original instrumental loop.
4. Android Media3 renders a base H.264/AAC vertical clip.
5. Cloud FFmpeg burns optional SRT subtitles, mixes optional WAV music, applies color preset, and follows a detected face when available.
6. Save the final MP4 to the Android gallery or share it.

## Build Android

Open this folder in Android Studio and sync Gradle. A Gradle wrapper is not included. Build a development APK with:

```bash
./gradlew assembleDebug -PAI_CLOUD_BASE_URL=https://your-domain.example -PAI_CLOUD_APP_TOKEN=your-development-token
```

Use Android Studio's installed Gradle if no wrapper is present. Do not put an OpenAI API key in the APK; set `OPENAI_API_KEY` only as a server-side secret. The static app token is for private development only; use authenticated short-lived tokens for public distribution.

## Run backend

```bash
cd backend
python -m venv .venv
. .venv/bin/activate
pip install -r requirements.txt
export OPENAI_API_KEY=...
export APP_ACCESS_TOKEN=...
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

Deploy behind HTTPS. Docker installs FFmpeg and the Python requirements install OpenCV headless for face detection.

## Important limitations

- This is a source project, not a prebuilt or device-tested APK. Android compilation and real-device QA remain necessary.
- Face follow is a cloud-side enhancement on the already-rendered 9:16 clip; it cannot recover image area removed by the initial device crop.
- Haar face detection can miss profiles, occlusion, low light, or small faces. If detection returns no faces, normal rendering continues. It is not identity recognition.
- AI scene analysis is based on sampled frames and transcript and may miss events between samples.
- Creative presets are deterministic FFmpeg filters, not generative video AI.
- Generated music is a simple original synthesized instrumental loop, not a licensed commercial music library.
- Cloud finalization uploads the rendered clip to your configured server. Configure upload limits, retention, privacy, and cost controls before public release.


## v1.1.0 Editing Intelligence
New backend endpoints:
- `POST /v1/word-timestamps` accepts `{ "segments": [{ "start": 0.0, "end": 1.0, "text": "..." }] }` and returns estimated word-level caption timings.
- `POST /v1/analyze-beats` accepts multipart audio and returns estimated BPM and onset timestamps.

Android `CloudAiClient` now exposes `estimateWordTimestamps(transcript)` and `analyzeBeats(audioFile)`.

Word timings are interpolated from segment timestamps, not true forced alignment. Beat detection uses a lightweight RMS onset heuristic, not a studio-grade beat tracker. Both features are intended as editing assistance and should be checked on varied audio before production use.

Run backend tests from `backend/` with:
`python -m unittest discover -s tests`


## v1.2.0 — Subtitle & Creative Presets
The app now includes **Pop Cyan** and **Compact** subtitle styles, plus **Anime**, **Gaming**, and **Vlog** creative color presets. These are conventional FFmpeg post-processing filters, not generative video effects. See `CHANGELOG_v1.2.0.md`.


## Web App
See `WEB_README_ID.md` for Indonesian setup and deployment instructions. The browser UI is served at `/` by the FastAPI backend.
