# Resume Format

Authoritative formatting spec, rewritten cleanly on 2026-07-27 from a
full round of explicit, confirmed decisions. This is the fixed
structural baseline for every resume this plugin produces going
forward: `resume-tailor` treats every rule below as fixed and only
makes content decisions (which bullets, which skills) within it. It
never re-derives, improvises, or substitutes formatting per
application. Formatting changes only happen through an explicit user
instruction that edits this file directly; `template-extractor` may
flag structural drift observed in newly added source resumes, but never
auto-applies a change here (see `agents/template-extractor.md`).

**2026-07-29 drift review**: `template-extractor` flagged that several
recent source resumes had drifted to justified body text, client names
on the company/location line instead of a dedicated line, and Education
before Certifications. Candidate reviewed all three and confirmed the
existing spec below is correct on all counts (left-aligned, dedicated
italic client-name line, Certifications before Education); the drifted
recent resumes were the ones out of spec, not this file. No rule changed.
Do not re-flag these three as open drift on future `/build-reference`
runs unless a new pattern emerges beyond what was already reviewed here.

## Page setup

- Margins: 0.75" on all four sides.
- Paper size: Letter (8.5" x 11"), single column.
- Max length: 2 pages, full stop. Never a third page. If content doesn't
  fit at the specified font/size, cut content (per the Brevity rule in
  `constraints.md`, and only after the Bullet completeness check below
  confirms the cut is warranted); don't shrink the font past the floor
  set in Typography, and don't skip a spacing increase elsewhere in this
  file just to buy back space, unless that specific rule already says to
  do so.
- Footer on every page: "Page X of Y" (update the total dynamically), in
  small gray text, right-aligned. A one-page resume gets no footer at
  all; only add the footer once the resume is confirmed to span 2 pages.

## Typography

- Font: **Arial only, throughout the entire document.** No font mixing
  between the name, section headers, body text, or any other element.
- Body text (summary, Core Skills, Technical Skills, bullets,
  certifications, education): Arial, **11pt by default**. Only step back
  down to 10.5pt if the tailored content exceeds the 2-page max at
  11pt; never go below 10.5pt or above 11pt.
- Name (top of page): Arial, bold, navy (#1F3864), 16-18pt.
- Tagline (directly beneath the name, above the contact line): Arial,
  regular weight, same size as body text, black.
- Client-name line (beneath each role header): Arial, italic, slightly
  smaller than body text (e.g. 9.5-10pt against an 11pt body).
- Section headers: Arial, **ALL CAPS**, bold, navy (#1F3864), **12pt by
  default** (step back to 11.5pt only if the resume exceeds 2 pages at
  12pt), with a paragraph-level bottom border beneath each heading (see
  Section header rule below); no character-level underline formatting.
- Job titles and company names: Arial, bold, same size as body text.
- Dates: Arial, regular, right-aligned on the same line as the
  title/company, same size as body text.
- Spacing: single line spacing throughout, with these specific values:
  - 6-8pt space after each bullet.
  - 10-14pt space after each role block (before the next role header or
    section header).
  - 12-16pt space before, 8-10pt space after each section header.
  - The Visa Status divider (see below) gets roughly 1.5-2x the standard
    section spacing-before value, so it reads as a clearly separated
    closing element.
  If any of this spacing pushes the resume past 2 pages, tighten toward
  the lower end of these ranges before cutting content.
- No italics for emphasis in body text (the client-name line is the one
  standing exception); use bold sparingly (name, section headers, job
  titles/companies, the Professional Summary's opening descriptor and
  tenure figure, and at most one standout metric per role, not per
  bullet).

## Color

- Section headers, name: navy (#1F3864), bold.
- Tagline, body text, client-name line: black.
- Footer page numbers: gray.
- Hyperlinks (LinkedIn): navy, no underline unless the renderer requires
  it for accessibility.
- No more than one accent color anywhere on the page.

## Alignment

**Left-aligned throughout, no exceptions.** Every element on the page is
left-aligned or one of the two explicitly centered/right-aligned
exceptions below; nothing is justified:

- Professional Summary, Core Skills line, Technical Skills lines,
  bullets, client-name lines, job headers, Certifications, Education,
  Visa Status line, section headers: left-aligned.
- Name and contact line: centered at the top of page 1.
- Dates within a role block: right-aligned against the left-aligned
  title/company line (standard two-column role header).

## Section header rule (border, not character underline)

- Apply a single, thin-weight, navy (#1F3864) bottom border to the
  section-header paragraph itself.
- The border spans the **full content width** of the page (edge to edge
  within the margins), independent of heading text length.
- Never use a character-level underline (`<w:u>`) on heading text; it
  only spans the text's own width, which is the wrong visual result.

## Required sections, in this exact order

1. **Header**: name (centered), a tagline directly beneath it (see
   Tagline rule), then the contact line (phone, email, LinkedIn,
   location; centered, single line, separated by vertical bars).
2. **Professional Summary**: 2-4 lines, no bullets, prose, tailored to
   the target role's track. Bold the opening descriptor phrase (e.g.
   "Cyber GRC leader") and the tenure figure (e.g. "12+ years") within
   the first sentence; bold nothing else in the summary. When the
   target JD's title sits above the candidate's actual title (e.g. JD
   says "Director," candidate is "Senior Manager"), use function-level
   language that overlaps with the JD's title vocabulary where
   truthfully applicable (e.g. "directing enterprise programs" mirrors
   "Director" language without claiming the title). Function-level
   language is fair game for ATS/keyword alignment; title-level language
   is not, per the Tagline rule below.
3. **Core Skills**: a single flat line, not sub-grouped or labeled,
   of domain competencies/frameworks/tools separated by "•", capped at
   **10-12 items maximum**. Populate only with items actually confirmed
   in `master-resume.md`, tailored per application to the target JD.
   Always titled "CORE SKILLS," always included, even when the tooling
   list is short (use the closest genuinely-used items from the corpus
   rather than thinning the section out; flag to the user if it would
   otherwise be empty).
4. **Professional Experience**: reverse chronological. Each role block
   is, in order:
   - Role header line: `<Title> (bold), <Company> (bold) | <Location>`
     with the date range right-aligned on the same line (see Job
     heading format below).
   - Client-name line: `Clients: <Name>, <Name>, <Name>`, italic,
     smaller than body text (see Client names below).
   - Bullets: count per role follows the tiered Bullet-count standard
     below; per-bullet length follows the 2-3 line cap in
     `constraints.md`, and each bullet targets 1-2 sourced metrics per
     the Metric density rule there.
5. **Technical Skills**: a separate, broader section from Core Skills,
   always organized into these **4 labeled groups**, in this order:
   "Frameworks & Regulations," "Security Platforms & Technologies," "AI
   & Automation," "Identity, Access & GRC Platforms." Draw items from
   `master-resume.md`'s Technical Skills Inventory (the full
   source-of-truth pool), curating each group down to roughly **5-8 of
   the most relevant/high-signal items for the target JD** rather than
   dumping the entire confirmed pool. `master-resume.md` itself is never
   curated down; only the per-application resume output is. Each group
   is one line: bold group label + comma-separated items. The AI &
   Automation group lists all items together on its one line regardless
   of employer-context vs. independent-project origin (e.g. "Microsoft
   Copilot, NIST AI RMF, ISO 42001, Claude, n8n Workflow Automation");
   do not carve out a separate sub-label for independent-project tools.
   Always its own section, positioned after Professional Experience,
   never folded into Core Skills.
6. **Certifications**: single line or short list.
7. **Education**: single line or short block.
8. **Visa Status line**: a short divider followed by a single line at
   the very end of the document, after Education, no section header of
   its own (see Visa Status line below).

No other sections are inserted between these eight, and none is
skipped.

## Job heading format

```
<Title> (bold), <Company> (bold) | <Location> ... <Dates> (right-aligned)
```

Designation (title) first, company second, e.g. "Senior Manager, Cyber
GRC, Fourth Technologies Inc. | NJ/CA" followed by the right-aligned
date range on the same line.

## Client names (resumes only)

Client names appear as a dedicated italic line directly beneath each
role's header line and above that role's bullets, never embedded in
bullet prose:

```
<Title>, <Company> | <Location>                          <Dates>
Clients: <Name>, <Name>, <Name>                    (italic, smaller than body text)
- <bullet text, no client names>
- <bullet text, no client names>
```

- Only use names confirmed corroborated and cleared for use in
  `master-resume.md`'s per-role `Clients:` line; never invent or infer
  one.
- Bullets stay client-name-free; do not restate a client name from the
  line above inside any bullet's text.
- This rule applies to resumes only; `coverletter-format.md`'s "no
  client names in the body" rule is unaffected and still governs cover
  letters.

## Tagline rule

Directly beneath the name (above the contact line), a single tagline in
this exact format:

```
<Seniority Level> | <Domain 1> | <Domain 2> | <Domain 3> | <Domain 4 (optional)>
```

Pipe-delimited, not comma-separated within a phrase. Confirmed standing
example for this candidate: "Senior Manager | Cybersecurity | Enterprise
GRC | TPRM | AI."

- **Seniority Level**: a short level-of-seniority phrase, not
  necessarily the full literal title string from `master-resume.md`, but
  never a level or title higher than the candidate's actual current
  seniority. If the target JD's title is more senior than the
  candidate's own (e.g. JD says "Director," candidate is "Senior
  Manager"), the tagline still states the candidate's real level. Never
  substitute, blend, or hedge toward the JD's title here (no
  "Director-Track," "Director-Ready," or similar), in the tagline or
  anywhere else on the resume; a title string next to the candidate's
  name reads as a claim, not a keyword. Resolve JD-title alignment
  through Domain areas and Professional Summary wording instead (see
  Required sections above), never through Seniority Level.
- **Domain areas**: 3-4 short phrases naming the domains most relevant
  to the target JD, pulled from sourced language in `master-resume.md`
  or the bundle's Skills Guide, not invented fresh.
- Never repeat certifications here; they have their own section.
- Centered, same alignment as the name and contact line.

## Bullet-count standard (tiered)

Bullet count per role is role-position-aware, not a single flat range:

- **Most recent/current role**: up to **8 bullets** (raised from 7 on
  2026-08-19 per direct candidate request). This is a ceiling, not a
  floor — select the strongest available bullets per the
  Achievement-Reframing Priority Map; do not pad to 8 with
  lower-priority bullets when a tailoring genuinely supports fewer.
- **Every other role**: **3-4 bullets**, no exceptions, regardless of
  how much corroborated content exists. Use the Achievement-Reframing
  Priority Map to select the strongest 3-4, not the first 3-4
  encountered.
- **Tenure floor**: any role with 5+ years of tenure requires a
  **minimum of 3 bullets**, even if it's a non-current role and even if
  the candidate's strongest material clusters elsewhere.

When trimming a non-current role down to 3-4, drop the lowest-priority
bullets per the Achievement-Reframing Priority Map and log the drop with
its reason, rather than cutting arbitrarily.

## Lead verbs

Every bullet opens with a strong past-tense action verb (present tense
only for a current role's ongoing responsibilities). No lead verb
repeats within a single role block. Across the whole resume, avoid using
the same lead verb more than twice total. Rotate across categories:

- Ownership/leadership: Directed, Led, Owned, Chaired, Spearheaded
- Build/design: Built, Designed, Architected, Established, Instituted
- Assessment/analysis: Assessed, Audited, Evaluated, Diagnosed, Quantified
- Improvement: Reduced, Streamlined, Consolidated, Remediated, Hardened
- Influence/communication: Briefed, Advised, Negotiated, Presented, Aligned
- Delivery: Delivered, Launched, Rolled out, Automated, Scaled

Never open two consecutive bullets, in any role, with the same verb
even in different tenses (e.g., "Led" and "Leading" count as the same
verb).

## Bullet completeness check (mandatory before finalizing)

Before a resume is considered final, re-verify that every bullet
originally selected from the Achievement-Reframing Priority Map and the
Bullet Plan actually made it into the document, in the order planned:

- If a bullet was dropped, confirm the reason was a failed source-trace
  or a genuine no-repeated-metrics conflict that couldn't be resolved by
  rephrasing.
- **Space-driven cuts made before completeness is verified are not
  allowed.** Tighten wording first (2-3 line cap, Brevity in
  `constraints.md`); only cut content as a last resort, explicitly
  logged with the reason, not silently.
- Re-run this check as a final step of `resume-tailor` immediately
  before producing the .docx.

## Visa Status line

The very end of every resume, after Education:

1. **A short divider**: a paragraph-level bottom border, same
   weight/color family as the section-header border, but deliberately
   **short, not full-width**, approximately 1/4 of the page's content
   width (achieved via a large right indent on the divider paragraph,
   roughly 75% of the content width). Visually distinct from
   section-header borders so it doesn't read as a new named section.
   Gets roughly 1.5-2x the standard section spacing-before value (see
   Typography), applied only if the resume still fits 2 pages
   afterward; otherwise use the standard spacing-before value instead.
2. **The line itself**, stating the candidate's status exactly as
   confirmed in `user-data/reference/user-profile.md` or direct user
   instruction, e.g.:
   ```
   Authorized to work in the U.S.: U.S. Citizen, no sponsorship required.
   ```
   Same body font/size, black, left-aligned, no bullet point. Per the
   no-em-dash rule in `constraints.md`, use a colon, period, or comma
   instead of an em dash, even if a request phrases it with one.

If the candidate's work authorization status isn't yet recorded
anywhere, `resume-tailor` should ask the candidate directly rather than
omitting the line or guessing; once confirmed, persist the value to
`user-profile.md` so it doesn't need to be re-asked.

## ATS compatibility

- No tables, text boxes, columns (other than the title/date two-column
  header), headers/footers with content the ATS needs, or images/icons.
- No graphics for skill "ratings" (bars, stars, dots).
- Standard section header text only (e.g., "PROFESSIONAL EXPERIENCE,"
  not a custom icon-based label).
- Save/export as .docx and PDF; PDF must be text-selectable, not a
  flattened image.

## File naming convention (permanent, resumes and cover letters)

```
Jordan_Smith_<ClientOrCompany>_<JobRole>_Resume.docx
Jordan_Smith_<ClientOrCompany>_<JobRole>_CoverLetter.docx
```

- `<ClientOrCompany>`: the hiring company's name if disclosed; if the
  posting comes through a recruiter/staffing agency for an undisclosed
  client (e.g., Optomi), use the recruiter/agency's name instead.
- `<JobRole>`: a short, filename-safe version of the target job title
  (spaces removed or replaced, no special characters), e.g.
  "ManagerITSecurityGRC" for "Manager, IT Security, Governance, Risk and
  Compliance."
- If a file of that exact name already exists in the output folder and
  can't be overwritten, append a versioned suffix (`_v2`, `_v3`, ...)
  rather than changing the naming pattern itself.
