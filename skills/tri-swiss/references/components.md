# Component Catalogue

Detailed patterns beyond the core set in `SKILL.md`. All snippets assume the theme
tokens from `assets/theme.css` are installed. Tailwind class names are shown; the
same tokens work as plain CSS custom properties on other stacks.

## Status pips (source / connection indicators)

Inline label with a leading dot whose color encodes state. State is expressed by the
dot color only — the text stays neutral. `--highlight` never appears here (see
"The `--highlight` token" in `SKILL.md`).

```
inline-flex items-center gap-1.5
font-mono text-xs uppercase tracking-[0.15em]

dot: h-1.5 w-1.5 rounded-full
  connected:    bg-foreground
  warning:      bg-primary
  loading:      bg-muted-foreground/30 animate-pulse
  disconnected: bg-foreground/25
```

## Modal overlay

Backdrop dims the page with a translucent background (not a shadow). The panel is a
hard-bordered box.

```
overlay: fixed inset-0 z-50 flex items-center justify-center bg-background/80 px-6
panel:   w-full max-w-[560px] border border-foreground bg-background p-6
         font-mono text-sm
```

## Toggle controls (theme, language, binary options)

Two (or more) inline options separated by a middot; the active option is
`text-foreground`, the rest are muted with a hover lift. No switch chrome, no pill —
just weight/contrast difference.

```jsx
<div className="flex items-baseline gap-1.5 font-mono text-xs uppercase tracking-[0.2em]">
  <button className={active ? "text-foreground" : "text-muted-foreground hover:text-foreground"}>
    Option A
  </button>
  <span className="text-muted-foreground/40">·</span>
  <button className={active ? "text-muted-foreground hover:text-foreground" : "text-foreground"}>
    Option B
  </button>
</div>
```

## SVG charts

All graphs are hand-rolled SVG by default. No chart library — Recharts / chart.js /
victory carry aesthetic opinions (rounded corners, default palettes, tooltips) that
fight this system. Palette is foreground / muted / primary, **plus `--highlight`
only when there's a genuine second data series to distinguish** — never a default color
choice.

Sizing: `width="100%"`, a fixed pixel height, and a `viewBox` computed from the data
range. No responsive charting library needed — the viewBox does the scaling.

Reference chart types:

- **Timeline strip** — a horizontal event bar. Discrete events render as dots along
  the bar; milestones/objectives as vertical tick marks.
- **Differential line** — a delta value over time with a zero-crossing fill:
  `foreground` above the axis, `primary` below.
- **Paired curve** — two lines on the same axes, one solid (primary series,
  `foreground`) and one dashed (secondary series, `muted-foreground`).
- **Share bars** — horizontal stacked bars for proportional data; the highlighted
  slice is distinguished with an outline ring, not a different hue.
- **Governed second-series line** — when a chart genuinely needs a *third* visually
  distinct line (not just primary/muted), draw it solid in `--highlight` — this is
  one of the only two sanctioned uses of Turquoise in the whole system. Never use
  it as a default third color "because it's there."

The through-line: encode categories and emphasis with fill opacity, stroke style
(solid vs dashed), and outline rings first — reach for `--highlight` only when
those tools are exhausted and a chart truly needs a third distinct line.

### Observable Plot (the one sanctioned library)

Default to hand-rolled SVG. When a library is warranted, use Observable Plot and
restyle it — never its defaults:

```js
import * as Plot from "https://cdn.jsdelivr.net/npm/@observablehq/plot@0.6/+esm";

const chart = Plot.plot({
  width: 640, height: 240,
  style: { background: "transparent", color: "var(--foreground)",
           fontFamily: "'Geist Mono', monospace", fontSize: "11px" },
  x: { label: null }, y: { label: null },
  marks: [
    Plot.gridY({ stroke: "var(--muted-foreground)", strokeOpacity: 0.15 }),
    Plot.ruleY([0], { stroke: "var(--muted-foreground)", strokeOpacity: 0.4 }),
    Plot.lineY(data, { x: "t", y: "v", stroke: "var(--foreground)", strokeWidth: 1.5 }),
    Plot.lineY(data, { x: "t", y: "u", stroke: "var(--muted-foreground)",
                       strokeWidth: 1.5, strokeDasharray: "4 3" }),
    // Only add this third mark when a genuine second series needs distinguishing
    // from both the primary and muted lines — the one sanctioned chart use of highlight:
    Plot.lineY(data, { x: "t", y: "w", stroke: "var(--highlight)", strokeWidth: 1.5 }),
  ],
});
document.querySelector("#mount").append(chart);
```

Rules: explicit palette colors per mark (no default scheme), no color legend,
square bars, muted grid, solid-vs-dashed for series, outline ring (not hue) for a
highlighted slice, `--highlight` reserved for a genuine third line only.

## Accent expansion — Red dividers, emphasis borders, Turquoise decoration

Beyond their original jobs (Red: primary action/destructive/ring. Turquoise:
chart second series/brand moment), both accents now recur more often across
a page — Red picks up two new structural jobs; Turquoise gets decorative
reuse. Guardrails: ink/cream still dominate any surface, Red and Turquoise
never touch on the same element, and each component gets at most one accent.

**Section-divider rule, red variant.** The default section-header rule is
`bg-border`; swap to `bg-primary` for a section that earns emphasis (used
selectively — a page's opening section, a "featured" callout — not on
every divider):

```jsx
<div className="mb-4 flex items-baseline gap-3">
  <h5>Section title</h5>
  <span className="h-px flex-1 bg-primary" />
</div>
```

**Selective card border.** One card in a set — the featured one, the
current one, the first-in-list — swaps its border from `border-border` to
`border-primary`. Never the default for a whole grid:

```jsx
<div className="border border-primary bg-card p-6">
  <p className="font-mono font-bold">Featured</p>
  <p className="text-sm text-muted-foreground">
    One card in a set — never the default for a whole grid.
  </p>
</div>
```

**Turquoise decorative accents.** Non-semantic, ornamental only — never on
a button, tag, status pip, or link:

```jsx
{/* Icon fill, used as a flourish rather than a state cue */}
<Icon className="icon" style={{ color: "var(--highlight)" }} />

{/* Underline beneath a heading or label */}
<span className="border-b-2" style={{ borderColor: "var(--highlight)" }}>
  Underlined label
</span>

{/* Background wash behind a block, softer than a hard border */}
<div className="bg-highlight/10 p-4">Separated without a border</div>

{/* Dot accent, matching the existing dot-indicator pattern */}
<span className="h-1.5 w-1.5 rounded-full" style={{ background: "var(--highlight)" }} />
```

## Iconography (geist-icons, restyled)

`geist-icons` is the only sanctioned icon set. Drop in the icon's SVG and add
`class="icon"`; the CSS restyle below overrides the library's own defaults (CSS
beats SVG presentation attributes, so the paths are untouched).

```css
.icon { width: 20px; height: 20px; fill: none; stroke: currentColor;
  stroke-width: 1.5; stroke-linecap: square; stroke-linejoin: miter; }
```

```html
<span class="inline-flex items-center gap-2 font-mono text-xs uppercase tracking-[0.2em]">
  <svg class="icon" viewBox="0 0 24 24"><!-- geist-icons 'arrow-right' paths --></svg>
  Read more
</span>
```

Sizing 16–20px. Never let an icon replace the mono text label — it augments it.
For React, import from `geist-icons` and pass the same stroke props (or the
`.icon` class):

```jsx
import { IconArrowRight } from "geist-icons";

<IconArrowRight className="icon" />
```

## Hero / display type (Geist Mono)

The hero title/wordmark and section/chapter dividers inside long-form
editorial content both use Geist Mono at display weight/size — the same
face as every other heading, just scaled up. No separate register needed.

```jsx
<h1 className="font-mono" style={{ fontSize: "3.5rem", fontWeight: 700, letterSpacing: "-0.01em" }}>
  Page Title.
</h1>

<div className="font-mono" style={{ fontSize: "1.5rem", fontWeight: 700 }}>
  02 — Chapter Title
</div>
```

## Annotations & captions (Space Mono, italic only)

The visible nod to Tri-Swiss's sibling system (Duotone Swiss). Reserved for inline
annotations and figure captions — never emphasis, never a heading.

```jsx
<figcaption className="font-annotation italic text-sm text-muted-foreground">
  Fig. 3 — quarterly share by category.
</figcaption>
```

## Cards

Elevation is a background step, never a shadow.

```
border border-border bg-card text-card-foreground p-6
```

Nest a section divider (see `SKILL.md`) inside for titled regions. Keep corners
square unless a `rounded-md` (0.5rem) genuinely helps. For the one emphasized
card in a set, see the selective-border pattern under "Accent expansion"
above — swap `border-border` for `border-primary`.

## Inputs

```
border border-border bg-input px-3 py-2 font-mono text-sm
placeholder: text-muted-foreground
focus: outline-none ring-1 ring-ring
```

Labels above inputs use the uppercase mono label pattern from `SKILL.md`.
