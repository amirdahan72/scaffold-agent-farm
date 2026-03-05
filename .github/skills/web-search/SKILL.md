```skill
---
name: web-search
description: 'Search the public web for a query and return summarized results. Use when asked to research competitors, find pricing, look up product features, gather market intelligence, or search the internet for any topic. Triggers: web search, search the web, look up, research online, find information about.'
---

# Web Search

Search the public web for a query and return concise, summarized results. Designed to keep the agent's short-term memory (STM) lean by returning summaries instead of full page content.

## When to Use This Skill

- User asks to "search the web", "look up", or "research" a topic online
- Agent needs public information about competitors, products, pricing, or market trends
- Gathering external evidence to support a brief, PRD, or analysis
- Any task requiring up-to-date information from the internet

## Prerequisites

- The `fetch_webpage` tool must be available in the agent's environment

## Workflow

### Step 1 — Construct search queries

Given a user query or topic, construct 1–3 targeted search queries. Prefer specific, fact-finding queries over broad ones.

Examples:
- Instead of "Competitor X" → "Competitor X pricing plans 2026"
- Instead of "Product Y features" → "Product Y enterprise features comparison"

### Step 2 — Fetch and read web pages

For each query:

1. Use `fetch_webpage` to retrieve relevant pages. Try URLs like:
   - `https://www.google.com/search?q=<url-encoded-query>` for search results
   - Direct URLs if the user provides them or if you know the target site
2. From search results, identify the **2–3 most relevant links**.
3. Use `fetch_webpage` to retrieve each relevant page.

### Step 3 — Summarize findings

For each page retrieved, extract and summarize:

- **Key claims** (product capabilities, positioning statements)
- **Pricing** (tiers, per-seat costs, free plans — if found)
- **Feature highlights** (top 5–8 features, differentiators)
- **Notable quotes** (exact quotes that support or contradict claims)
- **Source URL** (always include the full URL for attribution)

**Important — Summarize, do not dump.** Each page summary should be **10–20 lines max**. Full page content wastes context window capacity and causes the agent to lose earlier information.

### Step 4 — Return structured output

Return results in this format:

```markdown
## Web Search Results: <original query>

### Source 1: <page title>
- **URL:** <url>
- **Summary:** <2–3 sentence overview>
- **Key findings:**
  - <finding 1>
  - <finding 2>
  - <finding 3>
- **Notable quote:** "<exact quote>" — <source>

### Source 2: <page title>
...
```

## Rules

- **Never fabricate URLs or facts.** Only report what was actually found on the page.
- **Always include source URLs** for every claim.
- **Summarize aggressively.** 10–20 lines per source max. The goal is to preserve STM capacity.
- **If a page fails to load**, note it and move on. Do not retry more than once.
- **If no relevant results are found**, say so clearly rather than guessing.

```
