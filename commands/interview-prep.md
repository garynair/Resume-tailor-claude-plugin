---
command: /interview-prep <Company> <Role> [JD or URL]
status: active
---

# /interview-prep

Researches a target company's interview process and produces a
structured, source-cited prep package: process overview, per-round
breakdown, and likely questions with candidate-specific, result-first
answers grouped by who's asking (recruiter, hiring manager, technical
peer, or a mixed panel).

## Step 1: Resolve inputs

- If `user-data/applications/session_<company>_<role>.md` exists (a
  prior `/tailor-application` run for this company/role), use it as the
  primary source -- the JD and requirement mapping are already there.
- If it doesn't exist, and the user supplied a JD or URL alongside the
  command, proceed from that directly (read-only -- no session file is
  created, no gap dialogue runs, no resume/cover letter is drafted).
- If neither a session file nor a JD/URL is available, ask for one
  before proceeding.

## Step 2: Precondition check

Check `user-data/reference/user-profile.md` exists and has at least the
`Name:` field populated, same as `/tailor-application`. If it doesn't,
stop and direct the user to `/setup-profile` first.

## Step 3: Run the agent

Invoke `interview-prep` with the company, role, and resolved JD source.
It writes the prep package to
`user-data/output/<company>_<role>/interview-prep.md` and reports gaps
in the candidate's source material explicitly rather than glossing over
them.

## Step 4: Present the result

Summarize what was found (process shape, number of rounds, any gaps
flagged) and point the user to the written file. If any Likely
Questions answer had to be left unsourced, call that out by name so the
user can decide whether to prepare it themselves or treat it as a real
gap to address before the interview.
