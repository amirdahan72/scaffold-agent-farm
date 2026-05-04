```skill
---
name: adaptive-cards
description: 'Generate Adaptive Card JSON artifacts from markdown content for Teams, Outlook, and Copilot surfaces. Use when a farm writer produces summaries, action items, release highlights, competitive signals, or any concise artifact that benefits from card-based consumption. Triggers: "adaptive card", "Teams card", "card output", "card artifact", "card version".'
---

# Adaptive Cards Skill

Generate Adaptive Card JSON from structured markdown content. This skill transforms farm output artifacts (summaries, digests, action items, release highlights, battle cards) into valid Adaptive Card JSON for consumption in Teams, Outlook, Copilot, and other Microsoft surfaces.

## When to Use This Skill

- A farm writer produces a concise, structured artifact (action items, summaries, digests, release highlights, battle cards) that would benefit from card-based delivery
- The PM requests an Adaptive Card version of an output
- The orchestrator's `{{OPTIONAL_FORMATS}}` includes "adaptive-card"
- The artifact is **scannable and action-oriented** (not a 10-page strategy doc)

## When NOT to Use This Skill

- The artifact is long-form (strategy papers, full PRDs, design docs, onboarding guides)
- The artifact is already a slide deck or Word document (different consumption channel)
- The artifact requires rich formatting that cards cannot express (complex nested tables, footnotes, images)

## Prerequisites

- **adaptive-cards-mcp** MCP server must be configured (see MCP Setup below)
- The MCP server runs via `npx adaptive-cards-mcp` — Node.js 20+ required

## MCP Setup

The `adaptive-cards-mcp` server must be configured at user level in `%APPDATA%/Code/User/mcp.json`:

```json
{
  "servers": {
    "adaptive-cards-mcp": {
      "command": "npx",
      "args": ["-y", "adaptive-cards-mcp@latest"],
      "type": "stdio"
    }
  }
}
```

## Tool Selection Guide

The adaptive-cards-mcp server exposes 9 tools. For farm artifact conversion, use these:

| Scenario | Tool | Key Parameters |
|----------|------|----------------|
| Convert markdown summary to card | `generate_card` | `content` (required), `host`, `intent` |
| Convert structured data (tables, lists) to card | `data_to_card` | `data` (required), `presentation`, `title` |
| Generate and validate in one step | `generate_and_validate` | `content` (required), `host`, `intent` |
| Full pipeline (generate + validate + optimize) | `card_workflow` | `steps`, `content`, `host` |
| Check card works on target surface | `validate_card` | `card` (required), `host` |
| Optimize for accessibility | `optimize_card` | `card` (required), `goals: ["accessibility"]` |

### Decision Rules

1. For **simple summaries** (meeting follow-ups, weekly highlights, release notes) → `generate_and_validate`
2. For **tabular data** (action items, comparison matrices, signal dashboards) → `data_to_card`
3. For **multi-section cards** (competitive briefs with battle cards) → `card_workflow` with steps `["generate", "validate", "optimize"]`
4. Always pass `host: "teams"` unless the PM specifies a different surface

## Artifact-to-Card Mapping

Farm artifacts map to card intents as follows:

| Farm Artifact | Card Intent | Card Pattern | Notes |
|---------------|-------------|--------------|-------|
| Follow-up Summary (action items, owners, deadlines) | `list` | ColumnSet with status indicators | One card per meeting; Action.OpenUrl for detail links |
| Decisions & Open Questions Log | `dashboard` | FactSet + ColumnSet | Priority color-coding via Container styles |
| Weekly Competitive Digest (top signals) | `notification` | ColumnSet with signal icons | Limit to top 5-7 signals; link to full digest |
| Release Notes (feature highlights) | `notification` | ImageSet or ColumnSet per feature | Group by category (New, Improved, Preview) |
| Competitive Battle Cards | `report` | FactSet per competitor | One card per competitor or one combined |
| Strategic Recommendations | `status` | Numbered list with priority indicators | Action items with owners |

## Card Generation Workflow

When a farm writer needs to produce an Adaptive Card artifact:

### Step 1 — Prepare card content

Read the finalized markdown artifact. Extract the card-appropriate sections:

- **Title:** artifact title + date
- **Body:** key data points, tables, action items (not full prose)
- **Actions:** links to full document, approval buttons (if applicable)

**Content budget:** Adaptive Cards work best with 5-15 body elements. If the artifact has more, split into multiple cards or link to the full document.

### Step 2 — Generate the card

Call the appropriate tool (see Decision Rules above). Always include:

- `host: "teams"` (default) or the PM-specified target
- `intent:` mapped from the artifact type (see Artifact-to-Card Mapping)
- `content:` a structured summary of the artifact (not the raw markdown dump)

Example for a meeting follow-up:

```
generate_and_validate({
  content: "Meeting follow-up for Azure WAF Architecture Review on 2026-03-15. 
    Action items: 1) Jane: Update firewall rules by March 22 (High). 
    2) Bob: Review peering config by March 20 (Medium). 
    Decisions: Approved hub-spoke topology. Open: Budget for secondary region.",
  host: "teams",
  intent: "list"
})
```

### Step 3 — Write card JSON to disk

Save the generated card JSON to the run output folder:

- File: `{{RUN_PATH}}/output/<artifact-name>.adaptive-card.json`
- Convention: match the markdown artifact name with `.adaptive-card.json` suffix

Example output files:
```
{{RUN_PATH}}/output/followup-summary.md
{{RUN_PATH}}/output/followup-summary.adaptive-card.json
{{RUN_PATH}}/output/weekly-digest.md
{{RUN_PATH}}/output/weekly-digest.adaptive-card.json
```

### Step 4 — Validate (if not using generate_and_validate)

If you used `generate_card` or `data_to_card` directly, follow up with `validate_card`:

```
validate_card({ card: <generated JSON>, host: "teams" })
```

Fix any schema errors. Maximum 2 retry cycles — if still failing after 2 retries, save the best version and note validation issues in the report.

## Card Design Rules

1. **Schema version:** Use v1.5 for Teams (broadest compatibility); v1.4 if Outlook is a target
2. **Accessibility:** Always set `wrap: true` on TextBlocks; include `altText` on images; use heading styles (`size: "Large"`, `weight: "Bolder"`) for section headers
3. **Content density:** 5-15 body elements per card. Link to full document for overflow
4. **Actions:** Maximum 3-5 actions per card. Always include "View Full Report" linking to the markdown/docx
5. **Theming:** Use `default`, `emphasis`, `good`, `attention`, `warning` container styles for visual hierarchy — do not hardcode colors
6. **No fabrication:** Card content must come directly from the artifact; never add information not in the source

## Multi-Card Strategy

Some artifacts are too rich for a single card. Split strategy:

| Artifact Type | Split Approach |
|---------------|---------------|
| Weekly Digest (7+ signals) | One "highlights" card (top 3-5) + link to full digest |
| Competitive Brief (4+ competitors) | One executive summary card + one card per competitor |
| Release Notes (10+ features) | One summary card + one card per category (New, Improved, Preview) |
| Meeting Summary (3 artifacts) | One card per artifact (follow-up, decisions, explainer-summary) |

When splitting, each card must include:
- A clear title indicating which part it represents
- A "View All" action linking to the full markdown/docx artifact

## Output Contract

After generating card artifacts, report to the orchestrator (or PM):

```
Adaptive Card artifacts:
- <artifact>.adaptive-card.json — <N> elements, v1.5, validated for Teams
  Target: paste into Teams Adaptive Card designer or send via bot/workflow
```

## Failure Handling

- **MCP server unreachable:** Do not attempt to hand-write card JSON. Report: "Adaptive Cards MCP server is not available. Card artifacts skipped. Run `npx adaptive-cards-mcp` to verify the server is working."
- **Validation fails after 2 retries:** Save the best version with a `_draft` suffix and note: "Card has N validation issues — review in the [Adaptive Cards Designer](https://adaptivecards.io/designer/)."
- **Artifact too long for cards:** Skip card generation for that artifact. Report: "Artifact exceeds card content budget. Card output skipped — use markdown/docx instead."

## References

- Adaptive Cards schema: https://adaptivecards.io/explorer/
- Adaptive Cards Designer (paste JSON to preview): https://adaptivecards.io/designer/
- Host compatibility matrix: check via `validate_card` with `host` parameter
```
