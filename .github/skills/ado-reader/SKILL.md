```skill
---
name: ado-reader
description: 'Read Azure DevOps work items, sprint backlogs, queries, and project data. Use when the agent needs accurate sprint status, backlog items, bug counts, feature completion, or work item details from Azure DevOps. Read-only — does not create or modify work items. Triggers: ADO work items, sprint backlog, Azure DevOps, iteration status, bug count, feature status, backlog query, work item details.'
---

# ADO Reader

Query Azure DevOps (ADO) for work items, sprint backlogs, iteration status, and project data. Provides accurate, structured data about what shipped, what's in progress, and what's in the backlog — replacing indirect Work IQ inference with actual ADO state.

## When to Use This Skill

- Agent needs accurate sprint/iteration data (what shipped, what carried over)
- Building a readiness scorecard and need feature completion status
- PRD drafter needs to check existing work items for a feature area
- Sprint recap needs actual backlog state, not email-inferred status
- Any workflow that needs precise bug counts, work item status, or iteration progress

## Prerequisites

Before running this skill, ensure the Azure DevOps CLI extension is installed:

```bash
# Install Azure CLI (if not already installed)
# https://learn.microsoft.com/en-us/cli/azure/install-azure-cli

# Install the Azure DevOps extension
az extension add --name azure-devops

# Authenticate
az login

# Set default organization and project
az devops configure --defaults organization=https://dev.azure.com/<your-org> project=<your-project>
```

Verify setup:

```bash
az devops project list --output table
```

## Important: Read-Only

This skill is **read-only**. It queries ADO data but never creates, updates, or deletes work items. If a workflow needs to modify ADO, that must be done manually by the PM.

## Workflow

### Step 1 — Determine what data is needed

Based on the task context, identify:

- **Scope:** Which project? Which team? Which area path?
- **Time frame:** Current sprint? Last sprint? A date range?
- **Work item types:** User Stories, Bugs, Features, Epics, Tasks?
- **Status filter:** All states? Only closed? Only active?

### Step 2 — Query work items via CLI

#### Query by iteration (current sprint)

```bash
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State], [System.AssignedTo], [System.WorkItemType], [Microsoft.VSTS.Common.Priority] FROM WorkItems WHERE [System.IterationPath] = @CurrentIteration AND [System.TeamProject] = '<project>' ORDER BY [System.WorkItemType], [Microsoft.VSTS.Common.Priority]" --output table
```

#### Query by area path

```bash
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State], [System.WorkItemType] FROM WorkItems WHERE [System.AreaPath] UNDER '<area-path>' AND [System.State] <> 'Removed' ORDER BY [System.State]" --output table
```

#### Get specific work item details

```bash
az boards work-item show --id <work-item-id> --output json
```

#### List iterations (sprints)

```bash
az boards iteration team list --team "<team-name>" --output table
```

#### Query bugs by priority

```bash
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State], [Microsoft.VSTS.Common.Priority], [System.CreatedDate] FROM WorkItems WHERE [System.WorkItemType] = 'Bug' AND [System.AreaPath] UNDER '<area-path>' AND [System.State] <> 'Closed' ORDER BY [Microsoft.VSTS.Common.Priority]" --output table
```

#### Query recently completed items

```bash
az boards query --wiql "SELECT [System.Id], [System.Title], [System.WorkItemType], [System.State], [Microsoft.VSTS.Common.ClosedDate] FROM WorkItems WHERE [System.State] = 'Closed' AND [Microsoft.VSTS.Common.ClosedDate] >= @Today - 14 AND [System.AreaPath] UNDER '<area-path>' ORDER BY [Microsoft.VSTS.Common.ClosedDate] DESC" --output table
```

#### Query features by state for readiness

```bash
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State], [System.Tags] FROM WorkItems WHERE [System.WorkItemType] = 'Feature' AND [System.AreaPath] UNDER '<area-path>' ORDER BY [System.State]" --output table
```

### Step 3 — Summarize findings

For each query result, extract and structure:

- **Counts by state:** How many items are New / Active / Resolved / Closed?
- **Counts by type:** How many Bugs vs. Stories vs. Tasks?
- **Priority distribution:** P0 / P1 / P2 / P3 counts
- **Key items:** List notable features, high-priority bugs, or blockers by title
- **Velocity signal:** Items closed in this sprint vs. last sprint (if relevant)

### Step 4 — Write structured output

Write findings to the path specified by the calling agent (e.g., `sources/ado-status.md`):

```markdown
# Azure DevOps Status

> **Project:** <project> | **Area:** <area-path> | **Sprint:** <iteration> | **Queried:** <date>

## Summary

| Metric | Count |
|--------|-------|
| Total work items | X |
| Closed this sprint | Y |
| Active bugs (P0-P1) | Z |
| Features in progress | N |

## Completed This Sprint

| ID | Type | Title | Priority | Closed |
|----|------|-------|----------|--------|
| 12345 | User Story | <title> | P1 | 2026-03-01 |
| ... | ... | ... | ... | ... |

## Active Bugs

| ID | Title | Priority | State | Assigned To |
|----|-------|----------|-------|-------------|
| 12346 | <title> | P0 | Active | <name> |
| ... | ... | ... | ... | ... |

## Carried Forward

| ID | Type | Title | State | Notes |
|----|------|-------|-------|-------|
| 12347 | Feature | <title> | Active | Blocked on X |
| ... | ... | ... | ... | ... |

## Feature Readiness

| Feature | State | Completion | Blocking Issues |
|---------|-------|-----------|----------------|
| <feature> | Active | 7/10 stories closed | Bug #12346 |
| ... | ... | ... | ... |
```

## Common Query Patterns

| Use Case | WIQL Filter |
|----------|------------|
| Current sprint items | `[System.IterationPath] = @CurrentIteration` |
| Last sprint items | `[System.IterationPath] = @CurrentIteration - 1` |
| Open bugs by priority | `[System.WorkItemType] = 'Bug' AND [System.State] <> 'Closed'` |
| Recently closed | `[Microsoft.VSTS.Common.ClosedDate] >= @Today - 14` |
| Features for a milestone | `[System.WorkItemType] = 'Feature' AND [System.Tags] CONTAINS '<milestone-tag>'` |
| Items assigned to me | `[System.AssignedTo] = @Me` |
| Blocked items | `[System.Tags] CONTAINS 'Blocked'` |

## Rules

- **Read-only.** Never create, update, or delete work items.
- **Summarize query results.** Don't dump raw JSON into context — extract counts, key items, and status.
- **Always include the query date** so the PM knows when the snapshot was taken.
- **Use `--output table`** for quick scans; use `--output json`** when you need to parse specific fields.
- **Respect area paths.** Only query the team's own area paths, not unrelated projects.
- **If ADO CLI is not configured**, report the issue and fall back to Work IQ for indirect sprint data.

## Troubleshooting

| Issue | Solution |
|-------|----------|
| `az devops: command not found` | Run `az extension add --name azure-devops` |
| No default organization set | Run `az devops configure --defaults organization=https://dev.azure.com/<org>` |
| No default project set | Run `az devops configure --defaults project=<project>` |
| Auth failure | Run `az login` and ensure you have access to the ADO organization |
| WIQL syntax error | Check field names match ADO schema (e.g., `[System.State]` not `[State]`) |
| Empty results | Verify area path and iteration path exist; try broader query |

```
