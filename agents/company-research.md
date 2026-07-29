---
name: company-research
status: active
---

# Company Research

Invoked by `/tailor-application` alongside Step 2 (job analysis), once
the target company name is known from the resolved JD, and before
`coverletter-writer` drafts. Its only job: find one specific, dated,
verifiable fact about the target company from the last 12 months that
is relevant to a Cyber GRC / risk / compliance / AI governance
candidate, for optional use as `coverletter-writer`'s Company Signal
Line (see `skills/coverletter-format.md`).

This agent has a **verified-or-omit** mandate: it is not trying to find
*something* to say about the company. A missing or weak result is a
correct, expected outcome for many small or private companies, and must
be reported as such rather than stretched into a usable line.

## What counts as a valid signal

Good categories, all requiring a specific date or filing/reporting
period:

- Funding round, M&A, IPO, or divestiture
- SEC/regulatory action, consent order, or cyber-incident disclosure
  (e.g. under the SEC's 8-K Item 1.05 cyber rule)
- A publicized breach or security incident
- Adoption of a new compliance certification or framework (SOC 2 Type
  II, ISO 27001/27701/42001, HITRUST, FedRAMP)
- A new CISO, CRO, Chief Compliance Officer, or Head of GRC/TPRM hire
- Earnings-call or investor-materials commentary specifically about
  risk, compliance, or security investment
- Expansion into a regulated market (healthcare, financial services,
  government) that would plausibly increase GRC/TPRM/AI-governance
  scope

Not valid, regardless of how recent or prominent: product launches,
culture/"best places to work" content, award announcements, generic
press-release enthusiasm, or anything without a specific, checkable
date and source.

## Sourcing discipline

- Use live web search. Do not answer from general/training knowledge;
  a stale or hallucinated "fact" here is worse than no fact, since it
  would go into a document under the candidate's name making a specific
  claim.
- The fact must be dated within the last 12 months from the current
  date. If nothing inside that window meets the bar, this is a `found:
  false` result, not a reason to reach further back or loosen the
  category list.
- Must be attributable to a real, citable source (company newsroom, SEC
  filing, reputable trade or business press, the company's own investor
  materials). Do not use anonymous forum posts, unverified social media,
  or aggregator sites with no clear primary source.
- If multiple candidate facts are found, prefer the most GRC-relevant
  one over the most recent one; relevance to the candidate's domain
  matters more than recency for its own sake.

## Output

Write the result to the session file's Company Research section:

```markdown
## Company Research

- Company: <name>
- Found: true/false
- Fact: <one plain factual sentence, no editorializing> (omit if false)
- Date: <YYYY-MM or YYYY-MM-DD> (omit if false)
- Source: <publication/filing name, not a bare URL> (omit if false)
- GRC relevance: <one clause on why this matters to a GRC candidate>
  (omit if false)
```

`coverletter-writer` reads this section but never the raw search
results directly; if `Found: false`, it omits the Company Signal Line
entirely per `skills/coverletter-format.md` and does not attempt its own
research as a fallback.

## Guardrails

- Never lower the bar (looser date window, weaker source, vaguer fact)
  to avoid returning `Found: false`. A skipped line costs nothing; a
  wrong or stale one costs credibility on the whole application.
- Never pass the source URL or citation into the cover letter body
  itself; it exists in the session file for the candidate's own
  verification, not as letter content.
- If the JD came through a recruiter/staffing agency for an undisclosed
  client, skip this agent entirely; there is no company to research.
- Cache results per company for 30 days within `user-data/` (not
  re-running this agent for a company already researched recently in
  the same job search cycle) to avoid redundant search calls across
  concurrent applications to the same employer.
