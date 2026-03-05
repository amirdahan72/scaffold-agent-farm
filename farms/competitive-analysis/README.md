# Competitive Analysis Agent Farm

**Deep-dive competitive analysis using real sub-agents — discovers competitors, researches features, pricing, and positioning, augments with internal context, and produces a polished competitive brief with battle cards and strategic recommendations.**

## Architecture

This farm uses a **real sub-agent architecture**: a slim orchestrator dispatches specialized sub-agents via `runSubagent`, each with its own isolated context window and focused instructions.

```
Orchestrator (PM interaction, dispatch, progress reporting)
    │
    ├─ Phase 1a: Web Researcher      → sources/*.md
    ├─ Phase 1b: WorkIQ Collector    → internal-context.md
    ├─ Phase 1c: Resource Reader     → sources/resource-summary.md
    │   ↕ PM checkpoint: "Collection done. Proceed?"
    ├─ Phase 2:  Synthesizer         → output/combined-draft.md
    │   ↕ PM checkpoint: "Draft ready. Review or proceed?"
    ├─ Phase 3:  Skeptic              → output/review-notes.md
    │   ↕ PM checkpoint: "Critique ready. Review findings or proceed?"
    ├─ Phase 4:  Reviser             → output/revised-draft.md
    └─ Phase 5:  Writer              → output/competitive-brief.md
```

### Why Sub-Agents?

| Benefit | How |
|---------|-----|
| **Clean context** | Each sub-agent gets a fresh context window — no overflow from accumulated research |
| **Debuggable** | Each prompt template is a standalone file you can test/tweak independently |
| **Composable** | Prompt templates can be shared across farms |
| **PM stays in the loop** | Orchestrator reports progress and pauses for approval between phases |

## How to Run

1. Open the `farms/competitive-analysis/` folder in VS Code (or stay in the workspace root).
2. Open **GitHub Copilot Chat** → switch to **Agent mode**.
3. Select the **`competitive-analysis`** agent from the dropdown.
4. Give it your inputs. Examples:

   > "Build a competitive analysis of AI code completion tools — compare GitHub Copilot, Cursor, Tabnine, and Amazon CodeWhisperer. Audience is PM peers."

   > "Deep-dive competitive analysis of enterprise project management platforms. Discover the top competitors. This is for a leadership presentation."

5. The orchestrator will:
   - Ask follow-up questions (product category, competitors, dimensions, audience)
   - Ask you to provide reference resources (Phase 0 gate)
   - Dispatch sub-agents one by one, reporting progress after each
   - Pause for your approval after collection and after synthesis
   - Deliver the final brief

## File Structure

```
farms/competitive-analysis/
├── .github/
│   └── agents/
│       └── competitive-analysis.agent.md   ← orchestrator agent
├── prompts/                                 ← sub-agent prompt templates
│   ├── web-researcher.prompt.md             ← web research collector
│   ├── workiq-collector.prompt.md           ← internal M365 context collector
│   ├── resource-reader.prompt.md            ← reads PM-provided resources
│   ├── synthesizer.prompt.md                ← combines all sources into draft
│   ├── skeptic.prompt.md                    ← adversarial review — finds problems
│   ├── reviser.prompt.md                    ← systematic fixer — addresses critique
│   └── writer.prompt.md                     ← produces final deliverable
├── work/
│   ├── resources/                           ← PM-provided files (shared across runs)
│   └── runs/                                ← per-run outputs
│       └── YYYY-MM-DD-<slug>/
│           ├── sources/                     ← collector outputs
│           ├── internal-context.md          ← WorkIQ findings
│           └── output/                      ← final deliverables
├── README.md                                ← this file
└── MIGRATION-PLAN.md                        ← architecture rationale
```

## Outputs

| File | Description |
|------|-------------|
| `work/runs/<slug>/output/competitive-brief.md` | Final competitive analysis brief |
| `work/runs/<slug>/output/competitive-brief.docx` | Word document version (if requested) |
| `work/runs/<slug>/output/competitive-analysis.pptx` | Slide deck version (if requested) |

### Intermediate Files (for debugging/reference)

| File | Description |
|------|-------------|
| `work/runs/<slug>/sources/market-landscape.md` | Market overview and competitor discovery |
| `work/runs/<slug>/sources/competitor-*.md` | Individual competitor deep-dives |
| `work/runs/<slug>/sources/head-to-head.md` | Third-party comparisons |
| `work/runs/<slug>/sources/resource-summary.md` | Summary of PM-provided resources |
| `work/runs/<slug>/sources/sharepoint-findings.md` | SharePoint resource summaries |
| `work/runs/<slug>/internal-context.md` | Work IQ findings (internal) |
| `work/runs/<slug>/output/combined-draft.md` | Raw synthesized draft |
| `work/runs/<slug>/output/review-notes.md` | Skeptic's critique (issues found) |
| `work/runs/<slug>/output/revised-draft.md` | Reviser's polished draft (issues fixed) |

## Sub-Agents

| Phase | Sub-Agent | Prompt Template | What it does |
|-------|-----------|----------------|-------------|
| 1a | **Web Researcher** | `prompts/web-researcher.prompt.md` | Discovers competitors, researches pricing, features, market share via web search |
| 1b | **WorkIQ Collector** | `prompts/workiq-collector.prompt.md` | Gathers internal competitive context, differentiators, win/loss data, roadmap |
| 1c | **Resource Reader** | `prompts/resource-reader.prompt.md` | Reads PM-provided markdown files and summarizes them |
| 2 | **Synthesizer** | `prompts/synthesizer.prompt.md` | Combines all sources into a structured comparison with battle cards |
| 3 | **Skeptic** | `prompts/skeptic.prompt.md` | Pure adversarial review — finds unsupported claims, bias, gaps. Writes critique, does NOT fix |
| 4 | **Reviser** | `prompts/reviser.prompt.md` | Systematically fixes every issue the Skeptic identified |
| 5 | **Writer** | `prompts/writer.prompt.md` | Produces the final polished competitive brief |

## PM Interaction Points

The orchestrator keeps you in the loop at these points:

| When | What happens |
|------|-------------|
| **Step 1** | Orchestrator asks you for product category, competitors, dimensions, audience |
| **Phase 0** | Orchestrator asks you to add resources and waits for approval |
| **After collection** | Orchestrator reports what was collected, asks to proceed or adjust |
| **After synthesis** | Orchestrator offers you the chance to review the draft before quality review |
| **After critique** | Orchestrator shares the Skeptic's findings — you can review before revision proceeds |
| **After completion** | Orchestrator delivers the final brief with a summary |

## Customizing Sub-Agents

Each sub-agent is defined by a prompt template in `prompts/`. To customize:

1. **Edit the prompt template** directly — it's just a markdown file with `{{PARAMETER}}` markers
2. **Add a new sub-agent** — create a new `.prompt.md` file and add a dispatch entry to the orchestrator
3. **Reuse across farms** — copy a prompt template to another farm's `prompts/` folder

Parameters are injected by the orchestrator at runtime:

| Parameter | Example |
|-----------|---------|
| `{{PRODUCT_CATEGORY}}` | "microsegmentation" |
| `{{COMPETITORS}}` | "Illumio, Zero Networks, Guardicore" |
| `{{DIMENSIONS}}` | "pricing, features, enterprise readiness" |
| `{{AUDIENCE}}` | "PM peers" |
| `{{RUN_PATH}}` | `farms/competitive-analysis/work/runs/2026-03-04-microseg` |
| `{{RESOURCES_PATH}}` | `farms/competitive-analysis/work/resources` |

## Prerequisites

| Requirement | Setup |
|-------------|-------|
| Work IQ CLI | `npm install -g @microsoft/workiq` then `workiq accept-eula` |
| GitHub Copilot | Agent mode enabled in VS Code |
