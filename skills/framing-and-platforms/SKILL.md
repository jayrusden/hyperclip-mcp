---
name: framing-and-platforms
description: Choose the canvas, length and framing of a Hyperclip clip for a platform. Load when the user names LinkedIn, TikTok, Reels, Shorts, YouTube, X or an aspect ratio, asks what format or length to make, wants a wide recording turned vertical or square, says a speaker is cropped off or off-centre, or wants the full frame, bars or a zoom.
---

# Framing and platforms

## Pick the canvas

Set it with `canvas_preset` on `draft_clip` or `update_clip`:

| preset | aspect | use |
| --- | --- | --- |
| `landscape` | 16:9 | YouTube, websites, webinars, LinkedIn desktop |
| `square` | 1:1 | LinkedIn and X feeds, safe for mixed feeds |
| `portrait` | 4:5 | LinkedIn and Instagram feed, more screen than square |
| `vertical` | 9:16 | TikTok, Reels, Shorts, LinkedIn mobile |

A user-stated canvas always wins. Do not infer the platform from a duration, a "teaser" or "clip with captions". If the destination is unnamed, ask one short question before drafting (as part of the brief in `create-clip`).

Length guidance, which a stated number always overrides: LinkedIn 30 to 90 seconds with the hook in the first seconds; TikTok, Reels and Shorts roughly 20 to 60 seconds (Shorts cap at 3 minutes); excerpts for YouTube 1 to 3 minutes on landscape. If the story does not fit the target, say so before cutting its payoff.

## Fill, do not letterbox

When the source aspect differs from the canvas, fill the canvas by cropping to the person who matters. Empty bars read as a mistake; a crop reads as a decision. Same aspect needs no crop.

Letterbox (whole frame, bars) only when:
- the user asked for it ("keep the whole frame", "don't crop", "bars are fine", or complained that a crop cut something off), or
- the content is a slide, chart, screen share or demo where the whole picture is the point, or a group shot with no single subject.

Never letterbox because the source is low-resolution or "might look soft". If a crop will be soft, crop and mention it in one clause. A stated whole-frame preference persists for the clip: carry it onto new segments and canvas changes.

## Frame the speaker with `frame_speakers`

A wide recording drafted to `vertical`, `square` or `portrait` centre-crops each segment, which often slices a speaker out, and the draft may flag this. Use `frame_speakers(clip_id, preset)` to find the faces and crop each part to the speaker:

1. Call it once the parts and canvas are settled. With one face in a part it frames that person; with several it frames the active speaker.
2. When a part shows several faces and no clear active speaker, it leaves that part alone and says so. Pick the person from the transcript or the user's words and call again with `speaker_by_segment={"<part index from 0>": "speaker_0"}`. Use `apply=false` to only measure.
3. It also reports a caption position that clears the face; use it for captions `y` unless the user stated one.
4. Look with `get_clip_frame` before telling the user the framing is right, never before.

Hand-framing is the fallback when it finds no face or you need something specific:

1. `get_clip_frame` at a moment of speech to see where the speaker sits.
2. Set the crop per segment with `update_clip` and `ops`: `segment.patch` with `fields: {"transform": {"fit": "contain", "scale": ..., "x": ..., "y": ...}}`. Scale 100 shows the whole source frame; a 16:9 source fills a vertical canvas at roughly 3x; larger zooms in. `x` and `y` are the pan anchor, in percent from the top left (50, 50 is centre).
3. Look again with `get_clip_frame`, adjust, look once more, then stop fiddling and tell the user what you did.
4. Never claim the speaker is centred unless you looked at the frame.

Sources that already contain black bars (a panel floating on black, burned-in logos) may need a bigger zoom to clear them; check the frame and mention softness in a clause rather than keeping the bars.

## Several speakers

- Vertical or square: one face fills the frame at a time, usually the talker. Flip to the other speaker on a turn or a real reaction by splitting the segment on a gap between words and changing only the crop on each side.
- Landscape: both speakers keep their own footage slot for the whole clip (`apply_template` with `split`, or `thirds` with a graphic).
- Cross-speaker match: when a clip alternates between faces, compare head size and eyeline across the cuts and adjust the crops so they match. A cut from a big centred head to a small low one looks amateur. Do this after the crops are in, then look at one frame per speaker. An intentional difference the user asked for stays.

## Canvas-change checklist

Changing the canvas, or changing scale or fit so bars appear or disappear, re-opens everything below in the same turn:

1. **Crop.** The old crop was solved for the old canvas. If the new canvas matches the source aspect, remove the transform; otherwise re-frame it for the new canvas.
1b. **Faces.** For a wide recording going vertical, square or portrait, run `frame_speakers` rather than centring by eye.
2. **Caption size.** Re-size font size, width and words per line together for this canvas (`get_design_defaults` has the numbers; see `captions-and-style`).
3. **Caption position.** Re-place it on what the frame now shows. A position earned for another frame is stale.
4. **Bars.** If bars now exist, captions can live in a bar, not over a leftover position.
5. **Other overlays.** Overlays placed against an empty letterbox space now sit on the speaker. Move whatever no longer fits.
6. **Look once** with `get_clip_frame` before offering to render.

## Safe zones

Keep key text away from the top ~10% and bottom ~20% of vertical canvases and inside the middle 80% horizontally. Do not put text over a face.
