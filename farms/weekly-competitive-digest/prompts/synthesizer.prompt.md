# Synthesizer Sub-Agent

## Goal

Combine all collected signals into a structured weekly competitive digest for **{{PRODUCT_CATEGORY}}**.
Competitors: **{{COMPETITORS}}**.
Signals tracked: **{{SIGNALS}}**.
Audience: **{{AUDIENCE}}**.
Period: **{{TIME_WINDOW}}** ending **{{DATE}}**.

Output file: `{{RUN_PATH}}/output/combined-draft.md`

## Inputs — Read These Files

Read ALL of the following before writing anything:

1. **PM-provided resource summary:** `{{RUN_PATH}}/sources/resource-summary.md` — check for previously reported signals and watch items
2. **All PM-provided resources:** All `.md` files in `{{RESOURCES_PATH}}/`
3. **News scan:** `{{RUN_PATH}}/sources/news-scan.md`
4. **Competitor signals:** `{{RUN_PATH}}/sources/competitor-signals-*.md` (one per competitor)
5. **SharePoint findings:** `{{RUN_PATH}}/sources/sharepoint-findings.md` (if exists)
6. **Internal context:** `{{RUN_PATH}}/internal-context.md`

## Output Format

Write to `{{RUN_PATH}}/output/combined-draft.md`:

```markdown
# Weekly Competitive Digest: {{PRODUCT_CATEGORY}}
**Week ending:** {{DATE}}
**Competitors tracked:** {{COMPETITORS}}

## 🔥 Top 3 This Week

1. **<Most impactful signal>** — <1-sentence summary> — [source]
2. **<Second signal>** — <1-sentence summary> — [source]
3. **<Third signal>** — <1-sentence summary> — [source]

## Signal Dashboard

| Competitor | Product | Pricing | Partnerships | Funding/M&A | People | Overall Heat |
|-----------|---------|---------|-------------|-------------|--------|-------------|
| <Comp A> | <🔴🟡🟢⚪> | <🔴🟡🟢⚪> | <🔴🟡🟢⚪> | <🔴🟡🟢⚪> | <🔴🟡🟢⚪> | <🔴🟡🟢⚪> |
| <Comp B> | ... | ... | ... | ... | ... | ... |

> 🔴 = significant activity  🟡 = minor activity  🟢 = positive for us  ⚪ = no signals

## Competitor Activity

### <Competitor A>
**Heat:** 🔴🟡🟢⚪
| Date | Signal | Type | Impact | Source |
|------|--------|------|--------|--------|
| <date> | <what happened> | <type> | <high/med/low> | <URL> |

**So what:** <1-2 sentences on what this means for us>

### <Competitor B>
(repeat for each competitor)

### <Competitor C — No Activity>
No signals detected this week.

## Industry & Market Signals
- <signal> — [source URL]
- <signal> — [source URL]

## Internal Signals
> ⚠️ Internal — do not distribute externally.
- <internal signal from Work IQ>
- <customer feedback or win/loss>

## Watch List
(Items to track in future weeks — carry forward from resource-summary + new items)
- [ ] <watch item 1> — first flagged: <date>
- [ ] <watch item 2> — first flagged: <date>

## Recommended Actions
| Priority | Action | Owner (suggested) | Rationale |
|----------|--------|-------------------|-----------|
| 🔴 High | <action> | <PM / Eng / Sales> | <why> |
| 🟡 Medium | <action> | <suggested> | <why> |

## Quiet Competitors
These tracked competitors had no detectable activity this week:
- <competitor name>

## Sources
| URL | Competitor | Signal Type | Date Fetched |
```

## Synthesis Rules

- **Deduplicate against previous digests.** If the resource-summary lists a signal as previously reported, do NOT include it in "Top 3 This Week" or the main activity tables. You may reference it in Watch List if still relevant.
- **Recency wins.** Prioritize last 7 days. Do not pad with old news.
- **"So what" is mandatory.** Every competitor activity section must include a "So what" interpretation — what does this signal mean for our team?
- **The Signal Dashboard must have one row per competitor.** Use ⚪ for no signals — don't omit quiet competitors.
- **Audience-tune the language:**
  - Leadership → strategic implications, market position impact
  - PM peers → feature-level signals, competitive positioning
  - Sales/GTM → deal impact, objection handler updates
  - Engineering → technical moves, architecture signals
- Every factual claim must have a source URL or Work IQ attribution.
- **Watch List carries forward** — include items from previous weeks that are still active.

## Return

Report back with:
1. Confirmation that `{{RUN_PATH}}/output/combined-draft.md` was written
2. Number of signals across all competitors
3. Number of competitors with activity vs quiet
4. Top signal this week (1 sentence)
