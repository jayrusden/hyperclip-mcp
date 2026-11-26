---
name: clip-selection
description: Choose and cut the right moments from a long recording with the Hyperclip tools. Load when finding highlights, quotes or "the best bits" in a podcast, webinar, interview or talk, when setting clip start and end points, stitching several parts into one clip, trimming or tightening an existing clip, or hitting a target length.
---

# Selecting and cutting clips

You do the judging. The tools only read and write, so work from what was actually said.

## Find the moments

1. **Map first.** For an open brief ("best bits", "a 60 second case study", "what's in here"), read the whole source as text with `read_transcript` before searching. One call returns a page (default 5 minutes, max 15); follow `next_start_sec` while `has_more`. A map is complete and cheap. Sampling with guessed keywords is neither. With two or more sources, map each before you draft.
2. **Search is lexical, not semantic.** `search_transcript` finds literal words, so use it to locate a phrase you already know is there (a quote, a proper noun), with one to three word anchors. It cannot find themes. Each hit is a window of about a minute around the match (the match time is given); read the lines around it to find the real start.
3. **Stop rule.** After 2 searches in a row with no hits for the same goal, stop guessing keywords and read the source instead.
4. **Shortlist.** Pick two or three candidate windows, choose the strongest on specificity and "makes sense alone", and name your pick and why in one clause.
5. **Stand-alone test.** Good: a claim with its proof, a story with a payoff, a sharp answer to a clear question, a contrarian take. Reject warm-ups, housekeeping, and anything that needs the previous minute to make sense.
6. **Hook.** The opening 3 seconds must hold the claim, number, result or tension, never a greeting or setup. A clip that opens "So, um" or on a dangling "And" is not ready.

Skip the map when the user gave exact times or quoted the exact line: go straight to that window. Reuse transcript you already read in this conversation instead of fetching it again.

## Set the bounds

Read just the 20 to 40 seconds around each edge with `read_transcript` and `include_words=true`; every word comes back as `word@start-end` in seconds, after the readable text. Word timings cost far more than plain text (those pages are capped at 2 minutes), so only ask for them where you are cutting.

- Snap in and out points to sentence (or clause) edges.
- Add breathing room, but only in real silence: a short beat before the first word and a slightly longer one after the last. Defaults are about 0.15 s in (cap 0.2 s) and 0.3 s out (cap 0.4 s); the exact numbers are not critical. Never pad into another word or past the end of the video.
- No silence to pad into? Do not drag in a neighbouring sentence's tail and do not open on a dangling "And / So / But". Look a couple of seconds either way for a sentence start that has a pause before it. If none, keep the hard cut and tell the user in one clause.
- Exceptions: exact times from the user are used verbatim (no snapping, no padding); pasted quotes get the tightest word-bounded span.
- Never end on "so / and / but / the". Pull back to the last full sentence.
- Length: a stated target means within about 5 seconds either way. If the story needs much more, say so before drafting.

## Several parts in one clip

Use `draft_clip` with `segments` in playback order (up to 24).

- Every cut between parts of the same source lands in a gap between two words, never inside a word. Mid-word cuts replay a syllable.
- A join that is meant to be seamless stays contiguous: end of one part equals start of the next. Do not pad it.
- When you remove a short stammer or aside, start the next part at least 0.2 s after the last removed word, so a leftover syllable does not play.
- Check narrative continuity: skim the assembled words for a "so / because / that's why" pointing at a reason you cut, a pronoun with no antecedent, or a statistic with no lead-in. Restore the bridge, unless the user asked you to cut it.

## Titles and summary

Title: 3 to 6 words, sentence case, about the content ("Why bounce rate misleads"), never "Clip 3" or a time range. Summary: one to three sentences, true to what is said.

## After drafting

Hand back to `create-clip` for review and iteration. For details on edge cases (cuts that leak a word, excised joins, reading existing clip bounds), see [references/bounds-and-joins.md](references/bounds-and-joins.md).
