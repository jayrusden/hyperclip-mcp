---
name: clip-craft
description: Make a Hyperclip clip feel edited rather than static. Load when choosing pacing, cuts, punches, or a two-speaker layout, when a clip needs to feel more engaging, or when the user asks for graphics, overlays, name straps, titles or logos (to say they are not in the beta).
---

# Clip craft

Substance over polish: the audience for this work rewards a clip that looks like it came from a practitioner, not a studio. A stated request always wins over anything here.

## Name the scenario

Decide which of four rhythms the clip has, from its content, and change it if the job changes mid-clip.

| Scenario | Content | How the picture changes |
| --- | --- | --- |
| Points | a list, steps, a comparison | each new point is a new picture: a cut or a layout change |
| Conversation | two or more people trading lines | cut to the new talker on a turn; break a long turn with a punch |
| Reveal | one setup, one payoff | changes slow toward the payoff, which is the last change and is held |
| Lesson | one speaker teaching | a punch or layout change on each new idea, not a cut to another face |

Cadence: a clip that holds one picture for too long reads as static. Change the picture inside the interval for the scenario (Points 2 to 4 s, Conversation 3 to 5 s, Reveal about 3 s slowing toward the payoff, Lesson 5 to 13 s), and never hold one picture past 13 s, unless the user asked for a long hold. Exact numbers for punches, reaction inserts and edge padding: [references/pacing-and-timing.md](references/pacing-and-timing.md). A picture change is a cut to another segment or face, a punch, or a layout switch. Land every change on a pause or a turn, never inside a word.

## Punch

A punch is a zoom-in on a high-trust claim or a pivot in the argument, held for 2 to 5 seconds and released on a pause. It is a transform change on the segment (see `framing-and-platforms` for how to adjust a crop), not a layout.

## Layout: only footage

`apply_template(clip_id, layout, name, slots, ...)` with `layout` one of `full`, `split` (side by side on landscape, stacked on tall canvases) or `split-stack`, and every slot set to `"footage"`. Use a split when two speakers should each keep their own picture for the whole clip. Switch layout only when the relationship changes; every second of a clip sits under a layout, and a draft without one shows plain footage. After a layout change, look with `get_clip_frame` inside the span (see `clip-qa`). Two speakers on a vertical or square canvas, and the cross-speaker rules, are in `framing-and-platforms`.

## What is not in the beta (be upfront)

- Captions are the only text on screen. There are no title hooks, name straps, lower thirds, stat cards, quote cards, logos, slides or picture-in-picture, and no tool to add them. If the user asks, say it is not available in the beta and offer what is: stronger cuts, a punch, captions styled to their brand colours.
- Do not say a capability is unsupported when it is: multi-part stitching, footage layouts, speaker framing and captions all work.
- `get_brand_kit` reads the workspace's brand colours and font (there may be none). Use them for captions; with no kit, ask for hex colours rather than guessing "the brand".
- `get_design_defaults` returns the house numbers for padding, hook window, held pictures and caption size when you need them.

More detail on scenarios and layouts: [references/scenarios-and-layouts.md](references/scenarios-and-layouts.md).
