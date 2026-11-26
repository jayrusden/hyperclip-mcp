---
description: Add a video to your Hyperclip library so you can clip it
argument-hint: "[optional title or note, e.g. \"Q3 pricing webinar\"]"
---

Call `upload_video` right away so the upload card appears in the chat. Do not ask questions first.

Optional note from the user: $ARGUMENTS

Tell the user in one line to use "Upload in Hyperclip" on the card, drop the video on the page that opens, and come back. Do not poll: the card goes from Transcribing to Ready by itself.

When the card shows Ready, ask what they want to clip (platform and angle), then load the `create-clip` skill and follow it from the brief gate. If they gave a note above, use it to recognise the new video in `list_library`.

In clients without cards (Claude Code), call `get_upload_link` instead, give the user the link, and check `list_library` once they say the upload is done.

If the card says there is no topic summary yet, that is fine: `get_video_topics` may be empty, so find moments with `read_transcript` instead. Never make the user wait for topics before clipping.
