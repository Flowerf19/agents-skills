---
name: skills-ppt-sop
description: Generate professional HTML presentations with strict design standards. Use when creating (1) complete multi-slide presentations with navigation, (2) individual slide pages (cover, TOC, content, summary, end), (3) data visualizations (tables, charts). Output is single-file HTML with zero external dependencies.
argument-hint: A presentation topic, slide outline, or dataset to turn into HTML slides.
---

# HTML Presentation Generator

Generate professional HTML presentations following the Within7 design standard with strict design constraints.

## Decision tree

Choose output format based on requirements:

| Requirement | Output format | When to use |
| --- | --- | --- |
| Complete presentation with navigation | Single HTML file | Default choice, most presentations |
| Individual standalone slides | Multiple HTML files | User explicitly requests separate files |
| Quick prototype | Single HTML file | When testing layout/design |

## Workflow

### Step 1 — Analyze requirements

Identify which slide types are needed:

- **Cover page** — title, subtitle, optional background
- **TOC page** — table of contents with numbered sections
- **Content pages** — data tables, charts, text content
- **Summary page** — key metrics and takeaways
- **End page** — thank you / contact information

### Step 2 — Load templates

**MANDATORY — READ ENTIRE FILE:** before proceeding you MUST read `references/templates-guide.md` completely.

For each slide type, load the corresponding template:

- Cover → `templates/cover.html`
- TOC → `templates/toc.html`
- Content → `templates/base-template.html`
- Summary → `templates/summary.html`
- End → `templates/end.html`

### Step 3 — Integrate components

**MANDATORY — READ ENTIRE FILE:** for data visualization you MUST read `references/components-guide.md` completely.

Copy component HTML from the `components/` directory:

- Tables → `components/data-table.html`
- Bar charts → `components/bar-chart.html`
- Radar charts → `components/radar-chart.html`
- Icons → `components/icons.html`

### Step 4 — Assemble presentation

**For single-file output:**

1. Start with `templates/navigation-template.html` as the base.
2. Replace slide sections with content from the templates.
3. Update the slide count and navigation logic.
4. Verify all CSS variables are defined.

**For multi-file output:**

1. Create a separate HTML file for each slide.
2. Use individual templates directly.
3. Name files `slide-01.html`, `slide-02.html`, etc.

## Design system constraints

### Core rules

- Canvas size: **960×540px** (16:9 at 72dpi).
- Safe margins: **40px** on all sides.
- Responsive: **ONLY** use `transform: scale(...)`, NO reflow.
- Single-file HTML, zero external dependencies.

### CSS variables

```css
--bg-main: #FFFFFF;
--bg-header: #000000;
--accent: #F85d42;
--gray: #74788d;
--aux1: #556EE6;
--aux2: #34c38f;
--aux3: #50a5f1;
--aux4: #f1b44c;
--gray-light: #F5F5F5;
--page-bg: #e0e0e0;
```

### Layout system

- 12-column grid (60px columns, 20px gaps).
- Card: 8px border-radius, 20px padding.
- Header: 60px height, black background.

## NEVER do these

### Common mistakes

**Breaking layout**

- Never change canvas size (960×540px is mandatory).
- Never use responsive reflow (use `transform: scale` only).
- Never remove safe margins (40px minimum).

**Design violations**

- Never use external CSS/JS files.
- Never use system fonts other than specified.
- Never modify CSS variable values.
- Never add custom animations.

**Content errors**

- Never exceed 5–6 bullet points per slide.
- Never use font sizes smaller than 16px.
- Never place text within 20px of edges.
- Never use more than 3 accent colors per slide.

### Anti-patterns

**Generic AI-generated designs**

- Purple gradients on white backgrounds.
- All corners rounded to 8px.
- Cookie-cutter card layouts.
- Generic, personality-free styles.

**Poor data visualization**

- Charts without axis labels.
- Tables without total rows.
- Numbers left-aligned (always right-align).
- Inconsistent decimal formatting.

## Quick reference

### File structure

```
skills-ppt-sop/
├── SKILL.md
├── references/
│   ├── templates-guide.md
│   └── components-guide.md
├── templates/
│   ├── navigation-template.html
│   ├── cover.html
│   ├── toc.html
│   ├── base-template.html
│   ├── summary.html
│   └── end.html
└── components/
    ├── data-table.html
    ├── bar-chart.html
    ├── radar-chart.html
    └── icons.html
```

### Template selection guide

| Slide type | Template | Key features |
| --- | --- | --- |
| Cover | `cover.html` | Gradient background, centered title |
| TOC | `toc.html` | Numbered list, 60px row height |
| Content | `base-template.html` | 12-column grid, card system |
| Summary | `summary.html` | 3-column metric cards |
| End | `end.html` | Centered thank-you message |

### Component usage

| Component | File | Best for |
| --- | --- | --- |
| Data table | `data-table.html` | Financial data, metrics |
| Bar chart | `bar-chart.html` | Q1–Q4 comparisons |
| Radar chart | `radar-chart.html` | Multi-dimensional metrics |
| Icons | `icons.html` | Visual indicators |

## Output verification

Before delivering, verify:

- All CSS variables are defined.
- Canvas size is 960×540px.
- No external dependencies.
- Navigation works (for single-file output).
- All numbers are right-aligned in tables.
- Font sizes meet minimum requirements.
- Safe margins maintained.
