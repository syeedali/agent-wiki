# Fleet Dashboard

> One page, whole fleet. Updated by the Context factory; correct anything by hand.

## Topics

| Topic | Status | Agents | Loops |
|---|---|---|---|
| [[../agent-wiki-itself/README|agent-wiki-itself]] | 🟢 seeded | 5 (seed roles) | 2 proposed, 0 active |
| [[../ai-research-digest/README|ai-research-digest]] | 🌱 new | 0 (1 proposed) | 0 (1 proposed) |

## Fleet roster (all machine actors)

| Actor | Tier | Kind | Status |
|---|---|---|---|
| context-factory | T3 seed | subagent | 🟢 |
| skills-factory | T3 seed | subagent | 🟢 |
| agents-factory | T3 seed | subagent | 🟢 |
| loops-factory | T3 seed | subagent | 🟢 |
| fleet-warden | T3 seed (read-only) | subagent | 🟢 |
| concierge | T2 (front door; writes limited to injections) | subagent | 🟢 |

Budgets (KERNEL §4): agents 6/12 fleet-wide (fleet-level roles) · loops 0 active.
Golden exemplars for all tick types: `topics/ai-research-digest/` (live flywheel
run, 2026-07-11) — see `.claude/skills/goop-local-models/SKILL.md`.

## Controls

- 🛑 **Kill switch:** create `KILL` at vault root
- 🔍 **Audit:** ask for a warden audit
- 📜 **History:** `log.md`

<!-- goop: fable-5-seed @ 2026-07-11 -->
<!-- goop: context-factory @ 2026-07-11 · added ai-research-digest row to Topics table -->
