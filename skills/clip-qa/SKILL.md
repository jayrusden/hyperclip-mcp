---
name: clip-qa
description: Pre-render quality check for a Hyperclip clip. Load before offering to render or export any clip, after finishing edits on a clip, or when the user asks you to check, review or proof a clip. Covers trims against the transcript, openings and endings, joins, length, and when a visual look is owed.
---

# Clip QA

Run this on every clip before you offer to render. It is silent: fix what you find in one edit, and tell the user only about problems you could not fix. Never say "QA", "check" or "gate" to the user.

## 1. Transcript checks

Call `get_clip` for the current bounds, then `read_clip_transcript` for what the clip actually plays: the words in play order with clip-relative times, a marker at every join, and the cut check (silence at the start and end, a wordless tail, dangling opener or closer, leftover speech or split words at joins). Fix what the cut check lists. Use `read_transcript` with `include_words=true` on the source only where you need exact word times to move a cut (a short range, since word timings are long). Then confirm:

- **Trims.** The start and end are where you think they are. If the user asked to cut or end on a particular word, the tool output shows it is out.
- **Opening.** The first words are a hook or a whole thought, not "And", "So", "But", a half sentence, or a leftover word from the previous line. The first word does not land mid-syllable.
- **Closing.** The clip ends on a full sentence, not on "so / and / but / the".
- **Padding.** There is a short beat of silence before the first word and after the last (about 0.15 s in and 0.3 s out), unless the user gave exact times, and not much more: a tail of a second or more with no words is dead air, so trim it. If the source has no silence there, you cannot add one: keep the cut and disclose it (see 4).
- **Joins.** Each cut between parts sits in a gap between words, with at least 0.2 s after a removed phrase. Seamless joins are contiguous.
- **Continuity.** In a multi-part clip, no "because / that's why / so" points at something you cut, and no pronoun lacks its referent. Restore the bridge, unless the user asked you to remove it.
- **Length.** Within about 5 seconds of any target the user stated. If not, trim or say why not.

## 2. A look is owed after pixels change

Use `get_clip_frame` once after your last edit when you changed any of the list below. It returns one still per call (`frame_sec`, seconds into the clip), so pick the one or two moments that matter, such as inside an overlay's window or on a speaker change, rather than sampling many:

- an overlay, card or name strap (look inside its window, not at the clip midpoint)
- a crop or zoom
- captions: position, size or style
- typography or spacing
- the canvas

Also look when an edit result says a look is required, or warns about overlap. What to look for: text readable and inside the frame, nothing over a face, captions not clashing with an overlay, the right speaker framed. Fix what you see and look once more.

Stop rules: never request the same frame twice on an unchanged clip (use a different time or accept you could not verify), and stop after two blank or unusable frames in one turn. If the frame is blank but the preview in the studio shows picture, trust the studio; if it is really empty, tell the user the preview frame could not be rendered rather than changing styles blindly.

## 3. When rules clash: the resolution ladder

1. What the user stated beats everything. A stated trim, canvas or overlay stays even if a frame or finding clashes with it. Name the clash in one clause and ask once if they want it changed; do not quietly undo their choice.
2. What you measured (a tool result for that exact question) beats a default.
3. A default is the fallback only when nothing was stated or measured.

A value you picked yourself is rung 3: it gives way to a measurement that arrives later. A measurement stays good until the thing it was measured against (canvas, scale, bars) changes.

## 4. Honesty

- Describe what you saw, not what you set. If you did not look, say "I set captions to the lower third", not "they sit clear of her face".
- Never claim motion, highlighting or an animation that you did not observe; stills cannot show motion.
- One problem you could not fix, one clause: "It cuts in hard on the first word, as the recording has no pause before it." Do not offer to add padding that does not exist. Do not narrate millisecond numbers.
- Do not call a clip "final" or "polished".
