# Optional Production Settings at Start

At least one of theme, story, attachment body text, or a readable explicit source is required. Only when all are missing do we ask the user for content. Everything else is a preference and never a stop-work condition. Missing runtime capability or quality failures are fixed or accurately reported by the model — never padded with fake audio, images, authorizations, or pass records.

## Default Plan & Waiting

Before opening the selection window, the model writes `JOB/options/defaults.json` from the content — do not pin every work to the same catalog number, arm, or voice. Advertising and watermarks default to blank; unless the user provided text, never invent brands, accounts, or copyright notices. Fonts and subtitle forms follow backgrounds and the user's previously approved works.

```json
{
  "content":"The user's actual theme or story summary",
  "style":{"mode":"inferred"},
  "hand":{"preset":"pen-only"},
  "voice":{"voice":"iapetus"},
  "advertising":{"text":""},
  "watermark":{"text":""},
  "captions":{"mode":"quiet"},
  "rationale":{
    "style":"This everyday-mechanism piece needs independently designed concise figures and props",
    "hand":"Pen-only reduces occlusion of small mechanism diagrams",
    "voice":"Iapetus suits this plain narration",
    "captions":"A light frame suits plateless dark-gray short sentences"
  }
}
```

The example is not a fixed default. Style modes: `catalog`, `catalog_auto`, `inferred`, `custom`; full conventions in [Style File](style-and-drawing.md). Subtitle modes: `quiet` or `plate`.

The user's already-decided choices go in `options/explicit.json` with the same six keys; those items skip waiting. When the user says "auto-pick everything", the model's plan is recorded as delegated auto-execution without forcing six rounds of options. Revisions of finished films carry over approved choices unless this session requests changes.

```bash
python3 scripts/production_options.py JOB --defaults JOB/options/defaults.json --explicit JOB/options/explicit.json
```

Omit `--explicit` when there are no decided options. Open the emitted `PRODUCTION_OPTIONS_URL`. The window presents style, hand, voice, advertising, watermark, and subtitles in order, 60 seconds each. The backend times independently — even without page refreshes it records the default plan and advances. Six minutes max; no re-issued questions to restart the clock. A user pick advances immediately; unsubmitted input never counts as confirmation.

Continue other independent work in ≤60s tool-wait slices, reading `options/selection.json`. After `complete:true`, stop the local service and continue production — never end the round with "waiting for your choice". When this environment cannot open HTML, ask item-by-item with available async Q&A tooling; after 60 unanswered seconds per item, record the default and continue — never fabricate user confirmation. Blank ad/watermark means none added.

## Applying to Production

All decisions keep `value/reason`; timeout causes are `timeout_default`. Never write them as manual user confirmations. Style choices are also saved to `style/selection.json`; the model then writes the actual `style/resolved.json` with full rationale.

After creating `job.json`, merge hands, voices, ads, watermarks, and subtitles with `production_options.apply_choices(JOB)`. CLI:

```bash
python3 scripts/production_options.py JOB --apply
```

The function preserves content and audio paths while updating `hand.preset/pen_text`, `voice_studio.voice`, `voxcpm.voice`, `watermark.text`, and `captions.background/color`. When the user adds preferences after timing ends but before generation starts, update the corresponding selection file first, then apply; explicit changes beat timeout values. Saving config via script is not evidence that style resolution or audio generation happened.

## Advertising, Watermarks & Voices

- Advertising currently lives on the pen barrel only, max 16 Unicode characters; source text is preserved, never auto-truncated. Over-limit optional input shows an error while keeping the timeout mechanism, defaulting to none. 16 characters is a format cap — long phrases may still be illegible on thin barrels; prefer wide barrels or pen-only assets, verified at normal size.
- Watermarks use the user's text, allow line breaks, max 180 characters. Default: low-contrast dark gray, 24% opacity, slow small drift; in-frame, clear of bottom subtitles. Leaving the selector blank adds none. Actual parameters may be tuned by the model per frame — never let a dark watermark cover primary content.
- Six hands plus `pen-only`. Only arm assets require the forearm bottom edge to stay off-frame; full-pen assets skip that forearm check.
- Voices show only six entries: Lifelike Male, Lifelike Female, Sweet Senior, Iapetus, Intellectual Female Lecturer, Story Narrator. The first two play corresponding finished-film excerpts; the other four play same-text samples. Finished-film excerpts are for audition only — production still uses each film's original reference; without the original reference, audition listening is allowed but generation selection is not. User-imported references may only override these six; other voices and legacy names in old directories never reappear.
