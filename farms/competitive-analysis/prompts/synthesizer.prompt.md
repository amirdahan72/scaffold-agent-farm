# Synthesizer Sub-Agent

## Goal

Combine all collected research into a structured competitive analysis draft for **{{PRODUCT_CATEGORY}}**.
Competitors: **{{COMPETITORS}}**.
Dimensions: **{{DIMENSIONS}}**.
Audience: **{{AUDIENCE}}**.

Output file: `{{RUN_PATH}}/output/combined-draft.md`

## Inputs — Read These Files

Read ALL of the following before writing anything:

1. **PM-provided resource summary:** `{{RUN_PATH}}/sources/resource-summary.md` — treat as **primary context**
2. **All PM-provided resources:** All `.md` files in `{{RESOURCES_PATH}}/` — treat as **primary context**
3. **Market landscape:** `{{RUN_PATH}}/sources/market-landscape.md`
4. **Competitor profiles:** `{{RUN_PATH}}/sources/competitor-*.md` (one per competitor)
5. **Head-to-head comparisons:** `{{RUN_PATH}}/sources/head-to-head.md`
6. **SharePoint findings:** `{{RUN_PATH}}/sources/sharepoint-findings.md` (if exists)
7. **Internal context:** `{{RUN_PATH}}/internal-context.md`

## Output Format

Write to `{{RUN_PATH}}/output/combined-draft.md`:

```markdown
# Competitive Analysis: {{PRODUCT_CATEGORY}}

## Executive Summary
<3-5 sentence strategic summary: who are the top competitors, where do we win, where are we at risk, what should we do about it>

## Market Landscape
<Market size, trends, momentum — from market-landscape.md>

## Comparison Matrix
| Dimension | Our Product | Competitor A | Competitor B | Competitor C | Competitor D |
|-----------|-------------|-------------|-------------|-------------|-------------|
| Pricing | <details> | <details> | <details> | <details> | <details> |
| <Dimension 2> | ... | ... | ... | ... | ... |
(Include ALL dimensions the PM specified)

## Competitor Deep-Dives

### <Competitor A>
- **Overview:** ...
- **Pricing:** ...
- **Key Features:** ...
- **Strengths:** ...
- **Weaknesses:** ...
- **Recent Moves:** ...
- **Sources:** <URLs>

### <Competitor B>
(repeat for each competitor)

## Head-to-Head Analysis
<From head-to-head.md — third-party comparisons, who wins where>

## Internal Context
> ⚠️ Internal — do not distribute externally.
- Our differentiators: ...
- Competitive concerns: ...
- Customer win/loss insights: ...
- Our roadmap advantages: ...

## Battle Cards

### vs. <Competitor A>
| Dimension | We Win Because | They Win Because | Objection Handler |
|-----------|---------------|-----------------|-------------------|
| <dim 1> | <our advantage> | <their advantage> | <how to respond> |

(One battle card table per competitor)

## Strategic Recommendations
1. **<Recommendation 1>** — <evidence and rationale>
2. **<Recommendation 2>** — <evidence and rationale>
3. **<Recommendation 3>** — <evidence and rationale>

## Gaps & Open Questions
- [ ] <gap 1 — where we lack data>
- [ ] <gap 2>

## Sources
- <all URLs cited, grouped by competitor>
```

## Synthesis Rules

- When PM-provided resources and web research cover the same topic, **prefer PM-provided data** and note where web research confirms or contradicts it.
- **Populate every cell** in the comparison matrix. Use `[NEEDS DATA]` for missing information rather than leaving blank or fabricating.
- **Audience-tune the language:**
  - Leadership → strategic framing, business impact
  - PM peers → feature-level detail, technical comparison
  - Sales/GTM → objection handlers, battle cards, competitive positioning
  - Engineering → technical depth, architecture comparisons
- Battle cards must be actionable — "We win because..." + "They win because..." + "Objection handler."
- Every factual claim must have a source URL or Work IQ attribution.
- Keep the document well-structured and scannable — use tables, bullet points, headers.

## Return

Report back with:
1. Confirmation that `{{RUN_PATH}}/output/combined-draft.md` was written
2. Number of competitors covered
3. Number of `[NEEDS DATA]` gaps found
4. 2-3 sentence summary of the key strategic picture
