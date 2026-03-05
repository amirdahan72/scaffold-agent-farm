```skill
---
name: ppt-creator
description: 'Create professional internal slide deck presentations from structured content. Use when asked to build a deck, create slides, make a presentation, generate a PowerPoint, or produce a pptx file. Transforms briefs, PRDs, or any structured markdown into assertion-evidence style slides using the Pyramid Principle. Text-only slides (no shapes) enable PowerPoint Design Suggestions. Style inferred from internal master template. Uses PptxGenJS (Node).'
---

# PPT Creator

Create professional slide deck presentations from structured content. Uses the **Pyramid Principle** (conclusion → reasons → evidence) and **assertion-evidence** headings (complete sentences, not topic labels).

Style is inferred from the internal master template (`example-ppt-for-master-template.pptx`). All slides are **text-only** (no shapes) so that PowerPoint's built-in **Design Suggestions** tool can restyle them automatically after opening.

## When to Use This Skill

- User asks to "build a deck", "create slides", or "make a presentation"
- A deliverable needs to be presented to stakeholders
- Converting a markdown document (PRD, brief, analysis) into slides
- Producing a `.pptx` file for sharing

## Prerequisites

Before running this skill, install required packages:

```bash
npm install pptxgenjs
```

## ⚠️  Critical Rule: No Shapes

**Never use `addShape()`** in any slide. PowerPoint's **Design Suggestions** feature only activates on slides that contain text boxes and tables — not shapes. Every visual element must be built with `addText()` (with optional `fill` for colored backgrounds) or `addTable()`. This is a hard requirement.

Forbidden:
- `slide.addShape(pres.ShapeType.rect, ...)`
- `slide.addShape(pres.ShapeType.roundRect, ...)`
- `slide.addShape(pres.ShapeType.ellipse, ...)`
- Any `pres.ShapeType.*` reference

Allowed alternatives:
- `slide.addText("text", { fill: { color: "XXXXXX" } })` — text box with background fill
- `slide.addTable(...)` — data tables
- `slide.background = { color: "XXXXXX" }` — full-slide background color

## Brand Standards (Inferred from Master Template)

### Color Palette

| Role | Color Name | Hex | Usage |
|------|-----------|-----|-------|
| **Heading** | Heading Blue | `#0070C0` | Section headings, summary page titles |
| **Accent** | Accent Cyan | `#00B0F0` | Subheadings, secondary emphasis |
| **Azure** | Azure Blue | `#0078D4` | Azure-branded moments, links |
| **Stat Callout** | Coral | `#F65567` | Large stat numbers, KPI highlights |
| **Text (Body)** | Black | `#000000` | Body text, bullet points |
| **Text (Secondary)** | Medium Gray | `#747474` | Subtitles, secondary labels |
| **Background** | White | `#FFFFFF` | Default slide background |
| **Table Header** | Navy Dark | `#0E2841` | Table header row fill |
| **Table Alt Row** | Gray Light | `#E8E8E8` | Alternating table row fill |
| **Theme** | Steel Blue | `#156082` | Chart series 1, tertiary accents |
| **Theme** | Orange | `#E97132` | Chart series 2, attention callouts |
| **Theme** | Teal | `#467886` | Chart series 3, supporting data |
| **Theme** | Mauve | `#96607D` | Chart series 4 |

### Typography

| Element | Font | Size (pt) | Color |
|---------|------|-----------|-------|
| Title slide — title | Segoe UI Semibold | 36 | `#FFFFFF` (on dark fill) or `#000000` (on white) |
| Title slide — subtitle | Segoe UI | 18 | `#000000` or `#747474` |
| Title slide — event/context | Aptos Display | 20 | `#747474` |
| Summary heading | Segoe UI Semibold | 28 | `#0070C0` |
| Content heading | Segoe UI Semibold | 36 | `#000000` |
| Body text | Segoe UI | 14 | `#000000` |
| Bullet points | Segoe UI | 13–14 | `#000000` |
| Stat callout (number) | Segoe UI Semibold | 36 | `#F65567` |
| Stat callout (label) | Segoe UI | 12 | `#000000` |
| Footnotes / sources | Segoe UI | 9 | `#747474` |
| Table header | Segoe UI Semibold | 10 | `#FFFFFF` (on `#0E2841` fill) |
| Table body | Segoe UI | 9 | `#000000` |

> **Font fallback:** If Segoe UI is unavailable, fall back to `Calibri` → `Arial` → `Helvetica`.

### Slide Layouts

**Layout 1 — Title Slide**
- White background
- Title: left-aligned, Segoe UI Semibold 36pt, white on dark text-box fill (`#222120`) — or black on white
- Subtitle: left-aligned, Segoe UI 18pt, `#000000`
- Context/event line: Aptos Display 20pt, `#747474`
- Date/author: left-aligned, Aptos 18pt, `#000000`

**Layout 2 — Summary / Key Message**
- White background
- Heading: Segoe UI Semibold 28pt, `#0070C0`, left-aligned
- Body: below heading, Segoe UI 14pt bullets, `#000000`
- Optional stat callouts: Segoe UI Semibold 36pt, `#F65567` (number) + Segoe UI 12pt label below
- Slide number: bottom-right, Segoe UI 9pt, `#747474`

**Layout 3 — Content Slide (default)**
- White background
- Heading: Segoe UI Semibold 36pt, `#000000`, left-aligned
- Body area: Segoe UI 14pt bullets, `#000000`
- Slide number: bottom-right, Segoe UI 9pt, `#747474`

**Layout 4 — Two-Column Content**
- Same heading as Layout 3
- Two equal text-box columns below (left: x=0.5, w=5.8; right: x=6.8, w=5.8)
- Use for comparisons, before/after, pros/cons

**Layout 5 — Table Slide**
- Heading: Segoe UI Semibold 28pt, `#0070C0`
- Table: header row with `#0E2841` fill + white text; body rows alternate `#FFFFFF` and `#E8E8E8`; borders `#E8E8E8` 0.5pt

**Layout 6 — Section Divider**
- White background
- Section name: centered, Segoe UI Semibold 28pt, `#0070C0`
- Optional tagline: centered, Segoe UI 16pt, `#747474`

**Layout 7 — Closing / CTA**
- White background
- "Next Steps" heading: Segoe UI Semibold 28pt, `#0070C0`
- Action items: Segoe UI 16pt bullets, `#000000`

## Workflow

### Step 1 — Analyze input content

Read the source document (competitive brief, PRD, etc.) and identify:

- **Key message** (the one thing the audience should take away)
- **Supporting arguments** (3–5 main points)
- **Evidence** (data, quotes, comparisons for each argument)

### Step 2 — Structure using the Pyramid Principle

```
Slide 1: Title slide (Layout 1) — deck title + key message as subtitle
Slide 2: Executive summary (Layout 2) — conclusion first, optional stat callouts
Slide 3: Section divider if needed (Layout 6)
Slides 4-N: Content slides (Layout 3/4/5) — one argument per slide, with evidence
Final slide: Call to action / next steps (Layout 7)
```

### Step 3 — Write assertion-evidence headings

Every slide heading must be a **complete sentence** that states the slide's conclusion:

| Bad (topic label) | Good (assertion heading) |
|---|---|
| "Market Overview" | "The WAF market is growing 18% annually, driven by API security needs" |
| "Competitor Pricing" | "Our pricing is 30% below Cloudflare's enterprise tier while matching features" |
| "Feature Comparison" | "We lead in three of five categories that enterprise buyers prioritize" |

### Step 4 — Generate the deck

Use PptxGenJS (Node.js) with the inferred brand styling. **Do not use `addShape()` anywhere.**

```javascript
const pptxgen = require("pptxgenjs");
const pres = new pptxgen();

// ── Brand constants (inferred from master template) ──
const WHITE        = "FFFFFF";
const BLACK        = "000000";
const GRAY_LIGHT   = "E8E8E8";
const NAVY_DARK    = "0E2841";
const STEEL_BLUE   = "156082";
const ORANGE_ACC   = "E97132";
const TEAL         = "467886";
const MAUVE        = "96607D";
const HEADING_BLUE = "0070C0";
const ACCENT_CYAN  = "00B0F0";
const AZURE_BLUE   = "0078D4";
const CORAL        = "F65567";
const MEDIUM_GRAY  = "747474";
const FONT         = "Segoe UI";
const FONT_SB      = "Segoe UI Semibold";

// ── Presentation metadata ──
pres.title = "<Deck Title>";
pres.author = "<Author>";
pres.layout = "LAYOUT_WIDE"; // 13.33" × 7.5" widescreen

// ── Title Slide (Layout 1) ──
const titleSlide = pres.addSlide();
titleSlide.background = { color: WHITE };
// Title in a dark text box (text-box fill, NOT a shape)
titleSlide.addText("<Deck Title>", {
  x: 0.5, y: 2.0, w: 9.5, h: 1.8,
  fontSize: 36, fontFace: FONT_SB, color: WHITE,
  fill: { color: "222120" },
  align: "left", bold: true, lineSpacingMultiple: 1.2
});
titleSlide.addText("<Subtitle / key message>", {
  x: 0.5, y: 4.2, w: 9.5, h: 0.5,
  fontSize: 18, fontFace: FONT, color: BLACK, align: "left"
});
titleSlide.addText("<Date>  |  <Author>", {
  x: 0.5, y: 5.2, w: 6.0, h: 0.5,
  fontSize: 18, fontFace: "Aptos", color: BLACK, align: "left"
});

// ── Summary Slide (Layout 2) ──
const summarySlide = pres.addSlide();
summarySlide.background = { color: WHITE };
summarySlide.addText("<Summary assertion heading>", {
  x: 0.5, y: 0.7, w: 11.4, h: 0.5,
  fontSize: 28, fontFace: FONT_SB, color: HEADING_BLUE, align: "left"
});
summarySlide.addText([
  { text: "Key finding 1", options: { bullet: true, fontSize: 14, fontFace: FONT, color: BLACK } },
  { text: "Key finding 2", options: { bullet: true, fontSize: 14, fontFace: FONT, color: BLACK } },
  { text: "Key finding 3", options: { bullet: true, fontSize: 14, fontFace: FONT, color: BLACK } }
], { x: 0.5, y: 1.5, w: 7.0, h: 5.0, valign: "top" });
// Optional stat callouts (text only — no shapes)
summarySlide.addText("42%", {
  x: 8.5, y: 1.5, w: 3.0, h: 0.6,
  fontSize: 36, fontFace: FONT_SB, color: CORAL, align: "left"
});
summarySlide.addText("Market share growth", {
  x: 8.5, y: 2.1, w: 3.0, h: 0.4,
  fontSize: 12, fontFace: FONT, color: BLACK, align: "left"
});
summarySlide.slideNumber = { x: 12.5, y: 7.0, fontSize: 9, fontFace: FONT, color: MEDIUM_GRAY };

// ── Content Slide (Layout 3) ──
const contentSlide = pres.addSlide();
contentSlide.background = { color: WHITE };
contentSlide.addText("<Assertion heading — complete sentence>", {
  x: 0.6, y: 0.3, w: 12.0, h: 0.6,
  fontSize: 36, fontFace: FONT_SB, color: BLACK, align: "left"
});
contentSlide.addText([
  { text: "Evidence point 1", options: { bullet: true, fontSize: 14, fontFace: FONT, color: BLACK } },
  { text: "Evidence point 2", options: { bullet: true, fontSize: 14, fontFace: FONT, color: BLACK } },
  { text: "Evidence point 3", options: { bullet: true, fontSize: 14, fontFace: FONT, color: BLACK } }
], { x: 0.6, y: 1.2, w: 12.0, h: 5.5, valign: "top" });
contentSlide.slideNumber = { x: 12.5, y: 7.0, fontSize: 9, fontFace: FONT, color: MEDIUM_GRAY };

// ── Table Slide (Layout 5) ──
const tableSlide = pres.addSlide();
tableSlide.background = { color: WHITE };
tableSlide.addText("<Table assertion heading>", {
  x: 0.5, y: 0.7, w: 11.4, h: 0.5,
  fontSize: 28, fontFace: FONT_SB, color: HEADING_BLUE
});
tableSlide.addTable(
  [
    // Header row — navy dark fill
    [
      { text: "Dimension", options: { fill: { color: NAVY_DARK }, color: WHITE, fontSize: 10, fontFace: FONT_SB, bold: true } },
      { text: "Our Product", options: { fill: { color: NAVY_DARK }, color: WHITE, fontSize: 10, fontFace: FONT_SB, bold: true } },
      { text: "Competitor", options: { fill: { color: NAVY_DARK }, color: WHITE, fontSize: 10, fontFace: FONT_SB, bold: true } }
    ],
    // Data rows — alternate white / light gray
    [
      { text: "Feature A", options: { fill: { color: WHITE }, fontSize: 9, fontFace: FONT, color: BLACK } },
      { text: "✓", options: { fill: { color: WHITE }, fontSize: 9, fontFace: FONT, color: TEAL } },
      { text: "✗", options: { fill: { color: WHITE }, fontSize: 9, fontFace: FONT, color: ORANGE_ACC } }
    ],
    [
      { text: "Feature B", options: { fill: { color: GRAY_LIGHT }, fontSize: 9, fontFace: FONT, color: BLACK } },
      { text: "✓", options: { fill: { color: GRAY_LIGHT }, fontSize: 9, fontFace: FONT, color: TEAL } },
      { text: "✓", options: { fill: { color: GRAY_LIGHT }, fontSize: 9, fontFace: FONT, color: TEAL } }
    ]
  ],
  { x: 0.3, y: 1.5, w: 12.7, border: { type: "solid", pt: 0.5, color: GRAY_LIGHT } }
);

// ── Section Divider (Layout 6) ──
const dividerSlide = pres.addSlide();
dividerSlide.background = { color: WHITE };
dividerSlide.addText("<Section Name>", {
  x: 0.8, y: 2.8, w: 11.7, h: 1.2,
  fontSize: 28, fontFace: FONT_SB, color: HEADING_BLUE,
  align: "center", bold: true
});

// ── Closing / CTA Slide (Layout 7) ──
const closingSlide = pres.addSlide();
closingSlide.background = { color: WHITE };
closingSlide.addText("Next Steps", {
  x: 0.5, y: 0.7, w: 11.4, h: 0.5,
  fontSize: 28, fontFace: FONT_SB, color: HEADING_BLUE, align: "left"
});
closingSlide.addText([
  { text: "1. Action item one", options: { fontSize: 16, fontFace: FONT, color: BLACK } },
  { text: "2. Action item two", options: { fontSize: 16, fontFace: FONT, color: BLACK } },
  { text: "3. Action item three", options: { fontSize: 16, fontFace: FONT, color: BLACK } }
], { x: 0.5, y: 1.5, w: 11.4, h: 4.5, valign: "top" });

// ── Save ──
pres.writeFile({ fileName: "output.pptx" });
```

### Step 5 — Quality check

Before delivering, verify:

- [ ] Every slide heading is a complete sentence (assertion, not topic)
- [ ] Evidence supports each assertion (data, quotes, sources)
- [ ] Deck follows Pyramid Principle (conclusion first)
- [ ] No slide has more than 5–6 bullet points
- [ ] Source URLs are included in slide notes or footnotes
- [ ] File saves successfully to the output directory
- [ ] **No shapes:** Zero `addShape()` calls — only `addText()` and `addTable()`
- [ ] **Brand compliance:** Heading Blue (`#0070C0`) for summary headings; black for content headings
- [ ] **Typography:** Segoe UI family used throughout (Semibold for headings, Regular for body)
- [ ] **Tables:** Header row uses Navy Dark (`#0E2841`) fill; body rows alternate white and `#E8E8E8`
- [ ] **Stat callouts:** Coral (`#F65567`) for numbers, placed as text boxes (no shapes)
- [ ] **Widescreen:** Presentation uses `LAYOUT_WIDE` (13.33" × 7.5")
- [ ] **Design Suggestions ready:** Open the .pptx in PowerPoint → click Design → Design Ideas should activate

## Rules

- **No shapes — ever.** Never use `addShape()`. Only use `addText()` (with optional `fill`) and `addTable()`. This ensures PowerPoint Design Suggestions works on every slide.
- **Brand palette required.** Use the inferred color palette from the master template.
- **Assertion headings only.** No topic-label headings like "Overview" or "Pricing".
- **Evidence required.** Every claim slide must have supporting data or quotes.
- **Source attribution.** Include source URLs in slide notes or footnotes (Segoe UI 9pt, `#747474`).
- **Keep slides lean.** 5–6 bullets max per slide. If more, split into two slides.
- **Install packages first.** Always run `npm install pptxgenjs` before generating.
- **Widescreen only.** Always use `LAYOUT_WIDE`.
- **Font fallback.** If Segoe UI is unavailable on the system, use `Calibri` → `Arial`.
- **Stat callouts are text only.** Use bold colored `addText()` for large numbers — never shapes.

## Output

Save the generated `.pptx` file to the `work/output/` directory (or the current run folder `work/runs/<run-slug>/output/` if run versioning is active).

```
