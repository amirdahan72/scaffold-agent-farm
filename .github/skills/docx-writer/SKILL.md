```skill
---
name: docx-writer
description: 'Create, read, and edit Microsoft Word (.docx) documents. Use when asked to produce a Word document, export markdown to docx, create a formatted report, build a document with headers/tables/images, or convert content into a professional .docx file. Supports Node.js (docx package) and Python (python-docx). Triggers: create docx, make a Word doc, export to Word, generate document, write a report.'
---

# DOCX Writer

Create, read, and edit Microsoft Word (.docx) documents. Converts structured markdown content (PRDs, briefs, specs, reports) into professionally formatted Word documents with proper headings, tables, lists, and styling.

## When to Use This Skill

- User asks to "create a Word doc", "export to docx", or "make a document"
- A deliverable needs to be shared as a `.docx` file (e.g., for stakeholders who prefer Word)
- Converting a markdown document into a formatted Word document
- Producing a `.docx` alongside or instead of a markdown deliverable

## Prerequisites

Before running this skill, install the required package:

```bash
# Node.js option (recommended)
npm install docx

# Python option (alternative)
pip install python-docx
```

## Workflow

### Step 1 — Analyze input content

Read the source document (markdown PRD, brief, spec, etc.) and identify:

- **Title and metadata** (author, date, version)
- **Section structure** (headings hierarchy: H1-H4)
- **Tables** (comparison tables, requirements tables)
- **Lists** (bullet points, numbered requirements)
- **Callouts** (notes, warnings, internal-context blocks)

### Step 2 — Map markdown to Word styles

| Markdown element | Word style |
|-----------------|------------|
| `# Heading 1` | Heading 1 (28pt, bold) |
| `## Heading 2` | Heading 2 (22pt, bold) |
| `### Heading 3` | Heading 3 (16pt, bold) |
| `#### Heading 4` | Heading 4 (14pt, bold) |
| Body text | Normal (11pt) |
| `- bullet` | List Bullet |
| `1. item` | List Number |
| `> blockquote` | Intense Quote (used for callouts/internal context) |
| `**bold**` | Bold run |
| `*italic*` | Italic run |
| Table | Table Grid with header row shading |

### Step 3 — Generate the document (Node.js)

```javascript
const docx = require("docx");
const fs = require("fs");

const { Document, Packer, Paragraph, TextRun, Table, TableRow, TableCell,
        HeadingLevel, AlignmentType, BorderStyle, WidthType } = docx;

const doc = new Document({
  creator: "<Author>",
  title: "<Document Title>",
  description: "<Description>",
  sections: [{
    properties: {},
    children: [
      // Title
      new Paragraph({
        text: "<Document Title>",
        heading: HeadingLevel.HEADING_1,
      }),

      // Metadata block
      new Paragraph({
        children: [
          new TextRun({ text: "Author: ", bold: true }),
          new TextRun("<author>"),
        ],
      }),
      new Paragraph({
        children: [
          new TextRun({ text: "Date: ", bold: true }),
          new TextRun("<date>"),
        ],
      }),

      // Section heading
      new Paragraph({
        text: "<Section Name>",
        heading: HeadingLevel.HEADING_2,
      }),

      // Body paragraph
      new Paragraph({
        text: "<paragraph content>",
      }),

      // Bullet list
      new Paragraph({
        text: "<bullet item>",
        bullet: { level: 0 },
      }),

      // Table (example: requirements table)
      new Table({
        width: { size: 100, type: WidthType.PERCENTAGE },
        rows: [
          new TableRow({
            tableHeader: true,
            children: [
              new TableCell({ children: [new Paragraph({ text: "ID", bold: true })] }),
              new TableCell({ children: [new Paragraph({ text: "Requirement", bold: true })] }),
              new TableCell({ children: [new Paragraph({ text: "Priority", bold: true })] }),
            ],
          }),
          new TableRow({
            children: [
              new TableCell({ children: [new Paragraph("FR-1")] }),
              new TableCell({ children: [new Paragraph("<requirement>")] }),
              new TableCell({ children: [new Paragraph("P0")] }),
            ],
          }),
        ],
      }),
    ],
  }],
});

// Save to file
Packer.toBuffer(doc).then((buffer) => {
  fs.writeFileSync("output.docx", buffer);
});
```

### Step 3 (alt) — Generate the document (Python)

```python
from docx import Document
from docx.shared import Pt, Inches, RGBColor
from docx.enum.text import WD_ALIGN_PARAGRAPH

doc = Document()

# Title
doc.add_heading('<Document Title>', level=1)

# Metadata
p = doc.add_paragraph()
p.add_run('Author: ').bold = True
p.add_run('<author>')

# Section heading
doc.add_heading('<Section Name>', level=2)

# Body paragraph
doc.add_paragraph('<paragraph content>')

# Bullet list
doc.add_paragraph('<bullet item>', style='List Bullet')

# Table
table = doc.add_table(rows=2, cols=3, style='Table Grid')
hdr = table.rows[0].cells
hdr[0].text = 'ID'
hdr[1].text = 'Requirement'
hdr[2].text = 'Priority'
row = table.rows[1].cells
row[0].text = 'FR-1'
row[1].text = '<requirement>'
row[2].text = 'P0'

# Internal context callout (blockquote style)
p = doc.add_paragraph(style='Intense Quote')
p.add_run('Internal Context (Work IQ): ').bold = True
p.add_run('<internal findings — do not distribute externally>')

doc.save('output.docx')
```

### Step 4 — Quality check

Before delivering, verify:

- [ ] Document opens correctly in Word / Google Docs
- [ ] Heading hierarchy is consistent (H1 → H2 → H3, no skipping)
- [ ] Tables render with visible borders and header row
- [ ] Bullet/numbered lists are properly indented
- [ ] Bold/italic formatting applied correctly
- [ ] Internal context sections are clearly marked
- [ ] Source URLs are included (as text or hyperlinks)
- [ ] File saves successfully to the output directory

## Formatting Guidelines

### Page layout
- **Margins:** 1 inch all sides (default)
- **Font:** Calibri 11pt for body, Calibri Light for headings
- **Line spacing:** 1.15 (Word default)

### Tables
- Use **Table Grid** style with header row shading
- Keep column widths proportional to content
- Bold header row text
- No empty cells — use "N/A" or "—" if no data

### Internal context blocks
- Use blockquote/Intense Quote style
- Prefix with "**Internal Context (Work IQ):**"
- Add a note: *"Do not distribute externally."*

### Source attribution
- Include hyperlinks where possible: `doc.add_paragraph().add_run('Source').add_hyperlink('https://...')`
- If hyperlinks aren't supported in the chosen library, include full URLs as plain text in a Sources/References section at the end

## Rules

- **Install packages first.** Always run `npm install docx` or `pip install python-docx` before generating.
- **Preserve structure.** Map markdown heading levels to Word heading levels 1:1.
- **Tables must have headers.** Always include a bold header row.
- **Label internal content.** Work IQ findings must use the callout/blockquote style.
- **Include sources.** Every factual claim needs a URL in the document or in a References section.
- **Don't over-style.** Keep formatting clean and professional — no colored text, no decorative fonts.

## Output

Save the generated `.docx` file to the `work/output/` directory (or as specified by the calling agent) with a descriptive filename like `prd-feature-name.docx` or `competitive-brief.docx`.

```
