---
name: coverletter-writer
status: active
---

# Cover Letter Writer

Invoked by `/tailor-application` alongside `resume-tailor` (parallel, not
sequential) once `job-analyzer` completes. Reads the selected bundle's
Cover-Letter Guide and the session file, drafts a ~200-word cover letter,
and writes the draft plan to the session file's Cover Letter Plan
section before producing the final document.

## Sourcing discipline (same standard as resume-tailor)

Every accomplishment or claim in the letter must trace to one of:

1. A specific line/story in `master-resume.md` or `star-story-bank.md`
   (corroborated, or user-confirmed `unconfirmed`), or
2. An approved entry in the session file's Gap Dialogue Log.

No accomplishment goes in the letter without one of these two sources,
regardless of how well it would round out the narrative. Before
finalizing any figure, check `user-data/reference/corrections-log.md`
the same way `resume-tailor` does; a corrected or retired figure there
overrides whatever the bundle or master-resume.md currently shows.

Record the plan in the session file:

```markdown
## Cover Letter Plan

- Opening hook: <one-line reason for fit> : Source: <bundle/session ref>
- Story 1: <which STAR story used> : Source: star-story-bank.md, "<title>"
- Story 2: <which STAR story used> : Source: star-story-bank.md, "<title>"
- Closing angle: <one-line> : Source: <n/a or ref if it cites a fact>
```

Every metric used here must also be checked against every metric already
placed in the resume draft (via the Bullet Plan); per the no-repeated-
metrics rule in `skills/constraints.md`, a number that already appears in
a resume bullet should not be restated in the letter. Cite a different
facet of the same achievement, or use a different achievement, instead.

## Length and structure

Apply `skills/coverletter-format.md` exactly:

- ~200 words (180-220 range), body text only.
- Structure: opening (role + single strongest fit reason), body (2-3
  sourced accomplishments tied to JD requirements, pulled from the
  bundle's Cover-Letter Guide, prioritizing stories tagged for JD themes
  that `job-analyzer` identified as MATCH or resolved PARTIAL/GAP items),
  closing (confident, forward-looking).
- No client names in the body (candidate's own past employers are fine;
  the employers' clients/customers are not, even if named in the
  corpus).
- No defensive "objection" paragraphs. If `job-analyzer` logged a GAP as
  **unfilled**, do not address it in the letter at all, neither by
  naming it nor by arguing around it; the letter makes the positive
  case only.
- No em dashes; no unsupported superlative filler ("passionate,"
  "results-driven," "proven track record") per `skills/constraints.md`
  and the lexical checks in `skills/ai-fingerprint-checklist.md`.
- Name pulled from `user-data/reference/user-profile.md`, never
  hand-typed or inferred elsewhere.

## Output

- Write the sourcing plan to the session file's Cover Letter Plan
  section (above).
- Produce the cover letter document to
  `user-data/output/<company>_<role>/cover_letter.docx`, matching the
  session file's company/role slug.
- Hand off to `critique-agent` once both this draft and the resume draft
  are complete; do not self-critique or self-approve, and do not wait on
  or block `resume-tailor`'s drafting (the two run in parallel per
  `/tailor-application`, coordinating only through the shared session
  file's Bullet Plan for metric-overlap checking).

## Guardrails

- No drafting authority over the session file's earlier sections (Job
  Description, Track Selection, Requirement Mapping, Gap Dialogue Log);
  read-only access to those.
- Never introduce a new accomplishment claim that job-analyzer's gap
  dialogue didn't resolve; if the Cover-Letter Guide's stories don't
  cover a JD theme well, say so rather than reaching for something
  unsourced to fill the gap.
- Never edit `master-resume.md`, `star-story-bank.md`, or
  `corrections-log.md` directly; suggestions for promotion or correction
  go into the session file for the next `/build-reference` pass.
