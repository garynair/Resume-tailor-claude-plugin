---
name: coverletter-format
description: "Authoritative cover letter formatting spec: length, structure, header block, no client names in body, no defensive objection paragraphs, tone, and file naming convention."
---

# Cover Letter Format

## Page setup

- Margins: 0.75" on all four sides (matches `resume-format.md`'s resume
  margins).
- Font: Arial, matching the resume's typography, single column, one page.
- **Spacing (permanent, increased)**: paragraph spacing throughout the
  letter is increased slightly from a tight single-spaced default,
  consistent with the resume's own spacing increase, so the letter
  doesn't read as cramped, with these specific rules:
  - **Date line**: generous space both **before and after** the date
    (roughly 4x a standard single-paragraph spacing value on each side),
    so it reads as a clearly separated element sitting above the rest of
    the header, not hugging the top margin or crowding into the address
    block below it.
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
- **Body alignment**: **left-aligned throughout, permanent** (supersedes
  the brief justified experiment). The entire letter body (opening,
  body, closing), the header block (date, address lines, Re: line,
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

1. **Header block** (not counted in the word total): date, then a short
   three-line address block, then the salutation. The header is
   deliberately minimal:
   - Date.
   - **Address block (permanent, three separate lines, not one combined
     line)**:
     ```
     Hiring Manager
     [Company Name] (or recruiter placeholder, see below)
     [City, State]
     ```
     Each element gets its own line rather than a single comma-separated
     line. Do not repeat the candidate's own name, phone, email,
     LinkedIn, or location here; that contact information already lives
     on the resume and doesn't need to be duplicated in a full
     letterhead block on the cover letter.
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
   - **"Re:" line (permanent, new)**: directly beneath the address
     block, before the salutation, add a single reference line stating
     the exact target job title from the job description:
     ```
     Re: Application for the role of <Job Title>
     ```
     for example: "Re: Application for the role of Manager, IT Security,
     Governance, Risk and Compliance." Use the job title exactly as it
     appears in the job posting, not a shortened or reworded version.
   - Salutation: always "Dear Hiring Manager," regardless of whether a
     company name is disclosed. The company or recruiter name goes only
     in the address block above; it is never restated in the body
     paragraphs below (this is separate from, and in addition to, the
     no-client-names rule for third-party clients/customers below).
2. **Opening (1-2 sentences, up to 3 with the Company Signal Line)**:
   state the role and, in one sentence, the single strongest reason this
   candidate fits it. No "I am writing to apply for..." throat-clearing.
   Optionally, a third sentence may add one **Company Signal Line**: a
   specific, dated, verifiable fact about the target company (recent
   funding, M&A, a regulatory action, a breach disclosure, a new
   compliance mandate, a leadership hire in security/risk/compliance,
   earnings-call commentary on risk/compliance investment) connected
   directly to why the candidate's GRC background is relevant to it
   right now. Never a generic "I admire your innovative culture" line;
   it must name a specific, checkable event. See Company Signal Line
   sourcing rules below.
3. **Body (2 short paragraphs, or 1 paragraph plus 2-3 tight bullets)**:
   2-3 accomplishments drawn directly from the corpus/reference files,
   each tied to a requirement in the job description. Every metric here
   must also respect the no-repeated-metrics rule against the resume; do
   not restate a resume bullet verbatim.
4. **Closing (1-2 sentences)**: forward-looking, confident, inviting next
   steps. No hedging.
5. **Signature block** (not counted in the word total): "Sincerely," then
   the candidate's name (from `user-profile.md`). No contact line here
   either; per the minimal-header rule above, contact information is not
   duplicated anywhere in the cover letter since it already lives on the
   resume.

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
