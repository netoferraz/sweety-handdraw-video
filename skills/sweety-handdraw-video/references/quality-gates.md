# Verification & Rework

Engineering checks and perceptual checks are recorded separately. `verify` emits engineering results plus sampled frames from the actual MP4; `finalize` issues the delivery receipt only when engineering results match the current file and human/model review records explicitly pass.

## Engineering Checks

- Reject production when missing model narrative-route judgment, art-direction dependency, prompts/briefs, image pass records, or matching hashes; atlases admit only reviewed crops.
- Every illustration pixel has a semantic part assignment; fill starts only after all outlines complete, both phases in the same part order; characters are isolated in their own start/end windows; true glyph bounds keep whitespace on all four sides — never clip descenders or extensions. Pen lifts separate strokes.
- For arm modes, the forearm cut edge stays off-canvas; pen-only checks the full pen body and nib; the cover holds static 0.0–0.5s, fades on actual pixels 0.5–1.0s, and narration starts only after 1s.
- Fully decode audio/video, verifying resolution, frame rate, codec, audio-video duration delta, and loudness.
- Every subtitle maps to an actual speech segment; post-split text preserves every character; timing is ordered with no out-of-bounds entries. Subtitles use an independent regular typesetting layer, plateless by default, no hand-drawn animation, complete glyph edges.
- Check the final track's last sentence is uncut, and separately view the finished frame and tail sound after the final sentence; file completeness does not equal a natural ending.
- Body elements stay in-canvas, use drawn paths; the same master-pen event never overlaps itself; a short hold follows the subject's completion.
- Text and line art appear near the nib; every colored pixel's appearance has real brush coverage — no leftover color blocks filled in wholesale on the last frame.
- Separately check small color patches, uncolored-fill art, short punctuation, thick stroke endpoints, long titles, and varying page counts. Do not hide missing paths by loosening assertions.
- Report cleaned-image metadata and source attributes separately; never present privacy processing as AI-detection guarantees.

## Perceptual & Content Checks

View the final export: all page finished frames, representative drawing passes, subtitle wrapping and transitions, cover head/tail. Check continuous playback with an available player; listen with audio tooling when available, otherwise use ASR plus track checks and state listening as unverified.

Record `review.json`:

```json
{
  "video_sha256": "actual video hash",
  "content": {"status": "pass", "evidence": "facts/qualifications/sources and subtitles verified"},
  "visual": {"status": "pass", "evidence": "frames, segments, and fixes actually viewed"},
  "motion": {"status": "pass", "evidence": "drawing/coloring/transitions actually viewed; player or consecutive-frame method noted"},
  "audio": {"status": "pass", "evidence": "ASR or listening basis, whether final track was re-verified"},
  "privacy": {"status": "pass", "evidence": "cleanup tooling, re-verification results, residual boundaries"},
  "limitations": ["subjective acceptance not actually completed, e.g. listening"]
}
```

`pass`, `fail`, `not_checked` must rest on observation — never auto-filled pass by scripts. ASR-only checks must not be written as "fully listened". Model-viewed sample frames can verify glyphs and layout but never alone prove full-film motion smoothness.

## Rework Rules

First locate whether the problem is content, assets, sound, paths, or scheduling; change only that layer. E.g. fix glyph paths for jumping short strokes; simplify the frame or add reasonable segments for too-fast strokes; fix crops or regenerate for AI-atlas cross-image contamination; prefer redoing the whole-piece narration for misread words. With genuine model length limits, finish sound editing and verify continuity first, lock the new complete master, then recompute downstream timelines — never let video pages become sound cut points.

After the same failure repeats three times without improvement, change approach or report the specific blocker. Do not generate paid assets endlessly, pass the same failure off as new candidates, or mask mechanism problems by merely lengthening the video. Delivery records keep final pass evidence plus still-unverified boundaries.

## Director Final Review and Sound Boundaries

Per the packaged omniailab `FILM-ART`, check each page serves the explanation, focus is clear, motion conflicts with speech nowhere, and the closing answers the opening. Record actually seen shortcomings and fixes — never just "director skill loaded". When art motion matches huashu types, enable its type methods while keeping this pipeline's hard requirements.

All sound file-existence, synthesis-success, and ASR-correctness results prove only availability and recognition; "natural diction, emotional, lecture-ready" requires actual listening. Without listening tooling, write audio evidence as "text/track checks passed, subjective listening unverified" and keep that point in limitations and delivery notes. Never auto-fill "sounds great".

Representative consecutive frames can reveal part ordering, leaks, sudden fills, and arm edge breaks, but sparse samples never prove frame-by-frame smoothness. Check the drawing phase of every page at minimum; play the complete film when a player is available. Keep the check scope — never generalize one film's pass into stability for arbitrary inputs.

Part review also checks occlusion: umbrella surface showing through hair gaps belongs to the umbrella; held objects do not belong to arms; bag/leg intersections split by actual front-back order. Programs can only verify execution order for a given assignment — never prove the model's semantic segmentation. Leftover regions admit only explicit auxiliaries like rain lines and ground — never swallow heads, arms, or prop subjects.

New-option checks: ads capped at 16 characters with source text untruncated; watermark preserves source text and line breaks without fighting subtitles; decided options are never re-asked, each item times out forward; voice same-text auditions, actual backends, ASR, and subjective listening are recorded separately.

## Whole-Piece Sound Check

Record narration source text, generation method, master hash, and alignment hash. Check every sentence for dropped readings, repeated readings, truncated endings, and proper nouns; re-verify suspicious ASR spots with contextual clips. Check all transition points for added silence or waveform jumps, preserving true breaths and semantic pauses. Never mask errors with blanket silence removal, shortening all pauses, or whole-film crossfades.

Before build and render, verify master and timestamp hashes, checking body narration sample-identical with silence added only before the cover and after the whole piece. Re-verify full text and final sentence after mixing and AAC encoding; these engineering checks never substitute for actual listening acceptance. Changing pagination must not change narration waveforms; changed narration requires recomputed visuals and subtitles.

## Narrative Facts Must Hold in the Art

Each illustration's drawing-plan must list narrative_requirements: id, narration_quote source text, observable condition. Quotes must come from the current script; e.g. "body severed" needs two visible gaps plus separated snake segments, "mouth full of blood" needs biting contact plus recognizable red blood at the mouth. Never let restrained art styling erase plot facts.

View actual images and record status plus concrete evidence per matching id in the drawing-review's narrative_checks. Unchecked, failed, missing, or image-hash-changed items block the build. Evidence must point at visible positions or relations; "subject correct" or "image exists" never justify content passes. Engineering can constrain check completeness and evidence-to-image correspondence — never auto-comprehend "severed".

Re-check these conditions against final decoded frames to keep crops, occlusion, or reveal timing from hiding key outcomes. No background music by default; verify actual mix inputs, the complete final sentence, and the short ending per audio-and-ending.md. When the user reports content mismatch, withdraw related pass verdicts and fix that layer — never offset feedback with old pass records.

## Subtitle Uniqueness Is a Hard Delivery Gate

The same narration sentence may show only one subtitle at any moment. No two subtitle layers may overlap, whether from baked-in frame subtitles, subtitle events, embedded MP4 subtitle tracks, page `<track>` elements, or player auto-loaded subtitle files. Default: white text, thin black stroke, no plate; the uniqueness gate still applies when the user picks another form.

Re-synthesize only from subtitle-free masters plus an independent subtitle layer; never stack a second layer onto a finished film that already has hard subtitles. Build and render checks require cues to correspond one-to-one with subtitle events, with consistent roles and no time overlap; delivery checks require no extra subtitle tracks in the MP4 and no SRT/ASS/VTT subtitle files in the same directory. Edited subtitles go only in `outputs/editing/`; preview pages get no subtitle tracks. Finally, check long sentences, sentence transitions, and the ending in actual decoded frames; any double subtitles mean FAIL — fix before delivery. Auto-translate/live subtitles enabled proactively by third-party players must be switched off separately; an unreproduced display source must not be treated as identified.
