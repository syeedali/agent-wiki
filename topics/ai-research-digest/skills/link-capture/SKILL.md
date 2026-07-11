---
name: link-capture
description: The capture convention for this topic — how a found AI/agent paper, repo, or post gets logged as one line in reading-log.md so it stops dying in an open tab.
topic: ai-research-digest
---

# link-capture

One link, one line, appended to `topics/ai-research-digest/reading-log.md`.
The point is zero friction: if capturing takes more than ten seconds, tabs win.

## Load this skill when

- You (human or agent) have a link to an AI/agent paper, repo, or post worth keeping.
- You are the digest-writer (or any reader of `reading-log.md`) and need the exact
  line grammar to parse entries.

## Do NOT load this skill when

- You want to summarize, rank, or write about the week's links — that is the
  `weekly-digest` skill. This skill never judges links, only records them.
- The link is not about AI/agents — this topic's log is not a general bookmark pile.
- You want to fetch or archive the page contents. Capture stores the pointer only.

## The entry format

One line, four parts, in this order:

```markdown
- YYYY-MM-DD [type] <url> — why it caught the eye
```

- **Date**: today, ISO. The digest uses it to window the week.
- **Type**: one of `paper` | `repo` | `post` | `other`. Infer from the URL when
  obvious (arxiv.org → paper, github.com → repo, blog/substack/x.com → post);
  otherwise `other`. Never stall on classification.
- **URL**: the bare link, no markdown wrapping needed.
- **Why**: one clause, the honest first impression ("claims 10x cheaper agent evals",
  "tiny repo that replaces LangChain for my use"). If no why was given, write
  `— (no note)` and move on — a bare captured link beats a perfect lost one.

Example:

```markdown
- 2026-07-11 [paper] https://arxiv.org/abs/2607.01234 — agent benchmarks that resist memorization; skeptical but the setup looks clean
```

## Procedure

1. If `topics/ai-research-digest/reading-log.md` does not exist, create it with the
   header block from this file's "First run" section below.
2. Grep the file for the URL. If present, do not re-add; if the new why-line adds
   something, append it to the existing entry's why after ` · `.
3. Append the entry line at the **bottom** of the file (append-only; entries are
   never reordered, edited, or deleted — KERNEL §7 applies to every unsigned line).
4. If you are a machine actor, follow your appended line(s) with a signature
   comment: `<!-- goop: <actor> @ <ISO date> -->`. Human entries need no signature.

## First run

Seed `reading-log.md` with:

```markdown
# Reading log — ai-research-digest

Append one line per link at the bottom (format: `- YYYY-MM-DD [type] <url> — why`).
The weekly-digest skill consumes everything below since the last digest.

---
```

## What this skill does not do

No fetching, no summarizing, no tagging taxonomy, no read/unread status tracking.
If those needs become real, they are new queue items — not silent scope growth here.

<!-- goop: skills-factory @ 2026-07-11 -->
<!-- goop rubric (skills-factory @ 2026-07-11) — winner: candidate A "structured one-liner".
     Scores /2: Evidence 2 (queue item link-capture + continuity "collected somewhere",
     notes/inbox.md 2026-07-11); Safety 2 (writes one file inside own topic, no new
     capability); Completeness 2 (dedup, missing-why, unknown-type, first-run all handled;
     non-goals stated); Clarity 2 (exact grammar + example, executable cold); Reusability 1
     (deliberately this-topic-shaped; type vocab open but path/format fixed); Fit 2
     (registry checked — only goop-* exist; emits the exact grammar weekly-digest consumes).
     Total 11/12 — PASS (bar ≥8, no zero on Safety/Evidence).
     Rejected: B "metadata cards" (friction kills the capture habit; evidence says one line),
     C "file-per-week" (contradicts queue item's singular reading-log.md; duplicates week
     logic owned by weekly-digest). -->
