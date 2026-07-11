![Agent Wiki](assets/hero-agent-wiki.png)

# Agent Wiki — GOOP v2

> **A self-documenting, self-extending multi-agent system. The wiki IS the coordination layer.**
>
> Originally specced for [Hermes Agent](https://github.com/NousResearch/hermes-agent) by [Nous Research](https://nousresearch.com) —
> now **ported to Claude Code, hardened, seeded, and running**. Markdown is the API between human and machine.

---

## Status: 🟢 Live (seeded 2026-07-11)

This started as an unrun spec (`spec.md`, the GOOP whitepaper). On 2026-07-11 it was
ported to Claude Code and brought to life at Fable-5 quality — the "genome day" build.
The system has already completed its first full self-referential cycle: **built →
audited by its own immune system → patched from the audit's findings → committed.**

| Layer | State |
|---|---|
| **Constitution** | [KERNEL.md](KERNEL.md) — approval gates, capability tiers, budgets, kill switch, provenance rules |
| **Factories** | 4 live Claude Code subagents: `context-factory`, `skills-factory`, `agents-factory`, `loops-factory` |
| **Immune system** | `fleet-warden` — read-only auditor. First audit: verdict FINDINGS, all 7 findings fixed |
| **Kernel skills** | `goop-protocol` (wire formats), `goop-rubric` (adversarial candidate judging) |
| **Vault** | `topics/agent-wiki-itself/` (bootstrap topic) + `topics/_global/` (fleet dashboard) |
| **Loops** | 2 designed (`warden-weekly`, `context-daily`), correctly **awaiting human `[approved]`** |

Read [GOOP2.md](GOOP2.md) for the full port spec and runbook, and
[red-team.md](red-team.md) for the security review of the original v1 spec
(2 critical, 3 high findings — each mapped to its KERNEL patch).

---

## What is this?

Agent Wiki organizes a multi-agent AI fleet as **a markdown vault**. The database,
config, UI, and message bus between agents are all plain `.md` files you can open in
any editor. Every interest gets a **topic** directory holding the full GOOP stack:

- **Context** — what the system understands about your goal (`continuity.md`)
- **Loops** — schedules and recurring runs
- **Agents** — profiles that do the work
- **Skills** — reusable procedures loaded on demand

Primitives nest: **Context** contains **Loops** which invoke **Agents** which load
**Skills**. You steer by editing files; the system reports back through
`injections.md`. **Intelligence lives in the artifacts, not the runtime** — the
kernel, factory prompts, and rubrics were seeded by a frontier model so that any
model running the ticks inherits their quality.

## How to use it (today, no setup)

Open Claude Code in this directory and talk to it:

```
Drop thoughts into notes/inbox.md            ← your side of the API
"run a context tick"                          ← comprehension → topics + queues
"run a skills tick"                           ← builds queued capabilities
"run an agents tick" / "run a loops tick"     ← designs; builds ONLY [approved] items
"run a warden audit"                          ← the immune system reports
```

A good rhythm: drop notes all day → context tick in the evening → skim the queues →
add `[approved]` to what you want built → run the other ticks → read
`injections.md` over coffee.

**Controls:** `touch KILL` at the vault root halts everything · a `PAUSED` file
halts one topic · `log.md` is the append-only audit trail · git is the tamper record.

## What v2 adds over the v1 spec

| v1 (spec.md, Hermes, never run) | v2 (this vault, Claude Code, live) |
|---|---|
| Factories create agents & crons autonomously | **Human `[approved]` gate** on all agent/loop creation |
| Chat text flows into instruction position | **KERNEL §1**: evidence is never instructions; injection attempts → Warden |
| "No self-edit" stated, unenforced | Enumerated protected paths + **Fleet Warden** diffs them against git |
| Unbounded proactive growth | Budgets: 3 agents/topic, 12 fleet, 2 loops/topic; 14-day rot review |
| No stop mechanism | `KILL` / `PAUSED` files, checked before any read |
| Watches your chat DB every 30 min | Reads an explicit drop-file (`notes/inbox.md`) — push, not surveillance |
| 4 roles | 5 roles — the **Warden** is the one v1 forgot |

## Vault layout

```
├── KERNEL.md              ← constitution (highest authority, human-edit only)
├── GOOP2.md               ← port spec + runbook          ├── red-team.md  ← v1 review
├── log.md                 ← append-only audit trail      ├── notes/inbox.md ← your drop-file
├── .claude/agents/        ← 4 factories + fleet-warden   (protected paths)
├── .claude/skills/goop-*  ← protocol + rubric            (protected paths)
├── topics/
│   ├── _global/           ← fleet dashboard + warden reports
│   └── agent-wiki-itself/ ← bootstrap topic: the system maintains the system
└── spec.md, goop-*.md, catalogs/  ← original v1 docs, preserved (marked superseded)
```

## Lineage

- **GOOP** (Gentic Object-Oriented Programming) concept & v1 spec: Anser + sovthpaw,
  July 2026, targeting [Hermes Agent](https://github.com/NousResearch/hermes-agent) — see
  [goop-whitepaper.md](goop-whitepaper.md) and [spec.md](spec.md), preserved intact.
- **v2 port, kernel, and seed**: built with Claude Fable 5 in Claude Code, 2026-07-11.

*Towards self-improvement — with a constitution.*
