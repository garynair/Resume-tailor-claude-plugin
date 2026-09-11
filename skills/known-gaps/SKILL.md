---
name: known-gaps
description: Mechanism for a non-negotiable register of claims the candidate can't confidently make, paired with pre-approved phrasing for sensitive or ambiguous claims. Consulted by tailor-application and reflexion-self-critique before any resume or cover letter is finalized. Never overridden by JD keyword pressure. Candidate-specific content lives outside this file (see below), not here.
---

# Known Gaps & Pre-Built Framings

This file defines the **mechanism**, not the candidate's specific facts.
The candidate's actual known gaps (e.g., specific in-progress
certifications) and any candidate-specific pre-built framings live in a
private, untracked file: `user-data/reference/known-gaps.md` (a sibling
directory to this plugin, gitignored, never committed to version control
— the same handling every other file under `user-data/` gets). This
SKILL.md is the public, versioned description of how that private file
is used; it intentionally contains none of the candidate's own personal
specifics (updated 2026-09-11 — this file previously stated real
certification details directly; those moved to the private file below
once this plugin's repo was confirmed public).

## Priority

This mechanism sits alongside `resume-tailor-plugin:constraints` in the
priority hierarchy: protected metrics and known gaps are both hard
constraints, not stylistic preferences, once the private file below is
populated.

## Pattern: Known Gaps

A "known gap" is a claim the candidate is uncertain about, has partial
exposure to, or wants explicitly bounded so a JD's keyword pressure never
pushes the agent into overstating it. Common shape: an in-progress
credential, certification, or capability that must never be stated as
complete. See `user-data/reference/known-gaps.md` for this candidate's
actual entries.

## Pattern: Pre-Built Framings

A "pre-built framing" is the candidate's own approved phrasing for a
claim that's true but easy to overstate, reused verbatim rather than
generating new phrasing each session. Common categories:

- Collaborative/team project attribution (avoiding solo-attribution
  language for team efforts).
- Advisory vs. implementation work (not claiming "implemented" or
  "delivered" for advisory-only involvement).
- Protected metrics context (scoping a metric to its actual engagement
  rather than letting it read as a universal claim).

See `user-data/reference/known-gaps.md` for this candidate's actual
approved framings.

## How this gets used

- `tailor-application` checks any generated claim against
  `user-data/reference/known-gaps.md` before drafting.
- `reflexion-self-critique` treats deviation from that file as a
  hard-fail, same tier as a protected-metric violation.
- If `user-data/reference/known-gaps.md` doesn't exist yet, treat this as
  an empty register (no known gaps recorded), not an error — but flag it
  to the candidate once, since an unpopulated file silently provides no
  protection.
- This file grows over time; each tailoring session that surfaces a new
  ambiguous claim should propose an addition to the private file, never
  to this SKILL.md.
