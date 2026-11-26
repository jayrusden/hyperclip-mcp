# Pacing and timing numbers

These numbers are the shipped Hyperclip defaults. They can be tuned, so prefer a number a tool returns (`get_design_defaults`), and anything the user states.

## Picture-change interval (how often something changes on screen)

| Scenario | Change the picture every |
| --- | --- |
| Points | 2 to 4 s (each new point is its own change) |
| Conversation | 3 to 5 s; a long turn runs 5 to 13 s before it needs a punch or a card |
| Reveal | about 3 s, slowing toward the payoff; the payoff is the last change and is held |
| Lesson | 5 to 13 s |

Use only the row for the scenario you named. A picture change is a cut to a new segment or the other face, a punch, a layout switch, or a card; each one resets the clock. A single picture held past 13 s is flagged as too long unless the user asked for the hold. When the same picture reaches the top of its row, punch, change layout, or cut away. Every cut and punch lands on a pause or a turn, never inside a word.

## Fixed durations

| Thing | Length |
| --- | --- |
| Punch-in (zoom on a high-trust claim or a pivot) | held 2 to 5 s, then back out on a pause |
| Reaction insert (cut to the listener on a real nod, laugh or turn, vertical or square only) | 1.5 to 2.5 s, then back to the talker on a pause |
| Hook: claim, number, result or tension must land | within the first 3 s |
| Name strap, if the user asked for one | on screen within the first 2 s |
| Title hook on a non-Reveal clip | clear it once the hook has landed, within about 5 s |

A layout change uses the house transition length: do not state one unless the user did. A reaction must be real and seen in a frame: with none in range, the change is a punch or a card, never an invented face flash.

## Edge padding (set at every trim)

- In-point: about 150 ms of real silence before the first kept word (cap about 200 ms).
- Out-point: about 300 ms after the last kept word (cap about 400 ms).
- Only in silence: never into another word. Under about 50 ms of silence available means there is nothing to pad into: keep the hard cut and tell the user (see `clip-selection`).
- Interior joins that are meant to be seamless stay contiguous with no pad. Exact user times and verbatim quotes get none either.
- A dangling opener ("And", "So", "But") of 1.5 s or less at a clip edge can be trimmed off automatically; longer than that, look for a better sentence start.

## What is not covered

There are no rules here for audio (music, ducking or levels are handled by the host), for transitions between shots beyond the layout morph, or for reordering a line to the front of a clip. Do not invent them: if the user asks for one, say it is not available rather than improvising.
