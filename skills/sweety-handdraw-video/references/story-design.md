# Story & Storyboard

New themes first produce `design.json` with at minimum:

- `narrative_route`: the model judges whether the default everyday-problem route applies, writing decision, reason, director_review; non-default routes add director_plan.
- `topic`, `audience`, `explanation_goal`: concrete things viewers will understand or be able to do.
- `source_scope`: original theme, user article, external article adaptation; source links and factual basis.
- `visual_direction`: character setup, color rationale, line/texture, scene and shot density.
- `beats`: per-segment voiceover, concrete on-frame objects, actions, text, emphasis placement, and correspondence with the voiceover.
- `cover`: accurate short title, hero visual, consistency with the body.
- `asset_prompts`: generation requirements, character-reference duties, whether text is included, image source records.

These fields record design judgment — not a demand that every theme use the same narrative. Life-opinion pieces may open with a concrete dilemma; mechanism explanations may use contrasts or processes; stories keep character action and causality; article adaptations must not drop conditions for drama.

## How Visuals Serve the Explanation

Every image must answer "what does this explain beyond showing the sentence directly". For costs, draw what is already spent plus the upcoming choice; for time, show the same figure's sequential actions. Do not represent every abstraction with a figure standing beside paper, light bulbs, and arrows.

Titles and short labels navigate; the voiceover carries the complete argument. Avoid copying whole subtitle passages into body text. A page may accumulate several related elements, transitioning only after one complete idea is told; layout, count, and proportions follow the content.

## Generated Assets

First complete [Style Selection & Drawable Conventions](style-and-drawing.md); no historical case's style is the new film's default.

Every image first reads the packaged sweety-image-art-direction and writes its own art brief; catalog modes keep the hand-drawn style's complete recipe; independent-inference and custom modes use the recorded independent style specification. Briefs, final prompts, cleaned hashes, and actual-viewing verdicts correspond one-to-one.

Consider part-by-part independent tracing for people and props before generation. List part order and expected stroke complexity at design time — never discover undecomposable art after drawing. The model annotates actual part polygons after viewing the finished image; typically top-to-bottom, left-to-right, with exceptions explained in the design. Fill starts only after all outlines complete.

First fix one identity and style reference for the film, then vary actions and props by scene. Keep costumes, face shapes, and linework recognizably consistent. Colors may be re-picked around the theme — never default to pear-yellow/leaf-green or any historical figure.

Illustrations suit clean white or transparent grounds, distinguishable outlines, and local color patches. Dense photographic textures and ultra-fine broken lines inflate path counts and drawing-speed problems; for such styles build a representative segment first — never batch-generate the whole set.

Controllable body text is written by the runtime tooling; cover AI generates only hero visuals plus whitespace, with accurate titles composited in verifiable, editable text layers. Useless pseudo-text in generated images, neighboring-atlas fragments, and anomalous limbs must not be waved through on "render succeeded".

## Valid Short-Segment Verification

Pick a segment carrying one complete mini-idea — not a few seconds of stills merely proving encoding works. It should show the title texture emerging, text writing, illustration outlining, at least one nib-following color change, and a post-completion reading hold. If the fixed pen hand's sleeve cut edge enters frame, redesign the arm asset or shot — never accept cut edges as a normal effect.

## Sound Sets Storyboard Timing

Write the complete voiceover script; generate or record the complete narration first, preserving natural breaths and semantic pauses. Semantics may be planned first, but page timing is set after. Schedule transitions, tracing, fills, and subtitles from the locked master's timestamps; when frame complexity exceeds the speech window, merge drawing windows or simplify elements. Never re-record per page, force pauses, or reshape narration rhythm to finish drawing.
