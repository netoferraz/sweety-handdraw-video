# Built-in Dependencies & Unified Routing

Install only `sweety-handdraw-video`. Five dependencies ship with the package, pinned to fixed source versions and file hashes; runs never auto-update upstream. Guides are uniformly named `GUIDE.md` with no extra entry points exposed to skill installers. Original directory structure, licenses, and third-party call resources are preserved; only Uncle Hua's repository promo images/GIFs were removed — animation methods and implementation resources remain. The change list is in `dependencies/vendor-manifest.json`, sources in `dependencies/lock.json`.

| Dependency & Entry | Role in This Pipeline | When to Load |
|---|---|---|
| [Art Direction](../dependencies/sweety-image-art-direction/GUIDE.md) | Subjects, contact forces, composition, media, hierarchy, viewing size | Before every image generation or edit — required |
| [Hand-Drawn Styles](../dependencies/hand-drawn-styles/GUIDE.md) | Complete style recipes and required reference anchors; also read PROTOCOL.md | When selecting an in-catalog style or auto-matching in-catalog; independent inference and custom descriptions do not load its recipes |
| [AI Director](../dependencies/omniailab-ai-director/references/FILM-ART%20GUIDE.md) | Content, storyboarding, composition, rhythm, audio-visual sync, final review | Planning and final review; for non-default narratives also read the matching DIRECTOR-VISION section |
| [HuaShu Art Motion](../dependencies/huashu-art-motion/GUIDE.md) | Specific art styles, video motion mechanics, and narration grammar | Only when the input matches one of its art-motion types |
| [Image Privacy](../dependencies/sweety-image-privacy/GUIDE.md) | Metadata and macOS source-attribute cleanup | All generated images; reuse records for already-cleaned, unmodified files |

## Conflict Resolution

The user's explicit request for this session takes priority; this skill owns the unified operating conventions for hand-drawn explainer films. Upstream materials apply only within their roles in the table:

- Art style follows this skill's text-and-image selection window. When unspecified, wait at most 60 seconds, then adopt the model's content-based plan and continue. Director candidates add no fixed pauses. Independent-inference and custom modes must not be taken over by hand-drawn recipes.
- Upstream 3-second intros, fixed cases, and frame templates do not override this pipeline's 1-second cover, content-based storyboarding, and actual-voiceover durations.
- Upstream platforms, models, and voice vendors are not auto-enabled; this pipeline uses the currently available built-in image generation, VoiceStudio, explicitly configured local VoxCPM2, and local synthesis. No auto-executed paid generation, logins, model downloads, or publishing.
- Upstream whole-block fades, uniform scan-line text, and similar implementations must not replace per-character isolation, independent strokes, or part-by-part outline-then-fill.
- Art direction is required for every image in this video pipeline, without changing its standalone version's manual-trigger-only rule.
- The privacy upstream script writes simulated-camera fields; this pipeline removes them immediately after calling it. Use `scripts/clean_image.py`, which runs the original cleanup first, then strips EXIF/GPS/XMP/IPTC and saves a verification record.
- Formal characters, subtitles, and ad text are typeset with controlled font layers; AI copy inside hand-drawn images serves as draft only.

## Installation Boundaries

What ships are guides, code, and preset assets — this does not make VoiceStudio, Python, FFmpeg, Chinese fonts, or image-generation capability "already installed". VoiceStudio generates and edits narration; already-exported accurate audio can be synthesized offline. Some hand-drawn styles' original reference anchors download and verify against the upstream manifest on first use — that is asset fetching, not installing another skill.

For maintenance, update one dependency in the working copy first, diff the changes, adjust this routing, run regressions, then update the lock records. Do not overwrite the user's other installed skills. Upstream sources serve as production methods only and grant no extra write or publish authorization.

The style dependency is verified against the latest upstream lightweight commit; this project's 21 previews use online links instead, and five runtime anchors add verifiable GitHub mirrors. Public presets are hosted independently, see [Asset Mechanism](hosted-assets.md).
