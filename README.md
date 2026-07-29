# Resume-Tailor Plugin

A human-in-the-loop resume and cover letter tailoring system for
[Claude Code](https://claude.com/product/claude-code) and
[Cowork](https://claude.com/product/claude-cowork). It consolidates a
candidate's history of past resumes and cover letters into a sourced
reference layer, then drafts job-description-tailored resumes and cover
letters against that reference layer with strict sourcing rules, gap
dialogue for anything unconfirmed, and a mandatory fresh-context
critique pass before anything ships.

This is a companion to a fully automated job-application pipeline, not
a replacement for one. It exists for higher-stakes applications where
you want a human (or a second, independent AI pass) to verify every
claim before it goes out.

## What it does

- Turns a folder of old resumes and cover letters into one deduplicated,
  fully-sourced reference file (`master-resume.md`), a metrics vault of
  only multiply-corroborated figures, and a bank of STAR-shaped stories.
- Builds per-track bundles (e.g. GRC, TPRM, SecOps, IAM, PM) so a
  tailoring pass can pull the right material for a given job's domain.
- For a specific job posting: analyzes the JD, maps it against the
  reference layer, runs gap dialogue for anything not yet sourced, then
  drafts a resume and cover letter in parallel.
- Runs every draft through a fresh-context critique agent (a separate
  agent instance with no memory of the drafting session's reasoning)
  before finalizing, and produces a standing confirmation of every
  exclusion, metric swap, and formatting decision made along the way.
- Enforces a strict no-fabrication policy: every bullet must trace to a
  specific line in the reference layer or a confirmed answer from this
  session's gap dialogue. Nothing is invented to fill a JD requirement.

## Install

Package this folder as a `.plugin` file (zip it, keeping
`.claude-plugin/plugin.json` at the root) and install it via
Customize → Plugins → Upload plugin in Cowork, or Claude Code's plugin
install flow. See `plugin.json` for the full manifest.

## First-time setup

1. `/setup-profile` — collects your name, contact info, target job
   tracks (ranked), certifications, and standing formatting preferences
   into `user-data/reference/user-profile.md`.
2. Drop your old resumes and cover letters into
   `user-data/raw-corpus/old-resumes/` and
   `user-data/raw-corpus/old-coverletters/` (`.docx` or `.pdf`).
3. `/build-reference` — consolidates that corpus into the reference
   layer (`master-resume.md`, `master-metrics-vault.md`,
   `star-story-bank.md`) and builds one bundle per target track. If the
   corpus is empty, this falls back to a structured interview instead
   of fabricating a history.

## Tailoring an application

`/tailor-application` — paste in a job description. This runs track
selection, requirement mapping, gap dialogue for anything unsourced,
parallel resume/cover-letter drafting, and a fresh-context critique
pass, then writes the final `.docx` files to
`user-data/output/<company>_<role>/`.

## Structure

```
skills/            Cross-cutting rules: constraints, resume format,
                    cover letter format, AI-fingerprint checklist.
agents/             intake, corpus-builder, template-extractor,
                    bundle-builder, job-analyzer, company-research,
                    resume-tailor, coverletter-writer, critique-agent,
                    notion-sync-agent (stub).
commands/           /setup-profile, /build-reference,
                    /tailor-application.
user-data/          Your personal data (profile, reference layer, raw
                    corpus, past applications). Never bundled into the
                    plugin package; excluded via plugin.json.
```

## Design principles

- **Accuracy over everything.** The priority hierarchy is
  Accuracy > Relevance > Impact > ATS > Brevity (`skills/constraints.md`),
  and no bullet ships without a traceable source.
- **Formatting is fixed, content is not.** `skills/resume-format.md` is
  the authoritative structural baseline; `resume-tailor` only makes
  content decisions (which bullets, which skills to surface) inside
  that fixed structure, never per-application formatting improvisation.
- **Conflicts get resolved by evidence, not guessed.** Contradictory
  figures across source documents are resolved by source-corroboration
  count (`skills/constraints.md`'s data-conflict protocol), logged in
  `corrections-log.md`, never silently picked.
- **Nothing ships unchecked.** Every run ends with a fresh-context
  critique pass and an automatic standing confirmation of every
  exclusion, metric decision, and formatting deviation made.
