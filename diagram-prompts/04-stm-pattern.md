# Diagram Prompt: Short-Term Memory (STM) — Disk-Based Agent Communication

> **Use this prompt in M365 Creator (or any AI image/diagram tool) to generate a professional visual for Slide 7.**

## Prompt

Create a professional **data flow diagram** titled **"Short-Term Memory (STM) — How Sub-Agents Communicate Through Disk"** showing how agents pass information via files rather than shared memory. White background, modern tech-corporate style.

### Layout — Two Rows

**Top Row: "The Problem" (left) vs "The Solution" (right)**

**Left half — "Single Context (M365 Copilot)"** (crossed out / dimmed)
- Show a single large brain/context window icon
- Inside it, show overwhelming amounts of data pouring in: "15 web pages + 50 emails + docs + ADO items"
- X mark or warning triangle: "Context overflow → low quality, hallucination"
- Use muted/gray tones to indicate this is the wrong approach

**Right half — "STM Pattern (Agent Farms)"** (highlighted / active)
- Show multiple small brain/context windows, each isolated
- Each context window is clean and focused
- Check mark: "Fresh context per agent → high quality, auditable"
- Use vibrant teal/blue tones

**Bottom Row: "The STM Flow"**

Show a horizontal chain of agent and file interactions:

1. **Agent box** "Collector 1 (Web)" — Steel Blue (#156082), person icon
   - Arrow labeled "writes" → 
2. **File box** "sources/market.md" — Light gray, file icon
   - Arrow labeled "reads" →
3. **Agent box** "Collector 2 (M365)" — Steel Blue (#156082), person icon  
   - Arrow labeled "writes" →
4. **File box** "sources/internal.md" — Light gray, file icon
   - Both file boxes have arrows labeled "reads" →
5. **Agent box** "Synthesizer" — Orange (#E97132), merge icon
   - Arrow labeled "writes" →
6. **File box** "output/draft.md" — Light gray, file icon
   - Arrow labeled "reads" →
7. **Agent box** "Skeptic" — Coral (#F65567), critical eye icon
   - Arrow labeled "writes" →
8. **File box** "output/review.md" — Light gray, file icon
   - Arrow labeled "reads" →
9. **Agent box** "Reviser" — Teal (#467886), pencil icon
   - Arrow labeled "writes" →
10. **File box** "output/revised.md" — Light gray, file icon

### Key Annotations
- Add a bracket under the file boxes labeled: "All files are human-readable .md — fully auditable on disk"
- Add a callout: "Each agent starts with a fresh context window — no accumulated noise"

### Style
- Clean, flat design — no 3D  
- Segoe UI or similar sans-serif font
- Agent boxes should be colored rectangles with rounded corners
- File boxes should look like document/file icons with a folded corner
- Color palette: Navy (#0E2841), Steel Blue (#156082), Orange (#E97132), Coral (#F65567), Teal (#467886), Light Gray (#E8E8E8)
- 16:9 aspect ratio
- The flow should read clearly left to right
