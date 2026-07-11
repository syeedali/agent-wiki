# Loop (proposed): digest-sunday

- **Agent:** digest-writer (T2) — **itself only PROPOSED**, see Dependency below
- **Trigger + schedule:** calendar, weekly — Sundays 06:30 *local time* (cron `30 6 * * 0`),
  so the digest is on disk before "Sunday morning with coffee" (notes/inbox.md 2026-07-11).
  Slowest schedule that meets the need: the need is weekly, the loop fires weekly, once.
- **Exact prompt the loop fires:**

  > First: if a file named `KILL` exists at the vault root, or `PAUSED` exists in
  > `topics/ai-research-digest/`, append one line "digest-sunday: halted by kill switch"
  > to `log.md` and exit. Second: if `topics/ai-research-digest/agents/digest-writer.md`
  > does not exist (a `.proposed.md` file does not count), append one line
  > "digest-sunday: digest-writer not built yet — skipped" to `log.md` and exit.
  > Otherwise: you are digest-writer; load your SOUL at
  > `topics/ai-research-digest/agents/digest-writer.md` and run exactly one digest cycle
  > per the `weekly-digest` skill — write `topics/ai-research-digest/digests/<today
  > YYYY-MM-DD>.md`, sign it, append one `log.md` line, and add one `[UNREAD]` injection
  > saying the digest is ready (or that the week was quiet). Do nothing else.

- **Idle-tick cost ≈ 0:** halted / paused / agent-not-built fires are three file-existence
  checks, one log line, exit. A quiet-week fire is one read of a small log window plus the
  two-line quiet-week digest the skill prescribes — by contract that is a success, not
  waste. There is no polling between Sundays; the loop simply does not exist Mon–Sat.
- **Stop conditions:** `KILL` at vault root (instant no-op); `PAUSED` in this topic; the
  human deletes `loops/digest-sunday.md` or removes the scheduled routine. Structurally
  self-limiting besides: at most one fire per week, at most one artifact per fire, and a
  skipped Sunday loses nothing because `weekly-digest` windows by last digest, not by
  calendar week.
- **Human sees:** one new file `topics/ai-research-digest/digests/YYYY-MM-DD.md` each
  Sunday, one `[UNREAD]` injection announcing it, one `log.md` line. Nothing else changes.
- **Activation (only after `[approved]`, and only by the human):** in Claude Code,
  `/schedule` a weekly routine (Sun 06:30, working dir = this vault) with the trigger
  prompt above; or cron: `30 6 * * 0 claude -p "<trigger prompt above>"`. The
  loops-factory writes the config; it never schedules system jobs itself.

## Dependency — read before approving

`digest-writer` is not built: only `agents/digest-writer.proposed.md` exists, awaiting
`[approved]` on its own queue line (KERNEL §2). **Approve the agent first, then this
loop.** If this loop is approved and wired anyway, the not-built guard in the prompt
makes every fire a one-line no-op until the agent exists — safe, but useless.

Status: AWAITING `[approved]` on the queue line. Nothing is scheduled.

<!-- goop: loops-factory @ 2026-07-11 -->
<!-- goop rubric (loops-factory @ 2026-07-11) — winner: candidate A "single Sunday-dawn
     cron". Scores /2: Evidence 2 (queue item digest-sunday + continuity verbatims
     "Sunday morning with coffee" and the weekly-cadence inference; 06:30 derives from
     "ready with coffee"); Safety 2 (slowest schedule that meets the need, one fire/week,
     three explicit stop conditions incl. KERNEL kill switches, drives one T2 agent with
     a not-built guard, would be loop 1/2 in topic and 1st active fleet-wide per
     _global registry); Completeness 2 (handles halted, paused, agent-unbuilt, quiet
     week, skipped Sunday; states what it does NOT do — no mid-week fires, no polling);
     Clarity 2 (exact cron line + verbatim prompt; a cold scheduler could wire it);
     Reusability 1 (time and topic parameterize; deliberately Sunday-digest-shaped, no
     further); Fit 2 (checked loop registry — 0 active loops, no overlap with the two
     agent-wiki-itself proposals; composes with digest-writer's SOUL and weekly-digest's
     windowing instead of duplicating either). Total 11/12 — PASS (bar ≥8, no zero on
     Safety/Evidence). Adversarial read on A: likeliest failure is the human approving
     the loop before the agent, yielding silent Sundays — countered by the not-built
     guard logging a visible skip line and by the Dependency section ordering the
     approvals; second failure is UTC-vs-local drift delivering an afternoon digest —
     countered by stating local time in the activation instructions.
     Rejected: B "Saturday-night draft + Sunday-morning deliver" (6/12 — Safety 1: twice
     the fires the need requires; Evidence 1: no one asked for a Saturday draft; Fit 1:
     the draft/verify split duplicates the windowing the skill already owns). C "daily
     log-watcher that digests on Sundays" (6/12 — Safety 1: six no-op polls per week is
     busy polling the design taste forbids, even at low unit cost; Evidence 1: the daily
     cadence is invented; Fit 1: re-implements calendar logic a weekly trigger gets for
     free). -->
