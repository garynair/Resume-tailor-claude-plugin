---
name: resume-format
description: "Authoritative resume formatting spec: page setup, typography, alignment, required section order, Core Skills/Technical Skills formats, tagline, client-name line, bullet-count tiers, Work Authorization line, and file naming convention."
---

# Resume Format

Authoritative formatting spec. **Rewritten 2026-09-03** to match the
formatting rebuild carried out in the companion automated pipeline
(`Girish_Nair.docx` template, rebuilt 2026-09-01), reverse-engineered
directly from that template's OOXML (margins, fonts, sizes, colors,
spacing, borders, bullet definitions) plus that pipeline's cover-letter
generator code, then reconciled against this file's own prior
"permanent"/"confirmed" decisions where the two disagreed. This is the
fixed structural baseline for every resume this plugin produces going
forward: `resume-tailor` treats every rule below as fixed and only makes
content decisions (which bullets, which skills) within it. It never
re-derives, improvises, or substitutes formatting per application.
Formatting changes only happen through an explicit user instruction that
edits this file directly; `template-extractor` may flag structural drift
observed in newly added source resumes, but never auto-applies a change
here (see `agents/template-extractor.md`).

**2026-09-03 reconciliation log** (four points where the new template
disagreed with a previously "confirmed"/"permanent" rule in this file;
each was put to the candidate explicitly rather than silently
overwritten):

- **Certifications vs. Education order**: the new template lists
  Education before Certifications. Candidate confirmed this is
  unintentional drift, not a deliberate change, on best-practice grounds
  (active/in-progress certifications are more current signal for a GRC
  hiring manager at this seniority than a decades-old degree). The
  2026-07-29 confirmation stands: **Certifications before Education**.
  Do not re-flag this against the newer template's order.
  **2026-09-12 reversal**: candidate has now directly instructed the
  opposite — **Education before Certifications** — and this supersedes
  the 2026-07-29/2026-09-03 confirmation above. Do not revert to
  Certifications-before-Education, and do not re-flag this order as
  drift against either the 2026-07-29 or 2026-09-03 history recorded
  here. See the Required sections list and the Certifications entry
  below for the updated order and a related space-handling rule the
  candidate raised in the same conversation.
- **Client-name line**: the new template merges it into the same line as
  Company/Location. Candidate accepted this merge (see Client names
  below); the prior dedicated-line rule is superseded.
- **Page footer**: the new template has no footer at all despite running
  2 pages. Candidate reviewed this and reversed course (2026-09-03,
  later same day) — the prior "Page X of Y" rule stands, unchanged; the
  new template's missing footer is unintentional drift in that other
  tool, not a decision to carry over here.
- **Font**: the new resume template's actual rendered font (via
  `docDefaults`, no run-level override) is **Calibri**, not Arial. Note
  for awareness, not a reconciliation point: the companion pipeline's
  cover-letter generator code hardcodes "Aptos" for the cover letter,
  which does not match the resume's Calibri. This spec keeps
  `coverletter-format.md`'s existing "matching the resume's typography"
  rule and uses **Calibri for both documents**, treating the Aptos
  reference in that pipeline's cover-letter code as an unreconciled
  inconsistency in that other tool rather than a considered decision to
  carry over here.

**2026-09-03 correction (post-critique)**: the first pass of this
rewrite wrote the Client/company line pattern as `<Company> — <Location>
| Clients: ...` using a literal em dash, copying the rebuilt template's
own OOXML character-for-character. `critique-agent` correctly flagged
this as a Tier 1 blocking conflict with `constraints.md`'s zero-tolerance
no-em-dash rule, which is cross-cutting and wins over a single format
spec's literal template match. Corrected below to a comma separator
(`<Company>, <Location> | Clients: ...`); this is a bug fix in this
file, not a reconciliation point the candidate needed to weigh in on.

## Page setup

- Margins: **0.35" top and bottom, 0.57" left and right** (asymmetric;
  matches the rebuilt template's actual values — 500 twips top/bottom,
  820 twips left/right). This replaces the prior uniform 0.75" on all
  sides.
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
  (Kept unchanged from the prior spec — the rebuilt template drops the
  footer entirely, but the candidate confirmed that's unintentional
  drift in that other tool, not a deliberate change to bring here.)

## Typography

- Font: **Calibri only, throughout the entire document** (replaces
  Arial). No font mixing between the name, section headers, body text,
  or any other element.
- Body text (summary, Core Skills, Technical Skills, bullets,
  certifications, education): Calibri, **11pt**. Only step back down to
  10.5pt if the tailored content exceeds the 2-page max at 11pt; never go
  below 10.5pt or above 11pt.
- Name (top of page): Calibri, bold, **22pt**, black, mixed case (not
  ALL CAPS — the name is rendered exactly as it appears in
  `user-profile.md`, no case transform applied), no color override.
  (**Corrected 2026-09-15**: lowered from 24pt to 22pt per direct
  candidate instruction, given as a manual correction against a
  produced resume. The 2026-09-03 24pt figure is retired; do not revert
  to it. Navy is retired, see Color below.)
- Tagline (directly beneath the name, above the contact line): Calibri,
  regular weight, 11pt, **gray (#444444)** (previously same size as body
  in black; now explicitly gray per the rebuilt template).
- Contact line: Calibri, regular, **9.5pt**, gray (#444444).
- Client/company line (beneath each role header — see Client names
  below): Calibri, italic, 11pt (same size as body text now, not
  slightly smaller), gray (#444444).
- Section headers: Calibri, **ALL CAPS**, bold, **11pt**, black (navy
  retired), with a paragraph-level bottom border beneath each heading
  (thin, black — see Section header rule below); no character-level
  underline formatting.
- Job titles and company names: Calibri, bold, same size as body text,
  black.
- Dates: Calibri, regular, right-aligned on the same line as the
  title/company via a right tab stop, same size as body text, black.
- Spacing: single line spacing throughout, with these specific values
  (converted from the rebuilt template's twip values; tight by design —
  do not widen these to "read less cramped," the asymmetric margins
  above already assume this density):
  - Name: 2pt space after.
  - Tagline: 2pt space after.
  - Contact line: 5pt space after.
  - Section headers: 8pt space before, 3.5pt space after.
  - Professional Summary paragraph: 2.75pt space after.
  - Job title/dates line: 5.5pt space before, 0.5pt space after.
  - Client/company line: 2.5pt space after.
  - Bullets: 2.75pt space after each.
  If any of this spacing pushes the resume past 2 pages, tighten content
  before touching these values further; they're already at the compact
  end.
- No italics for emphasis in body text (the client/company line is the
  one standing exception); use bold sparingly (name, section headers, job
  titles/companies, and at most one standout metric per role, not per
  bullet). **Professional Summary is not bold anywhere** (corrected
  2026-09-15, retiring the prior rule that bolded the opening descriptor
  phrase and tenure figure — direct candidate instruction against a
  produced resume; do not revert to bolding any part of the summary).

## Color

- Section headers, section-header borders: **black** (navy #1F3864 is
  retired everywhere in the resume).
- Name: black.
- Tagline, contact line, client/company line, Work Authorization line:
  **gray (#444444)**.
- Body text, bullets, job titles/companies: black.
- No navy, and no more than the one accent color (gray) anywhere on the
  page beyond black.

## Alignment

**Left-aligned throughout, no exceptions.** Every element on the page is
left-aligned or one of the two explicitly centered/right-aligned
exceptions below; nothing is justified:

- Professional Summary, Core Skills line, Technical Skills lines,
  bullets, client/company lines, job headers, Certifications, Education,
  Work Authorization line, section headers: left-aligned.
- Name and contact line: centered at the top of page 1.
- Dates within a role block: right-aligned via a right tab stop against
  the left-aligned title line on the same paragraph.

## Section header rule (border, not character underline)

- Apply a single, thin (0.5pt / OOXML `sz="4"`), **black** bottom border
  to the section-header paragraph itself, with a 2pt gap between text
  and border.
- The border spans the **full content width** of the page (edge to edge
  within the margins), independent of heading text length.
- Never use a character-level underline (`<w:u>`) on heading text; it
  only spans the text's own width, which is the wrong visual result.

## Required sections, in this exact order

1. **Header**: name (centered), a tagline directly beneath it (see
   Tagline rule), then the contact line (phone, email, LinkedIn,
   location; centered, single line, separated by vertical bars).
2. **Professional Summary**: 2-4 lines, no bullets, prose, tailored to
   the target role's track. No bold anywhere in the summary (corrected
   2026-09-15 — the prior rule bolding the opening descriptor phrase and
   tenure figure is retired per direct candidate instruction; do not
   reintroduce it). When the
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
4. **Professional Experience** (labeled "EXPERIENCE"): reverse
   chronological. Each role block is, in order:
   - Role header line: `<Title> (bold)` with the date range right-aligned
     via a right tab stop on the same line (see Job heading format
     below).
   - Client/company line: `<Company>, <Location> | Clients: <Name>,
     <Name>, <Name>`, italic, gray, same size as body text (see Client
     names below).
   - Bullets: real bulleted-list paragraphs (● glyph, hanging indent —
     see Bullet formatting below); count per role follows the tiered
     Bullet-count standard below; per-bullet length follows the 2-3 line
     cap in `constraints.md`, and each bullet targets 1-2 sourced metrics
     per the Metric density rule there.
5. **Technical Skills**: a separate, broader section from Core Skills,
   always organized into these **4 labeled groups**, in this order:
   "Additional Frameworks and Platforms," "Security Platforms," "AI and
   Automation Tools," "Identity and Access" (renamed from the prior
   "Frameworks & Regulations" / "Security Platforms & Technologies" /
   "AI & Automation" / "Identity, Access & GRC Platforms" labels to match
   the rebuilt template). Draw items from `master-resume.md`'s Technical
   Skills Inventory (the full source-of-truth pool), curating each group
   down to roughly **5-8 of the most relevant/high-signal items for the
   target JD** rather than dumping the entire confirmed pool.
   `master-resume.md` itself is never curated down; only the
   per-application resume output is. Each group is one line: bold group
   label + colon + comma-separated items, same paragraph. The AI and
   Automation Tools group lists all items together on its one line
   regardless of employer-context vs. independent-project origin; do not
   carve out a separate sub-label for independent-project tools. Always
   its own section, positioned after Professional Experience, never
   folded into Core Skills.
6. **Education**: one block per institution — institution name (bold)
   on its own line, degree/field (italic) on the line beneath it. Reverse
   chronological or as sourced in `master-resume.md`. Positioned
   **before** Certifications per the 2026-09-12 reversal (see
   reconciliation log above).
7. **Certifications**: format flexes with available space now that this
   section sits last before Work Authorization — one certification per
   line when space allows (preferred, most readable); fold to a single
   comma-separated line only when needed to keep the resume within the
   2-page max, and only after Technical Skills curation and bullet-count
   trimming have already been applied. Never drop a held certification
   to save space — fold the list's formatting instead.
8. **Work Authorization line**: a short divider followed by a single
   line at the very end of the document, after Education, no section
   header of its own (see Work Authorization line below; renamed from
   "Visa Status line" to match the rebuilt template's actual label —
   same rule, new name).

No other sections are inserted between these eight, and none is
skipped.

## Job heading format

```
<Title> (bold) .......................................... <Dates> (right-aligned via tab stop)
<Company>, <Location> | Clients: <Name>, <Name>, <Name>   (italic, gray, same size as body)
- <bullet text, no client names>
- <bullet text, no client names>
```

Title only on the header line (bold), with the right-aligned date range
via a tab stop on the same line; company, location, and clients all move
to the dedicated client/company line beneath it (see Client names).

## Client names (resumes only)

Client names appear on the same line as the role's Company/Location,
directly beneath the title/dates header line and above that role's
bullets, never embedded in bullet prose:

```
<Title>                                                    <Dates>
<Company>, <Location> | Clients: <Name>, <Name>, <Name>   (italic, gray)
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

## Bullet formatting

Use a real Word bulleted list (not manually typed dash/hyphen
characters): solid round bullet glyph (●), hanging indent with the
bullet at 0.5" left and 0.25" hanging (matches the rebuilt template's
`numPr` list definition). This renders and copies cleanly through ATS
parsers.

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

- **Most recent/current role**: **7-8 bullets** (range widened from a
  flat 7 on 2026-08-19 per direct candidate request). This is a range,
  not a single ceiling — select the strongest available bullets per the
  Achievement-Reframing Priority Map; do not pad to 8 with
  lower-priority bullets when a tailoring genuinely supports fewer, and
  don't drop below 7 without a genuine reason (insufficient sourced
  material, not a preference for brevity).
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

- Ownership/leadership: Directed, Led, Owned, Chaired, Spearheaded, Managed
- Build/design: Built, Designed, Architected, Established, Instituted, Created
- Assessment/analysis: Assessed, Audited, Evaluated, Diagnosed, Quantified
- Improvement: Reduced, Streamlined, Consolidated, Remediated, Hardened, Improved, Increased
- Influence/communication: Briefed, Advised, Negotiated, Presented, Aligned
- Delivery: Delivered, Launched, Rolled out, Automated, Scaled

Never open two consecutive bullets, in any role, with the same verb
even in different tenses (e.g., "Led" and "Leading" count as the same
verb).

**Never use as a lead verb (added 2026-09-11)**: Orchestrated,
Leveraged, Facilitated, Championed. These read as AI-generated-text
tells (see `ai-fingerprint-checklist.md`) more often than not. If the
sourced action is accurately described by one of these, substitute the
closest truthful alternative from the categories above, matched to
actual scope, rather than defaulting to the banned word. Never let a
banned-verb substitution add scope the source material doesn't support
(e.g., substituting "Built" for what was actually configuration work).

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

## Work Authorization line

The very end of every resume, after Education (renamed from "Visa
Status line" to match the rebuilt template's label; same purpose and
placement):

1. **A divider**: a paragraph-level top border, full content width,
   thin (0.25pt / OOXML `sz="2"`), **light gray (#AAAAAA)**, with a 6pt
   gap between border and text (replaces the prior short, 1/4-width
   divider — the rebuilt template uses a full-width, very light hairline
   instead). 11pt space before the divider paragraph, 2pt space after.
2. **The line itself**, stating the candidate's status exactly as
   confirmed in `/mnt/d/CLAUDE/Resume-Tailor-Claude/user-data/reference/user-profile.md` or direct user
   instruction, in the short form used by the rebuilt template, e.g.:
   ```
   Work Authorization: U.S. Citizen
   ```
   Calibri, 11pt, **gray (#444444)**, left-aligned, no bullet point. Per
   the no-em-dash rule in `constraints.md`, use a colon, period, or
   comma instead of an em dash, even if a request phrases it with one.

If the candidate's work authorization status isn't yet recorded
anywhere, `resume-tailor` should ask the candidate directly rather than
omitting the line or guessing; once confirmed, persist the value to
`user-profile.md` so it doesn't need to be re-asked.

## ATS compatibility

- No tables, text boxes, columns (other than the title/date same-line
  tab-stop layout), headers/footers with content the ATS needs, or
  images/icons.
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
