---
description: Show Slack bot status and manage global session
---

Show the Slack bot integration status. Check the following:

1. **Configuration**: Read `.claude/claudeclaw/settings.json` and check if `slack.botToken` and `slack.appToken` are set (show masked tokens: first 5 chars + "..."). Show `allowedUserIds` and `listenChannels`.

2. **Global Session**: Read `.claude/claudeclaw/session.json` and show:
   - Session UUID (first 8 chars)
   - Created at
   - Last used at
   - Note: This session is shared across heartbeat, cron jobs, Telegram, Discord, and Slack DMs / non-threaded channel messages.

3. **Thread Sessions**: Read `.claude/claudeclaw/sessions.json` and list the `threads` whose key starts with `slk:` (Slack threads — the key format is `slk:<channelId>:<threadTs>`). For each, show:
   - Channel ID and thread timestamp (parsed from the key)
   - Session UUID (first 8 chars), turn count, last used at
   - Sort by last used (most recent first). Each Slack thread keeps its own isolated session; the bot re-reads recent thread history on every mention, so it has context even when other people have talked in between.
   - If there are none, say so.

4. **If $ARGUMENTS contains "clear"**: Delete `.claude/claudeclaw/session.json` to reset the global session. Confirm to the user. The next run from any source (heartbeat, cron, Telegram, Discord, or Slack) will create a fresh session. Note: this does not clear per-thread Slack sessions; those reset via the Slack `reset`/`clear` message to the bot, or by removing entries from `sessions.json`.

5. **Running**: Check if the daemon is running by reading `.claude/claudeclaw/daemon.pid`. The Slack bot runs in-process with the daemon (Socket Mode) when both `botToken` and `appToken` are configured. Look for `[Slack] Socket connected` in `.claude/claudeclaw/logs/daemon.log` to confirm the websocket is live.

Format the output clearly for the user.
