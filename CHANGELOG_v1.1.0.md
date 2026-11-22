# AI AUTO EDITOR v1.1.0

## Editing intelligence updates
- Added word-by-word caption timing endpoint derived from timed transcript segments.
- Added audio onset/beat analysis endpoint using FFmpeg PCM extraction and an RMS-energy peak detector.
- Added Android CloudAiClient methods for word timestamps and beat analysis.
- Added clear metadata indicating word timestamps are estimates, not forced alignment.

## Important limitations
- Word timings are interpolated from segment timestamps; precise per-word forced alignment is not claimed.
- Beat detection is a lightweight onset estimator. It can miss syncopated or quiet beats and should be treated as editing guidance.
- Android APK compilation and device testing are still required.
