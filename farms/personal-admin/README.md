# Personal Admin Agent Farm

A Work IQ-powered personal admin assistant that gathers your emails, meetings, chats, and action items from Microsoft 365 — then synthesizes them into polished, actionable deliverables.

## What It Does

This agent farm queries Work IQ to surface your internal M365 context and produces one of several deliverable types:

| Deliverable | Description |
|-------------|-------------|
| **Daily Briefing** | Priority items, key emails, today's meetings with context, outstanding action items |
| **Weekly Summary** | Accomplishments, decisions, open items, next-week priorities |
| **Meeting Prep Pack** | Attendee context, prior threads, open items, talking points |
| **Action Item Tracker** | Consolidated commitments and follow-ups from emails/meetings/chats |
| **Custom** | Any personal-admin output you describe |

## How to Run

1. Open GitHub Copilot Chat → **Agent mode**.
2. Select **personal-admin** from the agents dropdown.
3. Tell it what you need — e.g., *"Give me a daily briefing"* or *"Prepare me for my 2pm meeting with Sarah."*
4. Optionally add reference files (priority lists, project context) to `work/resources/`.
5. Let it run — it will checkpoint after collection and after critique for your review.

## Pipeline

```
Phase 0: Resource Gate     → PM adds optional reference files
Phase 1: Work IQ Collector → Queries emails, meetings, chats, action items
Phase 2: Synthesizer       → Combines into structured draft
Phase 3: Skeptic           → Adversarial review (finds gaps)
Phase 4: Reviser           → Fixes all critique issues
Phase 5: Writer            → Polished final deliverable
```

## Output Location

Each run creates a timestamped folder:

```
work/runs/YYYY-MM-DD-<slug>/
├── internal-context.md    ← Raw Work IQ findings
└── output/
    ├── combined-draft.md  ← Synthesizer output
    ├── review-notes.md    ← Skeptic critique
    ├── revised-draft.md   ← Reviser corrections
    └── <deliverable>.md   ← Final polished output
```

## Prerequisites

- **Work IQ CLI** installed: `npm install -g @microsoft/workiq`
- **EULA accepted**: `workiq accept-eula`
- **Microsoft 365** account with Copilot license
- Verify: `workiq version`

## Skills Used

| Skill | Purpose |
|-------|---------|
| `workiq-context` | Work IQ query patterns and output formatting |
| `doc-writer` | Structured markdown document production |
