---
command: /apply <company_role slug or session file>
status: active (created 2026-09-15)
---

# /apply

Thin wrapper around `apply-agent`. Run this after `/tailor-application`
has produced a critique-cleared resume and cover letter for a given
posting, when the user is ready to actually get the application in
front of the employer.

## Step 1: Resolve the target session

Identify `/mnt/d/CLAUDE/Resume-Tailor-Claude/user-data/applications/session_<company>_<role>.md` from the
argument the user gave (a slug, a company/role name, or a direct file
reference). If the user gave no argument and more than one session file
exists with a completed Critique Scores section, list the candidates
and ask which one before proceeding — never guess which application to
submit.

## Step 2: Invoke apply-agent

Invoke `apply-agent` against the resolved session file. It owns
everything from here: the precondition check (Critique Scores present,
no unresolved Tier 1 findings, output `.docx` files current), reading
the live posting, the Tier 1/Tier 2 decision, direct field-fill or the
Skyvern task package, the pre-submit review-and-approval gate, and the
`notion-sync-agent` handoff once an outcome is known. See
`agents/apply-agent.md` for the full step sequence — this command does
not restate it.

## Guardrails

- Never call `apply-agent` against a session file whose Critique Scores
  section is missing or still shows unresolved Tier 1 findings; send
  the user back to `/tailor-application`'s Step 5 revision path instead.
- Never submit, apply, or otherwise take an irreversible action on the
  user's behalf from this command directly — that authority belongs to
  `apply-agent`'s own approval gate, not to this wrapper.
- Never restate `apply-agent.md`'s tier logic, sensitive-field rules, or
  Skyvern package format inline here; load and cite the source file.
