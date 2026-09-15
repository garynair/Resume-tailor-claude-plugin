---
command: /interview-plan <Company> <Role> [JD or URL]
status: active
---

# /interview-plan

Builds the narrative/readiness document for a target interview: Job
Analysis, Company Deep Dive, Story Bank, Question Bank, The Pitch,
Final Polish, and a pointer into the round-by-round logistics in
`interview-prep.md`. This is the companion to `/interview-prep` — that
command covers what will happen in the room; this one covers what the
candidate actually says.

## Step 1: Resolve inputs

- If `user-data/applications/session_<company>_<role>.md` exists, use
  it as the primary source for the JD and requirement mapping.
- If it doesn't exist, and the user supplied a JD or URL alongside the
  command, proceed from that directly (read-only — no session file,
  no gap dialogue, no resume/cover letter draft).
- If neither is available, ask for one before proceeding.

## Step 2: Precondition check

Check `user-data/reference/user-profile.md` exists and has at least the
`Name:` field populated. If it doesn't, stop and direct the user to
`/setup-profile` first.

## Step 3: Run the agent

Invoke `interview-plan` with the company, role, and resolved JD source.
It writes `user-data/output/<company>_<role>/interview-plan.md`,
following the fixed step order in `skills/interview-plan-format/SKILL.md`
(company research before the pitch, never after), and reports any gap
in sourced material explicitly.

## Step 4: Present the result and offer next steps

Summarize what was built (story count, question count, any gaps
flagged) and point the user to the written file. Then offer:

- `/interview-prep <Company> <Role>` if that companion document doesn't
  exist yet for this company/role.
- `/schedule-mock-interview <Company> <Role>` to set up rehearsal
  sessions against the new Question Bank ahead of the interview date.
