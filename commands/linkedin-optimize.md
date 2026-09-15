---
command: /linkedin-optimize
status: active (created 2026-09-15)
---

# /linkedin-optimize

One-time/occasional command (not run per-application) that produces a
LinkedIn profile optimization proposal: headline, About, Experience,
Skills, and Featured recommendations, all sourced from the same
reference files `/tailor-application` uses. Never updates LinkedIn
itself.

## Step 1: Precondition check

Same check as `/tailor-application`: `user-data/reference/user-profile.md`
has `Name:` and `Target tracks (ranked):` populated, and
`user-data/reference/master-resume.md` has content. If either is
missing, stop and direct the user to `/setup-profile` and/or
`/build-reference` first.

## Step 2: Invoke linkedin-optimizer

Run `agents/linkedin-optimizer.md` end to end. It reads the target-track
ranking, `master-resume.md`, `master-metrics-vault.md`, `skills.json`,
and `certifications.json`, and writes
`user-data/output/linkedin/linkedin-optimization-<ISO date>.md`.

## Step 3: Present for approval

Show the user the proposal document location and a short summary of
what's proposed per section. Ask which specific sections or lines to
approve — do not treat a general "looks good" as blanket approval for
every proposed change, per `linkedin-optimizer.md`'s own guardrails.

This command does not perform the LinkedIn update itself, regardless of
whether a LinkedIn-capable connector is present in the session; the user
applies approved changes themselves (or explicitly directs an update in
a later, separate instruction).

## Guardrails

- Never skip the precondition check.
- Never treat this as a per-application step — it belongs in the
  one-time/occasional tier alongside `/setup-profile`, not in the
  `/tailor-application` → `apply-agent` → `notion-sync-agent` per-job
  cycle.
- Never let `linkedin-optimizer` write to LinkedIn directly.
