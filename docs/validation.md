# v1.2.3 six-timbre catalog (2026-10-13)

The current catalog keeps only realistic male, realistic female, sweet senior, Iapetus, intellectual female lecturer, and story narrator. Previews for the two realistic voices are extracted from released finished films; production references remain the local original recordings — no voices were re-synthesized and no finished films were modified. Legacy entries in personal and task directories cannot rejoin the picker; display names and original-film reference requirements are fixed by the main catalog.

New regressions verify that legacy entries, legacy defaults, and legacy names do not come back, and that the two finished-film previews stay separate from the original production references. The current female voice remains provisional; it must not be written up as listening-approved because of renaming. Historical version records below keep their original catalogs and check scopes.

# v1.2.2 cross-platform test fix (2026-10-13)

The pen-only test had hardcoded a macOS font path, failing Ubuntu CI at font loading. It now reuses the runtime's existing font-lookup function; Linux uses Noto CJK, macOS uses available system Chinese fonts. Assertions across all 57 tests are unchanged; production code is identical to v1.2.1. The v1.2 and v1.2.1 tags stay as-is.

# v1.2.1 art-direction patch check (2026-10-13)

The specified `sweety-image-art-direction/SKILL.md` and the project-bundled `GUIDE.md` share SHA-256 `fbbfd33d70f729011b6a6bc232851c116cd4a89ead2adea4462888a6ce26fc1a`.

The rules already covered every generated picture, but the program previously only checked that brief and prompt files exist. The patch adds non-empty and content-hash checks; bound commands never produce a visual-pass verdict. 7 new regressions cover all in-film image types, missing records, changed pictures, empty briefs/prompts, changed prompts, missing bindings, direction-version changes, unreviewed output, and blank review evidence. 57 regressions passed in total; offline short-film compositing passed. Public assets keep using the fixed v1.2 links.

These checks prove materials are complete and unreplaced; they do not prove the model truly understood the art direction, nor do they replace actual visual review. The v1.2 sample videos and audio are unchanged.

# v1.2 lock check (2026-10-13)

- 50 regressions: duplicate subtitle events, overlaps, extra subtitle tracks, adjacent subtitle files, black outlines; hosted-asset download, verification, offline cache, and cache corruption; existing drawing, voice, ending, and option regressions.
- 1,215 files across five bundled dependencies verified; exactly one Skill entry; 21 style previews switched to GitHub Release URLs.
- Offline end-to-end short-film compositing at 960×540 / 24fps, full H.264/AAC decode and checks passed.
- "How Happiness Imprisons You" keeps 40 subtitles on a single white-with-black-outline static subtitle layer. Old and new AAC track SHA-256 are both `cfca84c3a4ab3b441f17c88e76ef341f6ccfb5f97994d57bf3369b729f969a9e`; no voices were re-synthesized.
- A user reported duplicate subtitles; re-examined decoded frames of the old MP4 showed only one gray-text layer, with 0 embedded subtitle tracks and 0 web `<track>` elements. A same-named SRT once sat in the same directory, so external player overlay is a possible source — not confirmed as the sole root cause. Layer, timing, encoded-subtitle-track, and same-directory-external paths are all blocked now.
- Original-film voice references stay on the author's local machine. The current female voice is provisional; the user explicitly noted a remaining perceived gap; it must not be written up as final listening approval.
- Public assets are no longer packed into the code bundle; they download on demand on first use, then caches are verified. Five upstream reference anchors have GitHub mirrors; three had source/simulated-camera fields cleaned with pixel-identical results; audits kept.
- Netlify hosting is paused because the team quota is exhausted; per user confirmation, GitHub is used instead. No deployment, top-up, or site change was made on Netlify.

Historical version records follow; they do not mean every subjective acceptance was redone this round.

# v1.0.0 check record

This record covers the v1.0.0 implementation and the revised "Murphy's Law" film. The first public release was numbered v1.0.0; earlier local v1/v2 were production iterations, not public versions of this standalone repo.

## Findings and fixes this round

| Issue | Cause | Fix and verification |
|---|---|---|
| Chinese text clipped at the bottom | Layer height added a fixed margin to font size without measuring the font's real descender | Per-glyph bounds and left/right offsets computed per character, 5px margin on all four sides; subtitles reuse the same measurement. The screenshots' "leave smoothness to preparation" and "allow things to go wrong" lines plus mixed Chinese-English lines were pixel-compared on large-canvas reference renders to ensure no glyph pixels are lost |
| Users still had to install the privacy Skill | Four content dependencies were bundled but privacy cleanup lived outside the bundle | Added the fifth bundled dependency with a cleanup adapter; simulated camera fields are stripped and verified after the original script runs |
| Installer could discover multiple Skills | Upstream directory held multiple SKILL.md entries | Bundled guides unified as GUIDE.md with references updated; the whole install bundle verified to hold exactly one SKILL.md |
| Narrative, cover, model, and other upstream rules could conflict | Multiple upstream flows copied directly with no unified responsibility boundary | Separately maintained dependency routing: content methods load on demand; 1-second cover, VoiceStudio, line-drawing rules, and local rendering unified under the main flow |
| Existing narration still blocked by an online-service check | Doctor mixed "new narration can generate" with "existing task can synthesize" | Service status and existing-audio completeness reported separately; offline exported audio can synthesize; new narration still needs VoiceStudio |
| VoiceStudio address config inconsistent | Adapter read only the CLI default address, ignoring task settings | CLI overrides task settings, task settings override the 3900 default; still local HTTP only |
| Duplicate or private sample constraints | Entry, README, and history notes duplicated, with old topics and local paths | Trimmed the entry; detailed rules stay in their reference docs; historical task notes and local source paths removed |
| FFmpeg 6.1 mix truncated | One filter graph combined loudness processing and mixing; Linux test showed 14.17 s of picture keeping only 11.2 s of audio | Loudness and mixing done in two steps, duration re-checked after mixing; original audio/picture duration gates kept. Offline shorts passed on both macOS and Linux |
| Cross-sentence draw timing could overflow | Page indexes and reading holds not validated first | Reversed/out-of-range indexes, negative or non-finite pauses intercepted; drawable time judged against budget after deducting inter-stroke gaps |

## Executed

- Skill structure check passed; `npx skills add threerocks/sweety-handdraw-video --skill sweety-handdraw-video --agent codex --copy --yes` installed from remote into an isolated directory with exactly 1 Skill; post-install structure and regressions passed.
- 5 drawing regressions passed: per-character isolation, complete glyphs, semantic-region outline/fill order, missing-region interception, prop ownership inside hair gaps.
- 1214 bundled dependency file hash checks passed, exactly one installable entry; preset files complete.
- 1274 files of an independently unpacked install bundle matched the release manifest one by one; in-bundle regressions passed again.
- 960×540, 24fps, ~14 s offline short-film compositing passed: varied audio durations, Chinese punctuation, small color blocks, pure line art, full H264/AAC decode, legacy task-config interception, and test fixtures barred from delivery.
- "Murphy's Law" 87 drawing elements checked; final MP4 fully decoded, 1920×1080, 30fps, 3749 frames, ~124.965 s, audio and picture durations consistent. Loudness ~-17.07 LUFS, peak -5.01 dBTP.
- Viewed cover, 8 finished pages, 64 draw states across 8 pages, plus 8 opening states from the final decoded video; text descenders intact, female long-nail hand used, brush text `@leo_xiaolei`.
- 24 voiceover segments and the assembled narration WAV reused byte-for-byte, keeping per-segment ASR and the standalone closing-line check. The mix-compatibility fix changed the final mix, so final-PCM sameness is not claimed; AAC decoded sample counts match; the H264 video stream is byte-identical to the fully play-checked version.
- New brush-text composites ran the bundled privacy cleanup; embedded metadata empty, macOS `com.apple.provenance` residue recorded. Reused production sources keep their existing cleanup records.

Final video SHA-256: `9b688937b94c801fb47d86cddd4301ba45899556c90042a051b5f5e3d474a477`.

## Verification limits

The full local flow ran on macOS; [the same run's code checks passed on Linux CI](https://github.com/threerocks/sweety-handdraw-video/actions/runs/38248732958), including 5 regressions, bundled-dependency integrity, 14-second short compositing, and packaging. Windows, other VoiceStudio backends, and arbitrary new topics were not verified.

No actual subjective listening was done; timbre emotion, naturalness, and speaker-perceived quality were not accepted. ASR and track consistency cannot replace listening. Continuous-play state and representative-frame checks do not equal manual frame-by-frame review; semantic regions still need model viewing. Chinese is glyph-path drawing; standard calligraphic stroke order is not guaranteed.


# v1.1.0 local upgrade check

- Text-picture style picker dialog: directories built dynamically from the actual STYLES.md, full recipe snapshots saved; the specified install build is 19 styles, the in-bundle version is 21. All preview sources match upstream pinned hashes; thumbnails ship offline with the bundle.
- 16 regressions passed: four selection paths, standalone-style vs. in-library recipe isolation, custom-description retention, legacy-selection interception, body-part completeness, outline-before-fill, continuous pixel paths, pen lifts despite shared endpoints, no-outline rejection, and explicit outline masks.
- 14.172 s, 960×540, 24fps isolated test film fully built, rendered, decoded, and timeline-checked. A test run is not a finished film; finalize still rejects it.
- Real browser check of all 19 pictures loaded; number search, effect-picture zoom, custom-description input, empty description not confirmable, and post-save file read-back all passed. That save happened in an isolated test directory and did not pick this production's style for the user.
- Assets scale proportionally to the target frame, fixing the old implementation that only shrank and never enlarged.

The new version ran no remote CI and proves nothing about arbitrary styles producing natural hand-drawn motion. Semantic regions, occlusion, and per-stroke intent still need actual viewing; representative-segment motion and full finished films need separate acceptance. The 14 seconds above are a run regression, not a new production sample.

## v1.1.1 production regression fix (local, unreleased)

- Regions support `exclude_polygons`: forearms and held objects in front of the torso can be excluded from the pre-drawn torso area; regressions verify those pixels emerge only from the correct later region.
- Short subtitle segments in cross-sentence pictures with no standalone draw action no longer wrongly take the single-sentence draw budget.
- 18 automated regressions passed. Three actual productions used different model-designed styles and hand assets; full-film picture, motion, and listening verdicts live in each task's own review records, not replaced by these tests.

## v1.2.0 optional production settings and subtitle tightening (local, unreleased)

- Six optional settings shown step by step: style, arm/pen-only, voice, brush ad, floating watermark, subtitles. Each waits 60 s independently; even if the browser stops sending requests, the backend still saves the default plan and moves on. Missing topic/story content is explicitly asked for. Chosen plans are restorable; the same content is never asked twice; ads and watermarks default to off.
- Real browser-operation verification: picked a model-designed style, pen-only, and a specified Bilibili outro reference; clicked preview playback; a 17-character ad rejected with input kept, valid text saved; watermark and subtitles each auto-continued after 60 s waits; final files read back to confirm choices.
- Pen-only actually composited a 6.38 s, 1920×1080 run sample with a full 16-character ad, drifting watermark, and optional subtitle plate; build, full decode, and timeline verification passed; normal-size frames viewed; ad unclipped with no arm. That film is a test-audio fixture, not passed off as a story production.
- Seven same-text previews actually generated and ASR-checked against the copy; three story productions share one; two specified Bilibili outros merged into one. Sample audio is synthesized output; the bundle holds no original Bilibili video recording. No official timbre list was used to fake popularity stats; subjective listening and timbre similarity were not accepted.
- Subtitles support a standalone upright font, no background or an explicitly chosen light plate; the last line may extend reading hold and fade after the closing line. All 54 subtitle frames and normal-size endings of two remade productions viewed; whole films played from 0 s to end; 1535/2902 frames, player drop counts all 0. A previously accepted third video's subtitle background kept, watermark added.
- Original video, proofread script, and old/new finished films cross-checked for whole-sentence omissions; the wolf film's full-pass ASR once missed an APP sentence, confirmed present by standalone re-transcription of the final track. Subtitles, draw-completion times, and ending holds cannot be inferred from "complete script" alone.
- 29 automated regressions passed; 59 standalone-check assertions passed plus 1 behavior observation. HTTP boundary separately checked: 24 bad-token requests all rejected, no connection resets. New drawing assets and overlays ran sweety-image-privacy with empty embedded metadata; system provenance-attribute residue recorded separately.
- Final install bundle independently unpacked and 1327 file hashes verified; in-bundle 29 regressions passed again; 5 bundled dependencies, 1214 dependency files, 21 style previews, and the single install entry all complete.

This round ran no remote CI, Windows, or other voice-backend end-to-end flows. ASR, decoding, and play-completion do not stand in for subjective listening and viewer acceptance; the productions above prove nothing about arbitrary topics either.

## v1.3.0 full-narration master timeline (local, unreleased)

- 36 automated regressions passed: new full-master sample-by-sample retention, pagination not changing the track, rejection of padded silence / trimmed breaths / speed changes, timestamp coverage and hash invalidation, wrong-track interception, full script handed to TTS in one call, existing video time-map order, and cover retention.
- An actual six-page snake story was built on full narration with 28 assets verified, and a 9-second cross-dissolve representative segment rendered; that segment is flow verification, with the delivered video checked separately.
- Both TTS adapters now take the complete script in one request. VoxCPM2 defaults to reference mode to avoid continuation references overlapping the body-text start and skipping the opening; results must be verified — one call is never a completeness guarantee.
- 5 bundled dependencies, 1214 dependency files, and 21 style previews hash-passed; earlier production options kept.
- Standalone files, full transcriptions, per-page picture comparisons, and voice checks for three stories are kept in the production tasks' delivery records. This version had no remote CI or other-OS verification; ASR, waveform consistency, and player play-completion were not treated as subjective listening acceptance.

## v1.4.0 local verification

41 offline regressions cover: default mix reads no legacy music files, music refused without explicit request, effects need reasons and sentence references, complete closings must enter narration early, active endings and overlong tail silence refused, visible story conditions required before picturing, failed story checks block builds. Original region-by-region outlining, subtitle isolation, full narration, option, and watermark regressions kept. Final tracks verify both the full track and the last 2 seconds' encoding correlation; that does not replace subjective listening.

Local revisions of the snake and How-Happiness-Imprisons films; the "It's Not You Trying Hard" release stays unchanged. Voices use existing local VoxCPM2 with specified references, no model downloads. Nothing submitted, pushed, or released remotely.

## v1.4.1 original-film female voice and noise fix (local, unreleased)

- Diagnosis found the old specified female voice used a synthesized preview as the new reference instead of the original recording; the original reference itself also carried persistent background content. Now changed to original-film vocal separation, residual floor-noise cleanup, accurate verbatim-script combined conditions, and fixed parameters.
- Run code refuses synthesized previews standing in for required original-film timbres, binds audio and verbatim-script hashes, records actual references and parameters; personal primary timbres and original-film references live separately in local directories. Optional offline denoise binds local model hashes without auto-download; post-processing must not change narration length.
- 47 local regressions passed; the original-film same-text and body-text short samples won user confirmation ("timbre close, noise gone too"), and the 110.51-second full narration won confirmation ("full pass approved as the primary female voice"). Subjective acceptance comes from user feedback, not claimed as agent self-listening.
- "How Happiness Imprisons You" keeps original pictures and copy, re-aligned to 40 narration/subtitle segments on the new full master. Final video engineering checks and decoded frames are recorded separately in the production task. This round changed neither the snake video nor any public release.
