---
name: coverletter-format
description: "Authoritative cover letter formatting spec: length, structure, header block, no client names in body, no defensive objection paragraphs, tone, and file naming convention."
---

# Cover Letter Format

**2026-09-14 update**: corrected three places where this spec had drifted
from what the generator actually produces (confirmed against a real
output file, Girish_Nair_RalphLaurenCorporation_TPRMManager_
CoverLetter.docx): the letterhead name is 24pt, not 18pt; the signature
name is not bold; and the "Re:" line reads "Re: Application for <Job
Title>" with no "the". All three are corrected in place below rather than
the actual behavior being changed to match the old wording, since the
real output is what candidates have actually been sending. The companion
pipeline's `cover_letter.py` was updated the same day to match this
spec's Calibri/left-align/non-justified rules exactly (see Font and Body
alignment notes below, which previously flagged it as diverging).

**2026-09-03 update**: reconciled against the companion automated
pipeline's cover-letter generator, which was recently rebuilt alongside
the resume template in `resume-format.md`. Two changes adopted from that
rebuild (letterhead, colors); one explicitly rejected on best-practice
grounds (justified body text) despite the pipeline code using it — see
Body alignment below.

## Page setup

- Margins: 0.75" on all four sides (unchanged — matches
  `resume-format.md`'s resume margins; the pipeline rebuild left cover
  letter margins as-is even though it changed the resume's to asymmetric
  0.35"/0.57").
- Font: **Calibri**, matching the resume's typography (per
  `resume-format.md`'s Typography note, this plugin uses Calibri for
  both documents; the companion pipeline's cover-letter code previously
  hardcoded "Aptos" instead, which didn't match its own resume template's
  Calibri — fixed 2026-09-14, the companion pipeline now uses Calibri
  too), single column, one page.
- **Letterhead (adopted 2026-09-03; alignment reversed 2026-09-13; name
  size corrected 2026-09-14; name size corrected again 2026-09-15)**: a
  header at the very top of the letter:
  - Candidate's name: Calibri, bold, **20pt**, black (not navy), mixed
    case as it appears in `user-profile.md`. (This line previously said
    18pt, corrected to 24pt on 2026-09-14 to match actual generator
    output at the time, then lowered to 20pt on 2026-09-15 per direct
    candidate instruction against a produced cover letter, matching the
    same-day 22pt correction to the resume's name size in
    `resume-format.md`. Do not revert to 24pt.)
  - Contact line directly beneath it: Calibri, regular, 11pt, black,
    phone/email/location/LinkedIn separated by vertical bars (same
    fields and separator style as the resume's contact line).
  - **Left-aligned (2026-09-13 decision)**, not centered. The original
    2026-09-03 adoption centered this block to visually match the
    resume's centered header. Candidate reconsidered on pure
    readability grounds: with the rest of the letter (address block,
    body, signature) permanently left-aligned per Body alignment below,
    a centered block at the top read as a stylistic outlier rather than
    a unified page, whereas one consistent left margin top-to-bottom is
    the more contemporary business-letter convention. The resume's own
    header stays centered per `resume-format.md` — this reversal
    applies to the cover letter only, and the two documents no longer
    mirror each other's header alignment by design.
  - This supersedes the prior "no name/contact repeated here" rule; the
    letterhead is a deliberate, permanent element of the cover letter,
    not duplication to avoid.
- **Spacing (permanent, increased)**: paragraph spacing throughout the
  letter is increased slightly from a tight single-spaced default,
  consistent with the resume's own spacing increase, so the letter
  doesn't read as cramped, with these specific rules:
  - **Letterhead to date**: clear separation between the contact line
    and the date below it, so the letterhead reads as its own block
    before the rest of the header begins.
  - **Date line**: generous space both **before and after** the date
    (roughly 4x a standard single-paragraph spacing value on each side),
    so it reads as a clearly separated element sitting above the
    address block, not hugging the letterhead or crowding into the
    address block below it.
  - **Address block** (Hiring Manager / Company or recruiter placeholder
    / City, State): **tight spacing between these three lines**, since
    together they read as a single address unit, not three separate
    paragraphs. Use noticeably less space between these three lines than
    anywhere else in the header.
  - **Re: line** (see Structure below): normal-to-generous spacing
    before and after, since it functions as its own distinct header
    element between the address block and the salutation.
  - **Salutation to first body paragraph**: generous spacing, more than
    a standard single-paragraph gap, so the letter's opening line
    doesn't feel like it's crowding the salutation above it.
- **Body alignment**: **left-aligned throughout, permanent — reaffirmed
  2026-09-03, and the companion pipeline was brought into line with it
  2026-09-14.** The companion pipeline's cover-letter code had twice
  drifted to justified body paragraphs on its own side; this plugin
  never adopted that, and as of 2026-09-14 the pipeline's own code was
  fixed to match this rule instead of continuing to diverge. Justified
  text in a narrow, one-column document produces uneven word-spacing
  ("rivers") and reads as less professional, not more. The entire letter
  body (opening, body, closing),
  the header block (letterhead, date, address lines, Re: line,
  salutation), and the signature block are all left-aligned. No
  justified text anywhere in the cover letter, matching the resume's
  left-alignment rule in `resume-format.md`.

## Length

Body text runs approximately 200 words (180-220 acceptable range),
excluding date, address block, salutation, and signature block. If a draft
comes in over 220 words, cut before adding; do not compress by shrinking
font or margins. If it comes in under 180, do not pad with generic
enthusiasm; add another concrete, sourced detail instead, or leave it
short.

## Structure

1. **Header block** (not counted in the word total): the letterhead (see
   Page setup above), then the date, then a short three-line address
   block, then the "Re:" line, then the salutation:
   - **Letterhead**: candidate's name and contact line, centered, per
     Page setup above. (As of the 2026-09-03 update, contact information
     is intentionally repeated here — it did previously live only on the
     resume, but the letterhead is now a deliberate design element
     rather than an avoidable duplication.)
   - Date.
   - **Address block (permanent, three separate lines, not one combined
     line)**:
     ```
     Hiring Manager
     [Company Name] (or recruiter placeholder, see below)
     [City, State]
     ```
     Each element gets its own line rather than a single comma-separated
     line.
   - When the job posting doesn't disclose the hiring company's name
     (e.g., a staffing/recruiting firm posting on behalf of an
     undisclosed client), use the recruiter's own placeholder language
     in place of "[Company Name]" instead of inventing or guessing a
     company name. For example, if a posting says "Optomi, in
     partnership with a leading organization in the retail industry,"
     the address block becomes:
     ```
     Hiring Manager
     Optomi (on behalf of a retail industry client)
     [City, State if known]
     ```
     Never fabricate a specific company name that wasn't disclosed.
   - **"Re:" line**: directly beneath the address block, before the
     salutation, add a single reference line stating the exact target
     job title from the job description, not bold:
     ```
     Re: Application for <Job Title>
     ```
     for example: "Re: Application for Manager, IT Security,
     Governance, Risk and Compliance." (Wording corrected 2026-09-14:
     this line previously said "Re: Application for the <Job Title>," but
     actual generator output has never included "the" — corrected to
     match real behavior. The companion pipeline's cover_letter.py was
     also updated 2026-09-14 to drop "the" here, so both tools now agree.)
     Use the job title exactly as it appears in the job posting, not a
     shortened or reworded version.
   - Salutation: always "Dear Hiring Manager," regardless of whether a
     company name is disclosed. The company or recruiter name goes only
     in the address block above; it is never restated in the body
     paragraphs below (this is separate from, and in addition to, the
     no-client-names rule for third-party clients/customers below).
2. **Opening (1-2 sentences, up to 3 with the Company Signal Line)**:
   **problem-first ordering (permanent, added 2026-08-19)**: when a
   Company Signal Line is available (see below), it leads the opening,
   framing a specific, current, verifiable situation at the target
   company; the candidate-fit sentence then follows as the answer to
   that framing, not the other way around. When no Company Signal Line
   is available, open directly with the role and, in one sentence, the
   single strongest reason this candidate fits it. Either way: no "I am
   writing to apply for..." throat-clearing. The **Company Signal
   Line** itself is a specific, dated, verifiable fact about the target
   company (recent funding, M&A, a regulatory action, a breach
   disclosure, a new compliance mandate, a leadership hire in
   security/risk/compliance, earnings-call commentary on risk/compliance
   investment) connected directly to why the candidate's GRC background
   is relevant to it right now. Never a generic "I admire your
   innovative culture" line; it must name a specific, checkable event.
   Exactly one Company Signal Line per letter -- if `company-research`
   surfaces more than one qualifying fact, fold the strongest into a
   single tight sentence rather than adding a second signal line; this
   is a 200-word letter, not a research brief. See Company Signal Line
   sourcing rules below.

   **Confident domain-bridge (permanent, added 2026-08-19)**: when the
   candidate's specific industry/domain experience differs from the
   target company's, one clause may name that surface-level difference
   and immediately reframe it around the underlying shared competency
   or problem (e.g., "Runway's domain is different from mine, but the
   underlying problem is the same: proving an AI system is safe enough
   for the people who depend on it"). This is distinct from, and
   allowed alongside, the No-defensive-objection-paragraphs rule below:
   a domain-bridge is one confident clause that reframes relevance
   forward, contains no hedge language ("although," "despite," "while I
   don't have," "I may lack"), and is not apologizing for or arguing
   around a gap the employer raised. If it can't be written without a
   hedge word or without sounding like an apology, leave it out rather
   than force it.
3. **Body (2 short paragraphs, or 1 paragraph plus 2-3 tight bullets)**:
   2-3 accomplishments drawn directly from the corpus/reference files,
   each tied to a requirement in the job description. Every metric here
   must also respect the no-repeated-metrics rule against the resume; do
   not restate a resume bullet verbatim.
4. **Closing (1-2 sentences)**: forward-looking, confident, inviting next
   steps, and **naming one concrete first action** the candidate would
   take in the role (e.g., "My first move would be to benchmark your
   current control environment against SOC 2 and ISO 27001 requirements
   and sequence the gap-closure work from there"), not generic
   enthusiasm ("I'd welcome the opportunity to discuss..." on its own is
   not sufficient; a concrete action must come first, an invitation to
   discuss may follow it in the same sentence or a short second one). No
   hedging. The concrete action must be genuinely inferable from the JD
   and the candidate's sourced background, not an invented specific plan
   the candidate has no basis for proposing.
5. **Signature block** (not counted in the word total): "Sincerely," then
   the candidate's name (from `user-profile.md`), not bold (corrected
   2026-09-14 — this line previously said bold, but actual generator
   output has never bolded the signature name). No contact line here —
   the letterhead at the top of the letter is where contact information
   now lives (see Page setup above); it isn't repeated a second time at
   the bottom.

## No client names in the body

Never name a former employer's clients, customers, or the counterparties
of an engagement in the body of the letter, even if a client name appears
in the source corpus or an old cover letter. Refer to them generically:
"a Fortune 500 healthcare payer," "a global logistics client," "a
regulated financial services firm." This applies even when the client is
publicly known to be affiliated with the candidate's past employer.
Company names of the candidate's own past employers are fine to use;
it's the clients/customers of those employers that stay unnamed.

This is a separate rule from the hiring company/recruiter name: the
hiring company (or recruiter placeholder, per Structure above) belongs
only in the header address line, never restated in the body paragraphs
either. So the body of the letter names neither the candidate's former
clients nor the hiring company/recruiter itself; both live outside the
body (former employers stay nameable, hiring company/recruiter lives in
the header only).

## Company Signal Line (sourcing rules)

- Sourced only from `company-research`'s output in the session file's
  Company Research section, never generated fresh by `coverletter-writer`
  from training knowledge or recalled from memory. If that section shows
  `Found: false`, or doesn't exist at all, omit the line entirely; do
  not fall back to a vague or unverifiable claim to fill the space.
- Must be dated within the last 12 months per `company-research`'s own
  window; this agent does not re-loosen that window at drafting time.
- Must be paired with a one-clause bridge to GRC relevance in the same
  sentence (ties to TPRM, AI governance, regulatory exposure, audit
  readiness, or similar), not stated as a standalone trivia fact.
- Counts toward the 180-220 word body limit; it is not a bonus
  paragraph exempt from the length rule.
- The `Source`/citation itself never appears in the letter body; it
  exists only in the session file for the candidate's own verification.
- If the job posting is via an undisclosed-client recruiter, there is no
  company to research; this feature does not apply to that letter.

## No defensive "objection" paragraphs

Do not include a paragraph that anticipates and rebuts an objection the
employer hasn't raised: no "While I don't have direct experience in X,
I..." no "Although my background is primarily in Y, I am confident..."
no addressing an employment gap, career pivot, or missing certification
unprompted by the reader. If the corpus has a real gap relative to the job
description, that is handled upstream during gap dialogue (fill it with a
real, sourced accomplishment, or leave it out) rather than by naming the
gap and arguing around it in the letter itself. A cover letter makes the
positive case; it does not litigate.

This does not prohibit the Confident domain-bridge move described in the
Opening rule above; the distinguishing test is hedge language and
posture, not subject matter. "Although my background is primarily in Y,
I am confident..." is banned. "Y is different from mine, but the
underlying problem is the same: Z" is allowed, because it names no gap
as a deficiency and asks for no forgiveness -- it reframes forward
instead of defending backward.

## Tone

Confident, specific, and grounded in sourced fact, matching the
Accuracy-first hierarchy in `constraints.md`. No em dashes (see
`constraints.md`). No superlatives that aren't backed by a number or a
named outcome ("passionate," "results-driven," "proven track record" are
banned as unsupported filler; a stated result is not).

## File naming convention (permanent, resumes and cover letters)

Every cover letter this plugin produces is saved using this exact
pattern (identical rule to `resume-format.md`'s, repeated here since
both file types share it):

```
Jordan_Smith_<ClientOrCompany>_<JobRole>_CoverLetter.docx
```

- `<ClientOrCompany>`: the hiring company's name if disclosed; if the
  posting comes through a recruiter/staffing agency for an undisclosed
  client, use the recruiter/agency's name instead.
- `<JobRole>`: a short, filename-safe version of the target job title
  (spaces removed or replaced, no special characters).
- This replaces any earlier ad hoc naming for all future applications.
