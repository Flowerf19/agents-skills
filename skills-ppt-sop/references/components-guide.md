# Components Guide

Read this file completely before adding any data visualization. Components are
copy-paste HTML fragments meant to be dropped into a grid cell of `base-template.html`
(or any slide). They are pure HTML + inline `<style>` — no canvas, no SVG libraries, no
external scripts. Bars and radar shapes are built with plain CSS so they print and embed
in a single file.

All components inherit the shared `:root` tokens defined in `templates-guide.md`. If you
use a component standalone, paste that `:root` block first.

## Universal rules for data viz

- **Right-align all numbers.** Labels left, values right. Always.
- **Consistent decimals.** Pick a precision per column (e.g. `1` decimal) and never mix.
- **Every chart is labeled.** Axis labels, a legend, or per-bar value labels — never a bare
  shape. A chart with no labels is a defect.
- **Tables have a total/summary row** when they show additive quantities.
- **≤3 accent colors** per slide; reuse `--aux1..4` in order for series.
- **Thousands separators** for values ≥ 1,000.

## `data-table.html` — data table

Use for financial data and metric grids.

- Header row: `--bg-header` background, white text, 16px/600.
- Body rows: zebra striping via `--gray-light` on even rows.
- **Numeric cells**: `text-align: right; font-variant-numeric: tabular-nums;` so digits
  align in columns.
- **Total row**: bold, top border in `--accent`, sits at the bottom.

Customize:
1. Edit `<th>` labels (first column = row label, rest = numeric columns).
2. Duplicate `<tr>` body rows per record.
3. Keep the final `<tr class="total">` and sum the columns.
4. Match decimal precision across every numeric column.

## `bar-chart.html` — bar chart

Use for period comparisons (Q1–Q4, month-over-month).

- CSS-only vertical bars; height set inline as a percentage of the tallest value:
  `style="height: 72%"`.
- Each bar carries a **value label above** and a **category label below** (the required
  axis labels).
- Series colors come from `--aux1..4` / `--accent`, in order.
- A baseline (x-axis) rule sits under the bars; optional gridlines use `--gray-light`.

Customize:
1. For each datum, set the bar's inline `height: N%` where `N = value / max * 100`.
2. Update the value label (formatted, right number of decimals) and category label.
3. Add a `.legend` row if more than one series.

## `radar-chart.html` — radar chart

Use for multi-dimensional comparison (e.g. scoring across 5–6 axes).

- Pure CSS/inline-SVG polygon: a light grid web plus one filled polygon per series using
  `--aux1` / `--accent` at reduced opacity.
- Each axis has a text label at its outer vertex (axis labels are mandatory).
- Keep to 5–6 axes for legibility.

Customize:
1. Set the polygon `points` (SVG coords) from each axis value (0 = center, 1 = outer ring).
2. Label every axis vertex.
3. Add a legend if comparing two series.

## `icons.html` — inline icon set

Use as small visual indicators (status, trend up/down, check/cross, info).

- Inline SVG icons, 20×20, `currentColor` so they take the surrounding text color.
- Apply `--aux2` for positive, `--accent` for negative/alert, `--gray` for neutral.
- Never use an icon font or external sprite — these are inline `<svg>` only.

Customize: copy the single `<svg>` you need; set `color:` via the parent element.

## Placement in the grid

A typical content slide:

- Full-width table → cell with `grid-column: span 12`.
- Two charts side by side → two cells `span 6`.
- KPI + supporting chart → `span 4` (icon/number) + `span 8` (chart).

## Pre-flight checklist (per component)

- [ ] All numbers right-aligned with tabular figures.
- [ ] Consistent decimal precision within each column/series.
- [ ] Axis labels / legend present on every chart.
- [ ] Total row present on additive tables.
- [ ] No external CSS/JS/SVG; everything inline.
- [ ] ≤3 accent colors used on the slide.
