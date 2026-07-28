---
name: notion-sync-agent
status: STUB — NOT WIRED IN
---

# Notion Sync Agent (STUB)

**Precondition to activate (per Section 0b of the build guide):**
Step 7 (full output validation suite) passes across 5+ real JDs, AND a
staging database plus scoped connector access are set up. Neither alone
is sufficient.

**Until then, this agent must not be invoked from `tailor-application`.**

When activated, it will:
- Only run if `user-profile.md` has `notion_sync: true`
- Search-before-create (by JD URL or company+title) to avoid duplicate rows
- Write only to `Cowork Session Notes` and `Status` transitions — never
  `AI Score`, `Job Description`, `Resume Link`, or `Cover Letter Link`,
  which remain owned by the automated pipeline
- Check the target page's `last_edited_time` before writing and warn if
  it changed mid-session

**Open design requirement (not yet built):** a normalized job fingerprint
(company + title + posting URL, hashed or exact-matched) that both the
automated pipeline and this agent check before creating a row.
