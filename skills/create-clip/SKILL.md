---
name: create-clip
description: End-to-end workflow for making Hyperclip clips. Load it whenever the user asks to make, cut or create clips, highlights, shorts or reels from a recording, e.g. "make a clip about pricing", "cut highlights from this webinar", "turn this recording into shorts", "clips for LinkedIn or TikTok", or "make 5 clips". Covers the brief, drafting, live review, QA and rendering, and hands off to the other Hyperclip skills.
---

# Create a clip

This skill is also the `/create-clip` entry in the slash menu. If the user ran it with a request after the name ("3 LinkedIn clips from the pricing webinar"), that is the brief. If it names no source video, call `list_library`, show their recent videos in a short list and ask which one (plus the platform and angle if still unknown); if the library is empty, call `upload_video`.

The path from "make me a clip" to an exported MP4. The loop is: draft, edit in chat, then export when the user says it is final. You do the judging; the tools read and write. Hand off to the other skills at the steps below instead of improvising their rules.

## 1. Brief gate

Before touching the transcript, decide whether the brief is clear. It is clear when the user named a moment (a quote, a time range, a specific claim), or gave a job plus enough constraint (platform, audience, theme, length, must-include).

If it is not clear ("make clips", "clip this", "best bits"), ask once, in one message, and stop. Ask only for what is missing and would change which moments you pick:

- platform or channel (never guess it; see `framing-and-platforms`)
- length and how many clips
- the angle: what the clip should argue or emphasise, anything that must be included

Do not re-ask what they already said. Do not ask again after they answer: go and do the work. For a batch, ask once for the whole batch.

## 2. Find the moments

Find the video with `list_library`. If it is not there, call `upload_video`: it shows an upload card in the chat where the user adds the file and watches it transcribe. Do not poll; when the card shows Ready, ask what to clip. (`get_upload_link` is only a plain link, for clients that cannot show cards.) Then follow `clip-selection`: read broadly first, search only for phrases you know exist, shortlist two or three, name your pick.

## 3. Draft

Call `draft_clip` once per new clip, with a 3 to 6 word title and a one to three sentence summary. A story told from several places is one clip with `segments`, in playback order. Set `canvas_preset` if the platform is known. Captions come on by default. Read the result: it can carry framing and layout warnings and a cut check that you act on in step 5. To try a variant (another length, canvas or angle) without touching the original, `duplicate_clip` it and edit the copy.

Each "make a clip" request is a new draft. Only edit an existing clip when the user is clearly changing that clip.

## 4. Open the studio once

Right after the first draft call `open_clip` once (use `clip_ids` for a batch). It shows a compact clip card in the chat that refreshes itself after every edit. The user opens the full studio from that card with a click (it opens in Claude's side panel, and closing the panel returns the card), so do not call `open_studio` unless they ask you to open the studio. Never call `open_clip` again to refresh it; call it again only if the user asks or has closed it. To change a clip that already exists in the library (found with `list_library`) and that the user has not seen in this chat, call `open_clip` for it once before editing, so the card shows the clip you are working on.

## 5. Iterate in chat

Change the clip with `update_clip`, `apply_template` (footage-only layouts, such as a two-speaker split) and `frame_speakers` (crop to the speaker), using the specialist skills: `clip-craft` for layout and pacing, `captions-and-style` for captions and type, `framing-and-platforms` for canvas and cropping. `get_brand_kit` gives brand colours and font for captions and `get_design_defaults` the house numbers. Captions are the only text on screen in the beta; there are no graphics, title hooks or name straps. Rules for this loop:

- Describe each change in one plain line ("Tightened the start by half a second"). No tool names, no numbers the user did not ask for.
- When you do not have a clip's current bounds in this conversation, call `get_clip` before editing.
- Every clip stays editable, also after it is exported. Editing an exported clip marks it "edited since last export" and leaves the last export downloadable. Do not export after each edit, and do not re-export on your own after later edits. If the user seems done and the clip shows "edited since last export", offer to export it again once.
- Captions go on by default (see `captions-and-style`).

## 6. QA, then offer to export

Run `clip-qa` on every clip before you offer to export (it starts with `read_clip_transcript`, which lists what to fix). Fix what it finds in one edit, and disclose anything you could not fix in a clause.

Do not export until the user says the clip is final (each export makes an MP4 and uses their export capacity). When they do: `render_clips` with the clip ids. It returns at once and the chat shows an export card with progress and a Download button, so do not poll `get_job_status` and do not repeat the link; say in one line that it is exporting. Use `get_job_status` only if the user asks about an export or no card appears. Only a couple of exports run at once; queue bigger batches in small groups. Exporting a clip that was exported before makes a new MP4 and keeps the earlier ones. While a clip is exporting, edits are refused: the export renders the clip as it was when it started, so a mid-export edit would not be in the MP4. Tell the user to wait for the export card to finish, then make the edit (it then shows as edited since last export).

## Batches

Run the brief gate once for the whole batch. Then take every clip through draft, edits and QA, keeping a one-line status per clip (drafted, editing, checked, done, failed). Open the studio with `clip_ids` once. Finish with a short list: each clip's title, length and status. The batch is done when every clip is done or failed; never imply the whole batch worked if one failed.

## When something fails

- Fix the arguments from the error message and retry, up to 3 times on the same item.
- Then stop and say plainly what you produced, what failed and what you need from the user. Do not loop silently.
- A missing video or empty library is not a retry: use `upload_video`.
- If an export fails, say so in product terms ("the export failed"), try one targeted fix if the cause is a small setting you control, otherwise tell the user what to do.
- A stuck or choppy preview is not an edit. Leave the clip alone.

## Closing each turn

End with a short recap of what changed (one sentence for a small edit, a few bullets when several things changed), then one sentence offering the next step that would make the clip better. Never leave a turn silent after a long tool run.
