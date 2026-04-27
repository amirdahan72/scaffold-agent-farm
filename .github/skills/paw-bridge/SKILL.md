---
name: paw-bridge
description: "Automates the truth-sync loop between PAW (PM AI Workspace) and agent farms. Reads truth files from PAW before a farm run and writes new facts back after. Use when a farm orchestrator needs to 'load PAW truth', 'sync back to PAW', 'import product truth', or 'update PAW with findings'."
---

# PAW Bridge

Automates the handoff between PAW (PM AI Workspace) and agent farms. Eliminates manual file copying.

## What This Skill Does

1. **Pre-run: Pull truth from PAW** — reads `products/`, `people/`, `personal/lessons.md` from the PAW workspace and writes them into the farm's `work/resources/paw-truth/` folder.
2. **Post-run: Push findings back to PAW** — reads the farm's final output, extracts net-new facts (decisions, risks, quotes, open questions), and writes update files that PAW can absorb.

## Setup (One-Time)

The farm workspace needs a `paw-config.json` at its root (or the farm folder root):

```json
{
  "paw_root": "C:\\Repo\\amir-agent-packs\\PM AI Workspace",
  "auto_pull": {
    "products": true,
    "people": true,
    "lessons": true,
    "workstreams": false
  },
  "auto_push": {
    "update_products": true,
    "update_lessons": true,
    "copy_artifact": true
  },
  "product_filter": [],
  "people_filter": []
}
```

**Fields:**
- `paw_root` — absolute path to the PAW workspace. The only thing the PM configures.
- `auto_pull` — which PAW truth categories to pull before a run. All default true except workstreams.
- `auto_push` — what to write back after a run.
- `product_filter` — if non-empty, only pull these product files (e.g., `["ai-tools.md", "search.md"]`). Empty = pull all.
- `people_filter` — same for stakeholder cards. Empty = pull all.

## Pre-Run: Pull Truth

Called by the farm orchestrator at **Phase 0** (before resource-reader). The orchestrator (or a dedicated pre-phase) does this:

### Step 1 — Read config

```
Read paw-config.json from farm root (or workspace root).
If missing, ask the PM: "Where is your PAW workspace?" and create the config.
```

### Step 2 — Pull product truth

```
For each .md file in {paw_root}/products/ (excluding _template-*.md):
  - If product_filter is set and file not in filter → skip.
  - Read the file.
  - Write to work/resources/paw-truth/products/<filename>.
```

### Step 3 — Pull stakeholder cards

```
For each .md file in {paw_root}/people/ (excluding _template-*.md):
  - If people_filter is set and file not in filter → skip.
  - Read the file.
  - Write to work/resources/paw-truth/people/<filename>.
```

### Step 4 — Pull lessons

```
Read {paw_root}/personal/lessons.md
Write to work/resources/paw-truth/lessons.md
```

### Step 5 — Pull workstreams (optional)

```
If auto_pull.workstreams is true:
  For each .md file in {paw_root}/workstreams/ (excluding _template-*.md and _system/):
    Write to work/resources/paw-truth/workstreams/<filename>.
```

### Result

After pull, the farm's `work/resources/` looks like:

```
work/resources/
├── paw-truth/                    ← auto-pulled (DO NOT EDIT)
│   ├── products/
│   │   └── ai-tools.md
│   ├── people/
│   │   ├── amit.md
│   │   └── sarah.md
│   ├── workstreams/              ← optional
│   │   └── ai-gateway-launch.md
│   └── lessons.md
├── personas/                     ← PM-provided (manual, study-specific)
│   └── power-users.csv
└── sharepoint-links.md           ← PM-provided
```

Farm subagents treat `paw-truth/` as **read-only primary context**. They never modify these files.

## Post-Run: Push Findings Back

Called by the farm orchestrator as the **final phase** (after writer). A dedicated `truth-sync` subagent does this:

### Step 1 — Read the final artifact

Read the farm's output (e.g., `work/runs/<slug>/output/study.md` or `output/prd.md`).

### Step 2 — Extract net-new facts

Compare the artifact against the PAW truth files already in `work/resources/paw-truth/`. Identify:

- **New decisions** not already in `products/<x>.md → Recent Decisions`
- **New risks** not already in `products/<x>.md → Risks & Blockers`
- **New stakeholder quotes** worth preserving
- **New open questions** surfaced by the farm
- **Lesson candidates** — if the Skeptic/Reviser flagged recurring issues, they may be worth logging

### Step 3 — Write update files (not direct edits)

Do NOT write directly to PAW files. Instead, write **update proposal files** to the farm's output:

```
work/runs/<slug>/output/paw-updates/
├── products/
│   └── ai-tools.update.md       ← proposed additions to products/ai-tools.md
├── people/
│   └── amit.update.md            ← proposed additions (if any)
├── lessons.update.md             ← proposed new lesson rules
└── update-summary.md             ← human-readable summary of all proposed changes
```

Each `.update.md` file uses a structured format:

```markdown
# Proposed Updates to products/ai-tools.md
> Source: competitive-brief farm run 2026-04-26

## Append to: Recent Decisions
| Date | Decision | Context |
|------|----------|---------|
| 2026-04-26 | ... | Per competitive analysis: ... |

## Append to: Risks & Blockers
| Risk | Impact | Mitigation |
|------|--------|------------|
| ... | ... | ... |

## Append to: Open Questions
- [ ] ...
```

### Step 4 — Apply updates (with confirmation)

The orchestrator reads `update-summary.md` and presents it to the PM via `vscode_askQuestions`:

```
"Farm run complete. I have N proposed updates to PAW truth files:
 - 2 new decisions for products/ai-tools.md
 - 1 new risk
 - 1 lesson candidate

Options: [Apply all to PAW] [Let me review first] [Skip — I'll do it manually]"
```

If the PM approves:
- Read each `.update.md`
- Read the corresponding PAW truth file from `{paw_root}/...`
- Append the new rows to the appropriate sections
- Write back to the PAW file

If "review first": the PM reads `output/paw-updates/update-summary.md` and comes back.

## Why Update Files Instead of Direct Writes

1. **Auditability** — the PM can see exactly what the farm wants to change before it touches truth.
2. **Conflict safety** — if the PM ran PAW between farm start and farm end, direct writes could overwrite changes. Update files are additive.
3. **Batch review** — all proposed changes in one summary, one approval gate.
4. **Rollback** — update files persist in `work/runs/<slug>/output/paw-updates/` for history.

## Integration with Farm Orchestrators

### For the orchestrator's Phase 0 (add before resource-reader):

```markdown
## Phase 0a — PAW Truth Pull

1. Read `paw-config.json` from the farm root.
   - If missing: use `vscode_askQuestions` to ask for PAW workspace path, then create the config.
2. Use the `paw-bridge` skill to pull truth into `work/resources/paw-truth/`.
3. Report to PM: "Loaded N product files, N stakeholder cards, and lessons.md from PAW."
4. Proceed to Phase 0b (manual resource gate for study-specific files like persona CSVs).
```

### For the orchestrator's final phase (add after writer):

```markdown
## Phase N — Truth Sync Back to PAW

1. Dispatch the `truth-sync` subagent with the final output path.
2. Present the update summary to the PM via `vscode_askQuestions`.
3. If approved, apply updates to PAW files.
4. Report: "PAW truth updated. N decisions, N risks, N lessons added."
```

### Resource-reader awareness

Farm resource-readers should treat `work/resources/paw-truth/` as primary context:

```markdown
## PAW Truth (Primary Context)
Files in `work/resources/paw-truth/` are the PM's canonical product truth.
- Prefer these over external sources when they cover the same topic.
- If a web source contradicts PAW truth, flag the contradiction — don't silently override.
- Cite as: "Per PAW product truth (products/ai-tools.md): ..."
```

## Lessons Integration

The `lessons.md` pull means every farm's Reviser has access to the PM's accumulated correction history. Wire it in:

- Resource-reader: copies `paw-truth/lessons.md` into the inventory.
- Reviser prompt: add `Read work/resources/paw-truth/lessons.md before revising. Avoid any patterns the PM has previously corrected.`
- Truth-sync: if the Skeptic caught a pattern the PM confirmed was a real problem (i.e., the Reviser fixed it), propose a new lesson rule.

## Edge Cases

| Scenario | Behavior |
|---|---|
| PAW path doesn't exist | Ask PM to configure. Don't guess. |
| `products/` is empty (fresh PAW) | Pull nothing. Log "No PAW truth found — farm will start from scratch." |
| PM corrects a farm output | Truth-sync should detect the correction (diff revised-draft vs. final) and propose a lesson. |
| Multiple farms running concurrently | Each farm writes its own `paw-updates/` — they don't conflict. PM applies one at a time. |
| PM says "skip sync" | Respect it. The update files still exist in `output/paw-updates/` if they change their mind. |
