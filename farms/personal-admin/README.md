# Personal Admin Agent Farm

**Your AI personal admin  gathers your calendar, Teams messages, emails, and tasks via Work IQ, then builds a prioritized work plan with conflict detection, anomaly alerts, and actionable recommendations.**

## Architecture

This farm uses a **real sub-agent architecture**: a slim orchestrator dispatches specialized sub-agents via `runSubagent`, each with its own isolated context window and focused instructions.

```
Orchestrator (user interaction, dispatch, progress reporting)
    
     Phase 1a: WorkIQ Collector     sources/calendar.md, teams-messages.md, tasks-and-email.md
     Phase 1b: Resource Reader      sources/resource-summary.md
        User checkpoint: "Collection done. Proceed?"
     Phase 2:  Synthesizer          output/combined-draft.md
        User checkpoint: "Draft ready. Review or proceed?"
     Phase 3:  Skeptic              output/review-notes.md
        User checkpoint: "Issues found. Proceed with fixes?"
     Phase 4:  Reviser              output/revised-draft.md
     Phase 5:  Writer               output/weekly-plan.md
```

### Why Sub-Agents?

| Benefit | How |
|---------|-----|
| **Clean context** | Each sub-agent gets a fresh context window  no overflow from accumulated data |
| **Debuggable** | Each prompt template is a standalone file you can test/tweak independently |
| **Composable** | Prompt templates can be shared across farms |
| **User stays in the loop** | Orchestrator reports progress and pauses for approval between phases |

## How to Run

1. Open the `farms/personal-admin/` folder in VS Code (or stay in the workspace root).
2. Open **GitHub Copilot Chat**  switch to **Agent mode**.
3. Select the **`personal-admin`** agent from the dropdown.
4. Say something like:

   > "Plan my work for the next week."

   > "What are my highest priority items right now?"

   > "Build me a weekly plan and highlight anything urgent."

5. The orchestrator will:
   - Ask about planning horizon and preferences
   - Optionally ask for priority lists or reference docs
   - Dispatch sub-agents to collect and analyze your M365 data
   - Report progress after each phase
   - Deliver a prioritized work plan with action items

## File Structure

```
farms/personal-admin/
 .github/
    agents/
        personal-admin.agent.md          orchestrator agent
 prompts/                                  sub-agent prompt templates
    workiq-collector.prompt.md            calendar, Teams, email/task collector
    resource-reader.prompt.md             reads PM-provided priority lists
    synthesizer.prompt.md                combines data into prioritized plan
    skeptic.prompt.md                     finds conflicts, gaps, overloaded days
    reviser.prompt.md                     fixes all issues from skeptic review
    writer.prompt.md                      produces final work plan
 work/
    resources/                            PM-provided files (shared across runs)
    runs/                                 per-run outputs
        YYYY-MM-DD-<period>/
            sources/                      collector outputs
            output/                       final deliverables
 README.md                                 this file
```

## Outputs

| File | Description |
|------|-------------|
| `work/runs/<slug>/output/weekly-plan.md` | Your prioritized weekly/monthly work plan |
| `work/runs/<slug>/output/weekly-plan.docx` | Word document version (if requested) |

### Intermediate Files (for debugging/reference)

| File | Description |
|------|-------------|
| `work/runs/<slug>/sources/calendar.md` | Calendar events and meetings |
| `work/runs/<slug>/sources/teams-messages.md` | Teams messages with anomaly detection and timestamp decoding |
| `work/runs/<slug>/sources/tasks-and-email.md` | Tasks, deadlines, and email follow-ups |
| `work/runs/<slug>/sources/resource-summary.md` | Summary of PM-provided priorities |
| `work/runs/<slug>/output/combined-draft.md` | Raw synthesized work plan |
| `work/runs/<slug>/output/review-notes.md` | Skeptic's critique (conflicts, gaps) |
| `work/runs/<slug>/output/revised-draft.md` | Reviser's fixed plan |

## Sub-Agents

| Phase | Sub-Agent | Prompt Template | What it does |
|-------|-----------|----------------|-------------|
| 1a | **WorkIQ Collector** | `prompts/workiq-collector.prompt.md` | Gathers calendar, Teams messages, tasks/email via Work IQ. Decodes timestamps, runs anomaly detection |
| 1b | **Resource Reader** | `prompts/resource-reader.prompt.md` | Reads PM-provided priority lists, OKRs, and context documents |
| 2 | **Synthesizer** | `prompts/synthesizer.prompt.md` | Combines all data into a priority matrix + day-by-day plan |
| 3 | **Skeptic** | `prompts/skeptic.prompt.md` | Finds scheduling conflicts, overloaded days, dropped items, priority miscalibration. Does NOT fix |
| 4 | **Reviser** | `prompts/reviser.prompt.md` | Systematically fixes every issue the Skeptic identified, surfaces recommended actions |
| 5 | **Writer** | `prompts/writer.prompt.md` | Produces the final polished work plan with action items at the top |

## Special Features

### Timestamp Decoding
The WorkIQ Collector extracts epoch-millisecond message IDs from Teams URLs and converts them to precise timestamps. This ensures triage decisions use real times, not ambiguous labels like "this morning."

### Anomaly Detection
The collector automatically flags 7 types of signals in Teams messages:
1. Messages sent outside business hours (urgency indicator)
2. Direct name-mentions in message body (direct asks)
3. Unanswered direct questions
4. Senior stakeholder messages at odd hours (escalation pressure)
5. Personnel changes (ownership gaps)
6. Security/CVE/incident keywords
7. Competitive pressure signals

### Priority Levels

| Level | Meaning |
|-------|---------|
| **P0** | Hard deadline this week, or escalated by leadership |
| **P1** | Someone actively waiting on your response, or meetings needing prep |
| **P2** | Important but flexible deadline, strategic work |
| **P3** | Informational, nice-to-do, low urgency |

## User Interaction Points

| When | What happens |
|------|-------------|
| **Step 1** | Orchestrator asks for planning horizon and preferences |
| **Phase 0** | Orchestrator offers to load priority lists or reference docs |
| **After collection** | Orchestrator reports meetings found, anomalies flagged, tasks identified |
| **After synthesis** | Orchestrator shows priority breakdown, offers draft review |
| **After critique** | Orchestrator shares conflicts and issues found |
| **After completion** | Orchestrator delivers final plan with action item summary |

## Customizing Sub-Agents

Each sub-agent is defined by a prompt template in `prompts/`. To customize:

1. **Edit the prompt template** directly  it's just a markdown file with `{{PARAMETER}}` markers
2. **Add a new sub-agent**  create a new `.prompt.md` file and add a dispatch entry to the orchestrator
3. **Reuse across farms**  copy a prompt template to another farm's `prompts/` folder

Parameters are injected by the orchestrator at runtime:

| Parameter | Example |
|-----------|---------|
| `{{PLANNING_PERIOD}}` | "next 7 days" |
| `{{RUN_PATH}}` | `farms/personal-admin/work/runs/2026-03-04-weekly` |
| `{{RESOURCES_PATH}}` | `farms/personal-admin/work/resources` |

## Skills Used

| Skill | Purpose |
|-------|---------|
| `workiq-context` | Internal M365 data (calendar, Teams, email, tasks) |
| `doc-writer` | Final markdown work plan |
| `docx-writer` | Word document generation (optional) |
| `xlsx-writer` | Excel workbooks (priority matrices, task trackers) |
| `chart-creator` | PNG/SVG charts (workload distribution, meeting heat maps) |

## Prerequisites

| Requirement | Setup |
|-------------|-------|
| Work IQ CLI | `npm install -g @microsoft/workiq` then `workiq accept-eula` |
| GitHub Copilot | Agent mode enabled in VS Code |

## Not Yet Supported

- Calendar management (schedule, cancel, decline, accept, edit meetings)  requires `Calendars.ReadWrite` Graph scope, not currently available
- Teams messaging (send messages to direct reports, manager, peers)  requires `Chat.ReadWrite` and `ChatMessage.Send` Graph scopes, not currently available