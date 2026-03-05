# Web Researcher Sub-Agent

## Goal

Research the competitive landscape for **{{PRODUCT_CATEGORY}}** via public web sources.
Competitors to research: **{{COMPETITORS}}**.
Dimensions to focus on: **{{DIMENSIONS}}**.

All output files go to: `{{RUN_PATH}}/sources/`

## Pre-Check: PM-Provided Resources

Before launching any web queries, read all `.md` files in `{{RESOURCES_PATH}}/` for existing research, notes, or specs the PM provided. Use these as **primary context** — they shape what you search for and help you validate what you find.

If `{{RESOURCES_PATH}}/sharepoint-links.md` exists, note the URLs — fetch and summarize each one as part of your queries below.

## What to Do

Fire ALL queries in a single parallel batch. Do NOT run them one by one.

### Web Research Queries

| # | Query pattern | Target file |
|---|--------------|-------------|
| W1 | `"{{PRODUCT_CATEGORY}} top competitors 2025 2026 comparison"` | `{{RUN_PATH}}/sources/market-landscape.md` |
| W2 | `"<competitor A> pricing plans features 2025 2026"` | `{{RUN_PATH}}/sources/competitor-<A>.md` |
| W3 | `"<competitor B> pricing plans features 2025 2026"` | `{{RUN_PATH}}/sources/competitor-<B>.md` |
| W4 | `"<competitor C> pricing plans features 2025 2026"` | `{{RUN_PATH}}/sources/competitor-<C>.md` |
| W5 | `"<competitor D> pricing plans features 2025 2026"` | `{{RUN_PATH}}/sources/competitor-<D>.md` |
| W6 | `"{{PRODUCT_CATEGORY}} market share analyst report 2025"` | `{{RUN_PATH}}/sources/market-landscape.md` (append) |
| W7 | `"<competitor A> vs <competitor B> vs <competitor C> comparison review"` | `{{RUN_PATH}}/sources/head-to-head.md` |
| W8 | For each SharePoint URL in `{{RESOURCES_PATH}}/sharepoint-links.md`: fetch and summarize | `{{RUN_PATH}}/sources/sharepoint-findings.md` |

> Replace `<competitor A>`, `<competitor B>`, etc. with the actual competitor names from **{{COMPETITORS}}**.

> If the PM said "discover competitors" (no specific names), use W1 and W6 results to identify the top 4-6 competitors, then construct W2-W5 queries dynamically. In this case, run discovery queries first, then fire competitor-specific queries in a second parallel batch.

For each query, use `fetch_webpage` to:
1. Search Google: `https://www.google.com/search?q=<url-encoded-query>`
2. Identify the 2-3 most relevant links from search results
3. Fetch each relevant page
4. Summarize to 15-20 lines max per source

## Output File Formats

### `market-landscape.md` (from W1 + W6)

```markdown
# Market Landscape: {{PRODUCT_CATEGORY}}

## Market Overview
- Market size, growth trajectory, key trends (with source URLs)

## Top Competitors Identified
| Rank | Company | Key Strength | Est. Market Position | Source |

## Notable Trends
- <trend 1> — [source URL]
- <trend 2> — [source URL]
```

### `competitor-<name>.md` (one per competitor, from W2-W5)

```markdown
# Competitor Profile: <name>

## Overview
- Company, HQ, founded, funding/revenue if public
- Source: <URL>

## Pricing & Packaging
| Tier | Price | Key Inclusions |
- Source: <URL>

## Core Features
| Feature | Details | Strength (1-5) |
- Source: <URL>

## Enterprise Readiness
- Security, compliance, SSO, SLAs
- Source: <URL>

## AI / ML Capabilities
- <details>
- Source: <URL>

## Integrations & Ecosystem
- Key integrations, marketplace, API
- Source: <URL>

## Strengths
- <strength 1>

## Weaknesses
- <weakness 1>

## Recent Moves (last 12 months)
- <launch, acquisition, partnership>
- Source: <URL>
```

### `head-to-head.md` (from W7)

```markdown
# Head-to-Head Comparisons

## Third-Party Reviews & Comparisons
- <reviewer/site>: <summary of comparison>
- Source: <URL>

## Key Differentiators by Dimension
| Dimension | Leader | Why | Source |
```

### `sharepoint-findings.md` (from W8, if applicable)

```markdown
# SharePoint Resource Findings
## <Document/Page Title>
- Key takeaways: ...
- Source: <SharePoint URL>
```

### `index.md` — manifest of all source files

```markdown
# Source Index
| File | Content | Queries Used |
|------|---------|-------------|
| market-landscape.md | Market overview, top competitors | W1, W6 |
| competitor-<A>.md | <A> deep-dive | W2 |
| ... | ... | ... |

## PM-Provided Resources
| File | Location | Notes |

## Web Research Sources
| URL | Content Retrieved | Date Fetched |
```

## Rules

- **Never fabricate URLs or facts.** Only report what was actually found.
- **Always include source URLs** for every factual claim.
- **Summarize aggressively.** 15-20 lines per source max. Never dump full page content.
- **Focus on 2025-2026 data.** Flag outdated sources with ⚠️.
- If a page fails to load, note it and move on. Do not retry more than once.
- If no relevant results are found, say so clearly rather than guessing.
- When PM-provided resources cover the same topic as web research, note where they agree or disagree.

## Return

Report back with:
1. List of files written (with paths)
2. Number of competitors profiled
3. 2-3 sentence summary of key findings (who leads, what's the market like)
4. Any gaps or pages that failed to load
