# Loop (proposed): context-daily

- **Agent:** context-factory · **Schedule:** daily, morning
- **Trigger prompt:** "Check for KILL/PAUSED; exit if present. Read notes/inbox.md at the vault root (the human's drop-file for thoughts). If unchanged since your last log entry, exit with no writes. Otherwise run a context tick: update topics, continuity, and queues per your definition."
- **Idle cost:** ≈ one file read (the drop-file check), then exit.
- **Stop condition:** KILL file, or human deletes this loop file.
- **Design note:** on Claude Code there is no ambient session DB to mine (unlike Hermes), so this loop reads an explicit drop-file — `notes/inbox.md` — which is *better*: the human chooses what the system comprehends. Push, not surveillance.
- **Activation (human runs it):** `/schedule` a daily routine with the trigger prompt, or cron `claude -p "<trigger prompt>" --agents context-factory`.

Status: AWAITING `[approved]` on the queue line.
<!-- goop: fable-5-seed @ 2026-07-11 -->
