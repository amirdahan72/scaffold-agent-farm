```skill
---
name: xlsx-writer
description: 'Create Microsoft Excel (.xlsx) workbooks with formatted tables, conditional formatting, and multiple sheets. Use when asked to produce a spreadsheet, export data to Excel, create a feature matrix, build a scorecard, or generate a filterable comparison table. Triggers: create xlsx, make a spreadsheet, export to Excel, generate workbook, build a matrix, comparison table, scorecard.'
---

# XLSX Writer

Create Microsoft Excel (.xlsx) workbooks from structured data. Converts comparison matrices, scorecards, prioritized lists, and tabular data into professionally formatted Excel files with header styling, conditional formatting, auto-filters, and multiple sheets.

## When to Use This Skill

- User asks to "create a spreadsheet", "export to Excel", or "make a matrix"
- A deliverable is naturally tabular — feature gap matrix, scorecard, prioritized signal list, sprint tracker
- PM needs a filterable, sortable artifact they can share and annotate
- Producing an `.xlsx` alongside or instead of a markdown deliverable

## Prerequisites

Before running this skill, install the required package:

```bash
npm install exceljs
```

## Workflow

### Step 1 — Analyze input content

Read the source data (markdown tables, JSON, or structured text) and identify:

- **Sheet structure** — how many sheets? (e.g., "Gap Matrix", "Battle Cards", "Raw Data")
- **Columns** — headers, data types (text, number, date, URL)
- **Rows** — data entries, any grouping or hierarchy
- **Conditional formatting targets** — cells that need color-coding (🔴🟡🟢 → red/yellow/green fills)

### Step 2 — Map data to Excel structure

| Source element | Excel output |
|---------------|-------------|
| Markdown table | Sheet with auto-filter, styled header row |
| Priority ratings (P0-P3) | Conditional fill: P0=red, P1=orange, P2=yellow, P3=green |
| Status indicators (🔴🟡🟢) | Conditional fill: red (#FF4444), yellow (#FFD700), green (#4CAF50) |
| Score / percentage | Number format with conditional color scale |
| URL / source link | Excel hyperlink |
| Section grouping | Separate sheets or grouped rows |

### Step 3 — Generate the workbook (Node.js)

```javascript
const ExcelJS = require("exceljs");

const workbook = new ExcelJS.Workbook();
workbook.creator = "<Author>";
workbook.created = new Date();

// --- Sheet 1: Main data ---
const sheet = workbook.addWorksheet("<Sheet Name>");

// Define columns
sheet.columns = [
  { header: "Feature", key: "feature", width: 30 },
  { header: "Our Product", key: "ours", width: 20 },
  { header: "Competitor A", key: "compA", width: 20 },
  { header: "Competitor B", key: "compB", width: 20 },
  { header: "Gap Status", key: "gap", width: 15 },
  { header: "Source", key: "source", width: 40 },
];

// Style header row
const headerRow = sheet.getRow(1);
headerRow.font = { bold: true, color: { argb: "FFFFFFFF" }, size: 11 };
headerRow.fill = {
  type: "pattern",
  pattern: "solid",
  fgColor: { argb: "FF0E2841" },  // Navy dark
};
headerRow.alignment = { vertical: "middle", horizontal: "center" };

// Add data rows
const data = [
  // { feature: "...", ours: "...", compA: "...", compB: "...", gap: "🟢 Parity", source: "https://..." },
];
data.forEach((row) => sheet.addRow(row));

// Auto-filter on all columns
sheet.autoFilter = {
  from: { row: 1, column: 1 },
  to: { row: data.length + 1, column: sheet.columns.length },
};

// Conditional formatting for gap status column
sheet.addConditionalFormatting({
  ref: `E2:E${data.length + 1}`,
  rules: [
    {
      type: "containsText",
      operator: "containsText",
      text: "Missing",
      style: { fill: { type: "pattern", pattern: "solid", bgColor: { argb: "FFFF4444" } } },
    },
    {
      type: "containsText",
      operator: "containsText",
      text: "Partial",
      style: { fill: { type: "pattern", pattern: "solid", bgColor: { argb: "FFFFD700" } } },
    },
    {
      type: "containsText",
      operator: "containsText",
      text: "Parity",
      style: { fill: { type: "pattern", pattern: "solid", bgColor: { argb: "FF4CAF50" } } },
    },
  ],
});

// Freeze header row
sheet.views = [{ state: "frozen", ySplit: 1 }];

// Write to file
await workbook.xlsx.writeFile("<output-path>.xlsx");
console.log("Workbook created: <output-path>.xlsx");
```

### Step 4 — Multi-sheet workbooks

For complex deliverables, create multiple sheets:

```javascript
// Sheet 2: Summary / Scorecard
const summary = workbook.addWorksheet("Scorecard");
summary.columns = [
  { header: "Area", key: "area", width: 25 },
  { header: "Status", key: "status", width: 12 },
  { header: "Score", key: "score", width: 10 },
  { header: "Notes", key: "notes", width: 50 },
];
// ... style and populate

// Sheet 3: Raw data / Sources
const sources = workbook.addWorksheet("Sources");
// ... add source URLs and attribution
```

### Step 5 — Add hyperlinks for source attribution

```javascript
// Add a clickable hyperlink to a cell
const cell = sheet.getCell("F2");
cell.value = { text: "Source link", hyperlink: "https://example.com/source" };
cell.font = { color: { argb: "FF0078D4" }, underline: true };
```

## Common Workbook Types

| Deliverable | Sheets | Key Features |
|-------------|--------|-------------|
| Feature gap matrix | Matrix, Sources | Auto-filter, gap status color-coding, source hyperlinks |
| Readiness scorecard | Scorecard, Details | 🔴🟡🟢 conditional fills, score column |
| Sprint tracker | Shipped, Carried, Decisions | Status formatting, date columns |
| Customer signals | Signals (by priority), By Customer | P0-P3 conditional fills, sortable |
| Competitive pricing | Comparison, Notes | Number formatting, tier columns |

## Style Standards

| Element | Style |
|---------|-------|
| Header row fill | Navy `#0E2841`, white bold text |
| Alternating rows | Light gray `#F2F2F2` on even rows |
| Header font | 11pt, bold, white |
| Body font | 10pt, regular, black |
| Column width | Auto-fit to content, minimum 12 |
| Frozen panes | Always freeze row 1 (header) |
| Auto-filter | Always enabled on data tables |

## Rules

- **Always freeze the header row** and enable auto-filter — PMs expect sortable/filterable sheets.
- **Always include source attribution** — hyperlinks in a "Source" column or a separate "Sources" sheet.
- **Use conditional formatting** for any status/priority/score column — visual at-a-glance is the whole point of Excel.
- **Keep sheet names short** (max 31 chars, Excel limit).
- **One concept per sheet** — don't mix the gap matrix and the scorecard on the same sheet.
- **If source data is markdown tables**, parse them row-by-row. Don't try to dump raw markdown into cells.

## Troubleshooting

| Issue | Solution |
|-------|----------|
| `Cannot find module 'exceljs'` | Run `npm install exceljs` |
| Conditional formatting not visible | Check `argb` values include alpha channel (e.g., `FF` prefix) |
| Column too narrow | Set explicit `width` on column definition or use auto-fit |
| Sheet name too long | Excel limits sheet names to 31 characters |
| Hyperlinks not clickable | Use `{ text: "label", hyperlink: "url" }` object format |

```
