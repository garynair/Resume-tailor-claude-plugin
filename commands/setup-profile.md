---
command: /setup-profile
status: active
---

# /setup-profile

Entry point for populating (or re-populating) the candidate's standing
profile. This is the plugin's `first_run` command; a fresh install should
prompt the user toward this before anything else can produce useful
output, since every downstream agent trusts `user-profile.md` without
re-verifying it.

## Flow

1. Check whether `user-data/reference/user-profile.md` already exists and
   has non-empty values in every field (see the structure in
   `agents/intake-agent.md`'s Output section).
   - If it's fully populated, tell the user what's currently on file
     (name, target tracks, notion_sync setting) and confirm they want to
     replace it before proceeding, since `intake-agent` overwrites the
     whole file rather than patching individual fields.
   - If it's missing, empty, or only partially populated, proceed
     directly.
2. Invoke `intake-agent` to run the full field-collection flow described
   in `agents/intake-agent.md` (name, phone, email, LinkedIn, location,
   target tracks ranked, certifications, standing formatting rules,
   notion_sync yes/no).
3. `intake-agent` writes the result to `user-data/reference/user-profile.md`.
   This command does not write to that file itself; all writing
   authority stays with `intake-agent` per its guardrails.
4. After `intake-agent` reports completion, read the file back and show
   the user a short confirmation summary (name, ranked tracks,
   notion_sync setting) so they can catch a typo immediately rather than
   discovering it mid-application later.

## When this command should be suggested proactively

- On the very first interaction with this plugin (per `first_run` in
  `plugin.json`).
- If any other command (`/build-reference`, `/tailor-application`)
  detects a missing or incomplete `user-profile.md` mid-flow, it should
  stop and point the user back to `/setup-profile` rather than
  proceeding with guessed or blank values.

## Guardrails

- Never collect or write profile fields directly in this command; always
  delegate to `intake-agent`, which owns the sourcing rules (e.g.,
  treating old-resume values as suggestions requiring confirmation, not
  auto-fill) and is the only agent permitted to write
  `user-data/reference/user-profile.md`.
- Re-running this command replaces the entire profile file, not just the
  fields the user wants to change; say so before re-running if the user
  only mentioned wanting to update one field.
