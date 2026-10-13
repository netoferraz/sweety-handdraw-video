# Runtime Format & Commands

Task directories contain `job.json`, model-authored `design.json`, `design/art-direction.json`, `inputs/`, `work/`, and `outputs/`. `review.json` is initialized by verify and filled in by whoever actually reviews. Scripts never write content, pick routes, or design assets for the model.

## Minimal Field Example

Format illustration only; runnable only after images are generated, checked, cleaned, valid art records are written, and the complete narration plus timestamps are locked.

```json
{
  "version": 1,
  "title": "This session's theme",
  "design_file": "design.json",
  "art_direction": {"record": "design/art-direction.json"},
  "canvas": {"width": 1920, "height": 1080, "fps": 30},
  "cover": {"source": "inputs/cover.png", "seconds": 1.0},
  "hand": {"preset": "navy", "pen_text": ""},
  "voice_studio": {"voice": "iapetus"},
  "narration": {"source": "inputs/narration/master.wav", "alignment": "inputs/narration/alignment.json"},
  "audio": {"end_hold_seconds": 0.45},
  "pages": [{"beats": [{
    "id": "opening",
    "text": "Narration matching the audio character-for-character.",
    "actions": [
      {"type": "text", "text": "This segment's short title", "x": 120, "y": 60, "size": 80},
      {"type": "illustration", "source": "inputs/object.png", "x": 200, "y": 230,
       "width": 680, "height": 580, "weight": 3,
       "parts": [{"id": "single_object", "polygon": [[0,0],[1,0],[1,1],[0,1]]}]}
    ]
  }]}]
}
```

The single-part example suits one simple object only. People must be split into semantic parts from the actual image — head, neck, torso, arms, lower body — never glossed over with one whole-image rectangle.

## Design and Image Records

`design.json` must carry `narrative_route.decision/reason/director_review`. The default route decision is `default_life_concept`; other routes additionally carry `director_plan` with the omniailab-specific directing plan. Full content fields: see [Story & Storyboard](story-design.md).

Art record format:

```json
{
  "dependency_sha256": "actual SHA-256 of the packaged art-direction GUIDE.md",
  "images": [{
    "source": "../inputs/object.png",
    "sha256": "actual SHA-256 of the cleaned file",
    "method": "generated_under_guidance",
    "brief_file": "briefs/object.md",
    "brief_sha256": "actual SHA-256 of the pre-generation brief file",
    "prompt_file": "prompts/object.txt",
    "prompt_sha256": "SHA-256 of the actual complete prompt file used this time",
    "status": "pass",
    "evidence": "Subject, contact, composition, edges, and drawability judged after actual viewing"
  }]
}
```

All paths are relative to the record file. Covers, body source images, and custom hands are all covered; the six packaged hands carry their own records. Composited covers use method `composed_with_verified_layers` with editable text-layer notes. Atlases list approved pixel crops in an `approved_crops` array, each `[left,top,right,bottom]`; unapproved cells must not enter the finished film. Replaced images invalidate old hashes and require re-review. A prompt file's existence is not sufficient evidence that "guidance was followed".

## Drawing and Timing

- `text`: `text/x/y/size/color/weight`, one entry per line. Each glyph is traced in its own region; later characters never show early. Default: approximate glyph paths, standard calligraphic stroke order not guaranteed. Layers derive from each character's true bounds with at least 5px padding; never estimate height from font size.
- `marker`: `style` is `highlight/underline/circle/box`, using `width/height/color/line_width`.
- `illustration`: `source/x/y/width/height`, optional pixel `crop`. Aspect-fit into the box.
- `parts`: ordered array with unique `id` and normalized `polygon` coordinates. Coordinates are relative to the cropped image. On overlap, the earlier declaration owns the pixels; every visible pixel must have an owner. The model sets bounds after viewing the image, avoiding chopped fingers, outlines, or props. Complete all parts' outlines in array order, then fill in the same order. Pen lifts between strokes.
- Foreground arms, held objects, or other occluders may declare normalized exclusion polygon arrays via `exclude_polygons`; pixels excluded from head or torso must be picked up by the correct later part. An arm crossing the chest must not appear early during the torso phase.
- Complex occlusion may use a part's `exclude_hsv:[[hue_min,hue_max,saturation_min,value_min]]` to exclude background colors visible through gaps such as hair strands (hue 0–360, saturation/value 0–1). Excluded pixels must be picked up by the correct later part, verified by actual viewing — never infer semantics from color alone.
- `weight` only allocates drawing time within the matching audio window, not seconds. All elements may set `z`; base textures default below text.

A complete frame spanning multiple sentences may use the page's `visual_groups`, each `{"start_beat":0,"end_beat":2,"lead":0.54,"hold":0.72,"actions":[...]}` with 0-based indices. Beats' actions may then be empty or omitted. Start/end indices must be ordered and in-page, lead non-negative, hold at least 0.45s; multiple groups must not cause master-pen events to overlap. Do not mechanically assign one new image per sentence.

Production tasks must designate the complete narration master and matching timestamps via `narration.source/alignment`; fields: see [Continuous Narration Convention](continuous-narration.md). `audio.end_hold_seconds` controls only the frame hold after the whole piece ends; production tasks reject non-zero `sentence_pause_seconds/page_pause_seconds`, `pace` other than 1, and `trim_silence:true`. When the drawing window is too short, simplify the frame or merge drawing windows — never clip breaths, insert silence, or slow the narration to fit the art. Covers accept 1 second only: static first half-second, fading the second half, with body and narration starting after 1s.

## Hands, Fonts & Subtitles

`hand.preset` identifiers for the six arms and pen-only are in `assets/hands/catalog.json`. Unselected cases use the model's content-based default. `pen_text` caps at 16 characters — a format cap. Preserve user source text, checking legibility at normal video size; adjust barrel layout when needed, never silently abbreviate ad names. `pen_text_color` optional. Custom hands set `source/nib/wrist/label_segment/label_height` with measured anchors; source art forearms must extend to the bottom edge; the runtime auto-enforces a minimum scale keeping cut edges off-frame. New hand art follows the same art-direction and privacy cleanup.

`font` is `{"path":"/actual/font.ttc","index":2}`. Subtitles are decided independently by the model per [Subtitle & Ending Design](subtitle-design.md). `captions` supports `font:{path,index}/font_size/color/bottom_margin/max_width_ratio/max_lines/stroke_width/stroke_color`; `background:null` generates no plate; optional `background:{color,opacity}` light plate; never inherits the top-level handwriting font. Prefer single lines, two max. Beats may add `caption_cues:[{"text":"Complete short sentence.","start":0,"end":2.5}]` in seconds relative to the master's matching beat start; production subtitle timing lives in alignment's matching beat, with all text concatenated character-identical to the beat text; no need to regenerate or cut speech for subtitles. Subtitles render as a static typeset layer with no master-pen tracing. `paper` is a hex color. The default pipeline suits clean line art on white or transparent grounds; automatic separation on complex dark backgrounds is not guaranteed.

Optional `ending:{"fade_seconds":0.6}` fades to paper color; the fade must sit entirely within the hold after the final sentence. `audio.end_hold_seconds` defaults to 0.45s, range 0.2–0.8s, no fade by default. Total tail silence must not exceed 1.2s; see [Sound & Ending Hard Constraints](audio-and-ending.md).

## Commands

Substitute real paths, then run:

```bash
python3 /path/to/skill/scripts/voice_studio.py /path/to/job --transcribe
python3 /path/to/skill/scripts/handdraw_video.py doctor /path/to/job
python3 /path/to/skill/scripts/handdraw_video.py build /path/to/job
python3 /path/to/skill/scripts/handdraw_video.py verify /path/to/job --assets-only
python3 /path/to/skill/scripts/handdraw_video.py render /path/to/job --preview --duration 15
python3 /path/to/skill/scripts/handdraw_video.py render /path/to/job
python3 /path/to/skill/scripts/handdraw_video.py verify /path/to/job
```

In `doctor`, `ready` means existing task inputs can synthesize; `voice_studio.available` separately means the new-narration service is available — do not conflate them.

View a short segment before exporting the whole film. verify fully decodes and samples finished-film frames — engineering checks only. Fill in review evidence per [Verification & Rework](quality-gates.md) before running `finalize`. It rejects unreviewed records, expired hashes, and runtime test fixtures.

Outputs: `film.mp4/cover.png/captions.srt/script.txt/verification.json/verification.txt`. When saving aliased Chinese filenames, keep originals and hashes. Script, image, audio, design, or layout changes all require re-render and re-review. `runtime_fixture:true` is for in-package offline tests only and must never produce a delivery receipt.

## Style & Drawing-Plan Gates

Production tasks must complete [Style Selection & Drawable Conventions](style-and-drawing.md). `job.json` gains `drawing_plan`, `drawing_review`; each illustration action gains `drawing_plan_id`, plus `outline_mask` when independent outlines are needed. Old tasks cannot bypass the new gates.

## v1.2 Optional Production Settings

Collect preferences at start per [Optional Production Settings](production-options.md). `hand.preset` gains `pen-only`, showing the full pen only without forearm-length rules. `hand.pen_text` caps at 16 characters. Subtitles may set `background:{"color":"#243039","opacity":0.76}` with white text; empty backgrounds still work. These new conventions supersede old examples' 20-character cap and fixed plateless style.

`watermark:{"text":"user source text, wrappable","opacity":0.24}` adds a low-contrast, slowly drifting watermark; unconfigured or empty text adds none. Default ~27px at 1080p; `font/color/font_size` settable, opacity capped at 0.35. The fixed cover static phase never drifts; display starts with the 1s body, always in-frame and clear of bottom subtitles.

`ending.caption_hold_seconds` may extend the final sentence's reading hold; it must be ≤ the post-final-sentence hold minus fade time. Subtitle text is unchanged; SRT and actual frames extend together.

## v1.3 Whole-Piece Sound Master Timeline

Migrate old production tasks per the continuous-narration convention first — never splice old segmented files and claim intonation breaks solved. `runtime_fixture:true` keeps segmented synthesis for offline tests only, never as a delivery path.

Subtitle delivery uses a single burned-in layer. `timeline.captions` is fixed at `{"presentation":"burned","owner":"caption_events"}`; extra subtitle events, subtitle tracks, and same-directory subtitle files all reject delivery.

Since v1.2.1, briefs and prompts must be non-empty and hash-match their bindings. Before calling image tooling, run `python scripts/art_direction.py bind-evidence design/art-direction.json`; the command never marks images passed. Covers, body art, characters, props, custom hands, and all candidates/edits follow the same flow.
