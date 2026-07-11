# Loop (proposed): warden-weekly

- **Agent:** fleet-warden · **Schedule:** weekly (Mondays)
- **Trigger prompt:** "Check for KILL at vault root; if present, log and exit. Otherwise run a full warden audit per your definition and write the report."
- **Idle cost:** none — the audit IS the work; if the fleet is unchanged since last report, the report is one line: CLEAN, no changes.
- **Stop condition:** KILL file, or human deletes this loop file.
- **Human sees:** `topics/_global/.system/warden-report.md` + one injection line.
- **Activation (human runs it):** in Claude Code: `/schedule` a weekly routine with the trigger prompt above, working dir = this vault. Or cron: `claude -p "<trigger prompt>" --agents fleet-warden`.

Status: AWAITING `[approved]` on the queue line.
<!-- goop: fable-5-seed @ 2026-07-11 -->
