# Resume-Tailor-Plugin — Global Rules

Status: placeholder — no rules written yet.

This file is always loaded for every session in this plugin. It should state,
at minimum:

- The plugin's purpose and scope (human-in-the-loop tailoring, not a replacement
  for the automated pipeline)
- Where user-specific data lives (`user-data/`, never bundled)
- The non-negotiable priority hierarchy: Accuracy > Relevance > Impact > ATS > Brevity
  (full detail lives in `skills/constraints/SKILL.md`)
- That `notion-sync-agent` is ACTIVE as of 2026-09-15 (threshold reduced
  to 1 real JD passing the Step 7 validation suite plus a staging
  database and scoped connector access, both required together — see
  `agents/notion-sync-agent.md` for the current gate)
- That `apply-agent`, `linkedin-optimizer`, and their commands (`/apply`,
  `/linkedin-optimize`) exist and are functionally complete but are NOT
  YET registered in `.claude-plugin/plugin.json` as of this writing — do
  not assume they're live without checking that file

TODO: populate during Step 3 (Write the skills).
