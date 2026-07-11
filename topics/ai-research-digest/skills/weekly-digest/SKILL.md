---
name: weekly-digest
description: How to turn the reading-log entries since the last digest into the short Sunday-morning digest — what mattered, why, what's worth a deeper look. A boring week is reported as boring, never padded.
topic: ai-research-digest
---

# weekly-digest

Turn raw captures into one page the human actually reads with coffee. The contract,
in the human's own words: "what mattered, why, and what's worth a deeper look" and
"if a week was boring, say so" (notes/inbox.md 2026-07-11).

## Load this skill when

- It is digest time (the Sunday run, or the human asks for the digest early) and you
  are producing `topics/ai-research-digest/digests/YYYY-MM-DD.md`.

## Do NOT load this skill when

- You are capturing a link — that is `link-capture`.
- You are asked for a mid-week status, a monthly retrospective, or a summary of a
  single paper's contents. This skill produces exactly one artifact: the digest.

## Procedure

1. **Window.** Read `topics/ai-research-digest/digests/` and find the newest digest's
   "covers entries through" date. The window is every `reading-log.md` entry dated
   after it. No previous digest → the window is the whole log. (Windowing by
   last-digest, not by calendar week, means a skipped Sunday loses nothing.)
2. **Empty window → quiet-week digest.** Write the file using the quiet-week
   template below and stop. Do not skip silently; the absence of a digest should
   only ever mean the loop didn't run.
3. **Triage** each entry with one test: *would the human reopen this tab a second
   time?* Judge only from the entry's own type/URL/why-line — you have not read the
   linked content, and you must not pretend you have.
   - Yes, and it changes what the human should do or believe → **mattered** (cap: 5).
   - Yes, but it needs real reading time to pay off → **deeper look** (cap: 3).
   - Everything else → **also seen** (one line each) or silently dropped.
4. **Write** `digests/YYYY-MM-DD.md` (today's date):

   ```markdown
   # Digest — YYYY-MM-DD
   Covers reading-log entries through YYYY-MM-DD. N captured, M mattered.

   ## What mattered
   - [type] <url> — 1–2 sentences: what it is and why it matters *to this human's
     AI/agent work*. Your reasoning, not a restated why-line.

   ## Worth a deeper look
   - <url> — one line: the question a deeper read would answer.

   ## Also seen
   - <url> — one line. (Omit the section if empty.)
   ```

5. **Honesty gate** (hard rules, checked before saving):
   - Nothing cleared the bar → quiet-week template, even if the log was busy.
   - Never restate an entry's why-line as if it were analysis; add a reason or demote
     the entry.
   - Ceiling: one page (~350 words). Over it → cut "also seen" first, then mattered
     items, never the honesty.
6. **Sign** the file `<!-- goop: <actor> @ <ISO date> -->`. Never edit
   `reading-log.md` while digesting — it is append-only and its lines are the human's.

## Quiet-week template

```markdown
# Digest — YYYY-MM-DD
Quiet week: N links captured, none that need your Sunday. Skim "also seen" below
if curious; otherwise enjoy the coffee.
```

(With N=0: "Nothing captured this week. Nothing to report — that's the honest digest.")

## What this skill does not do

It does not fetch or read the linked pages, does not score or rank numerically
(pseudo-precision is padding in disguise), does not find "themes of the week", and
does not carry unread items forward as guilt — an entry not chosen is simply done.

<!-- goop: skills-factory @ 2026-07-11 -->
<!-- goop rubric (skills-factory @ 2026-07-11) — winner: candidate A "three-section
     template with honesty gate". Scores /2: Evidence 2 (queue item weekly-digest +
     continuity's verbatim "what mattered, why, deeper look" and "keep it honest" lines);
     Safety 2 (writes one file in own topic's digests/, no fetching, bounded output);
     Completeness 2 (empty week, skipped week, busy-but-boring week, over-length all
     handled; non-goals stated); Clarity 2 (numbered steps + both templates, executable
     cold — known subjective step, triage, is pinned to a single concrete test);
     Reusability 1 (window/day parameterize naturally; sections and voice are deliberately
     this-human-shaped); Fit 2 (consumes link-capture's exact grammar; no overlap with
     goop-* registry). Total 11/12 — PASS (bar ≥8, no zero on Safety/Evidence).
     Rejected: B "scored ranking pipeline" (1–5 significance scores are vibes dressed as
     data — invites the exact padding the human forbade), C "narrative theme essay"
     (forces false themes on scattered weeks; "find the theme" is not an executable step). -->
