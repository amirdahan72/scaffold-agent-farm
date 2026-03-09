# Release Notes Creator

An agent farm that produces customer-facing release notes from Azure DevOps feature data and internal Work IQ context.

## What It Does

Coordinates a team of sub-agents to:

1. **Collect** completed features from Azure DevOps (filtered by area path and date range)
2. **Enrich** with internal context from Work IQ (customer requests, messaging, limitations)
3. **Synthesize** into a structured release notes draft
4. **Review** the draft for accuracy, completeness, and tone
5. **Revise** based on critique findings
6. **Produce** a polished Word document (.docx)

## How to Run

1. Open GitHub Copilot Chat → **Agent mode**
2. Select **release-notes-creator** from the agents dropdown
3. Tell it what you need, e.g.: _"Create release notes for Azure Firewall from October 2025 to March 2026"_
4. Answer the intake questions (product, date range, ADO project/area path)
5. Optionally add reference files to `work/resources/`
6. Let it run — it will pause for your approval after collection and after synthesis

## Prerequisites

- **Azure DevOps CLI:** `az extension add --name azure-devops` + `az login`
- **Work IQ CLI:** `npm install -g @microsoft/workiq` + `workiq accept-eula`
- **Node.js** (for docx generation): `npm install docx`

## Outputs

Each run creates a folder under `work/runs/YYYY-MM-DD-<slug>/`:

```
work/runs/2026-03-08-oct2025-mar2026-release/
├── sources/
│   ├── features.md          ← ADO features data
│   └── index.md             ← source file listing
├── internal-context.md       ← Work IQ findings
└── output/
    ├── combined-draft.md     ← initial synthesis
    ├── review-notes.md       ← skeptic critique
    ├── revised-draft.md      ← post-review revision
    ├── release-notes.md      ← final markdown
    └── release-notes.docx    ← final Word document
```

## Sub-Agent Pipeline

| Phase | Agent | Source |
|-------|-------|--------|
| 1a | ADO Collector | Azure DevOps features |
| 1b | Work IQ Collector | Internal M365 context |
| 2 | Synthesizer | Combines into draft |
| 3 | Skeptic | Adversarial review |
| 4 | Reviser | Fixes all issues |
| 5 | Writer | Final .docx output |

## Skills Used

- `ado-reader` — Azure DevOps queries
- `workiq-context` — Internal M365 context
- `doc-writer` — Markdown formatting
- `docx-writer` — Word document generation
