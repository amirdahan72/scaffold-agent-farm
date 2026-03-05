# North Star Strategy Agent Farm

**Forward-looking strategic vision papers  researches market trends, technology shifts, competitive dynamics, and internal context, then produces a bold North Star strategy with strategic pillars, bets, phased roadmap, and measurable success metrics.**

## Architecture

This farm uses a **real sub-agent architecture**: a slim orchestrator dispatches specialized sub-agents via `runSubagent`, each with its own isolated context window and focused instructions.

```
Orchestrator (PM interaction, dispatch, progress reporting)
    
     Phase 1a: Web Researcher       sources/*.md
     Phase 1b: WorkIQ Collector     internal-context.md
     Phase 1c: Resource Reader      sources/resource-summary.md
        PM checkpoint: "Collection done. Proceed?"
     Phase 2:  Synthesizer          output/combined-draft.md
        PM checkpoint: "Draft ready. Review or proceed?"
     Phase 3:  Skeptic              output/review-notes.md
        PM checkpoint: "Critique ready. Review findings or proceed?"
     Phase 4:  Reviser              output/revised-draft.md
     Phase 5:  Writer               output/north-star-strategy.md
```

### Why Sub-Agents?

| Benefit | How |
|---------|-----|
| **Clean context** | Each sub-agent gets a fresh context window  no overflow from accumulated research |
| **Debuggable** | Each prompt template is a standalone file you can test/tweak independently |
| **Composable** | Prompt templates can be shared across farms |
| **PM stays in the loop** | Orchestrator reports progress and pauses for approval between phases |

## How to Run

1. Open the `farms/north-star-strategy/` folder in VS Code (or stay in the workspace root).
2. Open **GitHub Copilot Chat**  switch to **Agent mode**.
3. Select the **`north-star-strategy`** agent from the dropdown.
4. Give it your inputs. Examples:

   > "Write a North Star strategy for our developer platform. 5-year horizon. Focus on AI-native developer experiences and ecosystem expansion."

   > "Create a strategic vision paper for Azure networking. 3-year horizon. Key question: how do we defend against cloud-native networking disruptors?"

   > "Build a North Star paper for our security portfolio. Focus on platform convergence, AI-driven threat detection, and zero trust evolution. Audience is the VP staff."

5. The orchestrator will:
   - Ask follow-up questions (initiative, time horizon, strategic themes, audience)
   - Ask you to provide reference resources (Phase 0 gate)
   - Dispatch sub-agents one by one, reporting progress after each
   - Pause for your approval after collection and after synthesis
   - Deliver the final strategic paper

## File Structure

```
farms/north-star-strategy/
 .github/
    agents/
        north-star-strategy.agent.md     orchestrator agent
 prompts/                                  sub-agent prompt templates
    web-researcher.prompt.md              market research collector
    workiq-collector.prompt.md            internal M365 context collector
    resource-reader.prompt.md             reads PM-provided resources
    synthesizer.prompt.md                 combines all sources into draft
    skeptic.prompt.md                     adversarial review  finds problems
    reviser.prompt.md                     systematic fixer  addresses critique
    writer.prompt.md                      produces final deliverable
 work/
    resources/                            PM-provided files (shared across runs)
    runs/                                 per-run outputs
        YYYY-MM-DD-<slug>/
            sources/                      collector outputs
            internal-context.md           WorkIQ findings
            output/                       final deliverables
 README.md                                 this file
```

## Outputs

| File | Description |
|------|-------------|
| `work/runs/<slug>/output/north-star-strategy.md` | Final North Star strategic paper |
| `work/runs/<slug>/output/north-star-strategy.docx` | Word document version (if requested) |
| `work/runs/<slug>/output/north-star-strategy.pptx` | Slide deck version (if requested) |

### Intermediate Files (for debugging/reference)

| File | Description |
|------|-------------|
| `work/runs/<slug>/sources/market-trends.md` | Market trends and forecast |
| `work/runs/<slug>/sources/technology-shifts.md` | Emerging technology analysis |
| `work/runs/<slug>/sources/competitive-dynamics.md` | Competitive landscape and disruption vectors |
| `work/runs/<slug>/sources/market-sizing.md` | TAM/SAM growth projections |
| `work/runs/<slug>/sources/customer-evolution.md` | Buyer and segment evolution |
| `work/runs/<slug>/sources/platform-ecosystem.md` | Platform and ecosystem strategy |
| `work/runs/<slug>/sources/business-model-trends.md` | Monetization and business model shifts |
| `work/runs/<slug>/sources/analyst-perspectives.md` | Analyst predictions and forecasts |
| `work/runs/<slug>/sources/resource-summary.md` | Summary of PM-provided resources |
| `work/runs/<slug>/sources/sharepoint-findings.md` | SharePoint resource summaries |
| `work/runs/<slug>/internal-context.md` | Work IQ findings (internal) |
| `work/runs/<slug>/output/combined-draft.md` | Raw synthesized draft |
| `work/runs/<slug>/output/review-notes.md` | Skeptic's critique (issues found) |
| `work/runs/<slug>/output/revised-draft.md` | Reviser's polished draft (issues fixed) |

## Sub-Agents

| Phase | Sub-Agent | Prompt Template | What it does |
|-------|-----------|----------------|-------------|
| 1a | **Web Researcher** | `prompts/web-researcher.prompt.md` | Researches market trends, technology shifts, competitive dynamics, market sizing, analyst forecasts via web |
| 1b | **WorkIQ Collector** | `prompts/workiq-collector.prompt.md` | Gathers internal strategic context  vision, priorities, bets, risks, open debates |
| 1c | **Resource Reader** | `prompts/resource-reader.prompt.md` | Reads PM-provided markdown files and summarizes strategic positions |
| 2 | **Synthesizer** | `prompts/synthesizer.prompt.md` | Combines all sources into a structured North Star paper with vision, pillars, bets, roadmap |
| 3 | **Skeptic** | `prompts/skeptic.prompt.md` | Pure adversarial review  finds unsupported claims, weak bets, hedge language. Writes critique, does NOT fix |
| 4 | **Reviser** | `prompts/reviser.prompt.md` | Systematically fixes every issue the Skeptic identified |
| 5 | **Writer** | `prompts/writer.prompt.md` | Produces the final polished North Star strategy paper |

## PM Interaction Points

The orchestrator keeps you in the loop at these points:

| When | What happens |
|------|-------------|
| **Step 1** | Orchestrator asks for initiative, time horizon, strategic themes, audience |
| **Phase 0** | Orchestrator asks you to add resources and waits for approval |
| **After collection** | Orchestrator reports what was collected, asks to proceed or adjust |
| **After synthesis** | Orchestrator offers you the chance to review the draft before quality review |
| **After critique** | Orchestrator shares the Skeptic's findings  you can review before revision proceeds |
| **After completion** | Orchestrator delivers the final strategy paper with a summary |

## Customizing Sub-Agents

Each sub-agent is defined by a prompt template in `prompts/`. To customize:

1. **Edit the prompt template** directly  it's just a markdown file with `{{PARAMETER}}` markers
2. **Add a new sub-agent**  create a new `.prompt.md` file and add a dispatch entry to the orchestrator
3. **Reuse across farms**  copy a prompt template to another farm's `prompts/` folder

Parameters are injected by the orchestrator at runtime:

| Parameter | Example |
|-----------|---------|
| `{{PRODUCT_INITIATIVE}}` | "Azure networking" |
| `{{TIME_HORIZON}}` | "3-year" |
| `{{HORIZON_YEAR}}` | "2029" |
| `{{STRATEGIC_THEMES}}` | "technology shifts, competitive dynamics, platform strategy" |
| `{{AUDIENCE}}` | "VP-level leadership" |
| `{{RUN_PATH}}` | `farms/north-star-strategy/work/runs/2026-03-04-azure-networking` |
| `{{RESOURCES_PATH}}` | `farms/north-star-strategy/work/resources` |

## Skills Used

| Skill | Purpose |
|-------|---------|
| `web-search` | Public market research, analyst reports, competitive intelligence |
| `workiq-context` | Internal M365 context (emails, meetings, chats) |
| `doc-writer` | Final markdown document production |
| `docx-writer` | Word document generation (optional) |
| `ppt-creator` | Slide deck generation (optional) |
| `xlsx-writer` | Excel workbooks (metrics scorecards, market sizing tables) |
| `chart-creator` | PNG/SVG charts (TAM projections, growth curves, competitive maps) |

## Prerequisites

| Requirement | Setup |
|-------------|-------|
| Work IQ CLI | `npm install -g @microsoft/workiq` then `workiq accept-eula` |
| GitHub Copilot | Agent mode enabled in VS Code |