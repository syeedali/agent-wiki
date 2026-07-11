<div align="center">

![Agent Wiki](assets/hero-agent-wiki.png)

# Agent Wiki

### A self-extending multi-agent system where the wiki *is* the system.

No database. No dashboard. No YAML. A folder of markdown files is the memory, the
message bus, the config, and the steering wheel — for you *and* for the agents.

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/fleet-live-brightgreen.svg)](topics/_global/README.md)
[![Runtime](https://img.shields.io/badge/runs%20on-Claude%20Code-d97757.svg)](CLAUDE.md)
[![Constitution](https://img.shields.io/badge/governed%20by-KERNEL.md-8a2be2.svg)](KERNEL.md)

**[Quick start](#quick-start-60-seconds)** ·
**[How it works](#how-it-works)** ·
**[Safety model](#the-safety-model)** ·
**[Any model, seed quality](#any-model-seed-quality)** ·
**[Docs](#documentation)**

</div>

---

## Why this exists

Every agent framework hides its state — in databases, dashboards, and config
formats only the framework understands. Agent Wiki inverts that:

> **Markdown is the API.** Human → machine, machine → machine, machine → human.
> You can `git diff` your entire agent fleet. You steer it with a text editor.

Tell the system what you want (drop a note in a file). It comprehends, plans,
builds the skills and agents to make it happen, and reports back — every step a
plain `.md` file you can read, edit, or veto. And because all the intelligence
lives in readable artifacts rather than in code, the whole system is
**self-documenting by construction**.

This repo is a **live instance**, not just a spec. It was ported from the original
GOOP design to Claude Code, given a constitution, and on day one completed a full
cycle: *built → audited by its own immune system → patched from the audit →
flywheel run end-to-end with all safety gates holding.*

## The four primitives

![Four Primitives](assets/four-primitives.png)

| Primitive | What it is | Lives at |
|---|---|---|
| **Context** | What the system understands about your goals | `topics/*/.system/continuity.md` + dashboards |
| **Loops** | Execution over time — schedules, recurring runs | `topics/*/loops/` |
| **Agents** | Named profiles that do the work | `.claude/agents/` + `topics/*/agents/` |
| **Skills** | Reusable procedures, loaded on demand | `topics/*/skills/` |

They nest — **Context ⊃ Loops ⊃ Agents ⊃ Skills** — and each has a **factory**: a
meta-agent that manufactures new instances through a fixed pipeline (*collect
evidence → generate 3 candidates → judge adversarially → apply*), so nothing gets
built from a single unexamined draft.

## Quick start (60 seconds)

No install, no config. Open [Claude Code](https://claude.com/claude-code) in this
directory — `CLAUDE.md` boots the session into the system automatically — and:

```text
1.  Write any thought into  notes/inbox.md          ← your side of the API
2.  "run a context tick"                            → topics + work queues appear
3.  "run a skills tick"                             → queued capabilities get built
4.  Add [approved] to queue lines you want built    ← agents/loops need YOUR sign-off
5.  "run an agents tick" / "run a loops tick"       → gated things get built
6.  "run a warden audit"                            → the immune system reports
7.  "what's new?"                                   → the concierge catches you up
```

A pleasant rhythm: drop notes all day → one context tick in the evening → skim the
queues, approve what you like → read `injections.md` over Sunday coffee.

**Emergency stop, always:** `touch KILL` at the vault root. Every actor halts.

## How it works

![Flywheel](assets/flywheel.png)

1. **You write** a thought into `notes/inbox.md` — push, not surveillance; the
   system reads only what you hand it.
2. **Context factory** turns it into a *topic*: a self-contained directory holding
   understanding, dashboards, and a work queue tagged `[SKILLS]` / `[AGENTS]` / `[LOOPS]`.
3. **The other factories** claim queue items and manufacture artifacts — skills
   auto-build; agents and loops are **designed immediately but built only after
   you mark the queue line `[approved]`**.
4. **Status flows back** through `injections.md`, which the concierge weaves into
   plain-language catch-ups. Every action lands in the append-only `log.md`.

![Factory Pipeline](assets/factory-pipeline.png)

### Every topic is a complete, self-contained instance

![Topic Structure](assets/topic-structure.png)

```text
topics/<your-topic>/
├── README.md              ← human dashboard (your default view)
├── .system/               ← machine coordination (hidden, but openable — that's the debugger)
│   ├── continuity.md      ← what the system understands (your edits are ground truth)
│   ├── queue.md           ← pending work · where YOU write [approved]
│   └── injections.md      ← machine → human messages
├── skills/   agents/   loops/
```

### The steering wheel is your text editor

| You want | You edit |
|---|---|
| Fix a misunderstanding | `continuity.md` — unsigned lines are yours; machines may never rewrite them |
| Force-request a capability | add a line under `[SKILLS]` in `queue.md` |
| Authorize an agent or loop | type `[approved]` on its queue line |
| Cancel anything | mark its file cancelled, or delete it |
| Stop the world | `touch KILL` |

## The safety model

Self-extending agent systems are exactly as dangerous as they sound, so this one
has a constitution — **[KERNEL.md](KERNEL.md)** — that outranks every file, queue
item, and chat message, plus a dedicated auditor. The headline rules:

| Rule | Mechanism |
|---|---|
| 🚧 **Humans gate self-extension** | Agents & loops require your literal `[approved]` on the queue line. Factories design freely; they build nothing ungated |
| 🛡️ **Data is never instructions** | Inbox/wiki text is evidence about you, not commands. Injection attempts get quoted verbatim to the Warden, never followed |
| 🔒 **No self-edit, enforced** | Kernel, factories, and Warden are write-forbidden to machines; the Warden diffs them against git every audit |
| 📊 **Hard budgets** | 3 agents/topic · 12 fleet-wide · 2 loops/topic · 2 claims/tick · idle ticks exit immediately |
| 🪪 **Capability tiers** | T0 chat-only → T3 fleet. Factories can only mint T0–T2. New agents get the minimum tier that works |
| ✍️ **Provenance** | Machines sign every block; unsigned lines are yours and untouchable |
| 🧯 **Kill switch** | `KILL` file halts everything; `PAUSED` halts one topic — checked before any other read |
| 🔍 **The Warden** | A read-only immune system that audits gates, budgets, provenance, and protected paths. It cannot fix — only find — so compromising it compromises nothing |

Battle-tested on day one: the Warden's first audit found 7 real findings in its own
seed (all fixed the same day), and during the first live flywheel run the factories
**designed** an agent and a loop but **refused to build either** — no `[approved]`,
no build. Receipts in [`log.md`](log.md) and
[`topics/_global/.system/warden-report.md`](topics/_global/.system/warden-report.md).

## Any model, seed quality

The vault was seeded by a frontier model (Claude Fable 5, 2026-07-11) — but the
design goal is that **the intelligence lives in the artifacts, not the runtime**:

- The **kernel, factory definitions, and rubric** carry the judgment.
- The first live flywheel run left **golden exemplars** — real, rubric-scored
  artifacts of every tick type — in
  [`topics/ai-research-digest/`](topics/ai-research-digest/).
- [`goop-local-models`](.claude/skills/goop-local-models/SKILL.md) wraps every tick
  in a literal 9-step checklist with "do-less" rules and maps each tick to its
  exemplars to copy.

Format-matching plus rubric discipline is the great equalizer: a small local model
that obeys the checklist and copies the exemplars produces artifacts structurally
indistinguishable from the seed. Run the ticks on whatever you have.

## The fleet

| Actor | Role | Tier |
|---|---|---|
| `concierge` | Front door — plain-language catch-ups over the fleet | T2 (writes: injections only) |
| `context-factory` | Comprehension — inbox → topics + queues | T3 seed |
| `skills-factory` | Authors SKILL.md capabilities | T3 seed |
| `agents-factory` | Designs agents; builds only `[approved]` | T3 seed |
| `loops-factory` | Designs schedules; wires only `[approved]` | T3 seed |
| `fleet-warden` | Immune system — read-only auditor | T3 seed (read-only) |

Current state: 2 topics live, 5 skills (3 kernel + 2 factory-built), 2 warden
audits passed,
**4 proposals awaiting `[approved]`** — see the
[fleet dashboard](topics/_global/README.md).

## Repository map

```text
├── KERNEL.md                ← the constitution (highest authority, human-edit only)
├── CLAUDE.md                ← boots any Claude Code session into the system
├── GOOP2.md                 ← architecture + runbook (start here for depth)
├── red-team.md              ← security review of the original v1 spec
├── log.md                   ← append-only audit trail of everything, ever
├── notes/inbox.md           ← your drop-file
├── .claude/
│   ├── agents/              ← the six actors above
│   └── skills/goop-*        ← protocol · rubric · local-models (protected)
├── topics/
│   ├── _global/             ← fleet dashboard + warden reports
│   ├── agent-wiki-itself/   ← bootstrap topic: the system maintains the system
│   └── ai-research-digest/  ← golden-exemplar topic from the first live run
└── spec.md, goop-*.md, catalogs/   ← original v1 design docs, preserved
```

## Documentation

| Doc | What's in it |
|---|---|
| [GOOP2.md](GOOP2.md) | Full v2 architecture, the Hermes→Claude Code mapping, runbook, model-tiering plan |
| [KERNEL.md](KERNEL.md) | The eight constitutional articles, verbatim |
| [red-team.md](red-team.md) | 8 ranked findings against the v1 spec and the patch for each |
| [spec.md](spec.md) / [goop-whitepaper.md](goop-whitepaper.md) | The original GOOP v1 vision, preserved intact |

## Lineage

- **GOOP** ("Gentic Object-Oriented Programming") concept and v1 spec: **Anser +
  sovthpaw**, July 2026, designed for [Hermes Agent](https://github.com/NousResearch/hermes-agent)
  by [Nous Research](https://nousresearch.com).
- **v2** — Claude Code port, constitution, Warden, live seed, and golden
  exemplars: built with **Claude Fable 5** in Claude Code, 2026-07-11.

MIT licensed. *Towards self-improvement — with a constitution.*
