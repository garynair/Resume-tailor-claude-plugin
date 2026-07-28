---
name: ai-fingerprint-checklist
description: Lexical, structural, and formatting heuristics used to flag AI-generated-sounding text in drafted resumes and cover letters.
---

# AI-Fingerprint Checklist

Run this checklist against every resume and cover letter before it's
finalized (the critique-agent should apply it explicitly). The goal is a
document that reads like it was written by the candidate, not by a
language model. Flag any hit; more than two or three hits in one document
means a rewrite pass, not a spot-fix.

## Lexical heuristics

- **Em dashes.** Any em dash or double-hyphen-as-dash is an automatic
  flag (see `constraints.md`). Zero tolerance.
- **Stock intensifiers and filler adjectives.** "Passionate," "proven
  track record," "results-driven," "dynamic," "seasoned," "robust,"
  "cutting-edge," "leverage(d)" as a verb, "utilize" instead of "use,"
  "spearhead" used more than once, "synergy," "holistic," "streamlined"
  used as a reflex rather than describing an actual streamlining.
- **Triplet padding.** Lists of exactly three adjectives or three
  parallel clauses used as a rhetorical crutch ("strategic, innovative,
  and forward-thinking leader"). Real accomplishments rarely need three
  adjectives to land.
- **Hedge-then-assert constructions.** "It is worth noting that...",
  "It should be emphasized that...", "Not only X, but also Y."
- **Overuse of "ensure," "facilitate," "drive," "foster"** as all-purpose
  verbs standing in for a specific, sourced action.
- **Perfectly parallel sentence openers** repeated more than twice in a
  row ("Led the... Built the... Drove the...") without variation in
  rhythm or length; real writing has some irregularity.
- **Vague quantifiers without numbers**: "numerous," "a variety of,"
  "significant," "substantial" used where a real figure exists in the
  corpus and should be used instead.

## Structural heuristics

- **Perfectly even bullet lengths.** If every bullet in a role block is
  within a word or two of the same length, that's a tell; human-written
  bullets vary.
- **Formulaic bullet template repeated verbatim.** E.g., every single
  bullet following "Verb + object + resulting in X% improvement" with no
  structural variation across 20+ bullets.
- **Summary paragraph that restates the job posting back at the reader.**
  If the professional summary could be produced by paraphrasing the JD
  without knowing anything about the candidate, it's too generic.
- **Missing specificity anchors.** No named tools, frameworks, team
  sizes, timeframes, or dollar figures anywhere in a bullet; a bullet
  with zero concrete anchor is a generic-sounding bullet regardless of
  how it was produced.
- **Symmetrical three-part cover letter body** where each paragraph is
  exactly the same shape and length; vary paragraph length and rhythm.
- **Closing lines that restate the opening almost verbatim** ("As a
  strategic, results-driven leader, I look forward to..." echoing the
  opening's self-description).

## Formatting heuristics

- **Inconsistent or overly uniform bullet punctuation**: every bullet
  ending in a period when the rest of the resume doesn't, or bullets
  ending in a mix of periods and nothing with no clear rule; pick one
  convention and hold it everywhere.
- **Bold applied to entire bullets** or to more than one metric per
  bullet; over-bolding reads as an AI trying to manufacture visual
  "impact."
- **Emoji, arrows (→), or check marks** used as bullet separators or
  emphasis; not used in Big4-style resumes, and a common LLM tic.
- **Title Case Applied To Every Bullet** as if it were a heading; bullets
  are sentence case.
- **Curly/smart quotes mixed with straight quotes** inconsistently within
  the same document, or fancy typographic characters (curly quotes, the
  ellipsis character "…", non-breaking spaces) appearing in a document
  meant to be plain and ATS-safe.
- **Section headers that don't match the standard Big4 vocabulary** in
  `resume-format.md` (e.g., inventing "Career Highlights & Impact
  Narrative" instead of "Professional Experience").

## How to use this list

1. After drafting, scan the full document once per category (lexical,
   structural, formatting) rather than all three at once; it's easier to
   catch repetition when you're looking for one thing at a time.
2. For every hit, either cut it, replace it with a sourced specific
   detail, or (for structural issues) manually vary the sentence/bullet
   pattern.
3. Re-read the document aloud once at the end. If it doesn't sound like
   something a specific person with this specific career would say,
   revise again before handing it to the user.
