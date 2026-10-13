# Style Selection & Drawable Conventions

## Selection Popup

Each new task first fixes the style path. Explicit catalog numbers, explicit picks among the three options, explicit custom descriptions, and explicit model delegation may all be recorded and executed directly without re-asking; otherwise present the text-and-image selection via the [Production Options Window](production-options.md), waiting at most 60 seconds. On timeout, adopt the model's pre-prepared content-based plan, record `timeout_default`, and continue. This is a local HTML selection UI, not a native system dialog.

```bash
python3 scripts/style_choice.py prepare JOB --catalog-source /path/to/hand-drawn
python3 scripts/production_options.py JOB --defaults JOB/options/defaults.json
```

Use the currently available web-opening tooling to show the emitted `PRODUCTION_OPTIONS_URL`. The window presents catalog numbers, original style names, and true preview renders one by one, searchable and zoomable. It must end with:

1. **Don't choose; auto-match by model**: `catalog_auto` — pick the best fit from this snapshot's catalog by content, recording rationale and the complete recipe.
2. **Not in the list; model reasons it out**: `inferred` — independently reason lines, palette, shading, texture, and background from the content; do not load or borrow catalog recipes, and do not quietly fall back to some catalog number.
3. **Custom described style**: `custom` — the user description is the style authority, never replaced by catalog or content preferences; content only decides what to draw. When the description is thin, only fill in executable details; flag conflicts when they exist.

Clicking a card directly means `catalog`. All four paths obey the same drawing conventions.

When the user names a specific install path, that path's actual directory governs; never mix in another version's catalog numbers. Otherwise default to this package's pinned dependency. The catalog is extracted dynamically from `STYLES.md` — never hand-write a fixed count; snapshots save complete recipes and hashes. Previews prefer true local images, then in-package thumbnails, then upstream-manifest image URLs; when unavailable, fix the images before letting the user choose — never pass off imagined images as previews. Changed directory sources require re-prepared snapshots.

The popup listens on 127.0.0.1 only and saves that task's selection file. No auto-generation, paid tasks, or publishing. Stop this service after selection completes.

## Style Decision Files

`style/selection.json` is saved by the window. For explicit in-chat user picks/delegations it may be recorded by hand, with `confirmation` as `explicit_user_request` or `user_delegated`; after waiting, use `timeout_default`, noting actual wait time and the model default plan — never fabricate user-confirmation records.

```json
{"mode":"inferred","confirmation":"explicit_user_request","evidence":"User explicitly chose model-inferred output over the list"}
```

The model then writes `style/resolved.json`:

```json
{
  "status":"resolved",
  "mode":"inferred",
  "selection_sha256":"actual SHA-256 of selection.json",
  "style_name":"This film's actual style name",
  "rationale":"Why this rendering was adopted",
  "content_fit":"Correspondence with this film's specific content",
  "style_spec":{"line":"Distinguishable independent outlines","palette":"Concrete palette","shading":"Concrete shading","texture":"Concrete texture","background":"Background treatment"}
}
```

Both catalog modes additionally require `catalog_id` and the snapshot's `recipe_sha256`; catalog auto-match requires `content_fit`. Independent inference forbids these catalog fields. Custom additionally requires `style_basis` preserving the user description verbatim. Then run `python3 scripts/style_choice.py verify JOB`.

Catalog recipes' anchors, multi-stage generation, and existing requirements still apply. For styles originally lacking black outlines — paper-cut, pixel, gouache, flat, etc. — design an independently trackable outline layer or aligned outline mask up front; never pretend dark color blocks are the whole outline. Keep the selected style's look; when current tooling cannot have both, state the concrete conflict before generating — never quietly switch styles.

## Pre-Generation Drawable Plan

Before every image-generation or edit call, first write the plan and run:

```bash
python3 scripts/drawing_contract.py JOB JOB/design/drawing-plan.json
```

Input contains an `illustrations` array, each with a unique `id`, including:

- `composition`: actual subjects, actions, occlusion, whitespace layout.
- `drawability_reason`: why these parts trace and color independently, whether line complexity fits the voiceover duration.
- `outline_strategy`: `visible_source_ink` or `explicit_mask`; the latter provides a same-size grayscale `outline_mask` in the action, white = outline, black = fill.
- `subjects`: each object's `id` and `kind` (human/animal/object/environment). People must cover `head / neck / torso / left_arm / right_arm / lower_body`; invisible parts go in `omitted_roles` with cropping or occlusion reasons — never use this to skip actually visible parts. Left/right are frame-left/frame-right; stay consistently explained across viewpoints.
- `parts`: in actual drawing order, each with `id / subject / role / stroke_groups`. `stroke_groups` is a sketch array of independent strokes, e.g. head outer arc, left eye, right eye, nose, mouth; never substitute "draw the whole head in one pass".
- `outline_order`, `fill_order`: both the same ordered part-id array.
- `spatial_order`: typically `top_to_bottom_left_to_right`; composition/occlusion exceptions record `order_exception`. People default to head, neck, torso, left arm, right arm, lower body; non-defaults note `order_exception` on that subject.

Successful validation emits a same-named `.preflight.json` binding style and plan hashes. Changed plans require re-preflight; when art already exists, re-evaluate affected parts first — never backdate a new plan as pre-generation evidence for old art.

## Post-Generation Review & Execution

After actually viewing the image, write `drawing_plan_id` for the action, `parts` polygons in the plan's order, and exclusion zones as needed — every visible pixel owned by exactly one part. Polygons follow actual anatomy and occlusion, never top-to-bottom equal-height strips as semantic-part substitutes.

`job.json` designates `drawing_plan` and `drawing_review`. With `outline_mask`, the review additionally requires `outline_mask_sha256`, and the mask dimensions must match the full pre-crop source image. Review structure:

```json
{"style_sha256":"actual hash of resolved.json","plan_sha256":"actual plan hash","illustrations":[{
  "plan_id":"figure-01","status":"pass","source_sha256":"actual image hash","crop":null,
  "anatomy_and_occlusion":"What was seen, which parts are occluded",
  "part_boundaries":"How regions map to actual parts",
  "stroke_breaks":"Where strokes start, turn, end, and lift",
  "outline_then_fill":"Whether outlines are complete and independently traceable",
  "spatial_order":"Confirm part order or explain composition exceptions"
}]}
```

The top level of `art_direction.record` must also carry the same `style_sha256`, binding covers and generated assets to this session's style. Never carry over old image reviews after switching styles.

All illustrations fill only after the whole frame's outlines complete, both phases in the same part order. B&W line art may skip the fill phase, but never skip outlines the other way. Strokes advance only along continuous pixels; broken strokes require pen lifts — never jump-connect by nearest point; color appears only where the brush passes. When strokes are too short to show onset/offset or the frame needs frantic pen jumps to keep up with speech, cut nonessential lines or re-storyboard — never fake naturalness by shortening lifts.

Build, render, and verify reject production tasks missing style/plan/review and validate actual paths. Checks cannot auto-prove the model labeled anatomy correctly or honored the style; representative segments must actually check outline end-frames, fill starts, key part transitions, and continuous motion. Old tasks lacking this evidence need re-evaluation against true sources — no batch backfilled "passes". Runtime test fixtures serve engineering regression only, never final delivery.
