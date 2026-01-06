---
name: imsg
description: iMessage/SMS CLI for listing chats, history, watch, and sending.
homepage: https://imsg.to
metadata: {"clawdbot":{"emoji":"📨","os":["darwin"],"requires":{"bins":["imsg"]},"install":[{"id":"brew","kind":"brew","formula":"steipete/tap/imsg","bins":["imsg"],"label":"Install imsg (brew)"}]}}
---

# imsg

Use `imsg` to read and send Messages.app iMessage/SMS on macOS.

Requirements
- Messages.app signed in
- Full Disk Access for your terminal
- Automation permission to control Messages.app (for sending)

Common commands
- List chats: `imsg chats --limit 10 --json`
- History: `imsg history --chat-id 1 --limit 20 --attachments --json`
- Watch: `imsg watch --chat-id 1 --attachments`
- Send: `imsg send --to "+14155551212" --text "hi" --file /path/pic.jpg`

Notes
- `--service imessage|sms|auto` controls delivery.
- Confirm recipient + message before sending.

## Attachments from Daemon Context

⚠️ **Important:** When running as a daemon (launchd), Messages.app cannot access files from `/tmp` or other restricted directories.

**Always copy files to a user directory first:**
```bash
mkdir -p ~/Pictures/clawd-share
cp /tmp/photo.jpg ~/Pictures/clawd-share/
imsg send --to "+1234567890" --file ~/Pictures/clawd-share/photo.jpg
```

Files from `/tmp` will appear in the Messages DB but won't actually send (`is_delivered=0`).
