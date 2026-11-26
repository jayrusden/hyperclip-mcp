---
name: clip-craft
description: Make a Hyperclip clip feel edited rather than static. Load when choosing pacing, layouts, split screen, picture-in-picture, stat cards, title hooks, name straps, logos or brand colours, when a clip needs to feel more engaging, or when the user asks for a template, a graphic, a slide beside the speaker, or an overlay.
---

# Clip craft

Substance over polish: the audience for this work rewards a clip that looks like it came from a practitioner, not a studio. A stated request always wins over anything here.

## Name the scenario

Decide which of four rhythms the clip has, from its content, and change it if the job changes mid-clip.

| Scenario | Content | How the picture changes |
| --- | --- | --- |
| Points | a list, steps, a comparison | each new point is a new picture: a cut, a layout change or a card |
| Conversation | two or more people trading lines | cut to the new talker on a turn; break a long turn with a punch or a card |
| Reveal | one setup, one payoff | changes slow toward the payoff, which is the last change and is held |
| Lesson | one speaker teaching | a punch, heading card or layout change on each new idea, not a cut to another face |

Cadence: a clip that holds one picture for too long reads as static. Change the picture inside the interval for the scenario (Points 2 to 4 s, Conversation 3 to 5 s, Reveal about 3 s slowing toward the payoff, Lesson 5 to 13 s), and never hold one picture past 13 s, unless the user asked for a long hold. Exact numbers for punches, reaction inserts, hook and strap timing and edge padding: [references/pacing-and-timing.md](references/pacing-and-timing.md). A picture change is a cut to another segment or face, a punch, a layout switch, or a card. Land every change on a pause or a turn, never inside a word.

## Punch

A punch is a zoom-in on a high-trust claim or a pivot in the argument, held for 2 to 5 seconds and released on a pause. It is a transform change on the segment (see `framing-and-platforms` for how to adjust a crop), not a layout.

## Words on screen

- One card per line, on the line it belongs to, with its own span (`show_at` / `hide_at`, in seconds on the clip timeline) and its own `name`. Never one card holding every number for the whole clip.
- Spoken number or result: a stat card. A list being worked through: points, revealed one item per line. Before and after: compare. A quoted line: quote.
- Copy states only what was said or what the user told you. Copy numbers and names exactly as spoken. Do not decorate: no graphic without a job.
- A title hook is the on-footage opening line. It suits a Reveal, where it stays up through the clip. On other scenarios take it down once it has landed.
- Restraint: a handful of graphics before a frame feels cluttered. When in doubt, remove. One highlight colour per sentence.
- Colour restraint: a brand colour belongs on a strip, plate, border or highlight, not flooded over the whole background. Do not use per-word rainbow colours.

## Name straps and logos

Both are opt-in. Do not add either unless the user asked. For a name strap, ask for the exact spelling and an optional role if you were not given them, and never invent either. Find it with `find_templates` and place it within the first few seconds of the clip.

## Layout: follow the speaker relationship

`apply_template(clip_id, layout, name, slots, ...)`. Every second of a clip sits under a layout; a draft without one shows plain footage. Switch layout only when the relationship changes.

| Situation | Layout |
| --- | --- |
| speaker is still the point and a visual helps | `split` (side by side on landscape, stacked on tall canvases); `split-stack` for two visuals |
| the picture is the point (slide, chart, demo) | `full` with the graphic, voice carries |
| a slide or demo is the point and the speaker is a small inset | `pip` (`corner` picks the corner) |
| two speakers plus a visual on landscape | `thirds` |
| nothing to show | footage only; a person stays in frame |

A graphic that sits beside or replaces footage goes in a layout slot, not floated over the footage. Titles, captions and name straps are the overlays. Slots are `a`, `b`, `c`, each `"footage"` or `{"elementId": "...", "props": {...}}`. Find graphics with `find_templates` and use the props it lists; do not invent props or markup. Reusing a `name` replaces that span. Same element twice in one layout needs distinct ids.

After any layout change, look with `get_clip_frame` inside the span (see `clip-qa`). Two speakers on a vertical or square canvas, and the cross-speaker rules, are in `framing-and-platforms`.

## What the tools cannot do yet (be upfront)

- There is no tool to author a custom lower third, motion graphic or new element. Only graphics that `find_templates` returns can be placed.
- There is no brand kit read. Brand colours, fonts and logos come from the user (ask for hex colours; do not guess "the brand"). A logo cannot be placed from a URL or invented; say so and offer what is possible.
- Do not say a capability is unsupported when it is: multi-part stitching, layouts and captions all work.

More detail on scenarios and layouts: [references/scenarios-and-layouts.md](references/scenarios-and-layouts.md).
