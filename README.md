# sweety-handdraw-video · Hand-Drawn Narration

**Turn a topic, an article, or a complete story into a narrated draw-along video.**

The agent understands the content, writes the narration, designs the storyboard, and generates illustrations; local tools draw stroke-by-stroke outlines, fill colors, hand-write text, align voiceover, and export. What is drawn, in which art style, and how long it runs are all decided by the content.

[![The Snake That Died of Anger](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/docs--media--snake-cover.jpg)](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/docs--media--snake-story.mp4)

[Watch samples](#featured-samples) · [Install & environment](#install--environment) · [Get started](#get-started) · [All production options](#all-production-options) · [FAQ](#faq) · [Follow the author](https://x.com/leo_xiaolei)

> **Locked release: v1.2; latest patch: v1.2.3 (2026-10-13)**. Single black-outline subtitle track, full-narration workflow, production options, and on-demand asset caching are all part of this lock; the in-package `VERSION` and Git tags are authoritative.

## Featured samples

### "The Snake That Died of Anger" · 49 seconds

From the conflict between a snake and a saw to how anger amplifies harm. Best watched first for the short film's narrative density, the reveal order of picture regions, and how narration and pen strokes work together. Uses light-color narrative illustration, a brush hand, and plateless subtitles.

[![Snake: actual drawing process](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/docs--media--snake-drawing.gif)](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/docs--media--snake-story.mp4)

[Play or download the full MP4](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/docs--media--snake-story.mp4)

### "How Happiness Imprisons You" · 1 min 52 sec

Cool-toned angular illustration, female hand, continuous female narration. Shows picture consistency within a single story, and the workflow where the full narration is generated first and pictures are aligned to it afterward. The current female voice is provisional; perceived quality still needs follow-up tuning; the original timbre reference stays stored locally. This round only fixed subtitles; audio bytes are unchanged.

[![How Happiness Imprisons You](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/docs--media--wolf-cover.jpg)](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/docs--media--wolf-story.mp4)

[![Wolf: actual drawing process](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/docs--media--wolf-drawing.gif)](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/docs--media--wolf-story.mp4)

[Play or download the full MP4](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/docs--media--wolf-story.mp4)

Both MP4s are 1920×1080, 30fps, and keep the preview watermark from production; the watermark text does not reflect the current release date. The GIFs are silent excerpts of the finished films; judge timbre and pacing from the full videos.

<details>
<summary>More genres: concept explainers and longer narratives</summary>

**"Murphy's Law"**: about 2 min 05 sec, 8 pictures, 24 narration segments, explaining how preparation lowers the cost of mistakes. Uses light-color sketching, a female glitter-long-nail hand, and on-brush text. It is the public v1.0.0 historical sample and does not represent the latest continuous-narration workflow.

[![Murphy's Law](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/docs--media--murphy-cover.jpg)](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.0.0/murphy-law.mp4)

[Download "Murphy's Law"](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.0.0/murphy-law.mp4) · [See the author's showcase post for "It's Not You Trying Hard, It's Someone Wanting You to Try Hard"](https://x.com/leo_xiaolei/status/2109880459664752867)

Longer samples show narrative span; they do not mean short opinion pieces should also be made into long videos.

</details>

## What it is good for

| Input | Output direction |
|---|---|
| A topic | Organize an explanation route; design everyday scenes, mechanisms, and action advice |
| An article or reference material | Keep key facts, numbers, and qualifiers; rewrite into narration fit for listening and watching |
| A complete story or finalized script | Produce from the original text; when the original must be kept, do not rewrite it without authorization |
| Approved pictures, audio, and scripts | Reuse assets; revise subtitles, hands, pictures, or the timeline |

Good for knowledge explainers, story fables, opinion pieces, product principles, and teaching content. Its strength is putting narration, pictures, and the drawing process in one production pipeline:

- **Style follows content**: 21 in-library entries; the model may also design independently or take a custom direction.
- **Whole-script voice first**: keeps pauses and breaths between sentences, then arranges pictures and subtitles — avoiding fixed gaps from per-page voiceover stitching.
- **Drawn by region**: outlines are completed per semantic region first, then filled in the same order; text is drawn character by character with pen lifts between strokes.
- **Targeted revisions**: scripts, assets, audio, timestamps, and check records are kept, so fixing subtitles does not require regenerating pictures.
- **Runtime vs. perception checked separately**: whether files play, sound is clear, and the story is drawn correctly are different check items.

It is still a production Skill requiring an agent, image generation, and a local runtime; finished films do not use external image-to-video models. The current main rendering implementation is Python / Pillow / SciPy / FFmpeg.

## How the director participates

The built-in `omniailab-ai-director` director method is used for creative planning, storyboarding, and final review. The model must ground its judgment in concrete pictures and the timeline:

| Director judgment | How it is executed in hand-drawn narration |
|---|---|
| Audience and goal | State what viewers should understand after watching; decide the entry point and information density |
| Narrative route | Stories keep actions and causality; principles may use contrast, change, or process walkthrough; user-finalized scripts and original-text requirements take priority |
| Storyboard and picture function | Each picture states what it explains beyond directly showing the sentence; lists required visible actions, contact, and results |
| Composition and consistency | Set primary/secondary focus, character and prop references, colors, and whitespace; avoid arbitrary drift across pictures |
| Pacing and sound-picture relation | Finish one complete idea before changing scenes; set outlining, coloring, keywords, and hold times from the actual narration |
| Finished-film review | Check missing story actions, repeated information, occlusion, reading time, and audio correspondence; on failure, fix the affected parts |

You can specify the director treatment in the request, e.g. "start directly from the conflict", "do the comparison in the order a teacher would explain it", "hold on the key result so it can be seen clearly", "keep the original ending". When unspecified, the model plans from the content without forcing a fixed page count or per-page duration.

Director judgments go into the creative plan and are executed by local rendering; this is not a standalone director model, nor does it automatically enable upstream film-generation platforms. Broader director material is pulled in as needed and must not be taken as proof that this project has verified all upstream film capabilities. See [Story design](skills/sweety-handdraw-video/references/story-design.md) and [Dependency roles](skills/sweety-handdraw-video/references/dependencies.md).

## Install & environment

### 1. Install the Skill

With Node.js / npm available:

```bash
npx skills add threerocks/sweety-handdraw-video --skill sweety-handdraw-video
```

This command installs the repo's current public content. To pin this release, use the [v1.2.3 install package](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2.3/sweety-handdraw-video-v1.2.3.zip).

Manual install: place the **entire** `sweety-handdraw-video` folder from the package into your host's personal Skills directory. Codex example: `~/.codex/skills/sweety-handdraw-video`. Back up the old directory before updating; copying only `SKILL.md` will miss scripts, assets, and bundled dependencies.

### 2. Confirm runtime requirements

| Requirement | Purpose and limits |
|---|---|
| An agent that can read the Skill and run local commands | Full production is currently tested with Codex. Other host/model combinations have not completed the same verification |
| Working image-generation capability | Codex built-in image generation by default; text-chat-only capability is not enough for new illustrations |
| Python 3.9+ | Local drawing and compositing; a dedicated virtual environment is recommended |
| Pillow, NumPy, SciPy, requests | Installed from the bundled `scripts/requirements.txt` |
| FFmpeg and ffprobe | Compositing, encoding, decoding, and audio/video checks |
| Chinese fonts | Handwritten body font and regular subtitle font are configured separately; system commercial fonts are not bundled |
| Bun and ExifTool | Run `sweety-image-privacy` to clean image metadata and keep audit records |
| One working voiceover path | Configured local VoxCPM2, a compatible VoiceStudio, or a verified full narration with timestamps |
| Node.js / npm (optional) | Only for the install command above; manual install does not need it |

Install rendering dependencies from the repo root:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r skills/sweety-handdraw-video/scripts/requirements.txt
python skills/sweety-handdraw-video/scripts/validate_package.py
```

Above are macOS / Linux commands. Dependency version ranges: Pillow `>=10,<12`, NumPy `>=1.24,<3`, SciPy `>=1.10,<2`, requests `>=2.31,<3`. Installing the Python rendering dependencies does **not** also install the voiceover model, fonts, FFmpeg, Bun, or ExifTool.

### 3. Choose a voiceover path

**Timbre and voiceover backend are chosen separately.** Six built-in timbres provide real reference audio and same-text previews; the backend renders this production's body text into voice. You may also import your own reference or keep using an accepted local primary voice.

| Path | What to prepare | Verified scope |
|---|---|---|
| **VoxCPM2 local adapter** | Apple Silicon Mac, dedicated MLX environment, `mlx-audio` / `mlx` / `soundfile`, downloaded model with real paths | This project uses the VoxCPM2-8bit MLX build; the current adapter is not a Windows / NVIDIA deployment solution |
| **VoiceStudio local service** | A local service offering a compatible HTTP API with a configured TTS engine | Default `http://127.0.0.1:3900`; tested against `mlx-audio / Qwen3-TTS-12Hz-1.7B-Base-bf16` |
| **Ready-made full narration** | Checked audio and its timestamps | Can be arranged and rendered offline; no re-synthesis needed this round |

VoiceStudio needs the `/engines/tts`, `/generate`, and optional `/transcribe` endpoints. Any same-named software is not necessarily compatible. Both generation paths submit the complete script first, then check for skipped reads, repeats, tail artifacts, and timestamps.

**Why keep these paths?** This project adopted VoxCPM2 to improve long-passage narration naturalness and reference-timbre continuity, and the author actually tested the MLX 8-bit path on their Mac; the current new female voice is provisional and the author still hears a gap, so tuning continues. VoiceStudio stays because it keeps a working local service so not every user must switch voice environments. When a satisfactory full narration already exists, reusing it directly saves re-synthesis. There is no claim that one backend is better on all voices, scripts, and machines.

Local synthesis keeps references, parameters, and generation records; this path needs no separate cloud TTS account — at the cost of preparing models and compute yourself. VoiceStudio is a service interface; Qwen3-TTS / VoxCPM2 are the models. Do not confuse service names, timbre names, and model names.

Model weights are not in the Skill package; run scripts never buy services, download models, or switch the global voice engine for you. Noisy references may additionally need vocal separation or denoising; HTDemucs / DeepFilterNet3 are the optional tools used for this round's female-voice repair, not base dependencies every user must install.

**Tested hardware**: the author's current environment is M3 Max with 36GB RAM. The VoxCPM2-8bit weights used are about 3.23GB; weight size does not equal runtime memory needs. This project has not measured minimum requirements for 8GB / 16GB machines, nor does it promise render times for all models on that basis.

See [Local runtime](skills/sweety-handdraw-video/references/local-runtime.md) and the [Environment checklist](docs/environment.md) for detailed connection parameters.

## Get started

Confirm your local environment and art style with a tens-of-seconds short topic first, then handle long scripts. Send directly to the agent:

```text
Using $sweety-handdraw-video, turn "why anger makes bad decisions more likely"
into a hand-drawn explainer for ordinary adults. Explain one mechanism clearly,
about one minute. Choose style, hand, voice, and subtitles from the content.
No ads, no watermark.
```

```text
Using $sweety-handdraw-video, turn the complete story below into a video.
Keep the body text verbatim, no rewriting. Use a custom light-color sketch,
brush hand, plateless subtitles. Use my confirmed local reference for voice.
No background music.

【paste story】
```

```text
Using $sweety-handdraw-video, turn this article into a hand-drawn explainer video.
Keep numbers, sources, and conditions. Use style 17 for pictures, pen-only for hand.
Brush text: @leo_xiaolei. Watermark: @leo_xiaolei.
Check one representative scene first; complete the full film after text is readable.

【paste article】
```

The default is Simplified Chinese, 1920×1080, 30fps. Duration follows the actual narration; "about one minute" is a creative target, never padded with blank space. There is a 1-second cover by default: first 0.5 s static, last 0.5 s fading out.

Six choices are offered at the start: style, hand, voice, brush ad, watermark, subtitles. Already-decided items are skipped; each undecided item waits at most 60 seconds, then the model proceeds with its content-based plan. Saying "choose everything automatically" continues immediately. Ads and watermarks default to off.

## All production options

| Item | How to choose |
|---|---|
| Content | Topic, reference material, article, complete story; original text kept when explicitly required |
| Direction and narrative | Specify audience, entry point, narrative order, information density, emphasis, and ending; or delegate planning to the model |
| Art style | Specify an in-library number, model-matched from the library, model-designed independently, custom description |
| Hand | Six arm presets, pen-only; custom assets with positioning parameters supported |
| Voice | Six downloadable/synthesizable references, self-imported original references, accepted local voices, ready-made narration |
| Brush text | Max 16 Unicode characters; blank means none; long text needs a real readability check |
| Watermark | Max 180 characters with newlines; low contrast, slow drift; blank means none |
| Subtitles | Default white with black outline; `quiet` plateless / `plate` with plate; exactly one subtitle track for the whole film |
| Body text | Handwritten key sentences and illustrations arranged separately; glyph margins reserved on all four sides |
| Canvas and frame rate | Default 1920×1080, 30fps; other sizes need re-checks of composition, font size, and occlusion |
| Pacing | Outlining, coloring, text, pen lifts, and holds follow narration timestamps; pace requests allowed |
| Music and effects | Pure narration by default; required effects justified item by item; background music never auto-added |
| Cover and ending | 1-second cover; default 0.45 s ending hold; closing lines must be written into the full narration |
| Revisions | Approved pictures and audio may be reused; re-check affected parts after related assets change |

These are choices a production request may express, not a promise that each has its own visual slider. See [Production options](skills/sweety-handdraw-video/references/production-options.md) and [Subtitles & endings](skills/sweety-handdraw-video/references/subtitle-design.md) for detailed fields.

### 21 in-library style entries

| Selection method | Basis and execution |
|---|---|
| Pick a number directly | User picks from real effect previews and specifies; the matching complete recipe is used |
| Auto-match from library | Model picks from the library based on content, recording reasons and the complete recipe |
| Model-designed independently | Independently designs lines, colors, shading, texture, and background without falling back to a number |
| Custom description | User description is the style authority; the model only fills in executable details and must not switch styles from content preference |

The library holds **20 numbered styles + the 3.1 stable variant**, each with an online real-effect preview and complete recipe; images are no longer packed into the code package. They are static art directions; not every one has undergone video verification of equal depth.

| No. | Style | No. | Style |
|---|---|---|---|
| 1 | Minimal black-and-white explanatory comic | 11 | 2D watercolor / retro animation concept draft |
| 2 | Crayon child doodle | 12 | Warm-light child painting |
| 3 | Ghibli style | 13 | Nordic paper cutout |
| 4 | Bean-person black graffiti infographic | 14 | Nordic picture-book gouache |
| 5 | MS Paint messy scribble | 15 | Big-nose soft doll |
| 6 | Ballpoint single-line doodle | 16 | Spotlight gouache portrait |
| 7 | Crayon on photographed paper | 17 | Ink-line picture book |
| 8 | Ink wash freehand | 18 | Warm flat picture book |
| 9 | Retro pixel | 19 | Round-head red-line minimal child drawing |
| 10 | Emotional narrative light-color sketch | 20 | Warm-yellow ink-line emotional mini-theater |
| 3.1 | Crayon child doodle · scrawly self-drawn stable variant | | |

To step outside the style library, choose "model-designed output" or give a custom description. The model must still write a style plan, review the actual illustrations, then build the drawing paths. See [STYLES.md](skills/sweety-handdraw-video/dependencies/hand-drawn-styles/STYLES.md) for full definitions.


<details>
<summary>Show all 21 actual style previews</summary>

<table>
<tr><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--1.jpg" width="220" alt="1 minimal black-and-white lines"><br>1 · Minimal black-and-white lines</td><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--2.jpg" width="220" alt="2 crayon child doodle"><br>2 · Crayon child doodle</td><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--3.jpg" width="220" alt="3 Ghibli style"><br>3 · Ghibli style</td></tr>
<tr><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--3.1.jpg" width="220" alt="3.1 scrawly self-drawn stable variant"><br>3.1 · Scrawly self-drawn stable variant</td><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--4.jpg" width="220" alt="4 bean-person graffiti infographic"><br>4 · Bean-person graffiti infographic</td><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--5.jpg" width="220" alt="5 MS Paint messy scribble"><br>5 · MS Paint messy scribble</td></tr>
<tr><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--6.jpg" width="220" alt="6 ballpoint single-line doodle"><br>6 · Ballpoint single-line doodle</td><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--7.jpg" width="220" alt="7 crayon on photo"><br>7 · Crayon on photo</td><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--8.jpg" width="220" alt="8 ink wash freehand"><br>8 · Ink wash freehand</td></tr>
<tr><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--9.jpg" width="220" alt="9 retro pixel"><br>9 · Retro pixel</td><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--10.jpg" width="220" alt="10 light-color sketch"><br>10 · Light-color sketch</td><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--11.jpg" width="220" alt="11 2D watercolor"><br>11 · 2D watercolor</td></tr>
<tr><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--12.jpg" width="220" alt="12 warm-light child painting"><br>12 · Warm-light child painting</td><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--13.jpg" width="220" alt="13 Nordic paper cutout"><br>13 · Nordic paper cutout</td><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--14.jpg" width="220" alt="14 Nordic picture-book gouache"><br>14 · Nordic picture-book gouache</td></tr>
<tr><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--15.jpg" width="220" alt="15 big-nose soft doll"><br>15 · Big-nose soft doll</td><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--16.jpg" width="220" alt="16 spotlight gouache"><br>16 · Spotlight gouache</td><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--17.jpg" width="220" alt="17 ink-line picture book"><br>17 · Ink-line picture book</td></tr>
<tr><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--18.jpg" width="220" alt="18 warm flat picture book"><br>18 · Warm flat picture book</td><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--19.jpg" width="220" alt="19 round-head red-line child drawing"><br>19 · Round-head red-line child drawing</td><td align="center" width="33%"><img src="https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--style-previews--20.jpg" width="220" alt="20 warm-yellow ink-line mini-theater"><br>20 · Warm-yellow ink-line mini-theater</td></tr>
</table>

These previews show static styles; actual drawing pacing follows the finished films.

</details>

### Six arms and pen-only

![Six hand presets](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/docs--media--hand-presets.jpg)

| Preset | ID |
|---|---|
| Navy-sleeve marker | `navy` |
| Female glitter diamond long nails | `diamond-woman` |
| Child colored pencil | `child-pencil` |
| Shangguan Wan'er-inspired brush | `waner-brush` |
| White-shirt neutral pen | `teacher-pen` |
| Knit-sleeve fountain pen | `cardigan-fountain` |
| Pen only, no arm shown | `pen-only` |

Arm assets are calibrated by nib position and wrist distance, with the forearm crop edge kept off-canvas. Custom assets need nib-positioning parameters; an arbitrary image cannot be treated as already fitted. Brush ads sit on the brush barrel only; brand or copyright statements are never auto-added.

### Brush ads and floating watermarks

Two independent options, both empty by default. Author promo text in samples is not forced content after install.

| Option | Placement and purpose | Rules |
|---|---|---|
| Brush ad | Follows the brush; good for short brand or account names | Max 16 characters; original text kept; long text prefers wide barrels and needs a real readability check |
| Floating watermark | Inside the picture; for account, attribution, or short release info | Max 180 characters, newlines allowed; low contrast and slow-moving by default, avoiding bottom subtitles |

Example: "brush ad @leo_xiaolei; watermark 'hand-drawn narration · @leo_xiaolei', lighter, avoiding subtitles." Or simply say "no ads, no watermark".

### Voice catalog and references

Only the following six are kept; the picker and text guides use the same names and order.

| Name | Preview | Production reference |
|---|---|---|
| Realistic male | [Excerpt from "The Snake That Died of Anger"](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2.3/assets--voices--previews--realistic-man.wav) | Reuses that film's original reference; public installs must import their own |
| Realistic female | [Excerpt from "How Happiness Imprisons You"](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2.3/assets--voices--previews--bilibili-outro.wav) | Reuses the current film's extracted vocals; public installs must import their own |
| Sweet senior | [Same-text preview](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--voices--previews--sweet-student.wav) | Downloadable synthesized reference |
| Iapetus | [Same-text preview](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--voices--previews--iapetus.wav) | Downloadable synthesized reference, not a direct call to Gemini native TTS |
| Intellectual female lecturer | [Same-text preview](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--voices--previews--calm-woman.wav) | Downloadable synthesized reference |
| Story narrator | [Same-text preview](https://github.com/threerocks/sweety-handdraw-video/releases/download/v1.2/assets--voices--previews--story-narrator.wav) | Independently kept synthesized reference |

The first two play excerpts from the corresponding finished films; the other four play the same passage. Finished-film excerpts are for preview only and must never be used back as original references for cloning. The realistic female voice is currently provisional; perceived quality still needs tuning; renaming does not mean new listening acceptance.

The author keeps the original references of both realistic voices on their own machine and can select them directly. Public install packages exclude these recordings: you can preview them without importing, but cannot generate with them. See [Local voice runtime](skills/sweety-handdraw-video/references/local-runtime.md) for importing. Personal and task directories may only supplement references for these six; no other timbres or legacy names may be added back to the picker.

You may name one of these six directly or delegate the choice to the model. An existing local default voice takes priority when kept; an explicit choice this round wins. Voice reference, verbatim script, and generation parameters are kept separately; changing backend or script still requires a listening check.

## From input to finished film

1. **Decide what to say.** Clarify audience, original-text constraints, key facts, and explanation route; avoid stretching a simple point into a long film.
2. **Decide what to draw.** Build the storyboard, art direction, and drawable plan; generate illustrations and actually check key story beats and contact relations.
3. **Finish the voice first.** Synthesize the full narration, check text, noise, skipped reads, repeats, and ending, then lock the voice master.
4. **Arrange pictures from the voice.** Draw semantic regions from timestamps, hand-write key sentences character by character, fill colors, lay out subtitles and scene changes.
5. **Check a representative scene, then export the full film.** Engineering checks and actual picture-viewing / audio-listening are recorded separately; failed items are fixed before delivery.

Delivery includes **MP4, cover PNG, SRT, narration script, and check records**; the production directory keeps assets, configs, and timestamps for traceability and revision.

## FAQ

**How long and how much does one video take?**

The author's earlier answer for a roughly two-and-a-half-minute sample was "about 10 minutes"; in their own $200 subscription environment they observed quota usage of "under 1%". That is a self-report for a specific work and account, not uniformly metered, and cannot be converted into a fixed unit price or service promise. First-time environment setup, model downloads, queueing, image retries, voiceover repair, and manual checks all change the time. [Original time reply](https://x.com/leo_xiaolei/status/2109590999672725537) · [Original quota reply](https://x.com/leo_xiaolei/status/2109591088327749905)

Open source does not mean the whole pipeline is free. Model hosts and image-generation capability may charge; local voiceover still costs hardware, power, and time. The current finished-film pipeline adds no per-second-charged image-to-video service.

**To change one sentence, must everything be redone?**

Changing only subtitles, brush text, watermark, or picture layout can reuse unchanged pictures and audio, then re-render and re-check. Changing narration body text affects the continuous voiceover and usually needs a regenerated full master, re-alignment, and affected timeline rework; seamless single-sentence voice replacement is currently not promised.

**Can it steadily produce series or batches?**

A series may pin style recipe, image endpoint, character anchors, hand, and voice reference, and reuse approved assets. Character, story, or timbre drift is still possible, so every piece must be checked. It is currently not an unreviewed batch system promising stable pass-through for arbitrary input.

**Do Windows / Linux / Gemini / Claude work?**

Current full-pipeline verification comes from Apple Silicon Mac + Codex. v1.0.0 had Linux CI dependency, regression, and short-film compositing records; that does not equal new local-TTS verification on Linux. Windows and other agent / image-generation combinations have not completed end-to-end verification. Confirm execution, image-generation, font, and voiceover capability before judging portability.

**Why check for noise when there is no background music?**

Reference audio may carry music or noise, and voice synthesis may produce artifacts. No added music only means no extra mix track; it does not prove the narration is clean. Program metrics and ASR cannot replace listening acceptance.

**Does it write in standard Chinese stroke order? Any eraser transitions?**

Currently it draws character by character along glyph outlines; standard calligraphic stroke order is not guaranteed. Eraser transitions are a received suggestion with no delivered evidence in this release, so they are not listed as a working feature.

## Built-in dependencies, development, and verification

Only one Skill is registered in the install directory. The following five ship as fixed snapshots and need no separate install; the runtime never auto-tracks upstream updates:

| Built-in item | Purpose |
|---|---|
| `sweety-image-art-direction` | Subjects, contact relations, composition, proportions, and viewing priority |
| `hand-drawn-styles` | Style recipes, reference anchors, and previews |
| `omniailab-ai-director` | Director planning, narrative, sound-picture relations, and final review method |
| `huashu-art-motion` | Art-motion method for applicable genres |
| `sweety-image-privacy` | Image-privacy metadata cleanup and audit; this project then strips simulated camera fields |

See [Dependency notes](skills/sweety-handdraw-video/references/dependencies.md) and [Third-party notices](THIRD_PARTY_NOTICES.md) for sources, licenses, and pinned snapshots.

```bash
python -m unittest discover -s skills/sweety-handdraw-video/scripts -p 'test*.py' -v
python skills/sweety-handdraw-video/scripts/smoke_test.py --output /tmp/handdraw-smoke-new
python3 tools/package.py --output dist
```

v1.2.3 record: 59 regression tests passed, 1,215 bundled dependency files verified, 21 style previews checked. That result cannot replace video-perception, story-fact, listening, or learning-effect acceptance. See [Validation scope](docs/validation.md) for more records.

This v1.2 lock includes full continuous narration, narration-only default, story checks and ending protection, original-film reference import, voice source tracing, optional offline denoising, a local primary-voice catalog, plus subtitle uniqueness and asset-cache checks. Earlier local 1.3/1.4/1.4.1 were unpublished development iteration numbers; public tags are pinned to v1.2 as specified here.

## Credits and license

Main project code and docs use [MIT](LICENSE). Bundled projects keep their own copyrights and licenses; AI-generated hands, reference audio, and similar assets gain no real-person identity or official character authorization from that. Third-party content and voice use still needs your own rights confirmation.

Thanks to [huashu's huashu-art-motion](https://github.com/alchaincyf/huashu-art-motion), the OmniAiLab AI director, the hand-drawn style library, and the art-direction providers — and to commenters for concrete feedback on pauses, subtitles, female voice, script revisions, and runtime environments.

If useful, star the repo; production results and issues are welcome with version, runtime environment, and reproducible snippets. Demos and updates: [@leo_xiaolei](https://x.com/leo_xiaolei).

## v1.2 asset and subtitle rules

Public assets live separately as GitHub Release attachments; hand and voice presets download on first use and are verified by size and SHA-256; valid caches are reusable offline. Netlify is currently paused because the team quota is exhausted; GitHub is used instead per author confirmation. Original-film voice references stay local only. See [Hosting & caching](skills/sweety-handdraw-video/references/hosted-assets.md).

Exactly one layer owns subtitles. Duplicate events, extra embedded subtitle tracks, and same-directory auto-loaded subtitles are forbidden; editing files go in `editing/`; preview pages carry no subtitle track. Final actual frames must still be inspected; program checks never replace watching.

### Art direction before every image

v1.2.1 adds non-empty checks for briefs and actual prompts, content-hash binding, and 7 targeted regressions. Covers, body images, characters, props, new hands, candidates, edits, and rework must all apply the built-in `sweety-image-art-direction` first; missing direction records, empty files, changed prompts, changed images, or failed visual review all stop production. It constrains process and evidence; it cannot replace the model actually viewing images.
