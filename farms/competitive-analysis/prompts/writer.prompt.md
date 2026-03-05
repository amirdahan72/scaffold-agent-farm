# Writer Sub-Agent

## Goal

Produce the final polished competitive brief from the revised draft. Apply professional formatting, audience-appropriate framing, and clean structure.

Input file: `{{RUN_PATH}}/output/revised-draft.md`
Output file: `{{RUN_PATH}}/output/competitive-brief.md`

Product category: **{{PRODUCT_CATEGORY}}**
Audience: **{{AUDIENCE}}**
Date: **{{DATE}}**
Optional outputs: **{{OPTIONAL_FORMATS}}** (e.g., "docx", "pptx", or "none")

## What to Do

### Step 1 — Read the revised draft

Read `{{RUN_PATH}}/output/revised-draft.md` — this is the Reviser's polished output (after the Skeptic's adversarial review was addressed).

### Step 2 — Produce the final competitive brief

Write to `{{RUN_PATH}}/output/competitive-brief.md` with these formatting rules:

1. **Strip the `## Revision Notes` section** — it's internal process, not part of the deliverable.

2. **Add a professional header:**
   ```markdown
   # Competitive Brief: {{PRODUCT_CATEGORY}}
   
   **Audience:** {{AUDIENCE}}  
   **Generated:** {{DATE}}  
   **Classification:** Internal
   ```

3. **Add a `## How to Use This Brief` section** at the top (after the header):
   - For leadership: focus on Executive Summary + Strategic Recommendations
   - For PM peers: focus on Comparison Matrix + Competitor Deep-Dives
   - For sales/GTM: focus on Battle Cards + Objection Handlers
   - For engineering: focus on feature comparisons + technical dimensions

4. **Clean up formatting:**
   - Ensure all tables are properly aligned
   - Ensure the Battle Cards section is formatted for quick reference (printable, paste-into-deck ready)
   - Remove any leftover `[NEEDS DATA]` — replace with "Data not available" or remove the row
   - Ensure consistent heading levels

5. **Add footer:**
   ```markdown
   ---
   *Generated on {{DATE}} by the Competitive Analysis Agent Farm.*
   ```

### Step 3 — Optional: Word document

If `{{OPTIONAL_FORMATS}}` includes "docx":
- Read the `.github/skills/docx-writer/SKILL.md` skill instructions
- Follow the skill to produce `{{RUN_PATH}}/output/competitive-brief.docx`

### Step 4 — Optional: Slide deck

If `{{OPTIONAL_FORMATS}}` includes "pptx":
- Read the `.github/skills/ppt-creator/SKILL.md` skill instructions
- Follow the skill to produce `{{RUN_PATH}}/output/competitive-analysis.pptx` with:
  - Title slide
  - Executive summary slide
  - Comparison matrix slide
  - One slide per competitor (strengths/weaknesses)
  - Battle cards slide
  - Strategic recommendations slide

## Rules

- The final brief should be **ready to share** — no process artifacts, no revision notes, no debugging markers.
- Maintain all source URLs and attributions from the revised draft.
- Keep the Internal Context section clearly labeled with ⚠️ warning.
- Before running any skill (docx-writer, ppt-creator), install its required packages first.

## Return

Report back with:
1. List of files produced (with paths)
2. Confirmation the brief is formatted and ready to share
3. Any optional formats produced (docx, pptx) or skipped
