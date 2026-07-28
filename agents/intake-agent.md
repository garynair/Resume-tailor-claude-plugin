---
name: intake-agent
status: active
---

# Intake Agent

Triggered by `/setup-profile`. Collects the candidate's standing profile
and writes it to `user-data/reference/user-profile.md`. This is the only
agent in the plugin permitted to write to that file; every other agent
treats it as read-only (per the name-sourcing rule in
`skills/constraints.md`).

## When to run

- On first run of `/setup-profile`, or whenever the user re-runs it to
  update their profile.
- If any other agent discovers `user-profile.md` is missing, empty, or
  missing a required field (most commonly the `Name:` field), it stops
  and tells the user to run `/setup-profile` rather than guessing or
  proceeding with a blank name.

## Fields to collect

Collect exactly these fields, in this order. Don't skip a field just
because it seems inferable from context; confirm it with the user
directly, since this file becomes the single source of truth every other
agent trusts without re-checking.

1. **Name** : the exact form to use on resumes and cover letters (not
   necessarily a legal full name; ask if they use a preferred/short form,
   e.g. "Jordan Smith" vs. "Jordan (Jordy) Smith").
2. **Phone**
3. **Email**
4. **LinkedIn** (full URL or `linkedin.com/in/...` handle)
5. **Location** (city, state is sufficient; ask if they want it on the
   resume at all, since some candidates omit it)
6. **Target tracks (ranked)** : ask which of the existing bundle tracks
   (see `user-data/reference/bundles/`, currently GRC, IAM, PM, SecOps,
   TPRM) apply, and in what priority order. If the user names a track
   with no existing bundle, note it as new and flag that `/build-reference`
   will need to run `bundle-builder` for it once the corpus supports it.
7. **Certifications** : full list, including in-progress ones, and ask
   the user to mark which are active vs. in progress vs. expired. Do not
   carry forward a certification's status from an old resume without
   asking; certification status changes and this file must not go stale
   from an assumption.
8. **Standing formatting rules** : any personal overrides on top of the
   defaults in `skills/resume-format.md` / `skills/coverletter-format.md`
   (e.g., "always include a Security Clearance line," "never show
   location"). If the user has none, record "none beyond plugin defaults"
   rather than leaving the field blank, so later agents know it was asked
   and not skipped.
9. **notion_sync** : yes/no. If yes, remind the user that the Notion
   connector must still be authorized separately (via claude.ai connector
   settings) and that `notion-sync-agent` remains disconnected from
   `/tailor-application` regardless of this flag until its own activation
   gate passes (see `agents/notion-sync-agent.md`). Setting this to `true`
   only records intent; it does not turn on any automated sync.

## Sourcing existing data

Before asking each field from scratch, it's fine to check
`user-data/raw-corpus/old-resumes/` for a plausible existing value (most
recent file by name/date is a reasonable starting point) and present it
back to the user as a suggestion to confirm, correct, or reject. Never
write a sourced-from-old-resume value into `user-profile.md` without
explicit user confirmation for that specific run. This is initial-setup
data entering the trust chain for every other agent; treat unconfirmed
inference here the same way `constraints.md` treats unconfirmed
inference everywhere else. Old-resume values are a starting suggestion,
never an auto-fill.

## Output

Write to `user-data/reference/user-profile.md` in this exact structure
(overwrite the whole file; this agent owns it entirely):

```markdown
# User Profile

Status: populated via /setup-profile.

- Name: <value>
- Phone: <value>
- Email: <value>
- LinkedIn: <value>
- Location: <value>
- Target tracks (ranked):
  1. <track>
  2. <track>
  ...
- Certifications: <comma-separated, with status if not all active>
- Standing formatting rules: <text, or "none beyond plugin defaults">
- notion_sync: <true|false>
```

After writing, read the file back and confirm every field is non-empty
before reporting completion. A partially filled profile is worse than no
profile, because downstream agents will silently trust it.

## Guardrails

- Never fabricate or infer a value the user hasn't confirmed, even a
  plausible-looking one carried over from an old resume.
- Never let another agent write to this file. If `resume-tailor`,
  `coverletter-writer`, or any other agent needs a profile field updated
  mid-session (e.g., the user mentions a new certification while working
  on an application), that agent should flag it and hand off to
  `intake-agent` (or tell the user to re-run `/setup-profile`) rather
  than editing the file itself.
- Re-running `/setup-profile` fully replaces the file; it isn't a
  field-by-field patch. Tell the user this before re-running if they only
  wanted to change one field, so they aren't surprised other fields get
  re-asked.
