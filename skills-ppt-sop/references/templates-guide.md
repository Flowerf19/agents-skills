# Templates Guide

Read this file completely before assembling any presentation. It explains the shared
design tokens, the canvas contract, and how each template in `templates/` is meant to be
used and customized.

## The canvas contract (applies to every template)

Every slide is a fixed **960×540px** stage. Nothing reflows. To fit different screens the
stage is scaled as a whole with `transform: scale(...)`, never by changing the internal
layout.

```html
<div class="stage">   <!-- 960×540, position: relative, overflow: hidden -->
  <div class="safe"> <!-- 40px inset on all sides: the only area content may occupy -->
    ...slide content...
  </div>
</div>
```

- **Canvas**: `width: 960px; height: 540px;` — mandatory, never change.
- **Safe area**: 40px inset on all four sides. No text or meaningful graphics outside it.
- **Inner gutter**: keep text ≥20px from any edge of its own container.
- **Scaling**: the page wrapper may apply `transform: scale(var(--fit))` for preview, but
  the stage's internal pixel dimensions stay 960×540.

## Shared design tokens

Every template begins with the same `:root` block. Copy it verbatim — do not change values.

```css
:root {
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

  /* derived layout tokens */
  --safe: 40px;
  --col: 60px;     /* 12-column grid column width */
  --gap: 20px;     /* grid gutter */
  --radius: 8px;   /* card corner radius */
  --header-h: 60px;
  --font: -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
}
```

### Typography scale (never below 16px)

| Role | Size | Weight |
| --- | --- | --- |
| Cover title | 44px | 700 |
| Slide title (header) | 24px | 700 |
| Section heading | 20px | 600 |
| Body | 16px | 400 |
| Caption / label | 16px | 500 |

### The 12-column grid

Content slides use a 12-column grid: `12 × 60px` columns with `20px` gaps. Inside the safe
area (880px wide) this resolves cleanly. Use the grid for all multi-column content — never
eyeball pixel positions.

```css
.grid {
  display: grid;
  grid-template-columns: repeat(12, var(--col));
  gap: var(--gap);
}
.card { background: var(--gray-light); border-radius: var(--radius); padding: 20px; }
```

Span helpers: a half-width block is `grid-column: span 6`, a third is `span 4`, a quarter
is `span 3`.

## Template-by-template guide

### `cover.html` — title slide

- Full-bleed background using an `--accent` → dark gradient (the one place a gradient is
  allowed; keep it tasteful, not the generic purple-on-white anti-pattern).
- Centered title (44px/700) + subtitle (20px/400, `--gray`).
- Optional small kicker line above the title.
- Customize: replace `{{TITLE}}`, `{{SUBTITLE}}`, `{{KICKER}}`.

### `toc.html` — table of contents

- Numbered list, each row **60px** tall, separated by hairline dividers.
- Number badge uses `--accent`; section label is 20px/600.
- Keep to 5–6 entries max (matches the bullet limit).
- Customize: duplicate the `.toc-row` block per section; renumber.

### `base-template.html` — content slide

- This is the workhorse. Black **60px header** with the slide title, then a 12-column grid
  body inside the safe area.
- Drop `components/*` HTML into grid cells (a table spanning 12, two charts spanning 6 each,
  etc.).
- Customize: set `{{SLIDE_TITLE}}`, then fill `.grid` with cards/components.

### `summary.html` — key metrics

- Three equal metric cards (`grid-column: span 4` each) showing a big number + label +
  delta.
- Use `--aux2` (green) / `--accent` (red-orange) for positive/negative deltas.
- Customize: `{{METRIC_n_VALUE}}`, `{{METRIC_n_LABEL}}`, `{{METRIC_n_DELTA}}`.

### `end.html` — closing slide

- Centered "Thank you" (or custom) message, optional contact line in `--gray`.
- Same gradient family as the cover for bookend symmetry.
- Customize: `{{END_TITLE}}`, `{{CONTACT}}`.

## Navigation (single-file output)

`navigation-template.html` wraps N slides as sibling `<section class="slide">` elements,
shows one at a time, and provides prev/next + keyboard arrows + a slide counter. When
assembling:

1. Paste each slide's inner markup into its own `<section class="slide">`.
2. Set `data-total` / the counter to the real slide count.
3. The included script handles index state, arrow keys, and the on-screen buttons — no
   external JS.

## Pre-flight checklist (per template)

- [ ] `:root` token block present and unmodified.
- [ ] Stage is exactly 960×540; content inside the 40px safe area.
- [ ] No `<link>` / `<script src>` to anything external; no web fonts.
- [ ] No animation beyond the allowed slide-switch opacity (no decorative keyframes).
- [ ] Font sizes ≥16px; ≤3 accent colors on the slide; ≤6 bullets.
