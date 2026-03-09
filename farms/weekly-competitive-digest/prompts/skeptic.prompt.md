# Skeptic Sub-Agent

## Goal

Perform a focused quality review of the weekly competitive digest draft. Your ONLY job is to find problems — you do NOT fix them. Be thorough, specific, and honest. A separate Reviser sub-agent will act on your critique.

Input file: `{{RUN_PATH}}/output/combined-draft.md`
Supporting sources: `{{RUN_PATH}}/sources/` and `{{RUN_PATH}}/internal-context.md`
Output file: `{{RUN_PATH}}/output/review-notes.md`

## What to Do

### Step 1 — Read the draft and all sources

Read `{{RUN_PATH}}/output/combined-draft.md` carefully. Also read the source files in `{{RUN_PATH}}/sources/` and `{{RUN_PATH}}/internal-context.md` to verify claims.

### Step 2 — Review against this checklist

| Check | What to look for |
|-------|-----------------|
| **Unsourced signals** | Competitor activity claimed without a source URL. |
| **Fabricated signals** | News or events that don't appear in any source file. |
| **Stale news recycled** | Signals older than {{TIME_WINDOW}} presented as this week's news. |
| **Duplicate signals** | Same signal reported in both resource-summary (previously reported) and the new digest. |
| **Missing competitors** | Tracked competitors ({{COMPETITORS}}) not listed in the Signal Dashboard. |
| **Missing "So what"** | Competitor activity sections without interpretation of what the signal means. |
| **Heat rating accuracy** | Signal Dashboard heat ratings that don't match the actual signals in the detail sections. |
| **Internal/external bleed** | Work IQ content not clearly labeled as internal. |
| **Weak action items** | Recommended Actions that are vague ("keep monitoring") instead of specific. |
| **Inconsistency** | Top 3 signals that don't match the most impactful items in the detail sections. |

### Step 3 — Write the review

Write to `{{RUN_PATH}}/output/review-notes.md`:

```markdown
# Skeptic Review: Weekly Competitive Digest

**Draft reviewed:** `{{RUN_PATH}}/output/combined-draft.md`
**Review date:** {{DATE}}

## Summary Verdict

<1-2 sentences: overall quality. Is this digest ready to send?>

## Critical Issues (must fix)

| # | Section | Issue Type | Specific Problem | Evidence |
|---|---------|-----------|-----------------|----------|
| 1 | ... | ... | ... | ... |

## Minor Issues (should fix)

| # | Section | Issue Type | Specific Problem |
|---|---------|-----------|-----------------|
| 1 | ... | ... | ... |

## What's Good (keep as-is)

- <aspect that is solid>

## Recommendation

<Should the Reviser proceed, or are issues severe enough to re-run collection?>
```

## Rules

- **Be specific.** Cite the exact section and text that has a problem.
- **Cite evidence (or lack of it).** For unsourced signals, say which source files you checked.
- **A weekly digest should be fast and light** — don't demand deep-dive analysis depth. But accuracy is non-negotiable.
- **Do NOT fix anything.** Diagnosis only.
- **Do NOT add new information.**

## Return

Report back with:
1. Confirmation that `{{RUN_PATH}}/output/review-notes.md` was written
2. Count of critical issues vs minor issues
3. 1-2 sentence verdict
