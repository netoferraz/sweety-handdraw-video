# Environment checklist

Applies to local v1.2. Installing the Skill, installing Python rendering dependencies, preparing image-generation capability, and deploying voice models are separate steps.

## Have the agent check these item by item before production

```text
Check the current version and full directory of sweety-handdraw-video.
Check Python, FFmpeg/ffprobe, Chinese handwriting font, subtitle font, Bun, ExifTool.
Confirm image generation is callable for this task.
Confirm one genuinely working voiceover path: local VoxCPM2, compatible VoiceStudio,
or the full narration and timestamps I provide.
Report missing items and the actually available path first; do not auto-download
large models, do not buy services.
After the environment passes, build one representative scene from my content first,
then complete the video.
```

## What each part needs

| Part | Depends on | Not provided by this step |
|---|---|---|
| Skill install | Installer, or manual copy of the full directory | Does not include agent subscription, image-generation rights, or TTS model weights |
| Local rendering | Python 3.9+; Pillow / NumPy / SciPy / requests; FFmpeg/ffprobe | Does not provision model services for you |
| Text | Handwritten Chinese font and regular subtitle font | System commercial fonts are not bundled |
| Image privacy | Bun, ExifTool, bundled cleanup script | Cleaning metadata does not guarantee passing AI-image detection |
| VoxCPM2 adapter | Currently the Apple Silicon / MLX path; installed model | Cannot be used directly as a CUDA / Windows deployment guide |
| VoiceStudio adapter | Compatible local HTTP API with configured model | Same-named desktop software is not necessarily compatible |
| Ready-made narration | Checked audio, full text, and timestamps | Without timestamps, alignment must still be done |
| Optional voice cleanup | Installed vocal-separation / denoise models | No guarantee timbre is undamaged; listen before and after |

## Hardware and support status

- Full sample production environment: author's M3 Max, 36GB RAM, macOS, Codex, local voice tooling.
- VoxCPM2-8bit weights are about 3.23GB; that is not minimum RAM. 8GB / 16GB machines have not completed minimum-config testing in this project.
- Linux has only v1.0.0 CI / short-film render verification, which does not equal v1.2 MLX voiceover verification.
- Windows, other agents, and other VoiceStudio backends have not completed the same end-to-end testing.
- Existing pictures, verified voice, and timestamps can reduce generation steps; whether fully offline works depends on whether these inputs are complete.

## Common blockers

| Symptom | Check first |
|---|---|
| Can write scripts, cannot produce pictures | Whether the host really has working image-generation capability |
| VoiceStudio connection fails | Whether the local service runs, and port and endpoints match |
| VoxCPM2 import fails | Whether you use the dedicated Python with MLX / mlx-audio, not the render venv |
| Chinese boxes or clipped glyphs | Whether fonts exist, glyph coverage fits, and layout was re-checked |
| Missing closing narration | Whether the full master skipped reads; do not mask missing words by stretching still frames |
| Noise despite no music | Check the original reference and generated narration themselves, not just the mix config |
| Specified ending voice not selectable | Whether the real original film and accurate verbatim script were imported; old previews cannot stand in |
| Fewer options after install | Check VERSION; public older packages exclude unpublished local changes |

See [Local runtime](../skills/sweety-handdraw-video/references/local-runtime.md) for detailed APIs, parameters, and import commands.

Public presets need GitHub download access on first use, then verified local caches can be used. Female-voice perceived quality has not passed final acceptance; the original recording stays on the author's local machine.
