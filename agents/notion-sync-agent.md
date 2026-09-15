---
name: notion-sync-agent
status: ACTIVE (threshold reduced 2026-09-15)
---

# Notion Sync Agent

**Precondition to activate (revised 2026-09-15, per direct user
instruction):** the original 5+ real JD threshold assumed bulk-run
validation, which doesn't match how this candidate actually works
(one JD at a time). Reduced to: Step 7 (full output validation suite)
passes on **1 real JD**, AND a staging database plus scoped connector
access are set up. Both are still required together — neither alone is
sufficient.

**Separate from this file's own gate:** actual write access still
depends on a Notion connector with write/create-page scope being
connected for the session that invokes this agent. A read-only or
search-only Notion connection (as seen in at least one Cowork session)
cannot execute the writes described below even with this agent
unblocked — verify write scope is actually present before relying on
this working end-to-end.

`user-profile.md`'s `notion_sync` flag was set `true` 2026-09-15. This
agent may now be invoked from `tailor-application` once the 1-JD
validation pass above has actually been confirmed, not automatically
just because the flag is true.

**Field scope (revised 2026-09-15 against the real "Job Search Tracker"
database schema, replacing the original `Cowork Session Notes` field,
which never existed):**

- Writes: `Status`, `Company`, `Job Description`, `Employment Type`,
  `Location`, `Date Found`, `Local Resume Path`, `Local Cover Letter
  Path`, and an append-only dated line in `Notes` (e.g. "Resume
  regenerated 2026-09-15 via Resume-Tailor-Claude, supersedes prior
  local version") rather than overwriting `Notes` wholesale.
- Never touches: `AI Score`, `AI Score Label`, `AI Summary`, `Score
  Threshold Met` (this plugin computes no such score — confirmed
  2026-09-15, nothing in `resume-tailor.md`/`job-analyzer.md`/
  `critique-agent.md` produces one), `Resume Link`, `Cover Letter Link`
  (the Google Docs links — see open Drive item below), `Duplicate
  Flag`, `Recruiter Name`, `Recruiter Email`, `Make Run ID`, `Email
  Thread`, `Salary`/`Compensation` unless the JD explicitly discloses
  a figure.
- `Local Resume Path`/`Local Cover Letter Path` point at this session's
  own output (`D:\CLAUDE\Resume-Tailor-Claude\user-data\output\...`),
  not the axionsec-job-engine pipeline's output folder — overwrite these
  on a same-job re-run per direct user instruction (2026-09-15: re-running
  a job because the first draft wasn't satisfactory is an expected,
  approved case).
- Only run if `user-profile.md` has `notion_sync: true`.
- Search-before-create (by JD URL or company+title) to avoid duplicate
  rows.
- Check the target page's `last_edited_time` before writing and warn if
  it changed mid-session.

**Open design requirement (not yet built):** a normalized job fingerprint
(company + title + posting URL, hashed or exact-matched) that both the
automated pipeline and this agent check before creating a row.

**Open item — Drive upload (blocked, 2026-09-15):** candidate wants
`Resume Link`/`Cover Letter Link` (Google Docs) kept current
automatically too, new-file-per-version with old versions moved to an
Archive subfolder (not replace-in-place), so history is preserved.
Blocked on two things, not yet a coding task:
1. This session's Google Drive connector (tools: `share_file`,
   `trash_file`, `update_file`\* metadata-only, `search_files`) has no
   file-content upload/create tool at all — nothing to build against
   yet.
2. Account mismatch unresolved: candidate states Gmail and Drive are
   both connected as their `pgvb14@gmail.com` account and that they own
   the target Drive folder, but every file/folder this connector could
   actually see (owned and shared-with-me) on 2026-09-15 was attributed
   to `garynair@gmail.com`, and a direct `owner = 'pgvb14@gmail.com'`
   query returned no files. Candidate asked to park this rather than
   keep troubleshooting — revisit before building the upload step, since
   building against the wrong authenticated account would silently fail
   or write to the wrong Drive.
Once both are resolved: new file per version, moved-old-version-to-
Archive-subfolder model, re-share the new file with the same
collaborators/role via `share_file`.
