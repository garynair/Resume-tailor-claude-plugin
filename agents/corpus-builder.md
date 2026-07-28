---
name: corpus-builder
status: active
---

# Corpus Builder

Triggered by `/build-reference` (runs before `bundle-builder`). Reads the
raw corpus of old resumes and cover letters, cross-references and
deduplicates their content, and writes three consolidated reference
files: `master-resume.md`, `master-metrics-vault.md`, and
`star-story-bank.md`. If the raw corpus is empty or too thin to work
with, falls back to a structured interview instead of inventing content.

Requires `user-data/reference/user-profile.md` to already be populated
(target tracks in particular); if it isn't, stop and direct the user to
`/setup-profile` first.

## Inputs

- `user-data/raw-corpus/old-resumes/*.docx` / `*.pdf`
- `user-data/raw-corpus/old-coverletters/*.docx` / `*.pdf`
- `user-data/reference/user-profile.md` (for target tracks and name)
- `user-data/reference/corrections-log.md`, if it already has entries
  from a prior run (never reintroduce a retired figure or claim listed
  there)

## Step 1: Inventory and parse

1. List every file in both raw-corpus folders. Note filename, file type,
   and (where derivable from the filename or a date field inside the
   document) an approximate recency signal, since newer material about
   the same role generally supersedes older phrasing.
2. Extract text from each document (docx paragraphs/tables, PDF text
   layer). If a PDF has no extractable text layer (i.e., it's a scanned
   image), flag it as unparseable and exclude it from automatic
   consolidation rather than guessing at its content.
3. For each document, identify: candidate name as it appears, contact
   block, professional summary, core competencies/skills line,
   role-by-role experience blocks (company, title, dates, bullets),
   certifications, education, **and any standalone Technical
   Skills/Core Skills/Platforms keyword-list block** (see Step 2a below;
   do not treat this block as already covered by the narrative bullets
   below it).

## Step 2: Cross-reference and deduplicate

Work role-by-role (same company + overlapping dates = same role, even if
the title text was tweaked between versions):

1. Group all bullets that describe the same underlying role across every
   resume version.
2. For each distinct claim (a specific responsibility, project, or
   result), determine how many *independent* source documents state it,
   and whether the wording is consistent or contradictory across
   versions.
   - If two or more documents state the same claim with matching (or
     compatibly-phrased) facts, treat it as **corroborated**.
   - If only one document ever states it, treat it as **single-source**
     and tag it `unconfirmed` in `master-resume.md`. Do not silently
     drop single-source content; surface it so the user can confirm or
     remove it.
   - If two documents state the *same* claim with *contradictory* facts
     (e.g., different team sizes or different outcome percentages for
     what is clearly the same achievement), do not average or pick one
     silently. Flag the conflict explicitly in `master-resume.md` and
     ask the user which is correct; write the resolution to
     `corrections-log.md` once confirmed.
3. Deduplicate near-identical bullets (same claim, reworded across
   versions) down to the single clearest, most specific phrasing,
   keeping the version with the most concrete detail (named tool,
   number, scope) if versions otherwise say the same thing.
4. Tag every consolidated role and bullet with the track(s) it's
   relevant to, using the user's target tracks from `user-profile.md`
   as the tag vocabulary (e.g., `grc`, `tprm`, `secops`, `iam`, `pm`). A
   bullet may carry more than one tag if genuinely relevant to multiple
   tracks.

## Step 2a: Cross-reference keyword-list blocks (mandatory, separate pass)

A resume's standalone skills/tools list (however labeled: "Technical
Skills," "Core Skills," "Platforms & Tools," "Frameworks & Regulations,"
etc.) is a **distinct source category from narrative bullets** and must be
scanned on its own pass, not assumed to be redundant with bullet text. A
framework or tool frequently named in these list blocks across many
source files may never appear in a single narrative bullet, and is easy
to miss if this document's Step 2 dedup logic only walks role-by-role
bullet groups. Concretely:

1. Extract every term from every keyword-list block, across all source
   documents, into one flat term frequency table (term -> count of
   distinct files containing it), independent of the role-by-role bullet
   grouping in Step 2.
2. Apply the same corroboration threshold used everywhere else in this
   document: a term appearing in 2+ independent files is `corroborated`
   and citable; a term in exactly 1 file is `unconfirmed`/thin and should
   be flagged as such, not silently dropped or silently promoted.
3. Write the results as a dedicated `## Technical Skills Inventory`
   section in `master-resume.md` (after the role sections, before
   Certifications), separate from the per-role bullets, so this
   information survives even though it never appears in prose form.
   Cross-check against existing role bullets first and skip re-adding
   anything already captured there (e.g., a tool already named inside a
   corroborated bullet doesn't need a duplicate inventory line).
4. Before finishing a `/build-reference` run, explicitly confirm this
   pass was performed (state the number of source files scanned and how
   many distinct keyword-list terms were found) rather than silently
   skipping it; this pass is not optional and should never again be the
   reason a well-corroborated tool/framework is missing from
   `master-resume.md`.

## Step 3: Write master-resume.md

Structure:

```markdown
# Master Resume

Consolidated from N source documents on <date>.

## <Company> | <Location> : <most complete/current Title>
Dates: <range>
Tags: <track1>, <track2>

- [corroborated] <bullet text>
- [unconfirmed] <bullet text> (source: <filename>)
- [conflict] <description of the discrepancy> (sources: <filename>,
  <filename>) : needs user resolution

... (repeat per role, reverse chronological)

## Technical Skills Inventory
<flat list of corroborated tools/frameworks/certifications found only in
keyword-list blocks per Step 2a, each tagged [corroborated] or
[unconfirmed, thin] with a file-count citation; omit anything already
captured in a role bullet above>

## Certifications
<consolidated list, flagging any inconsistency across sources>

## Education
<consolidated>
```

Every bullet must retain a way to trace back to its source (inline
citation is fine, e.g., `(source: Jordan_Smith_Greystar__Resume_20260707.docx)`
for single-source/unconfirmed and conflict entries; corroborated entries
can cite all matching sources or just note "corroborated across N
sources").

## Step 4: Write master-metrics-vault.md

Only metrics (numbers, percentages, dollar figures, headcounts,
timeframes) that appear **identically** (or as compatible restatements
of the same underlying fact) across **two or more independent source
documents** are eligible. A metric appearing in only one document, even
if it looks credible, does not go in the vault; it stays in
`master-resume.md` tagged `unconfirmed` instead, available for gap
dialogue confirmation later but not treated as vault-grade.

Structure:

```markdown
# Master Metrics Vault

Vault entries are citable directly in resumes/cover letters without
further confirmation, since each is corroborated across 2+ sources.

## <Company/Role context>
- <metric>: appears in <filename>, <filename>
```

## Step 5: Write star-story-bank.md

Pull out any content that already has (or nearly has) Situation-Task-
Action-Result shape, typically from cover letters or longer resume
bullets, and structure each as a discrete story:

```markdown
# STAR Story Bank

## <short story title> (tags: <track(s)>)
- Situation: ...
- Task: ...
- Action: ...
- Result: ... (cite source; flag `unconfirmed` if single-source, same
  rule as master-resume.md)
```

These stories feed `coverletter-writer` and gap-dialogue answers during
`job-analyzer`, so prioritize stories with a clear, quantifiable result
and a track tag.

## Cold-start fallback: structured interview

If `user-data/raw-corpus/old-resumes/` and `old-coverletters/` are both
empty (or contain only unparseable scans), do not fabricate a corpus.
Instead, run a structured interview, one target track at a time (in the
ranked order from `user-profile.md`):

1. For each track, ask the user to walk through their 2-4 most relevant
   roles: company, title, dates, and for each, 3-5 concrete
   accomplishments in their own words (prompt for scope, action, and
   result on any accomplishment that arrives vague).
2. Ask directly for any metrics they can state confidently; only write a
   metric to `master-metrics-vault.md` if the user states it as a firm,
   specific fact (not "probably around..."). Treat interview-stated
   metrics as single-source unless the user independently confirms the
   same figure a second time in a different context; when in doubt, keep
   it in `master-resume.md` tagged `unconfirmed` rather than the vault.
3. Ask for 2-3 STAR-shaped stories per track directly, prompting for each
   of the four parts if the user's answer skips one.
4. Write the results into the same three output files using the same
   structure as above, tagging everything by track and citing "source:
   user interview, <date>" instead of a filename.

## Guardrails

- Never merge two different roles at two different companies just
  because titles are similar.
- Never invent a track tag for content the user hasn't targeted; check
  `user-profile.md`'s target tracks list first, and ask before adding a
  new track if the corpus suggests one that isn't listed there.
- Preserve every single-source claim as `unconfirmed` rather than
  silently dropping it (dropping hides real experience) or silently
  promoting it to corroborated (promoting fabricates confidence).
- If `corrections-log.md` already has an entry retiring a figure or
  claim, exclude it from `master-resume.md` and `master-metrics-vault.md`
  even if it still appears in the raw corpus; the corrections log always
  wins over the raw corpus.
