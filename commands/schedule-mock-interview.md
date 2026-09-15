---
command: /schedule-mock-interview <Company> <Role> <Interview date>
status: active
---

# /schedule-mock-interview

Sets up recurring mock-interview rehearsal sessions ahead of a real
interview date, using the host platform's scheduled-task feature (on
Cowork/Claude Code Remote, the `create_trigger` tool family — sometimes
called "scheduled tasks" or "Routines" in the product UI). **Never use
an in-session cron tool for this** — an in-process scheduler is lost
when the session ends, which would silently fail to deliver the
rehearsal the candidate is counting on.

If the host session has no scheduled-task tool available at all (for
example, a plain Claude Code session with no Cowork/Remote MCP
connected), say so plainly and offer to run `mock-interview` on demand
instead — do not pretend to schedule something that can't fire later.

## Step 1: Resolve inputs

- **Company** and **role** (required) — used to locate
  `user-data/output/<company>_<role>/interview-plan.md`.
- **Interview date** (required) — the actual interview date/time the
  candidate gave. Ask if not supplied; do not guess one.
- Confirm `interview-plan.md` exists for this company/role and has a
  Question Bank. If it doesn't, stop and point the candidate at
  `/interview-plan <Company> <Role>` first — there is nothing to
  rehearse against yet.

## Step 2: Compute firing times

Suggested cadence (confirm with the candidate rather than assuming
they want exactly this):

- **Session 1:** ~1 week before the interview date, mode `cold`.
- **Session 2:** ~1-2 days before the interview date, mode
  `targeted:panel-mixed` (or a more specific audience if the candidate
  knows which round is next).

If the interview is less than a week out, drop Session 1 and schedule
only Session 2; say so rather than scheduling a "1 week before" session
that would fire in the past.

## Step 3: Create the scheduled tasks

For each session, call the scheduled-task creation tool with a
**complete, self-contained prompt** — each firing starts a fresh
session with no memory of this conversation, so the prompt must name
everything the fresh session needs:

```
Run a mock-interview session for {Company}, {Role}. Mode: {cold |
targeted:<audience>}. Read the Question Bank from
user-data/output/{company}_{role}/interview-plan.md — pull the live
Question Bank at fire time, not a copy, since the candidate may have
updated interview-plan.md since this was scheduled. Invoke the
mock-interview agent from the resume-tailor-plugin plugin to run the
session per skills/interview-prep-format/SKILL.md's result-first framing.
```

Use a one-time firing (`run_once_at`, not a recurring cron expression)
for each session, since these are two specific rehearsal dates tied to
one real interview, not an ongoing recurring cadence. Name each task
clearly (e.g. "Mock interview — {Company} {Role} — cold, T-7d") so the
candidate can find and cancel it later if the interview date changes.

## Step 4: Confirm with the candidate

Report back the two (or one) scheduled firing times in the candidate's
own local timezone, and note that if the interview date changes, they
should ask to reschedule or cancel these rather than letting a stale
rehearsal fire against an outdated date.
