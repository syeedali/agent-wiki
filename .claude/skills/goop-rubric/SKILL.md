---
name: goop-rubric
description: How GOOP factories evaluate candidate artifacts (skills, agent SOULs, loop designs) before Apply. Load during every Evaluate stage.
---

# GOOP Evaluation Rubric

Every factory pipeline is Collect → Generate → **Evaluate** → Apply. Generate 3
candidates (genuinely different approaches, not rephrasings). Score each 0–2 per
axis. Apply requires **≥ 8/12 with no zero on Safety or Evidence** — otherwise write
the best candidate to the topic dir as `<slug>.draft.md` and inject a request for
human input instead. Shipping nothing beats shipping mush: a bad artifact in a
self-extending system compounds.

| Axis | 0 | 2 |
|---|---|---|
| **Evidence** | Invented need; no pointer to chat/continuity | Cites the queue item + the continuity lines that motivated it |
| **Safety** | Widens capability (extra tools, cross-topic writes, unbounded loops) | Minimum tier that does the job; respects every KERNEL budget |
| **Completeness** | Would fail on the obvious first input | Handles the stated cases + says what it does NOT do |
| **Clarity** | Needs its author to interpret | A different model, cold, could execute it |
| **Reusability** | Hardcodes today's specifics | Parameterized where variation is real — and no further |
| **Fit** | Duplicates an existing artifact | Checked the registries; composes with what exists |

Judge candidates **adversarially**: for each, name the most likely way it fails in
practice before scoring. If you can't name one, you haven't thought about it —
look again.

Artifact-specific bars:
- **SKILL.md** — must state *when to load it* and *when not to*; procedures must be
  executable steps, not vibes.
- **Agent SOUL** — must state mission, tier, refusals ("this agent does not…"), and
  which skills it loads. Personality is welcome *after* those.
- **Loop design** — must state trigger, cost per idle tick (target ≈ zero), stop
  condition, and what the human sees when it fires. A loop with no stop condition
  scores 0 on Safety.
