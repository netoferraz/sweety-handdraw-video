# sweety-handdraw-video

Installs one Skill that turns topics or articles into complete hand-drawn explainer videos.

- Entry point: [SKILL.md](SKILL.md)
- Project notes, install, and samples: [GitHub README](https://github.com/threerocks/sweety-handdraw-video)
- Starting options: [style, arms, voice, ads, watermark, and subtitles](references/production-options.md)
- Authoring tasks: [runtime format](references/runtime-contract.md)
- Subtitles and endings: [model design rules](references/subtitle-design.md)
- Bundled dependencies and conflict handling: [dependency routing](references/dependencies.md)

Narration supports a compatible local VoiceStudio service, or VoxCPM2 once installed and explicitly configured; verified exported audio can be reused offline. Timbres are limited to realistic male, realistic female, sweet senior, Iapetus, intellectual female lecturer, and story narrator. The first two ship excerpts from the corresponding finished films and need local original references for production; the other four ship synthesized references and same-text previews — see [Local voice runtime](references/local-runtime.md) for usage. New pictures additionally need image tooling, Bun, and ExifTool; Python and FFmpeg handle local compositing. Bundled reference guides do not need separate Skill installs.

### v1.2: whole-script voice first

Generate/record the full narration and verify it before locking the master; pictures and subtitles read the master timestamps. Production builds reject per-page audio stitching, fixed scene-change silence, breath trimming, and speed changes; master and timestamp hashes are verified, and the main narration is checked sample-by-sample for completeness. See `references/continuous-narration.md`.

### v1.2: specified original-film timbres and narration-only fix

Original-film timbres are no longer cloned on from synthesized previews. Imported references and verbatim scripts are hash-bound; VoxCPM2 uses the original reference plus the accurate verbatim script by default; accepted local primary timbres live in the personal voice directory, not in the public package. Reference cleanup, same-text comparison, body-text short samples, and full narration are accepted separately; no added score does not mean the narration itself is noise-free.

Subtitles default to white with black outline; duplicate subtitles are forbidden; editing subtitles live separately in `outputs/editing/`. Public presets and showcase assets use GitHub Release links, see [Hosting & caching](references/hosted-assets.md). The current female voice is provisional; the original-film reference is kept and tuning continues; this is not final listening acceptance.

### v1.2.1: stronger art-direction checks

Every image must read the bundled art direction first, with a non-empty brief, the actual prompt, and bound content hashes; real view conclusions are filled in after generation. Missing records, empty files, or changed prompts/images stop production. See the mandatory flow in SKILL.md.

### v1.2.3: timbre catalog narrowed

Picker, source, and guides are unified on the six items above. Legacy entries inside personal or task directories are not added back to the catalog, and old names do not override current names. The current realistic female voice remains provisional.
