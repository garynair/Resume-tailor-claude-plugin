---
name: interview-plan-format
description: "Authoritative interview-plan output spec: the narrative/readiness document (Job Analysis, Company Deep Dive, Story Bank, Question Bank, The Pitch, Final Polish, What Happens in the Room) that sits alongside interview-prep.md's process/logistics document. Defines step order, sourcing rules for the two STAR story files, and the accuracy rules carried over from interview-prep-format.md."
---

# Interview Plan Format

This is the output contract for `agents/interview-plan.md`. It is the
companion document to `skills/interview-prep-format.md`: that skill
governs the process/rounds/logistics document
(`user-data/output/<company>_<role>/interview-prep.md`); this skill
governs the narrative/readiness document
(`user-data/output/<company>_<role>/interview-plan.md`).

**Do not merge these two documents.** They serve different moments —
interview-prep.md is "what will happen in the room," interview-plan.md
is "how do I walk in ready" — and collapsing them into one file is the
exact structural problem this skill exists to avoid.

**Mock practice is not a step in this document.** It is a separate,
recurring, scheduled activity (`agents/mock-interview.md`,
`commands/schedule-mock-interview.md`). This document may reference the
Question Bank as the mock module's input; it never contains a
transcript, a rehearsal log, or a "do this rehearsal now" step of its
own.

## Step order (fixed — do not reorder)

Each step feeds the next. Building Step 5 (The Pitch) before Step 2
(Company Deep Dive) produces a generic pitch; the order below exists
specifically to prevent that.

1. Job Analysis
2. Company Deep Dive
3. Story Bank
4. Question Bank
5. The Pitch
6. Final Polish
7. What Happens in the Room

## Tag conventions (carried over, plus one addition)

Use `[inferred from JD]` and `[inferred]` exactly as defined in
`skills/interview-prep-format.md`. In addition, for Story Bank content:

- `[corroborated across N files]` — pulled from
  `user-data/reference/star-story-bank.md`.
- `[candidate-authored]` — pulled from
  `user-data/reference/star-story-inbox.md`. Single-source, same trust
  tier as a resolved gap-dialogue answer. Never presented as more
  certain than that.

**Never fabricate a specific fact, question, or story detail.** The
no-fabrication rule from `skills/constraints.md` and
`skills/interview-prep-format.md` applies to every step below without
exception.

## Step 1 — Job Analysis

```markdown
## Job Analysis
**Top 3 core technical needs (from the JD):**
1. {need} — "{quoted or closely paraphrased JD language}"
2. {need} — "{JD language}"
3. {need} — "{JD language}"

**Top 3 soft skills (from the JD):**
1. {skill} — "{JD language}"
2. {skill} — "{JD language}"
3. {skill} — "{JD language}"
```

Source every line to actual JD language — a quote or a close
paraphrase, not a generic restatement. If the JD only implies a need
without naming it, tag that line `[inferred from JD]`.

## Step 2 — Company Deep Dive

This step must run before Step 5 (The Pitch) exists — that is the whole
point of moving company research earlier than the original linear
draft had it. It also runs independently of, and is broader than,
`agents/company-research.md` (which finds one narrow, verified-or-omit
fact for a cover-letter Company Signal Line). This step may reuse a
fact `company-research.md` already found for the same
company/application cycle rather than re-searching for it, but its own
job is wider:

```markdown
## Company Deep Dive
- **Recent news (last ~12 months):** {dated items, each with a source}
- **Culture signals:** {stated values, engineering/GRC blog posts,
  employee-review themes — sourced, or "not enough public signal" if
  thin}
- **Competitors / comparable organizations:** {for a nonprofit,
  institution, or agency, use comparable-organization context instead
  of "competitors"}
- **Sources:** {links or citations for everything above}
```

Use live web search; do not answer from training knowledge (same
sourcing discipline as `agents/company-research.md`). If the company is
small, private, or yields little public signal, say so plainly rather
than padding the section — an honest "limited public signal" is a
correct outcome, not a failure.

## Step 3 — Story Bank

```markdown
## Story Bank

### Power Stories (STAR, with metrics)
1. **{Title}** — {tags} — source: `star-story-bank.md` |
   `star-story-inbox.md`
   - Situation: ...
   - Task: ...
   - Action: ...
   - Result: {tag per the conventions above} {metric exactly as sourced}

(five total)

### Technical Walkthroughs
1. **{Tool/framework/process named in the JD}** — source: {file}
   - {walkthrough tied directly to that named tool/framework/process,
     using result-first framing}

(two to three total)
```

Read **both** `user-data/reference/star-story-bank.md` and
`user-data/reference/star-story-inbox.md`, and cite which file each
story came from. Prefer a `[corroborated]` story over a
`[candidate-authored]` one when both cover the same underlying
achievement equally well; use the corroboration-count tiebreak from
`skills/constraints.md` when two corroborated variants conflict.
Technical walkthroughs must tie to a tool, framework, or process
actually named in the JD — not a generic technical story.

If five real power stories or two to three real technical walkthroughs
aren't supported by either source file, say so and list fewer rather
than padding with a weaker or less-relevant story.

## Step 4 — Question Bank

Reuse the audience-tagging convention from
`skills/interview-prep-format.md` (`recruiter-screen`,
`hiring-manager`, `peer-tech`, `panel-mixed`) so this does not regress
the round-by-round targeting that skill already does well.

```markdown
## Question Bank
### {N}. {Question} — audience: `{audience}`
{Best-answer draft using Headline → Effect → Rationale → Operations,
per skills/interview-prep-format.md's result-first framing. Draws only
from Job Analysis, Company Deep Dive, and Story Bank content already
established above — not new, unsourced material.}
```

Ten questions total, generated from the JD, the role archetype, and
(if available) real candidate/company interview reports — the same
source hierarchy `agents/interview-prep.md`'s Step 1 research already
uses. A question generated purely from JD analysis rather than a real
report carries `[inferred from JD]`.

## Step 5 — The Pitch

Both answers are **fully drafted prose, written to be read aloud** —
not bullets, not an outline.

```markdown
## The Pitch

### "Tell me about yourself"
{Past → present → future narrative, full prose.}

### "Why this role / why this company"
{Full prose. Must reference at least one concrete fact from Step 2
(Company Deep Dive) by name — a generic version of this answer that
could apply to any employer fails this step.}
```

## Step 6 — Final Polish

```markdown
## Final Polish
**Questions to ask the interviewer:**
1. {question specific to this role/org — not "what's the culture
   like" or another generic question}
2. ...
(3–5 total)
```

Every question must reference something specific to this role, team,
or company (a named initiative, a stated priority from the JD or
Company Deep Dive, a specific team structure) — a generic question that
could be asked at any company fails this step and should be replaced.

## Step 7 — What Happens in the Room

A condensed pointer into the companion document, not a duplicate of
it:

```markdown
## What Happens in the Room
See `interview-prep.md` in this same folder for the full process
overview, audience map, and round-by-round breakdown. Quick summary:
{1-2 sentence summary of round count/format, pulled from that file}.
```

If `interview-prep.md` doesn't exist yet for this company/role, say so
and suggest running `/interview-prep <Company> <Role>` — do not
reconstruct the round-by-round content here, since that would create
two documents to keep in sync.

## Accuracy carries through

Everything in this format answers to the same
Accuracy > Relevance > Impact > ATS > Brevity hierarchy and
no-fabrication rule as `skills/constraints.md` and
`skills/interview-prep-format.md`. A fabricated metric or invented
company fact is the same class of error whether it appears in a resume
bullet or a rehearsed spoken answer.
