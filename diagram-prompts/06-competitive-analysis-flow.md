# Diagram Prompt: Competitive Analysis Farm — End-to-End Flow

> **Use this prompt in M365 Creator (or any AI image/diagram tool) to generate a professional visual for Slide 11.**

## Prompt

Create a professional **end-to-end flow diagram** titled **"Competitive Analysis Farm — From One Prompt to Complete Brief"** showing how a single PM prompt triggers 7 sub-agents that produce a comprehensive competitive brief. White background, modern tech-corporate style.

### Layout — Top-to-Bottom Flow

**Top: PM Input**
- Person icon labeled "PM" with a speech bubble containing:
  *"Run competitive analysis for Azure ZTS. Competitors: Illumio, Guardicore, Zero Networks."*
- Arrow labeled "30 seconds" flowing down into the orchestrator

**Row 1: Orchestrator**
- Wide rounded rectangle: "Orchestrator (.agent.md)" — Navy (#0E2841) fill, white text
- Sub-text: "Coordinates phases, manages PM checkpoints"
- Arrows fanning out downward to 3 collectors

**Row 2: Collectors (Parallel)** — Three boxes side by side
1. "Web Researcher" — Steel Blue (#156082)
   - Input: "15+ web sources"
   - Output: "→ competitor-*.md, market-landscape.md"
2. "WorkIQ Collector" — Orange (#E97132)
   - Input: "M365 emails, chats, meetings"
   - Output: "→ internal-context.md"
3. "Resource Reader" — Teal (#467886)
   - Input: "PM-provided docs"
   - Output: "→ compete.md summary"

All three boxes have arrows flowing down to file boxes labeled "sources/*.md" (Light Gray)

**Row 3: Synthesizer**
- "Synthesizer" — Orange (#E97132)
- Reads from sources/*.md
- Output arrow: "→ combined-draft.md"

**Row 4: Skeptic**
- "Skeptic" — Coral (#F65567), stands out visually
- Reads combined-draft.md
- Output: "→ review-notes.md (8-12 issues found)"
- Small PM checkpoint diamond here

**Row 5: Reviser**
- "Reviser" — Teal (#467886)
- Reads review-notes.md + combined-draft.md
- Output: "→ revised-draft.md + change log"

**Row 6: Writer**
- "Writer" — Green (#107C10)
- Reads revised-draft.md
- Output: "→ competitive-brief.md + competitive-brief.docx"

**Bottom: Final Output**
- Show the final deliverable files with document icons:
  - "competitive-brief.md" (primary)
  - "competitive-brief.docx" (Word version)
- Duration badge: "Total: 15-20 minutes (automated)"

### Side Panel: File Tree
On the right side, show a semi-transparent panel with the actual file structure:
```
runs/2026-03-02-microsegmentation/
├── internal-context.md
├── sources/
│   ├── market-landscape.md
│   ├── competitor-illumio.md
│   ├── competitor-guardicore.md
│   ├── competitor-zero-networks.md
│   ├── head-to-head.md
│   └── index.md
└── output/
    ├── combined-draft.md
    ├── review-notes.md
    ├── revised-draft.md
    └── competitive-brief.md
```

### Key Visual Elements
- PM Checkpoint diamonds between key phases (after collection, after synthesis, after skeptic)
- Each agent should have a small icon reflecting its role
- The Skeptic phase should visually stand out (coral/red color + warning icon)
- Show time labels on the right margin: "15+ min" for collection, "3 min" for synthesis, etc.
- Add a comparison callout at the bottom: "Manual equivalent: 6-8 hours"

### Style
- Clean, flat design — no 3D, no gradients
- Segoe UI or similar sans-serif font
- Vertical flow (top to bottom) with clear phase separation
- Color palette: Navy (#0E2841), Steel Blue (#156082), Orange (#E97132), Coral (#F65567), Teal (#467886), Green (#107C10), Light Gray (#E8E8E8)
- 16:9 aspect ratio (landscape)
- Use dotted lines for file read operations, solid lines for file write operations
