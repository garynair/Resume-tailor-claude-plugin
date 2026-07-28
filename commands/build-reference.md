---
command: /build-reference
status: active
---

# /build-reference

Builds (or rebuilds) the master reference layer and per-track bundles
that `/tailor-application` draws from. Run this after `/setup-profile`,
whenever the raw corpus changes (new old resumes/cover letters added),
whenever `corrections-log.md` gets a new entry that should propagate
into the reference files, or periodically to pick up gap-dialogue
confirmations logged in past application sessions.

## Precondition

Check `user-data/reference/user-profile.md` exists and has at least the
`Name:` and `Target tracks (ranked):` fields populated. If not, stop and
direct the user to `/setup-profile` first; both `corpus-builder` and
`bundle-builder` depend on the target-tracks list to know what to tag
and build for.

## Flow

1. Invoke `corpus-builder`. It reads
   `user-data/raw-corpus/old-resumes/` and
   `user-data/raw-corpus/old-coverletters/`, cross-references and
   deduplicates content, and writes:
   - `user-data/reference/master-resume.md`
   - `user-data/reference/master-metrics-vault.md`
   - `user-data/reference/star-story-bank.md`

   If the raw corpus is empty or unparseable, `corpus-builder` runs its
   structured-interview fallback instead (per its own spec); this
   command doesn't need separate branching logic for that case, just
   wait for `corpus-builder` to report completion either way.

2. Once `corpus-builder` finishes, invoke `template-extractor` (Step
   1b). It scans `old-resumes/` for structural conventions (section
   order, alignment, spacing, skills-section format, client-name
   placement) separately from content, and reports any drift from the
   current `skills/resume-format.md` baseline. This step never writes
   to `resume-format.md`; it only surfaces observations for the
   summary in step 4. This step can run in parallel with step 3 below
   since neither depends on the other's output, but both must complete
   before step 4's summary.

3. Only after `corpus-builder` finishes (bundle-builder reads
   `master-resume.md` as its input, so this step is strictly sequential
   relative to `corpus-builder`, not parallel with it), invoke
   `bundle-builder`. It reads the freshly written `master-resume.md`
   and produces one `bundle-<track>.md` file per target track in
   `user-data/reference/bundles/`.

4. After `bundle-builder` and `template-extractor` both complete,
   report a summary to the user:
   - How many source documents were consolidated (or, if the interview
     fallback ran, which tracks were covered by interview).
   - Any `conflict`-tagged entries in `master-resume.md` that still need
     the user's resolution.
   - Which tracks got a bundle and which target tracks (if any) were
     skipped for lack of tagged content.
   - Any structural drift `template-extractor` flagged against the
     current `resume-format.md` baseline, if applicable.

## Guardrails

- Do not invoke `bundle-builder` before `corpus-builder` has fully
  written `master-resume.md`; bundles built against stale or partial
  master-resume content will misrepresent what's actually corroborated.
- Do not skip straight to `bundle-builder` alone as a "quick rebuild"
  option; bundles are always derived from the current state of
  `master-resume.md`, and the only supported way to regenerate that
  state is via `corpus-builder`.
- This command does not call `resume-tailor`, `coverletter-writer`,
  `job-analyzer`, `critique-agent`, or `notion-sync-agent`; it only
  builds the reference layer those agents later consume.
- If `corpus-builder` surfaces `conflict` entries needing user
  resolution, don't block `bundle-builder` on resolving every conflict
  immediately (conflicts can carry forward as flagged content), but do
  make sure the summary in step 3 highlights them so they don't go
  unnoticed indefinitely.
