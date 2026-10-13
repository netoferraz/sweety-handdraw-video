# Sound & Ending Hard Constraints

## Background Music Off by Default

The production mix contains the complete narration only; the code generates no background music. A legacy music.wav is never used even if present. Omit audio.music or set it false; it may be added only when the user explicitly requests it in this session and provides user_instruction and source. Every audio.sound_effects item must carry source, start, gain, beat_id, and reason, stating its necessary dramatic function; looped effects must not be used as disguised music. Asset changes invalidate the build fingerprint.

The mix report records the actual input tracks. With no extra tracks, the narration is loudness-processed and output directly; mixing chains that would drop the tail buffer are forbidden. Finished-film delivery must state: no background music added by default; you may mix your own or pick music on the publishing platform.

## Complete Pronunciation and Short Ending

First write the complete script; the user-specified closing goes in ending.closing_text and must match the end of the full text character-for-character. Then synthesize the whole piece. No marketing slogans are auto-added without a request. An appended closing must not become just another casually spliced ending sound.

Before locking, view the actual tail waveform and check the final sentence and final character against the final track. A complete master keeps at least 0.12s of protection after the last measurable voicing; when protection is missing, first judge whether speech is incomplete — never fake-fix a truncated character with padded silence. Unfinished words or sentences require regeneration or repair in the sound stage with re-verification.

audio.end_hold_seconds defaults to 0.45s, range 0.2–0.8s. Total silence from the master's last measurable voicing to the end of the video must not exceed 1.2s. The tail may be shortened before locking only after verifying it is outer silence; body breaths, pauses, and sentence endings must never be compressed. Detection uses 10ms RMS with a -60dBFS threshold — an engineering boundary, not proof of reading or emotional quality.

By default ending.fade_seconds is 0, holding the finished frame until the end. When fading is chosen, it must occur entirely after the master ends; avoid long all-white or all-black holds. The final subtitle covers the complete pronunciation, plus a short reading hold. The final ending checks audio track, subtitles, hand exit, and visuals together — passing on equal durations alone is not allowed.

If the local model always truncates the final character at EOS, a protective suffix that never enters the finished film may be added during sound production; after generating the whole piece, locate the natural pause between the final sentence and the suffix via actual waveform and timestamps, and remove only the suffix. The last character must be re-verified against the narration source, recording the difference between generated and delivered text; never cut inside active speech.
