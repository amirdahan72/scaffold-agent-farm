# Diagram Prompt: Three Building Blocks — Skills, Agents, Agent Farms

> **Use this prompt in M365 Creator (or any AI image/diagram tool) to generate a professional visual for Slide 5.**

## Prompt

Create a professional, clean **three-column diagram** titled **"Building Blocks: Skills → Agents → Agent Farms"** using a modern tech-corporate style with a white background.

### Layout — Three Columns (Left to Right with Arrows Between)

**Column 1 — "Skills" (Blue theme, #0070C0)**
- Rounded rectangle with light blue fill (#F0F7FF) and blue border
- Header: "Skills" in bold
- Subtitle: "Reusable instruction files (.github/skills/*/SKILL.md)"
- List 8 skill icons with labels, each on its own line:
  - 🔍 web-search
  - 📧 workiq-context  
  - 📝 doc-writer
  - 📊 ppt-creator
  - 📄 docx-writer
  - 📈 xlsx-writer
  - 📉 chart-creator
  - 🔧 ado-reader
- Footer text: "Build once, reuse across all farms"

**Large arrow →** between Column 1 and Column 2, labeled "compose into"

**Column 2 — "Agents" (Orange theme, #E97132)**
- Rounded rectangle with light orange fill (#FFF5F0) and orange border
- Header: "Agents" in bold
- Subtitle: "Markdown persona files (.agent.md / .prompt.md)"
- Show a small org chart inside:
  - Top: "Orchestrator" (coordinator icon)
  - Below, branching to 5 boxes:
    - "Web Researcher"
    - "WorkIQ Collector"
    - "Synthesizer"
    - "Skeptic"
    - "Writer"
- Footer text: "Each ~50 lines of plain English"

**Large arrow →** between Column 2 and Column 3, labeled "assembled into"

**Column 3 — "Agent Farm" (Green theme, #107C10)**
- Rounded rectangle with light green fill (#F0FFF0) and green border
- Header: "Agent Farm" in bold  
- Subtitle: "A complete multi-agent system in a folder"
- Show a folder structure:
  ```
  farms/competitive-analysis/
  ├── README.md
  ├── prompts/
  │   ├── web-researcher.prompt.md
  │   ├── workiq-collector.prompt.md
  │   ├── synthesizer.prompt.md
  │   ├── skeptic.prompt.md
  │   └── writer.prompt.md
  └── work/runs/YYYY-MM-DD-slug/
      ├── sources/
      └── output/
  ```
- Footer text: "PM types one prompt → full deliverable"

### Style
- Clean, flat design — no 3D, no gradients
- Segoe UI or similar sans-serif font
- Each column should be visually distinct with its color theme
- 16:9 aspect ratio
- The progression arrows between columns should be large and prominent
- Use icons where possible (folder, people, document icons)
