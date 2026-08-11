---
name: constraints
description: "Cross-cutting non-negotiable rules for the Resume-Tailor plugin: priority hierarchy, name sourcing, no em dashes, bullet-length cap, no repeated metrics, data-conflict protocol, metric-collision tiebreak, cover letter length, no fabrication."
---

# Constraints

These constraints are non-negotiable and apply to every agent in this plugin
(intake, corpus-builder, bundle-builder, job-analyzer, resume-tailor,
coverletter-writer, critique-agent, notion-sync-agent). When any two rules
conflict, resolve using the priority hierarchy below: do not average or
split the difference.

## Priority hierarchy

When a tradeoff must be made, resolve it in this order:

1. **Accuracy**: every claim must be traceable to `master-resume.md`,
   `master-metrics-vault.md`, `star-story-bank.md`, a bundle file, or an
   answer explicitly given by the user during gap dialogue. Nothing else
   outranks this. An accurate but less-relevant bullet beats a relevant but
   unverifiable one.
2. **Relevance**: of the accurate, available material, prefer what maps
   most directly to the target job description's required skills,
   keywords, and seniority level.
3. **Impact**: of the relevant, accurate material, prefer bullets that lead
   with outcome, scope, or scale over bullets that only describe activity.
4. **ATS**: once accuracy, relevance, and impact are satisfied, optimize
   phrasing and keyword placement for applicant tracking systems (exact-match
   job title/skill terms, standard section headers, no tables or text boxes
   that ATS parsers mangle).
5. **Brevity**: last. Cut words only after the above are satisfied. Never
   cut a qualifying detail (e.g., "reduced findings 40% across 3 business
   units") down to something vaguer just to save a line.

Never reorder this hierarchy to make a document look more impressive.
Accuracy failures are the one class of error that cannot be fixed later;
treat them as blocking, not stylistic.

## Name sourcing

The candidate's name, as it appears on every resume and cover letter, is
always pulled from `user-data/reference/user-profile.md` (the `Name:`
field). Never infer, abbreviate, expand, or "clean up" the name from a
job description, an old resume filename, or general convention. If
`user-profile.md` has no `Name:` value populated, stop and prompt the user
to run `/setup-profile` rather than guessing.

## No em dashes

Em dashes are not used anywhere in generated resumes or cover letters, not
for asides, not as a range substitute, not for dramatic pause. Use a period,
comma, colon, or parentheses instead. This includes the Unicode em dash and
any double hyphen standing in for one. This rule exists because em dash
density is one of the more reliable AI-generated-text signals (see
`ai-fingerprint-checklist.md`), and Big4-style resumes don't use them either.

## Bullet length cap (2-3 lines)

Every resume bullet must fit on **2-3 lines** at the target font size and
margins (see `resume-format.md`); 2 lines is the target, 3 is the ceiling,
not the goal. If a bullet runs past 3 lines:

- Cut qualifying clauses before cutting the metric or the outcome.
- Split into two bullets only if both halves are independently
  bullet-worthy (each has its own action plus result); otherwise, tighten.
- Never shrink font size or widen margins just to force a fit.

Don't use the extra third line as license to pad; a bullet that fits
comfortably in 2 lines should stay at 2. The extra room exists for cases
where fitting a required metric or qualifying detail (see Metric density
below) would otherwise force an artificial cut.

## Metric density (at least 1, at most 2 per bullet)

Every bullet should carry **at least 1 sourced metric** (a number,
percentage, dollar figure, headcount, or timeframe) and **no more than
2**. Two is acceptable only when both are genuinely different facets of
the same achievement (e.g., "reduced repeat findings 38% YoY across 5
business units" pairs an outcome metric with a scope metric); never two
instances of what's effectively the same figure restated.

This is a **preference, not a license to fabricate**. Per the No
Fabrication rule below, if a bullet's underlying achievement genuinely
has no sourced metric in `master-resume.md`, `master-metrics-vault.md`,
or an approved gap-dialogue answer, the bullet stays metric-free rather
than inventing one. When choosing which bullets make the cut for a role
(see the Bullet-count standard in `resume-format.md`), prefer bullets
that do carry a sourced metric over otherwise-comparable bullets that
don't, all else being equal on relevance and impact; but a strong,
accurate, metric-free bullet still beats a weaker or fabricated one, per
the Accuracy-first priority hierarchy above.

## No repeated metrics across bullets

A given number, percentage, dollar figure, headcount, or timeframe may
appear in at most one bullet across the entire resume, and independently,
at most once across the cover letter. If the same underlying achievement
is relevant to two bullets, cite a different facet of it (for example, the
budget figure in one bullet and the team size in another) rather than
restating the same figure twice. Before finalizing, scan all bullets for
duplicate numbers and rewrite the second occurrence.

## Data-conflict protocol

When two or more source documents disagree on a figure for what is
clearly the same underlying achievement (e.g., one set of resumes says
"20+ critical vendors" and another says "30+ critical vendors" for the
same TPRM program), resolve it this way, in order:

1. **Use the version corroborated by more sources.** Count independent
   source documents on each side of the disagreement. If one version of
   the figure has a clear numerical majority of corroborating sources
   over the other, use that version and note in `master-resume.md` (or
   the session file, if discovered mid-application) that a minority
   variant exists and was not used.
2. **If corroboration is tied, or the count is unclear or too close to
   call**, do not guess and do not average the two figures. Exclude the
   bullet entirely from the current output and flag it explicitly in the
   session file's Requirement Mapping or Bullet Plan (whichever is
   relevant), tagged as a `[conflict]` needing the candidate's direct
   resolution before it can be used in any future application. This
   mirrors how `corpus-builder.md` tags conflicts in `master-resume.md`
   itself; the same excluded-until-resolved standard applies at
   resume-drafting time, not just at corpus-build time.
3. Never silently pick a side, and never resolve a tie by picking
   whichever number sounds more impressive; that inverts the Accuracy
   priority above.

## Metric-collision tiebreak

When restoring a previously-cut bullet, adding a new bullet, or
otherwise editing a draft causes the same number/percentage/figure to
appear in more than one place in the resume (a "metric collision"),
resolve it in this order:

1. **Keep the metric on whichever bullet has no other sourced outcome.**
   If one of the colliding bullets has an additional metric or concrete
   qualitative result it can fall back on, and the other bullet would be
   left with no Outcome at all if the number were removed, the number
   stays on the bullet that needs it and is removed from the other.
2. **Find an alternate sourced metric for the bullet that loses the
   number**, if `master-resume.md` or `master-metrics-vault.md` has a
   second, non-colliding figure for that same bullet's underlying
   achievement (a different facet, e.g., team size instead of percentage
   improvement).
3. **If no alternate sourced metric exists**, drop the number from that
   bullet but keep the qualitative claim intact (e.g., "increasing
   delivery throughput" without restating a specific percentage already
   used elsewhere) rather than deleting the bullet outright or leaving
   it fabricated with a substitute number that isn't sourced.
4. Log which bullet kept the metric, which bullet lost it, and why, in
   the session file's Bullet Plan, the same way this was documented
   during the Optomi application's revision passes; this should happen
   automatically as part of drafting, not only when a problem is caught
   after the fact.

## Cover letter length

Cover letters run approximately 200 words (target range 180-220), counting
body text only and excluding date, address block, salutation, and
signature. See `coverletter-format.md` for the full structural spec.

## No fabricated metrics or experience

Never invent a number, employer, title, dollar figure, certification,
tool, team size, or outcome that isn't sourced from the corpus, the
reference files, or direct user confirmation during gap dialogue. This
includes:

- Rounding a real number in a way that changes its meaning (for example,
  turning an unconfirmed estimate into a stated fact).
- Inferring a metric because "it's probably around that" for a role of
  that seniority.
- Adding a certification, tool, or scope the user hasn't listed anywhere
  in the corpus.
- Extending a bullet's claimed scope (for example, "led" when the source
  material says "contributed to") to sound more senior.

If a job description calls for something the corpus doesn't support, flag
the gap and ask the user directly rather than papering over it. An honest
"not yet corroborated" beats a confident fabrication every time.
