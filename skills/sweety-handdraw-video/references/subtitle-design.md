# Subtitle & Ending Design

Subtitles track speech; they do not carry body titles or style performance. The model picks font, weight, size, color, position, and phrasing from the actual frames, speech rate, playback size, and audience, recording rationale in `design.json.subtitle_design`. Reference the restrained direction of Chinese-animation main-feature subtitles such as "Zhu Xian", "Xian Ni", and "Douluo Dalu"; do not guess font brands from titles, and do not mistake player sidecar subtitles for in-film subtitles.

## Default Form

- Default: white text, thin black stroke, no plate. Plates optional; keep a dark translucent plate with light text when frames are busy or the user approved plates. Prefer upright, clean regular bold/gothic or sans fonts, independent of body handwriting and pen-barrel text; never inherit calligraphic, handwritten, or decorative fonts.
- Usually bottom-centered, single short lines, stable position; two lines max when semantics require. No per-character tracing, bouncing, keyword recoloring, or large scaling. Subtitles appear whole per sentence, changing with speech.
- This version defaults to white-on-black-edge; follow explicit user color picks when given. Busy backgrounds may add a plate. Plates serve legibility — area, density, and size must not overpower body content.
- Size follows actual viewing size. 1080p may trial ~32–42px, but that is not a mandated size; representative frames must be viewed at normal size, balancing unobtrusiveness with legibility.
- Split sentences by complete meaning and pauses. When a short sentence's display time is insufficient, prefer regrouping neighboring sentences and times — never just shrink type to cram long passages. Preserve all text, numbers, and qualifications; never edit source narration to fit layout.

## Execution

`captions.font` names the actually present subtitle font, separate from top-level `font`. `background:null` generates no plate; `background:{"color":"#243039","opacity":0.76}` may generate a dark plate with `color:#FFFFFF`. `font_size/color/bottom_margin/max_width_ratio/max_lines/stroke_width/stroke_color` are provided after model design. Default to a regular Chinese font; on failure, report the missing font explicitly — never quietly fall back to handwriting.

One voiceover beat may contain multiple `caption_cues` without cutting or regenerating speech for them. Each cue's `start/end` is seconds relative to that beat's actually used audio; align by word times or actual listening — never fake exact sync from character ratios alone. All cue texts concatenated must equal the beat text, ordered with no overlap or out-of-bounds entries. Re-align cues after rate or silence-trim changes.

Subtitles are an independent static typeset layer, not master-pen drawing events; no faked strokes or hand trajectories. Body titles still follow the original drawing requirements.

## Ending

Check "did the words finish" and "does the viewer feel it finished" separately. First verify the final MP4's last sentence and track tail, then view the finished frame's hold, hand exit, and narration tail. Never judge ending quality on matching file durations alone.

The model picks `audio.end_hold_seconds` and optional `ending.fade_seconds` by final-sentence content and rhythm. Finish speaking, finish drawing, hold for reading, then end or fade gently; fades may only occur after narration ends — never cover unfinished content. Do not mechanically add identical seconds or extra slogans to every film.

## Acceptance

View the final export at normal size, covering long/short subtitles, sentence transitions, the densest frames, and the last seconds. Check the font is upright, backgrounds match the chosen design, body/subtitle layering is clear, Chinese edges are unclipped, timing matches the source audio, and the ending lands naturally. Engineering checks can verify text and timing — never substitute for reading and listening judgment.

Set `ending.caption_hold_seconds` when needed so the final sentence holds for reading after narration ends; it must end before any ending fade starts, never covering a next sentence. The final frame may complete early so viewers see the whole image while hearing the closing, instead of still deciphering the drawing process at the last character.

Ending parameters obey [Sound & Ending Hard Constraints](audio-and-ending.md): hold the finished frame by default, closing 0.45s after the master, at most 1.2s from last measurable voicing to film end; user-specified closings must join at the complete-script stage.

## Subtitle Uniqueness Is a Hard Delivery Gate

The same narration sentence may show only one subtitle at any moment. No two subtitle layers may overlap, whether from baked-in frame subtitles, subtitle events, embedded MP4 subtitle tracks, page `<track>` elements, or player auto-loaded subtitle files. Default: white text, thin black stroke, no plate; the uniqueness gate still applies when the user picks another form.

Re-synthesize only from subtitle-free masters plus an independent subtitle layer; never stack a second layer onto a finished film that already has hard subtitles. Build and render checks require cues to correspond one-to-one with subtitle events, with consistent roles and no time overlap; delivery checks require no extra subtitle tracks in the MP4 and no SRT/ASS/VTT subtitle files in the same directory. Edited subtitles go only in `outputs/editing/`; preview pages get no subtitle tracks. Finally, check long sentences, sentence transitions, and the ending in actual decoded frames; any double subtitles mean FAIL — fix before delivery. Auto-translate/live subtitles enabled proactively by third-party players must be switched off separately; an unreproduced display source must not be treated as identified.
