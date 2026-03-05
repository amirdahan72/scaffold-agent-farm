```skill
---
name: workiq-context
description: 'Query Microsoft Work IQ CLI to gather internal Microsoft 365 context. Use when the agent needs internal information from emails, meetings, chats, documents, or Teams discussions. Triggers: internal context, Work IQ, workiq, what have we discussed, internal emails about, meeting notes about, M365 context.'
---

# Work IQ Context

Gather internal Microsoft 365 context using the Work IQ CLI. This skill queries your organization's emails, meetings, chats, and documents to surface relevant internal information — prior discussions, decisions, stakeholder opinions, and existing artifacts.

## When to Use This Skill

- Agent needs internal context about a product, feature, or initiative
- User wants to know "what have we discussed internally about X"
- Building a PRD or brief that needs internal stakeholder input
- Checking whether an internal decision or direction already exists
- Gathering meeting notes, email threads, or Teams discussions on a topic

## Prerequisites

- Work IQ must be installed: `npm install -g @microsoft/workiq`
- EULA must be accepted: `workiq accept-eula`
- User must have a Microsoft 365 tenant with a Copilot license
- Work IQ admin consent must be granted in the tenant
- Verify installation: `workiq version`

## Important: CLI Mode ONLY

Work IQ is used as a **CLI tool**, not an MCP server. Always invoke it via terminal:

```bash
workiq ask -q "<your question>"
```

Do **not** attempt to use Work IQ as an MCP server or API endpoint.

## Workflow

### Step 1 — Formulate targeted queries

Based on the task context, create 2–5 specific Work IQ queries. Good queries are:

- **Specific to a topic:** `"What have we discussed internally about feature X?"`
- **Focused on decisions:** `"What decisions have been made about the pricing model for product Y?"`
- **Stakeholder-oriented:** `"What feedback has the engineering team given about Z?"`
- **Time-scoped when useful:** `"What were the key takeaways from last month's product review?"`

**Avoid overly broad queries** like "Tell me everything about our product" — they waste context window capacity.

### Step 2 — Execute queries via terminal

Run each query using the terminal:

```bash
workiq ask -q "What have we discussed internally about feature X?"
workiq ask -q "What are our differentiation points for product Y?"
workiq ask -q "Summarize recent emails about the Z initiative"
```

### Step 3 — Summarize and label findings

For each query result:

1. **Summarize** the key findings (do not dump raw output).
2. **Label as internal** — this is organizational context, not public fact.
3. **Note the source type** (email, meeting, chat, document).
4. **Flag actionable items** (decisions, open questions, blockers).

### Step 4 — Write structured output

Write findings to `work/internal-context.md` (or the path specified by the calling agent):

```markdown
# Internal Context (Work IQ)

> **Note:** This content is sourced from internal Microsoft 365 data via Work IQ.
> Do not present as public fact. Do not include in external-facing materials without review.

## Topic: <topic name>

### Key findings
- <finding 1> *(source: email thread, Jan 2026)*
- <finding 2> *(source: Teams chat, Feb 2026)*

### Decisions made
- <decision 1>
- <decision 2>

### Open questions
- <question 1>
- <question 2>

### Stakeholder positions
- **<Person/Team>:** <their position or feedback>
```

## Rules

- **Always label Work IQ output as internal context.** Never present it as public fact.
- **Summarize aggressively.** 5–15 lines per query result. Preserve STM capacity.
- **Do not include PII** beyond names already visible in M365 context.
- **If Work IQ returns no results**, note it and move on. The topic may not have internal history.
- **Separate internal context from public research.** Use a dedicated file (`internal-context.md`).

## Troubleshooting

| Issue | Solution |
|-------|----------|
| `workiq: command not found` | Run `npm install -g @microsoft/workiq` |
| EULA not accepted | Run `workiq accept-eula` |
| No results returned | Try broader or different query phrasing |
| Auth failure | Ensure you're signed in to M365; try `workiq version` to verify |

```
