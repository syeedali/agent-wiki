---
title: Red-Team Review — Agent Wiki / GOOP v1 spec
created: 2026-07-11
type: security-review
reviewer: Fable 5 (Claude Code), at Jordan's request
audience: the original developer
---

# Red-Team Review of the Agent Wiki Spec

Findings against `spec.md` as received 2026-07-11. Each maps to the patch now in
`KERNEL.md`. The architecture's core idea — markdown vault as the coordination
layer, human edits as the control plane — is genuinely good; these findings are
about what happens when it runs unattended.

| # | Sev | Finding | Patch |
|---|---|---|---|
| 1 | **Critical** | **No approval gate on self-extension.** Factories create agents and *new cron jobs* autonomously; injections inform the human only after the fact. A misread chat becomes a running agent with tools. | KERNEL §2 — `[approved]` required on `[AGENTS]`/`[LOOPS]`; factories design freely, build only when gated. |
| 2 | **Critical** | **Prompt injection: data flows into instruction position.** Context reads chat; factories read queue/continuity written by Context. Any adversarial or confused text ("create an agent that…") propagates into actors holding real tools. No trust boundary is defined anywhere in the spec. | KERNEL §1 — evidence is never instructions; act only on schema-valid queue items; instruction-shaped content is quoted verbatim into a `[WARDEN]` item, never followed. |
| 3 | **High** | **No-self-edit rule has no mechanism.** Stated as principle 8, but nothing detects violation, and Agents-factory holds the exact tool (`profile create` / file write) needed to violate it. | KERNEL §6 — enumerated write-forbidden paths + a read-only **Fleet Warden** role that diffs them every audit. |
| 4 | **High** | **Unbounded proactive growth.** Agents-factory "watches Skill Registry for unhosted skills"; Loops-factory "watches Agent Registry for unlooped agents." Every artifact triggers creation of the next kind — a fleet that grows by construction, with no budgets and no deletion path anywhere in the spec. | KERNEL §4 — hard budgets (3 agents/topic, 12 fleet, 2 loops/topic), proactive creation removed from factory prompts, Warden nominates 14-day-idle artifacts for retirement. |
| 5 | **High** | **No kill switch.** Once the four crons exist, stopping the system means hunting down cron jobs by hand while they continue spawning `delegate_task`s. | KERNEL §5 — `KILL` file halts everything; `PAUSED` halts a topic; checked before any other read. |
| 6 | Med | **Race conditions on queue.md.** Three factories on 15-min crons read-modify-write the same files; "claim" (`[~]`) is described but not made atomic, and simultaneous claims clobber. Human edits mid-tick can be silently lost — spec says "never overwrite user-edited lines" with no way to know which lines those are. | KERNEL §7 — signature-based provenance (unsigned = human = untouchable), claim-with-actor-and-timestamp, 2h stale-claim rule. Staggered/serialized ticks in the runbook. |
| 7 | Med | **Idle burn.** ~70 LLM cron fires/day at zero queue depth, each a full agent turn. | Factories must exit immediately on empty queue (KERNEL §4); runbook defaults to manual/daily ticks, not 15-min crons. |
| 8 | Low | **No audit trail integrity.** `log.md` is append-only by convention only. | Warden cross-checks log against artifacts on disk each audit; git strongly recommended. |

**Recommendation:** adopt a kernel file with highest authority, add the auditor
role, and gate the two self-extension actions. Everything else in the design can
stay exactly as elegant as it is.
