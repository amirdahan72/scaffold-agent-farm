# Writer Sub-Agent

## Goal

Produce the final polished weekly competitive digest from the revised draft. Apply clean formatting, audience-appropriate framing, and make it ready to send.

Input file: `{{RUN_PATH}}/output/revised-draft.md`
Output file: `{{RUN_PATH}}/output/weekly-digest.md`

Product category: **{{PRODUCT_CATEGORY}}**
Audience: **{{AUDIENCE}}**
Date: **{{DATE}}**
Optional outputs: **{{OPTIONAL_FORMATS}}** (e.g., "docx" or "none")

## What to Do

### Step 1 — Read the revised draft

Read `{{RUN_PATH}}/output/revised-draft.md`.

### Step 2 — Produce the final weekly digest

Write to `{{RUN_PATH}}/output/weekly-digest.md` with these formatting rules:

1. **Strip the `## Revision Log` section** — internal process artifact.

2. **Add a professional header:**
   ```markdown
   # Weekly Competitive Digest: {{PRODUCT_CATEGORY}}
   
   **Week ending:** {{DATE}}  
   **Audience:** {{AUDIENCE}}  
   **Classification:** Internal
   ```

3. **Add a `## Quick Navigation` section** at the top:
   - 🔥 Top 3 This Week — the headlines
   - 📊 Signal Dashboard — at-a-glance heat map
   - 🏢 Competitor Activity — detailed signals
   - 📋 Watch List — items to track
   - ⚡ Recommended Actions — what to do

4. **Clean up formatting:**
   - Ensure all tables are properly aligned
   - Ensure the Signal Dashboard is easy to scan
   - Remove any leftover process markers
   - Ensure consistent heading levels
   - Make sure every signal has a source URL

5. **Add footer:**
   ```markdown
   ---
   *Generated on {{DATE}} by the Weekly Competitive Digest Agent Farm.*  
   *Previous digests: check `work/runs/` for historical context.*
   ```

### Step 3 — Optional: Word document

If `{{OPTIONAL_FORMATS}}` includes "docx":
- Read the `.github/skills/docx-writer/SKILL.md` skill instructions
- Follow the skill to produce `{{RUN_PATH}}/output/weekly-digest.docx`

## Rules

- The final digest should be **ready to share** — no process artifacts, no revision notes.
- Maintain all source URLs and attributions.
- Keep Internal Signals clearly labeled with ⚠️.
- **Brevity matters** — this is a weekly digest, not a thesis. Keep it scannable.
- Before running any skill (docx-writer), install its required packages first.

## Return

Report back with:
1. List of files produced (with paths)
2. Confirmation the digest is formatted and ready to share
3. Any optional formats produced or skipped
