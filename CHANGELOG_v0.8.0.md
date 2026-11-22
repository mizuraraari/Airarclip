# AI AUTO EDITOR v0.8.0 — Creative Mode

## Added
- Creative Mode selector in Android UI: AI otomatis, Cinematic, Energetic, Clean.
- The app maps the AI analysis mood to a deterministic visual preset when AI otomatis is selected.
- Creative preset is sent as multipart `creative_mode` during cloud finalization.
- Backend applies the selected FFmpeg color/clarity filter after optional subtitle burn-in.
- Cloud finalization now also runs when no subtitle/music sidecar exists, so a creative visual preset can still be applied.
- Fixed the export call to pass subtitle style and creative mode explicitly into `exportClip` (the prior source referenced a local variable outside its scope).

## Current preset behavior
- `auto`: mild contrast and saturation.
- `cinematic`: stronger contrast, reduced saturation, vignette, and mild sharpening.
- `energetic`: stronger contrast/saturation and sharpening.
- `clean`: restrained contrast/saturation adjustment.

## Important limitations
- Presets run in the cloud finalization step; they require a configured HTTPS backend and FFmpeg. Without cloud configuration, the app still creates the basic local 9:16 clip but does not apply these presets.
- This release does not yet implement face tracking, keyframed/dynamic zoom, animated word-by-word captions, beat-synced transitions, or generated intro/outro title cards. Those remain future features, not completed features.
- The Android APK has not been compiled or device-tested in this environment.
