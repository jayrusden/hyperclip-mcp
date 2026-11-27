---
name: captions-and-style
description: Turn on, place, size and style burned-in captions on a Hyperclip clip, and fix mis-transcribed words. Load when the user mentions captions, subtitles, text size or style, caption position, brand colours or fonts on text, safe zones, or a wrong word or name in the transcript.
---

# Captions and style

Captions are burned in from the video's transcript through the clip's `Captions` overlay. New drafts come with captions on.

## Captions are on by default

Every clip ships with captions on (most viewers watch muted), unless the user said no; `draft_clip` already turns them on, and `update_clip(captions={"enabled": false})` turns them off. Restyling is a full authoring request: send everything in one `update_clip(clip_id, captions={...})` call.

- `enabled: true`, plus style keys in snake_case: `font_family`, `font_size`, `color`, `highlight` (karaoke-style word highlight), `highlight_color`, `words_per_line`, `caption_width` (percent), `y` (vertical position, percent from the top edge of the caption box), `text_align`, `pill` / `pill_color` / `pill_opacity`, `stroke_color` / `stroke_width`.
- Size for the canvas. Call `get_design_defaults(canvas)` for the house starting values per canvas (font size, width, words per line; on vertical that is 5 words per line, on landscape more). A fresh draft or a captions-on edit on vertical, square or portrait already carries them; a stated style or the clip's existing style always wins. Re-size together (font size, width and words per line) whenever the canvas changes, and check that lines are neither tiny nor running off the sides.
- Stated style wins: if the user gave a font or colours, use them. Otherwise, if `get_brand_kit` returns a kit, use its colours and font; with no kit keep defaults rather than inventing a look. Avoid heavy display fonts like Impact, per-word rainbow colours, and yellow on white.
- One highlight colour per sentence.

## Placement and safe zones

- Keep captions inside the middle 80% horizontally and clear of the platform's UI: roughly the top 10% and bottom 20% on vertical canvases.
- Keep them off faces. Captions are one clip-wide position: with several speakers pick a position that is safe for all of them. If no common position works, use the default and say so in one clause. Never average positions or invent a round number that you did not see work.
- Do not zoom the crop to make room for captions: move the captions (`y`) instead of shrinking the type.
- When the canvas or scale changes, old caption size and position are stale. Re-size and re-place in the same turn (see `framing-and-platforms`).
- Letterbox bars are caption space: put captions in a bar, not over the video at a leftover position. `y` pins the top edge of the box, so it hangs below that line.

## Look after a change

After a caption or typography change (position, size, spacing, font), call `get_clip_frame` at a moment with speech and check: readable, inside the frame, not over a face. Fix and look again. See `clip-qa` for the stop rules.

Do not claim what you did not observe. A still cannot show motion. The caption animations are per-word (highlight, fade, pop); nothing slides a highlight from word to word, and if the user asks for that, say it is not available instead of describing a substitute as if it were that. Describe what you configured when you did not look.

## Wrong words

Misheard names, product terms, numbers: `edit_caption_text(clip_id, find, replace)`. It rewrites the literal phrase in the stored transcript of the source video for that clip's time range. That is the source transcript, so it also changes every other clip cut from the same words, and captions elsewhere. Say that in one clause when it could matter. If the phrase is not found, read what the clip plays with `read_clip_transcript` (or the range with `read_transcript`) and copy the exact spelling. Fix wording through this tool, never by working around it with a text overlay.

## Style versus content

Do not restyle in answer to a content question and do not change timing to fix a style complaint. Report the change in one line ("Captions on, 3 words per line, lower third").
