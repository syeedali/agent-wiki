---
title: GOOP v2 — Agent Wiki on Claude Code
created: 2026-07-11
type: spec
tags: [goop, claude-code, port, runbook]
provenance: Seeded by Fable 5 on 2026-07-11 — the "genome day" build
---

# GOOP v2: Agent Wiki, Ported to Claude Code and Hardened

The original spec (`spec.md`) targeted Hermes and had never been run. This is a
**working port** to Claude Code — same four primitives, same flywheel, same
"markdown is the API" soul — plus the safety kernel the v1 spec lacked
(see `red-team.md` for why each patch exists).

## The mapping

| GOOP primitive | Hermes (v1 spec) | This vault (v2) |
|---|---|---|
| Skills | `SKILL.md` + `skill_manage` | per-topic `topics/<topic>/skills/`; `.claude/skills/` is kernel-only |
| Agents | profiles + SOUL.md | `.claude/agents/*.md` subagents; SOUL lives in the definition |
| Loops | `hermes cron create` | `/schedule` routines, `/loop`, or plain cron + `claude -p` |
| Context | wiki + ambient session DB | the vault + `notes/inbox.md` drop-file (explicit > ambient) |

What v2 adds over v1: **KERNEL.md** (constitution, highest authority), the
**Fleet Warden** (read-only auditor — fifth role), **approval gates** on agent/loop
creation, **budgets**, **kill switch**, **provenance signatures** making "never
overwrite user edits" mechanically checkable, and **atomic claims** on the queue.

One deliberate change of soul: v1's Context factory *surveilled* the chat DB every
30 minutes. v2's reads an explicit drop-file, `notes/inbox.md` — you push thoughts
to the system; it doesn't watch you.

## How to run it — day one (manual ticks, recommended)

Open Claude Code in this directory. The factories are real subagents; just ask:

```
"Drop a thought in notes/inbox.md, then: run a context tick"
"Run a skills tick"        ← builds anything queued under [SKILLS]
"Run an agents tick"       ← designs queued agents; builds only [approved] ones
"Run a loops tick"         ← same, for schedules
"Run a warden audit"       ← the immune system; read its report
```

A pleasant operating rhythm: drop notes into `notes/inbox.md` all day → one
context tick in the evening → skim the queues → mark `[approved]` on what you
want → run the other ticks → read `injections.md` over coffee.

## Going autonomous (only after a week of manual ticks feels boring)

Approve the two proposed loops in `topics/agent-wiki-itself/`, then schedule them
(`/schedule`, or cron with `claude -p`). Run ticks **serially, staggered** — never
two factories concurrently on the same vault (queue claims make collisions
detectable, not impossible). Keep the Warden's weekly audit; read its report.

**Emergency stop, always:** `touch KILL` at the vault root.

## Life after Fable (the model-tiering plan)

This vault was seeded on Fable-quality output where quality compounds — the
kernel, the factory prompts, the rubric. Day-to-day it does **not** need Fable:

- **Context tick, Warden audit** — Sonnet-class is fine; the definitions carry the intelligence.
- **Skills/Agents/Loops ticks** — Sonnet-class; the rubric forces 3 candidates +
  adversarial judging, which recovers most of the quality gap.
- **Save your strongest available model for:** approving/reviewing proposed agents,
  and any edit to KERNEL.md or the factory definitions (human-led, model-assisted).

The design intent: **intelligence lives in the artifacts, not the runtime.** That
is what "markdown is the API" buys you — the genome outlives the genie.

## Vault layout (v2)

```
agent-wiki-main/
├── KERNEL.md                ← constitution (highest authority, human-edit only)
├── GOOP2.md                 ← this file
├── red-team.md              ← findings vs the v1 spec (for the original dev)
├── log.md                   ← append-only audit trail
├── notes/inbox.md           ← your drop-file for the context factory
├── .claude/
│   ├── agents/              ← 4 factories + fleet-warden (protected paths)
│   └── skills/goop-*        ← protocol + rubric (protected paths)
├── topics/
│   ├── _global/             ← fleet dashboard + warden reports
│   └── agent-wiki-itself/   ← bootstrap topic: the system maintains the system
└── spec.md, goop-*.md       ← original v1 docs, untouched
```

<!-- goop: fable-5-seed @ 2026-07-11 -->
