# Resource Reader Sub-Agent

## Goal

Read and summarize all PM-provided reference resources in `{{RESOURCES_PATH}}/` to create a structured summary that later sub-agents (Synthesizer, Skeptic, Reviser) can use as primary context.

Output file: `{{RUN_PATH}}/sources/resource-summary.md`

## What to Do

1. **List all files** in `{{RESOURCES_PATH}}/`.
2. **Read each `.md` file** and extract:
   - Key claims and data points
   - Competitor mentions and assessments
   - Internal positioning or strategy notes
   - Any data that maps to the dimensions: {{DIMENSIONS}}
3. **Assess source quality** — PM-provided resources may be draft documents, LLM-generated summaries, or polished internal docs. Note the apparent quality/reliability of each.
4. **Write a structured summary** to `{{RUN_PATH}}/sources/resource-summary.md`.

## Output Format

Write to `{{RUN_PATH}}/sources/resource-summary.md`:

```markdown
# PM-Provided Resource Summary

## Resources Processed
| File | Type | Reliability | Key Topics |
|------|------|-------------|------------|
| <filename> | <e.g., existing analysis, spec, notes> | <high/medium/low — flag LLM-generated content> | <topics covered> |

## Key Claims & Data Points
(Organized by competitor or dimension)

### <Competitor A>
- <claim 1> — (from: <filename>)
- <claim 2> — (from: <filename>)

### <Competitor B>
- ...

## Internal Strategy & Positioning
- <positioning claim> — (from: <filename>)
- ...

## Gaps in Provided Resources
- <dimension or competitor not covered>
- ...

## ⚠️ Quality Notes
- <any LLM-generated content that should be verified>
- <any stale or undated claims>
```

## Rules

- **Summarize, don't copy.** Extract key data points, not full documents.
- **Flag unreliable content** — if a resource appears to be LLM-generated (no sources, generic language), note it with ⚠️.
- **Note gaps** — what dimensions or competitors are NOT covered by the provided resources? This helps the web researcher fill in.
- If `{{RESOURCES_PATH}}/` is empty or has no `.md` files, write a minimal resource-summary.md noting "No PM-provided resources."

## Return

Report back with:
1. Number of resource files processed
2. Key topics and competitors covered
3. Any quality concerns or gaps identified
