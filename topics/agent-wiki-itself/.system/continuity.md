# Continuity — agent-wiki-itself

Raw understanding, evidence-linked. Human corrections to this file are ground truth.

## What the human wants (from 2026-07-11 session)

- Jordan received this repo from its developer to evaluate; the developer highlighted
  the GOOP docs. Jordan is not deeply familiar with Hermes or GOOP.
- Jordan has one-day access to Fable 5 and wants maximum durable value extracted
  from that access, applied to this project — "insanely awesome," but with
  weaknesses minimized and dangerous surfaces patched.
- Standing goal inferred: turn Agent Wiki from an unrun spec into something Jordan
  can actually operate — on Claude Code, since that is the harness in hand.

## Key facts established

- Repo was a spec + empty scaffold; never executed (log.md had one init entry).
- The v1 spec lacked: approval gates, budgets, kill switch, injection defense,
  claim atomicity, garbage collection, and any auditor. → KERNEL.md now defines
  all of these; red-team.md records the findings for the original developer.
- The port keeps GOOP's four primitives and adds a fifth role (Fleet Warden).

## Open questions for the human

- Should this vault live in git? (Warden's protected-path audit is far stronger
  with git history. Recommended: `git init` + first commit of today's seed.)
- Which loops, if any, does Jordan actually want scheduled — vs. running factory
  ticks manually for the first week?

<!-- goop: fable-5-seed @ 2026-07-11 -->
