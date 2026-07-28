---
name: bundle-builder
status: active
---

# Bundle Builder

Triggered by `/build-reference`, immediately after `corpus-builder` runs.
Reads `user-data/reference/master-resume.md` (plus
`master-metrics-vault.md` and `star-story-bank.md`) and produces one
bundle file per target track in `user-data/reference/bundles/`, named
`bundle-<track>.md` (e.g., `bundle-grc.md`, `bundle-tprm.md`), matching
the track tags used in `master-resume.md` and the ranked list in
`user-profile.md`.

Bundles are the primary input `resume-tailor` and `coverletter-writer`
read from during `/tailor-application`; they should be self-contained
enough that those agents don't need to re-read the full master-resume.md
line by line for every draft.

## Precondition

`master-resume.md` must have at least one non-empty role entry for a
track before building that track's bundle. If a target track in
`user-profile.md` has no corresponding tagged content yet, skip it, note
it in the run summary, and suggest the user either re-run
`corpus-builder`'s interview fallback for that track or confirm the
track should stay on the target list without a bundle for now.

## Per-track build

For each track with tagged content, pull every role and bullet in
`master-resume.md` carrying that track's tag (a bullet tagged for
multiple tracks appears in more than one bundle) and produce a single
`bundle-<track>.md` with these five sections:

### 1. Role profile

A short factual restatement of the candidate's track record specific to
this track: which roles/companies/date ranges are most relevant, what
seniority level they support (based on actual title/scope in the source
material, not aspirational), and which of those roles supply the
strongest material for this track. This is a targeting aid for
`resume-tailor`, not resume-ready prose.

### 2. Summary guide

2-3 alternate professional-summary drafts (2-4 lines each, matching the
Structure section of `skills/resume-format.md`) built only from
corroborated or user-confirmed content, tuned for this track's
vocabulary and priorities. Mark clearly which source bullets each
summary draws from so `resume-tailor` can verify traceability later.

### 3. Achievement-reframing priority map

A ranked list of the strongest available bullets for this track, in
priority order, with a one-line note on *why* each ranks where it does
(e.g., "highest: vault-corroborated metric + direct match to typical
GRC program-maturity requirements" vs. "lower: unconfirmed single-source,
usable only after user confirms in gap dialogue"). This is what
`resume-tailor` consults first when selecting which bullets to lead
with for a given job description, and what `job-analyzer` can cross-
reference when deciding whether a JD requirement is a MATCH or a GAP.

### 4. Skills guide

The consolidated, deduplicated list of tools, frameworks, and keywords
relevant to this track pulled from `master-resume.md`'s core-competency
lines and bullets (e.g., for GRC: NIST CSF 2.0, ISO 27001:2022, SOX/ITGC,
SOC 2, NIST AI RMF). Group loosely by category (frameworks/standards,
tools/platforms, methodologies) so `resume-tailor` can pick the subset
that matches a given JD's language rather than dumping the whole list
into every resume.

### 5. Cover-letter guide

2-3 STAR stories from `star-story-bank.md` tagged for this track, ranked
by strength (prefer corroborated results over unconfirmed ones), plus a
short note on which JD themes each story answers well (e.g., "best for
JDs emphasizing vendor risk maturity" or "best for JDs emphasizing board
reporting"). This is what `coverletter-writer` selects from rather than
re-deriving stories from scratch per application.

## Output structure (per bundle file)

```markdown
# Bundle: <track>

Built from master-resume.md on <date>.

## Role Profile
...

## Summary Guide
...

## Achievement-Reframing Priority Map
...

## Skills Guide
...

## Cover-Letter Guide
...
```

## Guardrails

- Never introduce a claim, metric, or skill into a bundle that isn't
  traceable to `master-resume.md`, `master-metrics-vault.md`, or
  `star-story-bank.md`. Bundles are a re-organization of existing
  corroborated/flagged content, not a place to smooth over gaps by
  adding polish that isn't sourced.
- Preserve `unconfirmed` and `conflict` tags from `master-resume.md`
  when carrying content into a bundle; don't launder an unconfirmed
  bullet into confident-sounding bundle prose. `resume-tailor` needs to
  know a bullet is still pending confirmation.
- Re-running `/build-reference` regenerates all bundle files from the
  current state of `master-resume.md`; treat bundle files as derived
  output, not a place for the user to hand-edit permanent corrections
  (corrections belong in `corrections-log.md`, which `corpus-builder`
  respects on the next run).
- If a track's tagged content is thin (e.g., fewer than 2 roles or no
  vault-grade metrics), say so explicitly in that bundle's Role Profile
  section rather than padding it out to look more substantial than the
  corpus supports.
