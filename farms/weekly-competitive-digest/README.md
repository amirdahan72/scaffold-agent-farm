# Weekly Competitive Digest Agent Farm

**Recurring weekly scan of competitor activity — detects product launches, pricing changes, partnerships, funding, leadership moves, and analyst coverage, then produces a concise, actionable digest with signal heat map and recommended actions.**

## Architecture

This farm uses a **real sub-agent architecture**: a slim orchestrator dispatches specialized sub-agents via `runSubagent`, each with its own isolated context window and focused instructions.

```
Orchestrator (PM interaction, dispatch, progress reporting)
    │
    ├─ Phase 1a: Web Researcher      → sources/news-scan.md, sources/competitor-signals-*.md
    ├─ Phase 1b: WorkIQ Collector    → internal-context.md
    ├─ Phase 1c: Resource Reader     → sources/resource-summary.md
    │   ↕ PM checkpoint: "Scan complete. Proceed?"
    ├─ Phase 2:  Synthesizer         → output/combined-draft.md
    ├─ Phase 3:  Skeptic             → output/review-notes.md
    │   ↕ PM checkpoint: "Review done. Proceed?"
    ├─ Phase 4:  Reviser             → output/revised-draft.md
    └─ Phase 5:  Writer              → output/weekly-digest.md
```

## How to Run

1. Open the `farms/weekly-competitive-digest/` folder in VS Code (or stay in the workspace root).
2. Open **GitHub Copilot Chat** → switch to **Agent mode**.
3. Select the **`weekly-competitive-digest`** agent from the dropdown.
4. Give it your inputs. Examples:

   > "Run this week's competitive digest for cloud security — track CrowdStrike, Palo Alto, Zscaler, and Wiz."

   > "Weekly digest for microsegmentation. Competitors: Illumio, Guardicore, Zero Networks. Audience is PM peers."

   > "Generate this week's competitive update for the leadership team. Focus on ZTNA competitors."

5. The orchestrator will:
   - Ask follow-up questions (category, competitors, time window, audience)
   - Ask you to provide reference resources (Phase 0 gate) — **tip: add last week's digest to `work/resources/` to avoid duplicates**
   - Dispatch sub-agents one by one, reporting progress after each
   - Pause for approval after collection and after review
   - Deliver the final digest

## File Structure

```
farms/weekly-competitive-digest/
├── .github/
│   └── agents/
│       └── weekly-competitive-digest.agent.md   ← orchestrator agent
├── prompts/
│   ├── web-researcher.prompt.md             ← scans recent web activity
│   ├── workiq-collector.prompt.md           ← internal M365 signals
│   ├── resource-reader.prompt.md            ← reads PM-provided resources
│   ├── synthesizer.prompt.md                ← combines into structured digest
│   ├── skeptic.prompt.md                    ← accuracy review
│   ├── reviser.prompt.md                    ← fixes issues
│   └── writer.prompt.md                     ← produces final digest
├── work/
│   ├── resources/                           ← PM-provided files (previous digests, tracking docs)
│   └── runs/                                ← per-run outputs
│       └── YYYY-MM-DD-weekly-<slug>/
│           ├── sources/                     ← collector outputs
│           ├── internal-context.md          ← WorkIQ findings
│           └── output/                      ← final deliverables
└── README.md                                ← this file
```

## Outputs

| File | Description |
|------|-------------|
| `work/runs/<slug>/output/weekly-digest.md` | Final weekly competitive digest |
| `work/runs/<slug>/output/weekly-digest.docx` | Word document version (if requested) |

### Intermediate Files (for debugging/reference)

| File | Description |
|------|-------------|
| `work/runs/<slug>/sources/news-scan.md` | Industry headlines and funding/M&A activity |
| `work/runs/<slug>/sources/competitor-signals-*.md` | Per-competitor weekly signals |
| `work/runs/<slug>/sources/resource-summary.md` | Summary of PM-provided resources |
| `work/runs/<slug>/internal-context.md` | Work IQ internal signals |
| `work/runs/<slug>/output/combined-draft.md` | Raw synthesized draft |
| `work/runs/<slug>/output/review-notes.md` | Skeptic's critique |
| `work/runs/<slug>/output/revised-draft.md` | Reviser's polished draft |

## Key Differentiators vs. Deep-Dive Competitive Analysis

| Aspect | Weekly Digest | Deep-Dive Analysis |
|--------|--------------|-------------------|
| **Cadence** | Weekly (recurring) | One-time (ad hoc) |
| **Depth** | Signal-level (headlines, moves) | Full analysis (features, pricing, battle cards) |
| **Time focus** | Last 7 days | All-time landscape |
| **Output** | Scannable digest with heat map | Comprehensive brief with battle cards |
| **Length** | ~2-4 pages | ~10-20 pages |
| **Primary value** | "What changed this week?" | "How do we compare overall?" |

## Weekly Workflow Tips

1. **After each run,** copy the final `weekly-digest.md` to `work/resources/` so next week's run can detect duplicates.
2. **Add tracking notes** to `work/resources/` for items you want the digest to watch across weeks.
3. **Review the Watch List** section — it carries forward items that need ongoing attention.

## Sub-Agents

| Phase | Sub-Agent | Prompt Template | What it does |
|-------|-----------|----------------|-------------|
| 1a | **Web Researcher** | `prompts/web-researcher.prompt.md` | Scans for recent competitor news, launches, pricing, funding, partnerships |
| 1b | **WorkIQ Collector** | `prompts/workiq-collector.prompt.md` | Gathers internal signals — competitive discussions, customer feedback, strategy |
| 1c | **Resource Reader** | `prompts/resource-reader.prompt.md` | Reads previous digests and PM notes to avoid duplication |
| 2 | **Synthesizer** | `prompts/synthesizer.prompt.md` | Combines signals into structured digest with heat map and actions |
| 3 | **Skeptic** | `prompts/skeptic.prompt.md` | Verifies signal accuracy, checks for stale/fabricated news |
| 4 | **Reviser** | `prompts/reviser.prompt.md` | Fixes all issues identified by Skeptic |
| 5 | **Writer** | `prompts/writer.prompt.md` | Produces final polished digest |

## Parameters

| Parameter | Example |
|-----------|---------|
| `{{PRODUCT_CATEGORY}}` | "cloud security" |
| `{{COMPETITORS}}` | "CrowdStrike, Palo Alto, Zscaler, Wiz" |
| `{{TIME_WINDOW}}` | "last 7 days" |
| `{{SIGNALS}}` | "product launches, pricing, partnerships, funding, leadership" |
| `{{AUDIENCE}}` | "PM peers" |
| `{{RUN_PATH}}` | `farms/weekly-competitive-digest/work/runs/2026-03-09-weekly-cloud-security` |
| `{{RESOURCES_PATH}}` | `farms/weekly-competitive-digest/work/resources` |

## Prerequisites

| Requirement | Setup |
|-------------|-------|
| Work IQ CLI | `npm install -g @microsoft/workiq` then `workiq accept-eula` |
| GitHub Copilot | Agent mode enabled in VS Code |
