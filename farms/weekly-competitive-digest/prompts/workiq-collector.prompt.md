# WorkIQ Collector Sub-Agent

## Goal

Gather internal Microsoft 365 signals about **{{PRODUCT_CATEGORY}}** competitors from the **{{TIME_WINDOW}}**. Surface recent internal discussions, competitive mentions, customer feedback, win/loss signals, and any strategy changes.

Output file: `{{RUN_PATH}}/internal-context.md`

## What to Do

Fire ALL Work IQ queries in a single parallel batch. Do NOT run them one by one.

### Work IQ Queries

| # | Command | Section |
|---|---------|---------|
| I1 | `workiq ask -q "What recent discussions have we had about {{PRODUCT_CATEGORY}} competitors in the past week?"` | Recent Internal Discussions |
| I2 | `workiq ask -q "Any recent customer feedback, wins, or losses related to {{COMPETITORS}}?"` | Customer Signals |
| I3 | `workiq ask -q "Has anyone internally discussed competitor moves, launches, or pricing changes for {{PRODUCT_CATEGORY}}?"` | Competitor Move Awareness |
| I4 | `workiq ask -q "Any recent strategy changes, roadmap updates, or competitive responses planned for {{PRODUCT_CATEGORY}}?"` | Our Response & Roadmap |

## Output Format

Write to `{{RUN_PATH}}/internal-context.md`:

```markdown
# Internal Competitive Signals (Work IQ)
> ⚠️ Internal — do not distribute externally.
**Period:** {{TIME_WINDOW}} ending {{DATE}}

## Recent Internal Discussions
- <summary from I1>

## Customer Signals (Wins / Losses / Feedback)
- <summary from I2>

## Internal Awareness of Competitor Moves
- <summary from I3>

## Our Response & Roadmap Updates
- <summary from I4>
```

## Rules

- **Always label Work IQ output as internal context.** Never present it as public fact.
- **Summarize aggressively.** 5-10 lines per query result max.
- **Focus on recency.** Prioritize signals from the last 7 days.
- If a query returns no useful results, note "No relevant internal signals this week" in that section.
- If Work IQ CLI is not installed or not working, report the error clearly.
- Verify `workiq` is available by running `workiq version` first.

## Return

Report back with:
1. Confirmation that `{{RUN_PATH}}/internal-context.md` was written
2. 2-3 sentence summary of key internal signals
3. Any queries that returned no results or failed
