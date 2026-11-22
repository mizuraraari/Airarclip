# v1.0.0 — Smart Face Follow

- Added OpenCV headless to the cloud backend.
- Added sampled face-center detection (largest visible face, no identity recognition).
- Added piecewise-linear time expressions for dynamic FFmpeg crop positioning.
- Added a restrained 1.10x face-follow crop during cloud finalization.
- Added `face_tracking` multipart option and response headers reporting whether tracking was applied.
- Added graceful fallback: edits continue without face-follow when no face is detected.
- Bumped Android app version metadata to 1.0.0.
