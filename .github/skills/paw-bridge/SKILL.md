---
name: paw-bridge
description: "Pushes farm findings back to PAW (PM AI Workspace) as additive update proposals. Use when a farm orchestrator needs to 'sync back to PAW', 'update PAW with findings', or 'propose PAW truth updates'. Pull from PAW is handled by each farm's paw-reader.prompt.md collector — NOT by this skill."
---

# PAW Bridge (Push-Only / Truth Sync)

Closes the loop after a farm run by proposing additive updates to PAW truth files. Pull-from-PAW is no longer this skill's job — each farm has a dedicated `paw-reader` collector sub-agent (see `make-agent-farm` skill, "PAW Reader Template" section).

> **Migration note (2026-05):** This skill previously handled both pre-run pull and post-run push. The pre-run pull half has been removed. If you encounter a farm orchestrator that still calls `paw-bridge` at "Phase 0a — PAW Truth Pull", that orchestrator is out of date. Replace the call with a `paw-reader` sub-agent dispatch in the collector phase.

## What This Skill Does

**Post-run only** — reads the farm's final artifact, extracts net-new facts (decisions, risks, quotes, open questions, lesson candidates) that are not already in PAW truth, and writes update proposal files (`.update.md`) for the PM to approve.

This skill **never writes directly** to PAW. It writes proposals; the orchestrator applies them only after PM confirmation.

## Setup (One-Time)

The farm workspace needs a `paw-config.json` at the farm folder root:

```json
{
  "paw_root": "C:\\Repo\\amir-agent-packs\\PM AI Workspace",
  "auto_push": {
    "update_products": true,
    "update_lessons": true,
    "copy_artifact": true
  }
}
```

**Fields:**
- `paw_root` — absolute path to the PAW workspace. The only thing the PM configures manually.
- `auto_push.update_products` — propose appends to `products/<x>.md` (Recent Decisions, Risks, Open Questions tables).
- `auto_push.update_lessons` — propose new rows in `personal/lessons.md` when the Skeptic flagged a recurring pattern that the Reviser confirmed.
- `auto_push.copy_artifact` — copy the final farm artifact into a relevant PAW workstream folder for archival.

> **Deprecated fields** (silently ignored if present): `auto_pull`, `product_filter`, `people_filter`. Pull scoping now lives in each farm's `paw-reader.prompt.md`. Remove these fields from existing configs at your convenience.

## Push-Phase Protocol

Called by the farm orchestrator as the **final phase** (after writer/reviser). A dedicated `truth-sync` sub-agent in each farm runs this protocol.

### Step 1 — Read the final artifact

Read the farm's output (e.g., `work/runs/<slug>/output/revised-draft.md` or `output/connect.md`). This is the canonical content to mine for new facts.

### Step 2 — Read current PAW truth for comparison

Re-read the relevant PAW files **directly from `{paw_root}/...`** (not from any cached `paw-truth/` folder — that folder is no longer maintained by this skill). The `paw-reader` sub-agent already wrote a scoped summary at the start of the run; the truth-sync sub-agent reads the original files for diff accuracy.

Files to read are determined by the farm archetype — typically the same set the `paw-reader` consulted. If the orchestrator passed `{{PAW_FILES_READ}}` listing the files the `paw-reader` consulted, reuse that list.

### Step 3 — Extract net-new facts

Compare the artifact against PAW truth. Identify:

- **New decisions** not already in `products/<x>.md → Recent Decisions`
- **New risks** not already in `products/<x>.md → Risks & Blockers`
- **New stakeholder quotes** worth preserving (with attribution and date)
- **New open questions** surfaced by the farm
- **Lesson candidates** — recurring patterns the Skeptic flagged AND the Reviser confirmed/fixed

Skip facts already in PAW (avoid duplicates). Skip facts that are too granular (e.g., one-off meeting transcript snippets).

### Step 4 — Write update proposal files

Write to the farm's run output, **never** directly to PAW:

```
work/runs/<slug>/output/paw-updates/
├── products/
│   └── <product>.update.md       ← proposed appends to products/<product>.md
├── people/
│   └── @<alias>.update.md         ← proposed appends to people/@<alias>.md (only if directly evidenced)
├── lessons.update.md              ← proposed new lesson rules
└── update-summary.md              ← human-readable summary across all proposed changes
```

Each `.update.md` uses a structured format that mirrors PAW's table layouts:

```markdown
# Proposed Updates to products/azure-waf.md
> Source: <farm-name> run <YYYY-MM-DD-slug>

## Append to: Recent Decisions
| Date | Decision | Context |
|------|----------|---------|
| 2026-05-06 | ... | Per <farm-name>: ... |

## Append to: Risks & Blockers
| Risk | Impact | Mitigation |
|------|--------|------------|
| ... | ... | ... |

## Append to: Open Questions
- [ ] ...
```

The `update-summary.md` is the index — list every proposed change in one scannable place:

```markdown
# PAW Update Summary — <farm-name> run <slug>

## Files affected
- products/azure-waf.md — 2 new decisions, 1 risk
- personal/lessons.md — 1 lesson candidate

## Proposed lesson candidates
1. **<rule>** — <one-line context>
```

### Step 5 — PM confirmation

The orchestrator presents `update-summary.md` to the PM via `vscode_askQuestions`:

```
"Truth sync proposals ready — N updates to PAW truth files:
 - 2 new decisions for products/azure-waf.md
 - 1 new risk
 - 1 lesson candidate

Options: [Apply all to PAW] [Let me review first] [Skip — I'll do it manually]"
```

### Step 6 — Apply (if approved)

If PM approves:
- For each `.update.md`, read the corresponding PAW truth file from `{paw_root}/...`
- Append the new rows to the matching sections
- Write back to the PAW file

If "review first": the PM reads `update-summary.md` and re-runs Step 5 manually.
If "skip": leave the proposal files in place. The PM may apply later by hand.

## Why Update Files Instead of Direct Writes

1. **Auditability** — the PM sees exactly what the farm wants to change before it touches truth.
2. **Conflict safety** — if the PM edited PAW between farm start and farm end, direct writes could clobber. Update files are additive only.
3. **Batch review** — all proposed changes in one summary, one approval gate.
4. **Rollback** — proposal files persist in `work/runs/<slug>/output/paw-updates/` for history.

## Integration with Farm Orchestrators

Add as the final phase of the farm dispatch sequence:

```markdown
## Phase N — Truth Sync Back to PAW

1. Check for `paw-config.json` in the farm root. If absent, skip this phase entirely.
2. Dispatch the `truth-sync` sub-agent. Pass:
   - `{{RUN_PATH}}` — current run folder
   - `{{ARTIFACT_PATH}}` — final output file
   - `{{PAW_ROOT}}` — from paw-config.json
3. Use `vscode_askQuestions` to present `update-summary.md` to the PM.
4. If approved, apply updates. Otherwise leave proposal files in place.
5. Report: "PAW truth updated. N decisions, N risks, N lesson candidates added."
```

## Edge Cases

| Scenario | Behavior |
|---|---|
| `paw-config.json` missing | Skip the entire phase. Don't ask, don't error. |
| `paw_root` doesn't exist on disk | Ask PM to fix the path. Leave proposal files in place; do not apply. |
| Final artifact not produced | Skip the phase. No artifact = nothing to extract. |
| Farm produced findings but they all duplicate PAW | Write an empty `update-summary.md` saying "No net-new facts." Don't dispatch the question. |
| PM declines apply | Proposal files persist for manual review. |
| Concurrent farm runs | Each writes its own `paw-updates/` folder. PM applies them one at a time. |
| PAW file edited mid-farm | Re-read at Step 2 always, so the diff is against current state. |

## Lessons Integration

If the farm's Skeptic flagged a pattern (e.g., "draft used 'AppGw' but lessons.md says 'AppGW'") AND the Reviser fixed it, that's a candidate for `lessons.update.md`. The truth-sync sub-agent should:

1. Read `personal/lessons.md` from PAW directly.
2. Compare Skeptic findings against existing rules.
3. For each finding NOT covered by an existing rule, propose a new row in the appropriate category (Writing Style, Product & Domain, Tool & Workflow, Formatting & Tone) with date and context.

## What Replaced the Pre-Run Pull

The old "Pre-Run: Pull Truth" half of this skill is gone. Each farm now has a `paw-reader.prompt.md` sub-agent that:
- Reads `paw-config.json` for `paw_root`
- Reads only the PAW folders relevant to that farm's archetype (products, people, workstreams, lessons — selected at scaffold time)
- Writes a **scoped summary** to `work/runs/<slug>/sources/paw-context.md`, not raw file copies
- Enforces a privacy deny-list (never reads `personal/{1-1-notes,feedback,dailies,connect,pro-d}/`)

See the "PAW Reader Template" section of the `make-agent-farm` skill for the canonical template.
