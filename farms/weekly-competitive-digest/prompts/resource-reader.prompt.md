# Resource Reader Sub-Agent

## Goal

Read and summarize all PM-provided reference resources in `{{RESOURCES_PATH}}/` — especially previous weekly digests, competitor tracking docs, and any notes the PM added for this week's run.

Output file: `{{RUN_PATH}}/sources/resource-summary.md`

## What to Do

1. **List all files** in `{{RESOURCES_PATH}}/`.
2. **Read each `.md` file** and extract:
   - Key competitor mentions and assessments
   - Previous digest highlights (to avoid repeating stale news)
   - Tracking notes, watch items, or follow-ups from prior weeks
   - Any data relevant to: {{SIGNALS}}
3. **Assess source quality** — note whether each resource is a previous digest, raw notes, or polished analysis.
4. **Write a structured summary** to `{{RUN_PATH}}/sources/resource-summary.md`.

## Output Format

Write to `{{RUN_PATH}}/sources/resource-summary.md`:

```markdown
# PM-Provided Resource Summary

## Resources Processed
| File | Type | Key Topics |
|------|------|------------|
| <filename> | <previous digest / tracking doc / notes> | <topics covered> |

## Previously Reported Signals
(Signals from past digests — avoid re-reporting these as new)
- <signal already covered> — (from: <filename>)

## Active Watch Items
(Things the PM flagged to keep tracking)
- <watch item> — (from: <filename>)

## Competitor Notes & Context
### <Competitor A>
- <context> — (from: <filename>)

### <Competitor B>
- <context> — (from: <filename>)

## Gaps & Missing Coverage
- <competitors or signal types not covered in resources>
```

## Rules

- **Summarize, don't copy.** Extract key data points, not full documents.
- **Identify already-reported news** — this prevents the digest from repeating last week's headlines.
- **Note watch items** — things the PM wants tracked across weeks.
- If `{{RESOURCES_PATH}}/` is empty or has no `.md` files, write a minimal resource-summary.md noting "No PM-provided resources."

## Return

Report back with:
1. Number of resource files processed
2. Key watch items or follow-ups identified
3. Number of previously reported signals (to avoid duplication)
