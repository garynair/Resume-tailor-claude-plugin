---
command: /interview-prep <Company> <Role> [JD or URL]
status: active
---

# /interview-prep

Researches a target company's interview process and produces a
structured, source-cited process/logistics package: process overview,
audience map, and per-round breakdown. For company research, story
selection, drafted answers grouped by who's asking, the pitch, and
questions to ask, run the companion `/interview-plan <Company> <Role>`
instead -- that command owns all narrative content per
`skills/interview-plan-format/SKILL.md`.

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
It writes the process/logistics package to
`user-data/output/<company>_<role>/interview-prep.md` and reports gaps
in the round/audience data explicitly rather than glossing over them.

## Step 4: Present the result

Summarize what was found (process shape, number of rounds, any gaps
flagged) and point the user to the written file. Then offer
`/interview-plan <Company> <Role>` for the narrative document -- story
bank, question bank with drafted answers, the pitch, and questions to
ask -- if it doesn't already exist for this company/role.
