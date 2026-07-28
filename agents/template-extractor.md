---
name: template-extractor
status: active
---

# Template Extractor

Scans `user-data/raw-corpus/old-resumes/` for structural conventions
(section order, alignment, font/size, spacing, skills-section format,
client-name placement) as a **separate concern from content extraction**.
`corpus-builder` extracts what the candidate did (bullets, metrics,
tools); `template-extractor` extracts how past resumes were laid out.
The two should never be conflated: a corpus-builder pass must never
silently change formatting, and a template-extractor pass must never
touch bullet content or metrics.

## When this runs

Invoked as **Step 1b of `/build-reference`**, after `corpus-builder`
completes its content pass and before `bundle-builder` runs. Also
runnable on its own if the user adds new resumes to the raw corpus and
wants a fresh structural scan without a full `/build-reference` pass.

## Baseline status (as of 2026-07-27)

`skills/resume-format.md` was rewritten cleanly on 2026-07-27 from a
full round of explicit, confirmed user decisions (left-aligned
throughout; client names as an italic sub-line under each role header;
Core Skills as a single flat "•"-separated line capped at 10-12 items;
Technical Skills as 4 labeled groups curated to 5-8 items each; the
tagline format; the spacing rules; the file-naming convention; and
everything else currently in that file). That version is the
**authoritative baseline going forward**, not a draft to be re-derived.
`template-extractor` does not regenerate `resume-format.md` from
scratch each run; its job from this point forward is **drift
detection**, not template authorship.

## What this agent does on each run

1. Identify the most recent, most-polished resume(s) in
   `old-resumes/` (recency signal: filename date if present, otherwise
   file modification time; "most-polished" signal: the version bundle-
   builder/corpus-builder already treats as carrying the most complete,
   current-role content, per `master-resume.md`'s sourcing notes).
2. Extract structural observations only, never content:
   - Section order and section titles used.
   - Alignment (left/justified/centered) per section.
   - Font family/size conventions, if inferable from the document.
   - Spacing conventions (tight vs. generous, visually).
   - Skills-section format (flat line vs. labeled sub-groups vs. two
     separate sections).
   - Client-name placement (embedded in bullets, a separate line, a
     role-header suffix, or absent).
   - Tagline presence/format, if any.
3. Compare these observations against the current
   `skills/resume-format.md` baseline.
4. **Report drift, do not auto-apply it.** If a newly added source
   resume uses a structural convention that differs from the current
   baseline (e.g., a new resume in the corpus uses justified text, or a
   different skills-section format), surface this as a flagged
   observation in the `/build-reference` summary for the user to
   decide on, exactly like `corpus-builder` flags a `[conflict]` content
   item. Never silently update `resume-format.md` from a single new
   source file; formatting changes to the baseline only happen through
   an explicit user instruction, the same way every formatting decision
   in this plugin's history has been made.

## Output

Write findings to a `## Structural Observations` section appended to
`user-data/reference/master-resume.md` (below the Technical Skills
Inventory, above Certifications), in this shape:

```markdown
## Structural Observations (template-extractor, <date>)

- Most recent/most-polished source scanned: <filename>
- No structural drift detected from the current resume-format.md
  baseline. (or: list each specific drift observation here, flagged for
  user review, with the source filename)
```

If no drift is found (the expected steady state once the baseline is
finalized), keep this section short rather than padding it with
restated baseline content; a one-line "no drift detected" entry is a
complete, valid output.

## Guardrails

- Never write to `skills/resume-format.md` directly. This agent
  observes and reports; only an explicit user instruction (implemented
  by editing `resume-format.md` directly, the way every rule currently
  in that file was added) changes the baseline.
- Never extract or restate bullet content, metrics, or client names;
  that is `corpus-builder`'s job and duplicating it here risks drift
  between the two agents' outputs.
- If `old-resumes/` is empty or contains only unparseable scans, report
  that no structural scan was possible and skip this step rather than
  guessing at a template from nothing.
