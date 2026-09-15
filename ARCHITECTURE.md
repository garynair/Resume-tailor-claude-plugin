# Resume-Tailor Plugin — Architecture

Reference document for how this plugin is put together: skills, agents,
commands, data flow, and the workflows that tie them together. Written
2026-09-11, **updated 2026-09-15** to reflect the LinkedIn optimization,
Notion sync activation, and apply-mode (Skyvern-assisted) capabilities
added that day. Manifest (`plugin.json`) is at version 0.9.0. This is
documentation, not a spec — the authoritative rules live in
`skills/*/SKILL.md` and `agents/*.md`; this file explains how those
pieces fit together and should be updated (or flagged as stale)
whenever a structural change is made to the plugin, the same way
`resume-format.md`'s baseline is maintained deliberately rather than
left to drift.

**Registration gap (open as of 2026-09-15 — see "Open items"):**
`apply-agent`, `linkedin-optimizer`, `/apply`, and `/linkedin-optimize`
exist as complete files on disk but are **not listed in `plugin.json`**.
A file that isn't in the manifest doesn't load in a real session
regardless of how complete its content is. This document describes the
full designed system, including these four components, and flags each
one's registration status explicitly rather than silently treating
"file exists" as "wired in."

## What this plugin is

A human-in-the-loop resume and cover letter tailoring system, with
company-specific interview prep, a LinkedIn profile-optimization branch,
a Notion tracker sync, and a human-gated apply step (direct fill or a
Skyvern task-package handoff), built as a Claude Code / Cowork plugin.
It is the companion to a separate, existing automated job-application
pipeline; this plugin is used for higher-stakes applications that
warrant gap dialogue and manual QA rather than a fully automated run.
Nothing in this plugin submits an application or edits a public profile
without a per-item, in-session human approval — that boundary is
enforced at the agent level (see "The trust chain" and the apply-mode
workflow below), not left to convention.

Two governing design principles run through everything below:

1. **Accuracy over polish.** Nothing gets written into a resume or
   cover letter unless it traces back to a source document or a direct,
   confirmed answer from the candidate. See "The trust chain," below.
2. **Single source of truth per concept.** Formatting rules live in
   exactly one place (`resume-format.md` / `coverletter-format.md`),
   cross-cutting rules live in exactly one place (`constraints.md`), and
   a given application's state lives in exactly one file (the session
   file). Every agent is a reader/writer against these, not an
   independent source of judgment calls that could drift from them.

## Directory structure

```
Resume-tailor-plugin/
├── .claude-plugin/
│   ├── plugin.json           # skills/agents/commands manifest + version
│   └── marketplace.json      # local marketplace listing + version
├── skills/                   # canonical rule files (see below)
│   ├── constraints/SKILL.md
│   ├── resume-format/SKILL.md
│   ├── coverletter-format/SKILL.md
│   ├── ai-fingerprint-checklist/SKILL.md
│   ├── interview-prep-format/SKILL.md
│   ├── interview-plan-format/SKILL.md
│   └── known-gaps/SKILL.md
├── agents/                   # one .md per agent (13 registered + 2 built, unregistered)
├── commands/                 # one .md per slash command (6 registered + 2 built, unregistered)
├── user-data/                # candidate's own data — excluded from bundle
│   ├── reference/             # profile, master corpus, bundles, corrections
│   ├── raw-corpus/            # old resumes/cover letters, unprocessed
│   ├── applications/          # one session_<company>_<role>.md per JD
│   └── output/                # one <company>_<role>/ folder per JD
└── _backups/                 # timestamped pre-edit snapshots (this session's convention)
```

`excluded_from_bundle: ["user-data/"]` in `plugin.json` means the
candidate's actual data never ships as part of the plugin package —
only the rule files, agents, and commands do.

## Skills (7) — canonical rule files

Every skill lives at `skills/<name>/SKILL.md`. Legacy flat files at
`skills/<name>.md` existed alongside these and were the actual files
every agent prompt referenced until 2026-09-11 (see "Known issues fixed
this session" below) — they are now deprecated stubs, kept only because
they couldn't be deleted from this session, safe to delete manually.

| Skill | Covers |
|---|---|
| `constraints` | Cross-cutting non-negotiable rules: priority hierarchy (Accuracy > Relevance > Impact > ATS > Brevity), name sourcing, no em dashes, bullet-length cap, metric density, metric-context requirement, no repeated metrics, data-conflict protocol, metric-collision tiebreak, cover letter length, no fabrication, American English, pointer to `known-gaps`. |
| `resume-format` | Authoritative resume formatting spec: page setup, typography (Calibri throughout), color, alignment, section header borders, required section order (8 fixed sections), job heading/client-name format, bullet formatting, tagline rule, tiered bullet-count standard, lead-verb rotation + banned lead verbs, Work Authorization line, ATS compatibility, file naming convention. |
| `coverletter-format` | Cover letter structural spec: letterhead, ~200-word body, Re: line, opening/body/closing structure, Company Signal Line sourcing rule, no client names in body, no defensive objection paragraphs. |
| `ai-fingerprint-checklist` | Lexical/structural/formatting heuristics to catch AI-generated-sounding text (stock intensifiers, triplet padding, keyword-dumping, formulaic bullets, uniform bullet lengths, emoji/arrows, curly quotes, etc.). Applied by `critique-agent`. |
| `interview-prep-format` | Contract for the "what happens in the room" document: process overview, audience map, round-by-round breakdown, result-first (Headline → Effect → Rationale → Operations) answer framing. |
| `interview-plan-format` | Contract for the "how do I walk in ready" document: fixed 7-step build order (job analysis → company deep dive → story bank → question bank → the pitch → final polish → room pointer). |
| `known-gaps` | Standing register of claims the candidate is not confident making (e.g., in-progress certifications), paired with pre-approved phrasing for claims that are true but easy to overstate. Hard-constraint tier, same as `constraints`. |

## Agents (13 registered + 2 built, unregistered = 15 total)

| Agent | Registered in `plugin.json`? | Triggered by | Reads | Writes |
|---|---|---|---|---|
| `intake-agent` | Yes | `/setup-profile` | old resumes (suggestions only) | `user-data/reference/user-profile.md` (sole owner) |
| `corpus-builder` | Yes | `/build-reference` (step 1) | `raw-corpus/*`, `user-profile.md`, `corrections-log.md` | `master-resume.md`, `master-metrics-vault.md`, `star-story-bank.md` |
| `template-extractor` | Yes | `/build-reference` (step 1b, parallel to bundle-builder) | `raw-corpus/old-resumes/` | `master-resume.md`'s Structural Observations section (drift report only, never `resume-format.md`) |
| `bundle-builder` | Yes | `/build-reference` (step 2, after corpus-builder) | `master-resume.md`, `master-metrics-vault.md`, `star-story-bank.md` | `user-data/reference/bundles/bundle-<track>.md`, one per target track |
| `job-analyzer` | Yes | `/tailor-application` (step 1) | JD (text/URL), selected bundle, `user-profile.md` | creates the session file; writes JD text, Track Selection, Requirement Mapping (MATCH/PARTIAL/GAP), Gap Dialogue Log |
| `company-research` | Yes | `/tailor-application` (parallel to job-analyzer, once company name known) | live web search | session file's Company Research section (verified-or-omit) |
| `resume-tailor` | Yes | `/tailor-application` (step 2, parallel to coverletter-writer) | session file, selected bundle, `corrections-log.md` | session file's Bullet Plan + JD Coverage Report; `output/<co>_<role>/resume.docx` |
| `coverletter-writer` | Yes | `/tailor-application` (step 2, parallel to resume-tailor) | session file, selected bundle's Cover-Letter Guide, Company Research section | session file's Cover Letter Plan; `output/<co>_<role>/cover_letter.docx` |
| `critique-agent` | Yes | `/tailor-application` (step 3, fresh context, after both drafts complete) | finished resume + cover letter docs, session file, bundles, reference files | session file's Critique Scores section (scored, tiered findings; no drafting authority) |
| `apply-agent` | **No — built 2026-09-15, not in manifest** | `/apply` (also unregistered) | session file's Critique Scores + Requirement Mapping/Gap Dialogue Log, finished `.docx` files, `user-profile.md`, `known-gaps.md`, the live posting page (session's browser tools) | session file's Application Log (tier decision, Skyvern package if Tier 2, submission outcome); no direct submit without per-application user approval |
| `notion-sync-agent` | Yes | invoked by `apply-agent` after an outcome is recorded | `apply-agent`'s handoff (company/role/tier/outcome), session file | scoped field set on the Notion "Job Search Tracker" row (`Status`, `Company`, `Job Description`, `Employment Type`, `Location`, `Date Found`, local file paths, append-only `Notes` line) — see status note below |
| `linkedin-optimizer` | **No — built 2026-09-15, not in manifest** | `/linkedin-optimize` (also unregistered) | `master-resume.md`, `master-metrics-vault.md`, `skills.json`, `certifications.json`, `user-profile.md` | `user-data/output/linkedin/linkedin-optimization-<ISO date>.md` (proposal only — never writes to LinkedIn) |
| `interview-prep` | Yes | `/interview-prep` | session file (if exists) or pasted JD/URL, reference files | `output/<co>_<role>/interview-prep.md` |
| `interview-plan` | Yes | `/interview-plan` | session file (if exists) or pasted JD/URL, reference files, `interview-prep.md` (pointer only) | `output/<co>_<role>/interview-plan.md` |
| `mock-interview` | Yes | direct invocation or `/schedule-mock-interview`'s fired task | `interview-plan.md`'s Question Bank | nothing persisted — live critique loop only |

**`notion-sync-agent` status (revised 2026-09-15):** no longer a stub.
Activation threshold reduced from "5+ real JDs" to **1 real JD passing
Step 7 validation, plus a staging database and a Notion connector with
write/create-page scope** — both still required together.
`user-profile.md`'s `notion_sync` flag was set `true` 2026-09-15, but
the flag alone does not invoke the agent; `apply-agent` invokes it, and
only after the validation pass above is actually confirmed. A read-only
or search-only Notion connection cannot execute these writes even with
the agent unblocked — verify write scope before relying on this
end-to-end.

## Commands (6 registered + 2 built, unregistered = 8 total)

| Command | Registered? | Orchestrates |
|---|---|---|
| `/setup-profile` | Yes | `intake-agent` — first-run entry point, populates `user-profile.md` |
| `/build-reference` | Yes | `corpus-builder` → (`template-extractor` ∥ `bundle-builder`, bundle-builder strictly after corpus-builder) |
| `/tailor-application` | Yes | `job-analyzer` → (`company-research` ∥ gap dialogue) → (`resume-tailor` ∥ `coverletter-writer`) → `critique-agent` (fresh context) → report |
| `/apply` | **No — built 2026-09-15** | thin wrapper → `apply-agent` (precondition check → read posting → Tier 1/2 decision → fill or Skyvern package → approval gate → `notion-sync-agent` handoff) |
| `/linkedin-optimize` | **No — built 2026-09-15** | thin wrapper → `linkedin-optimizer` (one-time/occasional, not per-application) → proposal doc → user approves specific lines before any manual LinkedIn edit |
| `/interview-prep` | Yes | `interview-prep` agent (logistics/process document) |
| `/interview-plan` | Yes | `interview-plan` agent (narrative/readiness document) |
| `/schedule-mock-interview` | Yes | creates scheduled tasks (via the host platform's trigger tool, never an in-session cron) that later fire `mock-interview` |

## The trust chain (why accuracy holds end to end)

```mermaid
flowchart LR
    A[raw-corpus: old resumes/cover letters] --> B[corpus-builder]
    B -->|corroborated 2+ sources| C[master-resume.md / master-metrics-vault.md / star-story-bank.md]
    B -->|single-source| C
    C --> D[bundle-builder]
    D --> E[bundle-&lt;track&gt;.md]
    E --> F[resume-tailor / coverletter-writer]
    G[job-analyzer: gap dialogue] -->|user-confirmed answers| F
    F -->|every claim cites C or G| H[resume.docx / cover_letter.docx]
    H --> I[critique-agent: independent re-verification]
```

Every claim that reaches a final document traces to exactly one of two
places: a corroborated (or user-confirmed) line in the master reference
files, or a direct answer the candidate gave during this session's gap
dialogue. `critique-agent` re-derives this traceability independently,
in a fresh context, rather than trusting the drafting agents' self-report
— this is what catches a sourcing failure that "looked fine" mid-draft.

## Core workflow: `/tailor-application`

```mermaid
flowchart TD
    S1[Step 1: Precondition check<br/>user-profile.md + master-resume.md populated] --> S2
    S2[Step 2: job-analyzer<br/>resolve JD, create session file,<br/>track selection, Requirement Mapping,<br/>Gap Dialogue Log] --> S3a
    S2 -.parallel, once company known.-> CR[company-research<br/>writes Company Research section]
    S3a[Step 3: resume-tailor] -.parallel.- S3b[Step 3: coverletter-writer]
    CR --> S3b
    S3a --> S4
    S3b --> S4
    S4[Step 4: critique-agent<br/>fresh context, independent re-verification] --> S5
    S5[Step 5: Report<br/>score + Tier 1 findings first,<br/>then JD Coverage Report,<br/>then session file + output folder]
```

Every run produces, in `user-data/applications/session_<company>_<role>.md`:
Job Description, Company Research, Track Selection, Requirement Mapping,
Gap Dialogue Log, Bullet Plan, JD Coverage Report, Cover Letter Plan,
Critique Scores, and — once `/apply` runs against it — an Application
Log. This session file is the single source of truth for that
application — every agent after `job-analyzer` reads from it rather
than re-deriving state, and the final report to the candidate is built
from it, not reconstructed from memory.

## Apply-mode workflow: `/apply` (built 2026-09-15, unregistered)

Picks up exactly where `/tailor-application` leaves off — it will not
run against a session file with unresolved Tier 1 critique findings.

```mermaid
flowchart TD
    P[Precondition: session file has clean<br/>Critique Scores, resume.docx + cover_letter.docx<br/>are the post-critique versions] --> R
    R[Step 2: Read the live posting<br/>identify ATS platform + step count] --> T
    T{Step 3: Tier decision}
    T -->|Tier 1: single-page,<br/>reliable form e.g. LinkedIn Easy Apply,<br/>simple Greenhouse| F1
    T -->|Tier 2: multi-step Workday/iCIMS,<br/>or any brittle form<br/>incl. a failed Tier-1 attempt| SK
    F1[Step 4: Direct field-fill<br/>session-sourced answers only;<br/>sensitive fields stop and ask<br/>unless known-gaps.md pre-approves] --> APR
    APR{User approval<br/>on THIS application}
    APR -->|approved| SUB[Submit]
    APR -->|not approved| STOP1[Stop — no submit]
    SK[Step 5: Build Skyvern task package<br/>job URL, resume file, custom_prompt,<br/>other_information — written to<br/>session file's Application Log]
    SK --> UAPR{User reviews package,<br/>then starts the Skyvern run<br/>THEMSELVES}
    UAPR -->|user starts run outside this session| OUT2[User reports outcome back]
    SUB --> LOG
    OUT2 --> LOG
    LOG[Step 6: Record outcome in<br/>Application Log — status, timestamp, tier] --> NS
    NS[notion-sync-agent<br/>writes scoped fields to<br/>Job Search Tracker]
```

**Skyvern is never called directly by this plugin.** There is no live
Skyvern connector in any session this agent runs in. Tier 2 produces a
task package matching Skyvern's own "Job Application Recipe" schema
(dashboard: Recipes → Job Applications; API: `POST
/v1/recipes/jobs/apply`) and stops — the user starts the actual Skyvern
run themselves, outside this session, because **Skyvern's own "Apply"
run submits autonomously once started, with no second confirmation
checkpoint inside Skyvern itself.** The approval gate this plugin
enforces happens on the package, before that run starts, not mid-run.
`apply-agent` only learns the outcome when the user reports it back.

This is also why `apply-agent`/`/apply` are documented here as the
completed design even though unregistered: "apply mode" in this plugin
was never meant to mean autonomous submission — it means a human-gated
direct fill for simple forms, or a human-started, human-reported
Skyvern handoff for complex ones. That boundary exists in the agent's
guardrails regardless of registration status.

## LinkedIn optimization workflow: `/linkedin-optimize` (built 2026-09-15, unregistered)

A one-time/occasional branch, not part of the per-application cycle —
it does not read or write a session file and can run independently of
`/tailor-application`.

```mermaid
flowchart TD
    LP[Precondition: user-profile.md has Name +<br/>Target tracks; master-resume.md has content] --> LO
    LO[linkedin-optimizer:<br/>Headline → About → Experience entries →<br/>Skills → Featured, each cited to<br/>master-resume.md / master-metrics-vault.md /<br/>skills.json / certifications.json] --> AI
    AI[ai-fingerprint-checklist run<br/>against full proposal] --> DOC
    DOC[Write linkedin-optimization-&lt;date&gt;.md<br/>proposal document] --> UAP
    UAP{User approves<br/>specific sections/lines}
    UAP -->|approved lines| MANUAL[User applies the edit on<br/>LinkedIn themselves —<br/>agent never writes to LinkedIn]
    UAP -->|not approved| STOP2[No change made]
```

**Why resume-tailor and coverletter-writer run in parallel, not
sequentially:** both depend only on `job-analyzer`'s finished output,
not on each other — they coordinate metric-overlap (no repeated numbers
across the two documents) through the shared session file's Bullet Plan,
which coverletter-writer checks before finalizing, rather than through a
blocking handoff between the two agents.

**Why critique-agent must run in a fresh context:** the fresh read is
what catches things the drafting agents rationalized away mid-session —
this was a real, observed catch (a continuous-monitoring requirement
that both agents' self-reports missed) documented in this session's own
test run, not a theoretical concern.

## Reference-building workflow: `/build-reference`

```mermaid
flowchart TD
    P[Precondition: user-profile.md has Name + Target tracks] --> C
    C[corpus-builder<br/>parse raw-corpus, corroborate/dedupe/flag conflicts] --> Bb
    C --> TE
    TE[template-extractor<br/>structural drift report only, parallel to bundle-builder]
    Bb[bundle-builder<br/>one bundle-&lt;track&gt;.md per target track] --> Sum
    TE --> Sum
    Sum[Summary: sources consolidated, conflicts needing resolution,<br/>tracks built vs skipped, structural drift flagged]
```

`corpus-builder` has a cold-start fallback: if the raw corpus is empty or
unparseable, it runs a structured interview per target track instead of
fabricating content.

## Interview-readiness workflow

`/interview-prep` and `/interview-plan` are companions to the tailoring
pipeline, not part of it — neither writes to `user-data/reference/` or
produces a resume/cover letter, and both can run from a pasted JD/URL
even with no prior `/tailor-application` session for that company/role.

- `/interview-prep` → **logistics**: process overview, audience map,
  round-by-round breakdown ("what will happen in the room").
- `/interview-plan` → **narrative**: job analysis, company deep dive,
  story bank, question bank, the pitch, final polish ("what the
  candidate actually says"). Must run its 7 steps in fixed order —
  company research has to land before the pitch is drafted.
- `/schedule-mock-interview` → creates scheduled tasks (T-7d cold run,
  T-1/2d targeted run) that later fire `mock-interview` against the
  interview-plan's Question Bank. Uses the host platform's real
  scheduled-task tool, never an in-session cron (which would silently
  die when the session ends).
- `mock-interview` → live text-based Q&A and critique loop against
  Headline → Effect → Rationale → Operations; critiques delivery, never
  introduces new facts.

## Version and cache discipline

`plugin.json` and `marketplace.json` each carry their own `version`
field; they must be bumped **together, in the same change**, whenever
any agent, command, or skill file's content changes. A version left
unbumped (or the two files drifting out of sync with each other) is the
known failure mode that causes an edited file's content to not actually
take effect in a running/reinstalled session — observed directly in this
session before this document existed. After any content change:

1. Bump both `plugin.json`'s and `marketplace.json`'s `version` in the
   same commit.
2. `git push` — **required for the Cowork desktop app to see the
   change at all** (it reads from the GitHub remote), even though it
   isn't required for the WSL CLI (which reads local disk directly).
   See the confirmed facts below; skipping this step is the single most
   likely reason a version mismatch keeps recurring.
3. Uninstall and reinstall the plugin in *each* environment actually
   used (WSL CLI and/or the Cowork desktop app), then confirm the
   version shown in each before trusting a live run — the two have been
   observed to disagree even when both are technically "up to date"
   relative to their own source.
4. Start a fresh session before the next real run; reusing an
   already-open session has been observed to keep serving stale
   plugin content even after a correct reinstall.

**Confirmed 2026-09-11 — the CLI environment and git facts, resolved
end to end:**

- The `claude` CLI runs inside **WSL**, not native Windows. The plugin
  folder is `D:\CLAUDE\Resume-Tailor-Claude\Resume-tailor-plugin` from
  Windows, and `/mnt/d/CLAUDE/Resume-Tailor-Claude/Resume-tailor-plugin`
  from inside WSL — use the WSL path for every `claude plugin` command.
- `git status`/`add`/`commit` from a plain Windows PowerShell prompt at
  `D:\CLAUDE\Resume-Tailor-Claude` (the parent folder) fails with "not a
  git repository" — that's not the repo root. The actual repo root is
  `Resume-tailor-plugin` itself, one level down, with a real GitHub
  remote: `https://github.com/garynair/Resume-tailor-claude-plugin`
  (**public**).
- `claude plugin marketplace add` takes the **plugin repo root**, not
  the `.claude-plugin` subfolder — it appends `.claude-plugin/
  marketplace.json` itself. Passing the subfolder directly produces a
  "marketplace file not found" error with a doubled path.
- **Two separate install paths exist, reading from two different
  sources — this is the actual root cause of every "version doesn't
  match" symptom seen across this session and before it.** The WSL
  `claude` CLI resolves the local marketplace straight from the
  filesystem: committing locally (no push) and running `claude plugin
  uninstall` → `marketplace add` → `install` → `claude plugin list`
  correctly showed the new version with nothing pushed to GitHub. The
  **Cowork desktop app's Plugins screen instead reads from the GitHub
  remote** — verified directly: after a local-only commit it kept
  showing the stale pre-session version (`0.2.0`) even after removing
  and re-adding the plugin there; only after `git push` did removing
  and re-adding the plugin in the desktop app pick up the current
  version. **Operating rule: `git push` is required before the desktop
  app will see a change, even though the WSL CLI never needed it.**
  Always push as part of the standard version-bump/reinstall sequence
  above, not as an optional afterthought, if the desktop app is used at
  all alongside WSL.

## Known issues fixed 2026-09-11 (see `_backups/` for full diffs)

- **Legacy flat-file bug**: every agent/command prompt referenced
  `skills/<name>.md` (a legacy duplicate file), never the canonical,
  plugin.json-registered `skills/<name>/SKILL.md`. Fixed by repointing
  all 11 affected files; the 4 legacy files are now deprecation stubs.
- **job-analyzer over-triggering gap dialogue** on closely-adjacent,
  already-corroborated capabilities (e.g., asset-inventory tooling vs.
  M&A-specific integration work) — added an explicit "closely-adjacent
  capabilities are MATCH, not PARTIAL" rule.
- **Redundant manual cross-check** in `tailor-application.md`'s Step 3
  duplicated `critique-agent`'s existing Tier 1 duplicate-metric check —
  removed; critique-agent's independent pass is the single enforcement
  point now.
- **Style/voice rules added**: metric-context requirement (no bare
  percentages), banned lead verbs (Orchestrated, Leveraged, Facilitated,
  Championed), expanded approved lead-verb rotation, American English
  spelling standard, keyword-dumping heuristic.
- **`known-gaps` privacy split**: the skill previously stated the
  candidate's real certification status directly, which is a problem
  once the repo was confirmed public (see below). Split into a public
  `skills/known-gaps/SKILL.md` (mechanism only, no personal specifics)
  and a private `user-data/reference/known-gaps.md` (sibling directory,
  gitignored, holds the actual facts).
- **Public GitHub remote confirmed** on the `Resume-tailor-plugin` repo
  (`github.com/garynair/Resume-tailor-claude-plugin`, public). Anything
  committed here is visible to anyone; `user-data/` was confirmed never
  committed (`git log -- user-data/` returned nothing) and is gitignored
  going forward, same treatment now extended to the `known-gaps` split
  above.

## Open items

- **Registration gap (top priority):** `apply-agent`, `linkedin-optimizer`,
  `/apply`, and `/linkedin-optimize` are complete, guardrail-reviewed
  files dated 2026-09-15 but are **absent from `plugin.json`'s `agents`
  and `commands` arrays**. Until added there (and the version bumped per
  the discipline below, then pushed and reinstalled), neither command
  will actually load in a real session — "the file exists" is not "the
  feature works." This is the concrete next step to close out the
  LinkedIn / apply-mode story described in this document.
- **`marketplace.json`'s top-level `source` field** is literally the
  string `"..."`, not a real path — predates 2026-09-11's changes and
  still unexplained. Worth checking directly if the marketplace ever
  fails to resolve: a bare `"..."` may be a placeholder that was never
  filled in. Not blocking — the plugin's own `"source": "./"` entry
  resolves correctly regardless.
- **Notion sync** is no longer disconnected — see the status note under
  "Agents" above. Remaining open sub-item: a normalized job fingerprint
  (company + title + posting URL) shared between this agent and the
  separate axionsec-job-engine pipeline, so both check for an existing
  row before creating one; not yet built. Also blocked, separately: the
  Google Drive `Resume Link`/`Cover Letter Link` auto-upload feature
  (no file-content upload tool available in this session's Drive
  connector, plus an unresolved account-ownership mismatch between
  `pgvb14@gmail.com` and `garynair@gmail.com`) — parked, not a coding
  task until both are resolved.
- **Skyvern remains package-only by design**, not a temporary
  limitation — see the apply-mode workflow above. There is no plan to
  add a live Skyvern connector call from inside this plugin; the human
  start/report step is the intended control, not a placeholder for
  future automation.
- **Legacy flat skill files** (`constraints.md`, `resume-format.md`,
  `coverletter-format.md`, `ai-fingerprint-checklist.md`) are harmless
  deprecation stubs but not deleted — `device_bash` could not reach the
  repo folder this session (a Windows-update-related mount issue).
  Delete manually once convenient; nothing references them anymore.
