---
description: Make short captioned clips from a Hyperclip recording
argument-hint: "[video name or brief, e.g. \"3 LinkedIn clips from the pricing webinar\"]"
---

Load the `create-clip` skill and follow it from the brief gate.

The request: $ARGUMENTS

If that is empty, or names no source video, call `list_library` first, show the user their recent videos in a short list, and ask which one to use (plus the platform and angle if still unknown). If the library is empty, call `upload_video` so the user can add one (see the `/upload-video` command).
