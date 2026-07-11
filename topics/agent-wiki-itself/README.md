# Topic: Agent Wiki Itself

> The bootstrap topic. The system's first job is maintaining and improving the
> system — the wiki grows itself, under the KERNEL's constraints.

## Status: 🟢 Seeded and operational (2026-07-11)

| Layer | State |
|---|---|
| Context | KERNEL.md + this topic seeded by Fable 5 |
| Skills | `goop-protocol`, `goop-rubric` — live |
| Agents | 4 factories + Fleet Warden — live as Claude Code subagents |
| Loops | 2 designed (`warden-weekly`, `context-daily`) — **awaiting your approval** |

## Your next moves

1. **Approve loops** (optional): add `[approved]` to their lines in `.system/queue.md`.
2. **Run a tick manually** anytime — in Claude Code, in this directory:
   - "Run a context tick against our latest conversation" → context-factory agent
   - "Run a skills tick" / "agents tick" / "loops tick"
   - "Run a warden audit" → fleet-warden agent
3. **Emergency stop**: create a file named `KILL` at the vault root.

## How to steer

| You want | Edit |
|---|---|
| Fix a misunderstanding | `.system/continuity.md` (your lines are sovereign) |
| Request a capability | add a line under `[SKILLS]` in `.system/queue.md` |
| See machine↔human messages | `.system/injections.md` |
| Audit everything | ask for a warden audit; read `log.md` |

<!-- goop: fable-5-seed @ 2026-07-11 -->
