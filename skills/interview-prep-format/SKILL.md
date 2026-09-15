---
name: interview-prep-format
description: "Authoritative interview-prep output spec: process overview, audience map, round-by-round breakdown, and the no-fabrication/tagging rules for research vs. inferred content. Covers the process/logistics document only -- narrative content (Job Analysis, Company Deep Dive, Story Bank, Question Bank, The Pitch, Final Polish) lives in the companion skills/interview-plan-format/SKILL.md."
---

# Interview Prep Format

This is the output contract for `agents/interview-prep.md`. It defines
the structure the process/logistics document must render in, and the
accuracy rules that govern what can be asserted vs. what must be
tagged as inferred.

**This document covers "what will happen in the room" only.** The
companion document, `interview-plan.md`, covers "how do I walk in
ready" — Job Analysis, Company Deep Dive, Story Bank, Question Bank
(with drafted answers), The Pitch, and Final Polish, per
`skills/interview-plan-format/SKILL.md`. Do not draft candidate answers here;
if this agent is asked for likely-question answer drafting, point at
`/interview-plan` instead of building it in this document.

## Tag conventions (do not mix)

- `[inferred from JD]` -- a claim or a reported-question paraphrase
  derived from the job description itself, not from a candidate report,
  review site, or other external source.
- `[inferred]` -- a classification (e.g. an audience assignment in the
  Audience Map) made from indirect signals (round duration, title,
  position in the sequence) when the source data doesn't state it
  directly.

**Never fabricate a specific question.** If a source says "they asked
about distributed systems," report that generality -- do not invent a
specific distributed-systems question and present it as sourced. A
question paraphrase generated from JD analysis rather than a real
report must carry the `[inferred from JD]` tag, every time, with no
exceptions for a "probably realistic" invented question.

## Process Overview

```markdown
## Process Overview
- **Rounds:** {N} rounds, ~{X} days end-to-end
- **Format:** {e.g., recruiter screen -> technical phone -> take-home -> onsite (4 rounds) -> hiring manager}
- **Platform:** {call medium per round, e.g. Zoom / Teams / Google Meet / Phone -- if not detectable from available material, write "not stated, confirm before the call" rather than guessing}
- **Difficulty / sentiment:** {qualitative read from whatever review/source material is available, with source}
- **Known quirks:** {e.g. "pair programming instead of whiteboard", "no LeetCode, all practical"}
- **Sources:** {links or references the material came from}
```

If a field can't be supported by available material, write "unknown --
not enough data" rather than guessing. Never leave a field silently
blank; an explicit "unknown" is the honest state, a blank field reads as
an oversight.

## Audience Map

Classify every discovered round into exactly one of:

| Audience | Typical round | Primary evaluation |
|---|---|---|
| `recruiter-screen` | First call (15-30 min, recruiter/HR/TA) | Fit gate: motivation, comp, location/visa, timeline |
| `hiring-manager` | Manager / skip-level (30-45 min) | Why this role, scope alignment, leadership signals |
| `peer-tech` | IC technical (live problem-solving, case study, take-home review) | Depth + collaboration on the actual work |
| `panel-mixed` | Onsite / loop with multiple interviewer types in one block | Cross-cuts the above |

This is the same four-value convention `skills/interview-plan-format/SKILL.md`
uses for its Question Bank -- keep both in sync if this table ever
changes, so a question tagged `peer-tech` here and there means the same
thing in both documents.

When the audience for a round isn't stated, infer cautiously from
duration, position in the sequence, and any signal in the JD or posting,
and mark it `[inferred]`. Do not default round 2 to any one audience
without a signal for it -- many processes put a technical screen there,
others put the hiring manager. When genuinely ambiguous, mark
`panel-mixed [inferred]` and prep both packs rather than guessing wrong.

```markdown
## Audience Map
- **Round 1** (recruiter screen, 30 min) -> `recruiter-screen`
- **Round 2** (technical phone screen, 60 min) -> `peer-tech`
- **Round 3** (hiring manager call, 45 min) -> `hiring-manager`
```

## Round-by-Round Breakdown

For each round:

```markdown
### Round {N}: {Type} -- audience: `{audience}`
- **Duration:** {X} min
- **Conducted by:** {peer / manager / skip-level / recruiter -- if known}
- **Platform:** {for this specific round, if known}
- **What they evaluate:** {specific skills or traits}
- **Reported questions:**
  - {question} -- [source]
- **How to prepare:** {1-2 concrete, audience-appropriate actions --
    point at the relevant Question Bank entries in interview-plan.md
    rather than drafting new answer content here}
```

If round structure is entirely unknown, say so plainly and give the best
available intel on what to expect based on company size, stage, and role
level -- clearly labeled as an estimate, not a report.

## Accuracy carries through from `skills/constraints/SKILL.md`

Everything in this format still answers to the plugin's
Accuracy > Relevance > Impact > ATS > Brevity hierarchy and the
no-fabrication rule in `skills/constraints/SKILL.md`. A reported question or
round detail is not exempt from either just because it's process
information rather than a drafted answer -- a fabricated "known quirk"
is the same class of error as a fabricated resume metric.
