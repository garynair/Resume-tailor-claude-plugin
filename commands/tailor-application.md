---
command: /tailor-application <JD or URL>
status: active
---

# /tailor-application

Runs a full application-tailoring pass for one job description: analysis,
gap dialogue, drafting, and independent critique. Produces a session
file (single source of truth for the run), a JD Details document, and a
final resume + cover letter in `user-data/output/<company>_<role>/`.

## Step 1: Precondition check

Check `user-data/reference/user-profile.md` exists and has at least the
`Name:` field populated. If it doesn't, stop immediately and direct the
user to run `/setup-profile` first; do not proceed with a blank or
guessed name, per the name-sourcing rule in `skills/constraints.md`.

Also check `user-data/reference/master-resume.md` has content (i.e.,
`/build-reference` has run at least once). If it's still empty, stop and
direct the user to `/build-reference` first; there is nothing to source
tailored bullets from otherwise.

## Step 2: Job analysis and gap dialogue

Invoke `job-analyzer` with the JD or URL the user provided. This agent:

- Resolves the JD text and creates
  `user-data/applications/session_<company>_<role>.md`.
- Writes a standalone JD Details document
  (`user-data/output/<company>_<role>/<Name>_<Company>_<Role>_JD.docx`)
  capturing Designation, Company, Location, OnSite/Remote, Compensation,
  and JD Link, followed by the full JD text as inputted. See
  `agents/job-analyzer.md` Step 3.
- Applies the JD-keyword-to-track decision tree to select the target
  bundle (and secondary track, if applicable).
- Maps every JD requirement to MATCH/PARTIAL/GAP against the bundle.
- Runs closed-ended gap dialogue with the user for every PARTIAL and GAP
  item, logging each Q&A with a timestamp to the session file.

Wait for `job-analyzer` to fully complete (including gap dialogue)
before proceeding; `resume-tailor` and `coverletter-writer` both depend
on the finished Requirement Mapping and Gap Dialogue Log sections being
in a stable, complete state, not a partial one.

Once the target company name is known (from the resolved JD), invoke
`company-research` in parallel with the gap dialogue; it writes the
Company Research section to the session file independently and doesn't
block or depend on gap dialogue completing. Skip this invocation
entirely if the posting is via an undisclosed-client recruiter, per
`agents/company-research.md`'s guardrails.

## Step 3: Drafting (parallel)

Once `job-analyzer` finishes (and `company-research` has written its
section, or is skipped per the undisclosed-client case), invoke
`resume-tailor` and `coverletter-writer` together, not sequentially.
Both agents:

- Read the same session file and the same selected bundle.
- Have read-only access to the session file's earlier sections (Job
  Description, Track Selection, Requirement Mapping, Gap Dialogue Log)
  and write-only access to their own section (Bullet Plan for
  resume-tailor, Cover Letter Plan for coverletter-writer).
- Independently enforce the sourcing rule (every claim traces to
  `master-resume.md` or an approved gap-dialogue entry) and check
  `corrections-log.md` before finalizing any figure.

Coordination point: since both agents draft from the same underlying
material, after both finish drafting, cross-check the Bullet Plan and
Cover Letter Plan for the no-repeated-metrics rule in
`skills/constraints.md` (a number used in a resume bullet should not
also appear in the cover letter). If a repeat is found, this command
should flag it back to `coverletter-writer` for a quick revision before
moving on to critique, rather than letting `critique-agent` be the first
to catch it.

Each agent produces its output document once drafting is finalized:

- `user-data/output/<company>_<role>/resume.docx`
- `user-data/output/<company>_<role>/cover_letter.docx`

(folder slug matches the session file's `<company>_<role>` naming).

## Step 4: Critique (fresh context)

Invoke `critique-agent` only after both drafts are complete and their
output documents exist. Per `agents/critique-agent.md`'s non-negotiable
fresh-context rule, this must run as a genuinely separate context from
the one that did the drafting, not a continuation of the same
conversation/session state. If the runtime doesn't support a fully
isolated context for this step, at minimum instruct the critique pass to
disregard all prior reasoning from drafting and evaluate only the
finished documents and session file as a first-time reader would.

`critique-agent` reads the finished resume, cover letter, session file,
and reference files, independently re-verifies sourcing, runs the
three-persona read-through, and writes scored, tiered findings to the
session file's Critique Scores section. It has no drafting authority and
will not modify the output documents itself.

## Step 5: Report and finalize

After critique completes:

1. Show the user the weighted score and the Tier 1 (blocking) findings
   first, if any exist. Tier 1 findings (sourcing failures, format-spec
   violations, em dashes, duplicate metrics) should be resolved before
   the application is considered ready; offer to send the flagged items
   back to `resume-tailor`/`coverletter-writer` for a targeted revision
   pass rather than shipping known blocking issues.
2. Once there are no unresolved Tier 1 findings, confirm the final
   documents in `user-data/output/<company>_<role>/` are current (i.e.,
   if a revision pass ran, the `.docx` files reflect the revised
   content, not the pre-critique draft).
3. Present the session file location and the output folder (session
   file, JD Details document, resume, cover letter) to the user as the
   deliverable, along with a short summary of Tier 2/3 findings they may
   want to act on but that aren't blocking.

## Notion sync

This command does not invoke `notion-sync-agent` under any
circumstances, regardless of the `notion_sync` value in
`user-profile.md`. Per `agents/notion-sync-agent.md`, that agent remains
disconnected from this flow until its own activation gate (Step 7
validation suite passing across 5+ real JDs, plus a staging database and
scoped connector access) is met. If the user asks why their
`notion_sync: true` setting doesn't seem to be doing anything, explain
this gate rather than wiring the agent in as a workaround.

## Guardrails

- Never skip the precondition check in Step 1, even if the user seems in
  a hurry; a missing profile or empty master-resume.md produces garbage
  output no matter how good the JD analysis is.
- Never invoke `resume-tailor`/`coverletter-writer` before
  `job-analyzer`'s gap dialogue is fully logged; drafting against an
  incomplete Requirement Mapping reintroduces exactly the fabrication
  risk the sourcing rules exist to prevent.
- Never let `critique-agent` run in the same context that produced the
  drafts; that defeats the purpose of the independent read.
- Never treat a Tier 1 finding as optional polish; it must be resolved or
  explicitly acknowledged by the user as accepted risk before the
  application is called done.
