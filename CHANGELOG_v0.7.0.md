# AI AUTO EDITOR v0.7.0 — AI Editing Intelligence

## Added
- Timestamped frame samples are sent from Android to the cloud API. The backend can now ground scene notes in the source timeline rather than relying on frame order alone.
- Vision analysis asks for a structured semantic scene breakdown: scene label, visible objects, observable action, whether people are present, and confidence.
- Vision analysis returns practical editing notes, a transcript/visual-grounded opening hook, highlight reason, and hook-strength score.
- Highlight selection on Android now weights the model's hook-strength score alongside segment duration.
- Android status summary displays the opening hook, a sample scene/object summary, and editing notes.
- Backward compatibility: clients that omit frame timestamps receive evenly spaced timestamp defaults.
- Android versionCode/versionName updated to 7 / 0.7.0; FastAPI version updated to 0.7.0.

## Safety and quality notes
- The model is instructed not to identify people or infer sensitive traits, and to describe only observable actions.
- Visual analysis is probabilistic. It can miss events between sampled frames or produce incorrect object/action labels; the transcript and source video remain the authority.
- Frame timestamps improve grounding but do not provide frame-by-frame motion tracking.

## Validation
- Python backend syntax compilation passed with `python -m py_compile backend/app/main.py`.
- Android Gradle build/device test was not run in this environment. The project does not include a Gradle wrapper; open it in Android Studio with a compatible Android SDK/Gradle setup to compile and test.
