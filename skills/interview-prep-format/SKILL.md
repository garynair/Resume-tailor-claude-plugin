---
name: interview-prep-format
description: "Authoritative interview-prep output spec: process overview, audience map, round-by-round breakdown, per-audience likely-question packs with result-first answer framing, and the no-fabrication/tagging rules for research vs. inferred content."
---

# Interview Prep Format

This is the output contract for `agents/interview-prep.md`. It defines the
structure the prep package must render in, and the accuracy rules that
govern what can be asserted vs. what must be tagged as inferred.

## Tag conventions (do not mix)

- `[inferred from JD]` -- a likely question or claim derived from the job
  description itself, not from a candidate report, review site, or other
  external source.
- `[inferred]` -- a classification (e.g. an audience assignment in the
  Audience Map) made from indirect signals (round duration, title,
  position in the sequence) when the source data doesn't state it
  directly.

**Never fabricate a specific question.** If a source says "they asked
about distributed systems," report that generality -- do not invent a
specific distributed-systems question and present it as sourced. A
question generated from JD analysis rather than a real report must carry
the `[inferred from JD]` tag, every time, with no exceptions for a
"probably realistic" invented question.

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
- **How to prepare:** {1-2 concrete, audience-appropriate actions}
```

If round structure is entirely unknown, say so plainly and give the best
available intel on what to expect based on company size, stage, and role
level -- clearly labeled as an estimate, not a report.

## Likely Questions (per audience)

Group every discovered and inferred question by the audience that asks
it, never by question type. Draft candidate-specific answers using the
candidate's actual sourced material (`user-data/reference/master-resume.md`,
`user-data/reference/master-metrics-vault.md`, `user-data/reference/star-story-bank.md`,
`user-data/reference/user-profile.md`) -- never invent an achievement,
metric, or scope to answer a question, per `skills/constraints.md`'s
no-fabrication rule. A gap in the source material for a likely question
is a real gap: name it and move on, don't paper over it with a
plausible-sounding but unsourced answer.

For every answer, use result-first framing:

1. **Headline** -- the result, decision, or point.
2. **Effect** -- why it mattered to the business, system, users, or team.
3. **Rationale** -- what tradeoff or constraint shaped the choice.
4. **Operations** -- what the candidate actually did, with enough detail
   to be credible under a follow-up question.

Senior, technical, and leadership answers need all four. A short
recruiter-screen answer can compress this, but should still open with
the headline, not a narrative windup.

### Audience: `recruiter-screen`

Cover at minimum: the "walk me through your CV / why are you looking"
narrative; a concrete comp range anchored to whatever compensation
target the candidate has recorded, with a clean deferral script if
market data is thin; a specific (not generic) "why this company" tied to
a real, dated signal about the company; location/remote/visa position;
timeline and notice period as numbers, not vibes; and any background
element (gap, transition, pivot) addressed honestly and
forward-looking -- never defensively.

### Audience: `hiring-manager`

Cover: scope alignment to the actual role as posted, not a generic
version of the candidate's title; a specific reason this role over a
lateral move elsewhere; and any leadership/ownership signal the source
material actually supports.

### Audience: `peer-tech`

Cover: depth on the specific stack/domain named in the JD, using
result-first framing on real past work; and one candid, specific
"tell me about a time it went wrong" answer, since a too-polished
failure story reads as evasive to a technical panel.

### Audience: `panel-mixed`

Prep the union of the above, weighted toward whichever sub-audiences the
round description suggests are actually in the room.

## Accuracy carries through from `constraints.md`

Everything in this format still answers to the plugin's
Accuracy > Relevance > Impact > ATS > Brevity hierarchy and the
no-fabrication rule in `skills/constraints.md`. An interview-prep answer
is not exempt from either just because it's spoken rather than written --
a fabricated metric in a rehearsed answer is the same class of error as
one printed on a resume.
