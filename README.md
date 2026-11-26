# Hyperclip plugin for Claude

Bundles the Hyperclip remote connector with six skills and two commands that teach Claude how to cut good clips with it.

- `.mcp.json` points at the remote Hyperclip MCP server (`https://mcp.hyperclip.io/mcp`, streamable HTTP, OAuth).
  The first tool call starts the Hyperclip sign-in (Supabase OAuth) in the browser. Hyperclip only reads and edits the signed-in user's own library and sends clip data to that server; nothing else is fetched or run.
- `commands/upload-video.md`: `/upload-video [note]` opens the upload card.
- `commands/upload-video.md`: `/upload-video` adds a recording to your Hyperclip library so you can clip it.
- `skills/` (each is `SKILL.md`, with longer material in `references/`):
  - `create-clip`: the end-to-end task (brief, draft, live studio, iterate, QA, render, batches, failures). It is also the `/create-clip [source or brief]` entry in the slash menu; there is no separate command, so the menu shows it once.
  - `clip-selection`: map-first scouting, hooks, exact cut points, padding, multi-part joins, length, titles.
  - `clip-craft`: pacing scenarios, punches, cards, name straps, layouts and templates, what the tools cannot do yet.
  - `captions-and-style`: captions on by default, placement and safe zones, looking after changes, fixing wrong words.
  - `framing-and-platforms`: canvas presets and lengths, fill versus letterbox, manual reframing, canvas-change checklist.
  - `clip-qa`: the pre-render checklist and the rules for resolving clashes honestly.

Local trial: `claude --plugin-dir ./claude-plugin`, or add the connector URL above as a custom connector in Claude.ai (Settings > Connectors) and upload the skill folders.
Sign in with any Hyperclip account; you can create one free at https://hyperclip.io/signup.

## Privacy, terms and support

- Privacy policy: https://hyperclip.io/privacy
- Terms of service: https://hyperclip.io/terms
- Support: team@hyperclip.io
