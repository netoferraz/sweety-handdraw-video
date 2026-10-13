# Sound & Local Dependencies

Requires Python 3.9+, Pillow, NumPy, SciPy, requests, ffmpeg/ffprobe. Body handwriting fonts prefer macOS Hannotate; subtitles use an independent regular Chinese font. Font fallback must not be treated as accepted hand-drawn styling; do not copy system commercial fonts into the package.

## Local Voice Capability

Prefer the currently configured local voice capability. When VoiceStudio is unreachable, use the task's explicitly configured local VoxCPM2; when both are unavailable and no ready-made audio exists, report the capability gap. A preference timeout never justifies inventing audio, purchasing, or installing services. Existing verified full narration and its timestamps can synthesize offline.

The VoiceStudio adapter talks to the local `http://127.0.0.1:3900` by default and reads the current engine before generating; the current configuration is `mlx-audio` / `mlx-community/Qwen3-TTS-12Hz-1.7B-Base-bf16`. It does not launch apps, switch engines, download models, or read credentials.

```bash
python3 scripts/voice_studio.py JOB --transcribe
```

The voice catalog `assets/voices/catalog.json` pins six entries in this order:

| Name | ID | Production Reference |
|---|---|---|
| Lifelike Male | `realistic-man` | Original recording reference actually used in "The Snake That Died of Anger" |
| Lifelike Female | `bilibili-outro` | Extracted original-film vocals currently used in "How Happiness Imprisons You" |
| Sweet Senior | `sweet-student` | Existing synthetic reference |
| Iapetus | `iapetus` | User-provided Gemini Iapetus synthetic reference |
| Intellectual Female Lecturer | `calm-woman` | Existing synthetic reference |
| Story Narrator | `story-narrator` | Independently kept story-narration synthetic reference |

The female voice keeps its legacy internal ID for old-task compatibility; the display name is uniformly Lifelike Female. Both lifelike voices' original recordings exist only in the local personal voice library or task directories; the public package provides finished-film excerpt auditions. Without imported original-film references, audition-only listening is allowed but generation is not. The other four provide same-text auditions and downloadable synthetic references. Iapetus is synthesized via a local model from references, not the native Gemini API. Story Narrator and Lifelike Male are kept separate and must not substitute for each other.

## Auditions & Reference Import

Voice names are selection labels only, not quality acceptance. The current Lifelike Female is provisional; its perceived quality still needs tuning. Excerpt auditions must never double as cloning references for the two original-film voices. After changing copy, models, or backends, still verify the same-text, short body-text sample, and complete narration.

```bash
python3 scripts/import_voice.py JOB --id realistic-man --name "Lifelike Male" --audio reference.wav --transcript reference.txt --kind source_recording
python3 scripts/import_voice.py JOB --id bilibili-outro --name "Lifelike Female" --audio reference.wav --transcript reference.txt --kind source_recording
```

Import results land in `options/local-voices.json`, overriding only same-named references for that task — no seventh entry may be added. When merging personal and task directories, names and original-film reference requirements follow the in-package catalog; entries outside these six and legacy defaults in old directories no longer take effect. `reference_audio/reference_text` may configure references explicitly; the text must match the recording character-for-character.

Do not upload audio or credentials to unauthorized services. Whole-piece narration is cached by complete text, parameters, model, and reference-audio hashes; untracked existing audio is never overwritten. ASR homophones must be judged against the source text; complete recognition never equals subjective listening acceptance.

## Installed VoxCPM2

The current Python environment must already have `mlx-audio`, `mlx`, `soundfile`, and the local model. Task settings `voxcpm:{"model_path":"actually installed model directory","voice":"story-narrator","timesteps":10,"cfg_value":2.0}`, executed with that environment:

```bash
python scripts/voxcpm_voice.py JOB
```

The adapter reads the same reference directory, caching by audio, text, model path, and parameters; it downloads no models and changes nothing in VoiceStudio. Both adapters merge all beats' text in original order into one complete generation request, targeting `narration.source` (`inputs/narration/master.wav` when unconfigured). VoxCPM2 defaults to `mode:combined`, constraining the voice with both the original reference and the accurate verbatim script; `reference` mode is for explicit selection only. When the reference overlaps the script's opening, continuation tends to skip the opening — check separately. `max_tokens` defaults to 6500 and may be set explicitly per installed-model capability; it is only a cap and never proves the generation is complete. After generation, check actual duration, full text, sentence endings, dropped and repeated readings, then build alignment per [Continuous Narration Convention](continuous-narration.md) before laying out visuals.

## Six Hands, Pen-Only & Advertising

`assets/hands/catalog.json` provides six arms plus the `pen-only` full pen, all with art and privacy records. Arms are scaled by nib-to-wrist distance so the forearm's bottom edge stays off-frame at the highest stroke. `pen-only` with `kind:pen` skips the forearm check, but the nib must still align.

`hand.pen_text` is at most 16 characters, typeset only on the measured barrel segment. Preserve the original text with no auto-abbreviation; the cap does not guarantee legibility on thin barrels — prefer wide barrels or pen-only assets for long ads, verified at normal video size. `pen_text_color` may override preset colors. Custom hands provide `source/nib/wrist`, plus `label_segment/label_height` for ads; custom pen-only provides `kind:pen/source/nib/scale`. All scales must be positive finite numbers.

## Image Privacy

The built-in original script runs under Bun; this pipeline requires ExifTool to avoid implicitly installing sharp. Run `python3 scripts/clean_image.py image.png --audit privacy.json`: first apply the packaged privacy cleanup, then strip simulated-camera fields, verifying metadata and residual extended attributes. Missing dependencies are reported accurately, never silently skipped.

## Original-Film Voices & Clean-Narration Acceptance

First check whether the original-film reference itself mixes in music, wind, or floor noise. Apply vocal separation or denoising when needed, preserving raw inputs, processing parameters, and before/after hashes; denoising can damage timbre — passing on small residuals or complete ASR alone is not allowed. Changed reference audio or verbatim transcripts must be re-imported; generation records keep actual reference paths, hashes, modes, and all parameters.

For designated original-film voices, first run a same-text comparison and another short body-text sample, then the complete continuous narration. Compare original film, cleaned original, synthetic short sample, and long-segment head/middle/tail at similar loudness. User-unapproved synthetic auditions can neither stand in for the original film nor be labeled verified primary voices. No background music only proves no extra mix layer; the narration's own floor noise, hum, synthesis artifacts, and dropped characters must be checked separately. Without actual listening or user listening feedback, status stays pending-listening-acceptance — never claim voice match or absolute noise-free status from signal metrics, ASR, or playable files.

## Local Primary Voice

The personal voice directory lives at `~/.local/share/sweety-handdraw-video/voices/catalog.json`, separate from the distributable skill package. It keeps user-designated references and their actual acceptance states; provisional voices must never be recorded as final listening passes. `default` denotes the machine's primary voice. Voice defaults prefer the `preferred: true` entry returned by `load_voices`; the user's explicit choice and in-task imports take priority. The catalog records actual references, verbatim transcripts, hashes, cleanup processes, audition samples, and acceptance scope; unvetted parameters must not carry over "passed" marks. Original-film recordings never enter the public package.

Accepted configurations may include `enhancement_model_path`, pointing only at an already-installed local DeepFilterNet3 model. The adapter folds its weight hash into the generation cache; 48kHz post-processing must preserve sample counts. No models are auto-downloaded. Processing itself never sets listening acceptance; newly generated body copy still requires acceptance.
