# Bounds and joins: worked rules

## Reading bounds you already set
`get_clip` returns every segment's source video and start/end seconds and the total length. Read it before trimming a clip whose numbers are not already in this conversation. To check what a trim removed, compare against word timings: a phrase is inside a segment only if it starts before the segment ends and ends after the segment starts. Do not claim a phrase is cut until you have checked.

## Tightening and "cut the fluff"
1. `get_clip` for the bounds, then `read_transcript` over the clip's range (with words for the edges you will move).
2. For a segment, patch `start_sec` / `end_sec` through `update_clip`. For multi-segment clips use `ops` with `segment.patch`; use `segment.insert` / `segment.remove` / `segment.move` for structure. Patch the fewest values; do not rebuild a clip to move one word.
3. After a fluff cut, pad the new edges again (in silence) in the same edit. Do not leave new bounds flush against the words.
4. A user who sounds temporary ("what if we cut segment 2") wants it tried, not deleted for good; say what you removed so it can be restored.

## Splitting a segment
There is no split tool. Shorten the original with `segment.patch` to the split point T, then `segment.insert` a new segment from T to the old end, same video. T is in source seconds, not clip seconds. Pick T in the gap between two words (at or after the last word's end, at or before the next word's start). Carry over any crop or other settings to the new half when both halves should look the same.

## Join leaks and stutters
- A short excision (a stammer or an aside of a few seconds) can leave the removed word's tail audible at the next in-point. Start the next part at least 0.2 s after that word ends and skip a dangling opener.
- A seamless join whose time falls inside a word (round times like 2157.5) makes that word replay. Move both sides of the join onto the gap between words.
- If a leftover sliver of a removed phrase is under a second, drop the sliver segment rather than nudging into it.

## Exact times
"First 30 seconds", "2:00 to 5:00": use them as given. No snapping, no padding, no quiet corrections. A caveat in one sentence is fine if it lands mid-word.

## Continuity
For testimonials, case studies and before/after clips, read the assembled words once, end to end, before offering to render. A missing "why" or a pronoun with no antecedent is a defect you fix by inserting the bridge segment.
