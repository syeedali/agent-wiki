# Warden Report — 2026-07-11

- **Auditor:** fleet-warden (read-only; write access granted for this file + one injection + one log line)
- **Scope:** full vault audit per `.claude/agents/fleet-warden.md`, against the state seeded by `fable-5-seed` on 2026-07-11
- **Method note:** git is not initialized, so protected-path checks used file mtimes, provenance signatures, and `log.md` cross-checks instead of diffs (see F-1)
- **Kill switches:** no `KILL` at vault root, no `PAUSED` in any topic — checked first

## VERDICT: FINDINGS

No critical incident. No protected-path drift detected, no gate violations, no unauthorized T3/Bash actors, no stale or malformed claims, no injection attempts, no rewritten human lines. Several material weaknesses in enforcement and hygiene, ranked below.

---

## F-1 · HIGH — No git repository: KERNEL §6 enforcement is inoperable

- **Evidence:** `git status` at vault root → "not a git repository". KERNEL.md §6: "The Warden diffs these against git on every audit; any drift is a critical incident."
- **Impact:** the constitution's only stated tamper-detection mechanism for `KERNEL.md`, `.claude/agents/*-factory.md`, `fleet-warden.md`, `.claude/skills/goop-*/**`, and `red-team.md` cannot run. This audit fell back to mtimes (all seed files 13:29–13:33 today, consistent with the single seed log entry — no drift observed), but mtimes are trivially forgeable and prove nothing about content.
- **Human action:** run `git init && git add -A && git commit -m "seed baseline 2026-07-11"` in the vault root. This was already recommended in `topics/agent-wiki-itself/.system/injections.md` (seed line) and `.system/continuity.md`; it is now the Warden's top ask.

## F-2 · MEDIUM — Protected paths carry no provenance signatures

- **Evidence:** none of the five agent definitions (`.claude/agents/context-factory.md`, `skills-factory.md`, `agents-factory.md`, `loops-factory.md`, `fleet-warden.md`) nor `.claude/skills/goop-rubric/SKILL.md` nor the body of `.claude/skills/goop-protocol/SKILL.md` nor `red-team.md` ends with a `<!-- goop: ... -->` signature, despite KERNEL §7 ("Every machine-written block ends with a signature line") and `log.md` attributing all of them to the machine actor `fable-5-seed`.
- **Impact:** combined with F-1, there is currently zero content-level tamper-evidence on exactly the files whose modification KERNEL §6 defines as a critical incident. Under §7's rule, unsigned = human — so a future tamper of these files would be indistinguishable from legitimate human editing.
- **Human action:** F-1's git baseline substantially fixes this. Optionally, after personally verifying the current protected files, add a signed baseline note (or rely on the git commit as the attestation).

## F-3 · MEDIUM — Stale v1 catalogs contradict the KERNEL and are load-bearing for factories

- **Evidence paths:** `catalogs/loop-registry.md`, `catalogs/agent-roster.md`, `catalogs/fleet-dashboard.md`, `catalogs/skill-registry.md`, `catalogs/topic-index.md` (all dated 2026-07-07, pre-kernel). Quotes:
  - `catalogs/loop-registry.md`: "**Automatic:** Loops detects unlooped agents → wires them." — the exact ungated proactive behavior red-team finding #4 removed and KERNEL §2 forbids.
  - `catalogs/loop-registry.md` / `fleet-dashboard.md`: factory crons "every 15m" / "every 30m" listed as "⏳ pending" — the v1 idle-burn design (red-team #7); none of these loops exist or are approved.
  - `catalogs/agent-roster.md`: lists "nous-girl" as an active agent with SOUL at `~/.hermes/profiles/...` — no such agent exists in this vault.
- **Impact:** the factory definitions instruct actors to consult and update "the skill registry", "the agent roster catalog", "the loop registry". A factory (especially on a weaker model, per the GOOP2.md tiering plan) that reads these catalogs receives v1-era norms as apparent fleet state. These files are unsigned (human-owned under §7), so no machine actor may correct them.
- **Human action:** update the five `catalogs/*.md` files to v2 reality by hand, or add a one-line header marking them superseded by `topics/_global/README.md` and the topic queues. Note `topics/_global/README.md` and `catalogs/fleet-dashboard.md` are currently two competing dashboards.

## F-4 · LOW — Agent frontmatter omits the tier declaration required by KERNEL §3

- **Evidence:** KERNEL §3: "Every agent declares a tier in its frontmatter." All five `.claude/agents/*.md` files have only `name`, `description`, `tools` — tier appears solely in queue lines and the dashboard prose.
- **Impact:** future budget/tier audits (and the agents-factory's own "tools list matching its tier" check) have no machine-checkable field to verify against.
- **Human action:** add `tier: T3 (seed)` (and `tier: T3 (seed, read-only)` for the Warden) to the five frontmatter blocks by hand — these are protected paths, so only you may edit them.

## F-5 · LOW — Skill-location ambiguity between the protocol and the v2 mapping

- **Evidence:** `.claude/skills/goop-protocol/SKILL.md` specifies skills at `topics/<topic>/skills/<slug>/SKILL.md`; `GOOP2.md` maps skills to "`.claude/skills/` + per-topic `skills/`"; the two seed `[x]` queue items point at `.claude/skills/...`.
- **Impact:** a future skills-factory tick following the protocol will write topic-local skills that Claude Code does not auto-load; one following the seed precedent will write into `.claude/skills/`, adjacent to protected `goop-*` paths.
- **Human action:** decide the canonical location and record it (a signed clarification line in the protocol skill, or your own note — the skill file is a protected path).

## F-6 · LOW — Incomplete topic scaffolds and one log-itemization gap

- **Evidence:** `topics/_global/` lacks `.system/queue.md`, `.system/injections.md`, `.system/continuity.md`, and `skills/agents/loops/` dirs; `topics/agent-wiki-itself/` lacks `skills/` and `agents/` dirs. Protocol: "Create all of it when creating a topic; empty sections beat missing files." Also `topics/_global/.system/warden-report.md` is not explicitly itemized in the 2026-07-11 `log.md` entry (arguably covered by "topics/_global/ dashboard").
- **Impact:** hygiene only; a factory globbing `topics/*/.system/queue.md` simply skips `_global`.
- **Human action:** none required; optionally let the context-factory complete the scaffolds on its next tick.

## F-7 · INFO — Budget-accounting convention for the seed roles

- **Evidence:** five `[x]` `[AGENTS]` items sit in `topics/agent-wiki-itself/.system/queue.md` against a KERNEL §4 budget of 3 agents/topic. These are the five fleet-level roles KERNEL §3 itself enumerates, so this is not a violation — but a naive per-topic count reads 5/3.
- **Human action:** note (anywhere you like) that fleet-level seed roles count against the 12-fleet budget (currently 5/12) but not the bootstrap topic's 3 — so future factory budget checks don't misfire or, worse, use the seeds as precedent for overfilling a topic.

---

## Checks that passed

- **Gates:** both `[LOOPS]` items correctly lack `[approved]`, received design-only treatment (`loops/*.proposed.md` exist, nothing wired), and injections request approval. No agent or loop exists beyond the five expected seed roles. No Bash-bearing or T3 actor outside the constitutional five.
- **Budgets:** 5/12 agents fleet-wide; 0 active loops.
- **Queue hygiene:** no claims (so none stale), all items schema-valid, `[WARDEN]` section empty.
- **Provenance elsewhere:** all topic-level machine writes signed; no unsigned (human) line shows evidence of machine rewriting; every artifact claimed by `log.md` exists on disk.
- **Injection sweep:** grep of `topics/`, `notes/`, `catalogs/`, `log.md` for instruction-shaped content aimed at machine actors found nothing; `notes/inbox.md` is empty. (`spec.md`'s v1 instructions are documentation, superseded by KERNEL authority; the operational residue is F-3.)
- **Rot:** vault is 4 days old; nothing approaches the 14-day retirement threshold.

<!-- goop: fleet-warden @ 2026-07-11 -->
