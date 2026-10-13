# Complete Narration Sets the Visual Rhythm

## Non-Invertible Order

Complete voiceover script → single-pass generation or complete recording → text and audio verification → locked narration master → per-sentence/per-word timestamps → visuals and subtitles → mix and encode → final-track review.

Pages, subtitle phrases, and drawing actions are only markers on the sound, not audio cut units. Preserve breaths, emphasis, turns, and closings — do not read "continuous" as having no pauses at all. Never pad the track with silence to finish drawing, clip breaths, or warp speech rate.

Prefer single-pass synthesis of the whole text. When full-text generation is unstable or the model genuinely cannot handle long form, record or synthesize contextualized passages by semantic units during sound production, complete continuity editing and verification, and export a complete master; this is an explicitly recorded sound-editing path — never claim single-pass generation, and never segment per page. Rework the audio on model truncation, dropped words, or repetitions; never auto-deliver.

## Task Input

```json
{
  "narration": {
    "source": "inputs/narration/master.wav",
    "alignment": "inputs/narration/alignment.json"
  },
  "audio": {"end_hold_seconds": 0.45}
}
```

The master starts at the body text and excludes the fixed 1-second cover. Save a complete WAV; before locking, only whole-track head/tail de-clicking of a few milliseconds, overall loudness adjustment, or verified sound edits are allowed. Local fixes must pick natural low-energy inter-sentence zones in the actual waveform — never cut by ASR timestamps alone; preserve consonant onsets, sentence endings, and breaths, smoothing only a few milliseconds in low-energy zones when needed, and record the true edit count. No per-sentence/per-page edge fades. Body samples stay intact.

Seconds in `alignment.json` are relative to the master start:

```json
{
  "version": 1,
  "source_sha256": "SHA-256 of the actual master file",
  "script_sha256": "UTF-8 SHA-256 of all text concatenated in page and beat order",
  "beats": [
    {
      "id": "opening",
      "text": "Narration matching the audio character-for-character.",
      "start": 0.12,
      "end": 3.86,
      "caption_cues": [
        {"text": "Narration matching the audio character-for-character.", "start": 0, "end": 3.74}
      ]
    }
  ]
}
```

Beats must be complete, in order, text-identical, non-overlapping, and within the master. Marks may have gaps; natural sound in gaps keeps playing. caption_cues are relative to that beat's start, and their concatenated text must equal the beat text; without subdivision, show the full beat subtitle. Obtain timestamps with real ASR/alignment tooling and verify against the source text; never fake real timing by averaging over character counts. Image transitions follow the next page's first-sentence start; the current page holds across breaths.

## Execution and Verification

1. Keep existing options; do not re-ask about voice or art style. Both local voice adapters accept the entire script in one request and generate a complete master; no more per-beat generation loops.
2. Verify the full text, suspected misreadings, sentence endings, and truncation with actual transcription; obtain timestamps and write the alignment above.
3. `doctor/build/render/verify` validate master input, complete content, and hashes. `build` prepends 1 second of cover silence and appends the selected hold to the complete master; body samples are never cut or reordered.
4. Changing transition points only changes visual layout; when a new rhythm is needed, revise and re-verify the audio first, then rebuild timestamps and visuals. Never modify both master timelines at once.
5. Re-verify the complete track after mixing and encoding. Record subjective listening acceptance separately; waveform continuity, ASR coverage, and healthy streams never substitute for listening.

## Existing Finished Film: Sound-Only Fix

When the user requires visuals, subtitles, and watermarks to stay: reuse approved video frames, only remapping presentation times via a monotonic mapping from old subtitle boundaries to new speech boundaries, and re-check the ending hold per the sound & ending hard constraints — never carry over an overlong or clipped old ending; no image regeneration needed. Subtitle text, line breaks, fonts, and watermarks stay; timing follows the sound. The video must be re-encoded, so only content and design retention may be claimed — never byte-identical encoded pixels. Record source and new hashes, the time mapping, the whole-piece sound generation method, and spot-check corresponding frames plus all transition boundaries.

Legacy `beat.audio` in old tasks must not continue as production tracks. Remove non-zero page/sentence pauses, switch trim off, keep pace at 1, and add narration plus alignment. Test fixtures may keep the old segmented interface for regression tests, but finalize always rejects test fixtures.

Background music and final-character protection follow [Sound & Ending Hard Constraints](audio-and-ending.md): narration-only output by default, with at most 1.2s of silence at the end of the finished film.
