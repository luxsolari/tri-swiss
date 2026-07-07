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
never touch on the same element (one named exception: see "Tri-part
segment stripe" under Structural Block below), and each component gets at
most one accent.

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

**Turquoise decorative accents.** Non-semantic, ornamental only — never as
a button's, tag's, status pip's, or link's own state color (see the
"Structural Block" section below for the one sanctioned exception: a
purely decorative hover-flourish on nav links, layered alongside — never
replacing — the link's real state feedback):

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

## Structural Block — sidebar, hero band, bold word, and Turquoise's own forms

A third job for Swiss Red, on top of dividers and card borders: a genuine
layout element, not just a border or divider. Three forms — pick one
block form per layout; the bold-word form is independent and may combine
with either.

**Sidebar / nav rail.** Full-height solid Red panel, capped at ~25% of
viewport width (min 220px, max 280px), sticky/fixed. Holds wordmark,
in-page anchor nav, theme toggle, external link, copyright:

```html
<aside class="sidebar" style="width:22%; min-width:220px; max-width:280px;
  background:var(--primary); color:var(--primary-foreground);
  position:sticky; top:0; height:100vh; display:flex; flex-direction:column;
  justify-content:space-between; padding:32px 28px; box-sizing:border-box;">
  <div>
    <span style="font-family:var(--font-mono); font-weight:700; font-size:1.5rem;">Wordmark</span>
    <nav style="margin-top:40px; display:flex; flex-direction:column; gap:16px;
      font-family:var(--font-mono); font-size:0.8rem; text-transform:uppercase; letter-spacing:0.15em;">
      <a href="#section-one" style="color:var(--primary-foreground); text-decoration:none;
         opacity:0.75; border-bottom:2px solid transparent; padding-bottom:2px;">Section One</a>
    </nav>
  </div>
  <div style="font-family:var(--font-mono); font-size:0.7rem; text-transform:uppercase;
    letter-spacing:0.12em; opacity:0.75;">© Year Author</div>
</aside>
```

**Hero band.** Solid Red horizontal block, used once, for a hero/intro
moment — an alternative to the sidebar, never combined with it:

```html
<div style="background:var(--primary); color:var(--primary-foreground); padding:64px 48px;">
  <h1 style="margin:0;">Hero title.</h1>
</div>
```

**Bold word/phrase accent.** One word inside a heading or paragraph in
Red, at normal weight/size — independent of the two block forms, may
combine with either:

```html
<p>Two structural colors, one strong <span style="color:var(--primary);">accent</span>.</p>
```

**Tri-part segment stripe.** Three equal solid blocks — ink, Red,
Turquoise — used as a static decorative divider or spacer, at any
length: a small marker before a heading, a section divider, a wider
closing flourish. The one named exception to "Red and Turquoise never
touch": scoped to this pattern only. Always three *equal* segments in
this exact order; static and decorative only, never interactive or
meaningful; used selectively (a handful of times per page) rather than
replacing the default `bg-border` divider throughout.

```html
<!-- Small marker before a heading -->
<div style="display:flex; gap:3px; width:36px;">
  <div style="height:3px; flex:1; background:var(--foreground);"></div>
  <div style="height:3px; flex:1; background:var(--primary);"></div>
  <div style="height:3px; flex:1; background:var(--highlight);"></div>
</div>

<!-- Wider closing flourish -->
<div style="display:flex; gap:6px; width:140px;">
  <div style="height:3px; flex:1; background:var(--foreground);"></div>
  <div style="height:3px; flex:1; background:var(--primary);"></div>
  <div style="height:3px; flex:1; background:var(--highlight);"></div>
</div>
```

**Turquoise hover-flourish on nav links.** Purely decorative, identical
regardless of state — layered alongside the link's real ink/red hover
feedback, never replacing it:

```html
<a href="#section" style="color:var(--primary-foreground); text-decoration:none;
   opacity:0.75; border-bottom:2px solid transparent; padding-bottom:2px;"
   onmouseover="this.style.borderBottomColor='var(--highlight)'"
   onmouseout="this.style.borderBottomColor='transparent'">Section</a>
```

(In `docs/index.html` this is implemented via a CSS `:hover` rule rather
than inline `onmouseover`, shown in Task 3 — the inline-handler form above
is illustrative of the effect for consumers who copy this pattern into a
project without the page's own stylesheet.)

**Turquoise callout/note panel.** Solid-Turquoise highlight box for tips,
notes, or dashboard callouts — content-sized, never full-bleed, capped at
a modest max-width matching prose/card width. May recur a few times per
page — a content-level device, not page chrome:

```html
<div style="background:var(--highlight); color:var(--highlight-foreground);
  padding:14px 16px; max-width:60ch;">
  <p style="margin:0; font-family:var(--font-mono); font-size:0.7rem;
    text-transform:uppercase; letter-spacing:0.12em; font-weight:700;">Tip</p>
  <p style="margin:8px 0 0; font-size:0.9rem;">Content-sized, never full-bleed.</p>
</div>
```

**Turquoise second-moment panel.** One larger solid-Turquoise section,
used once per page, appearing later in the page flow — never in the
hero, which stays Red's territory. Capped at roughly 15% of viewport
height, matching the closing band's cap:

```html
<section>
  <div style="background:var(--highlight); color:var(--highlight-foreground); padding:40px;">
    <p style="margin:0; font-family:var(--font-mono); font-weight:700; font-size:1.5rem;">
      One genuine moment.
    </p>
  </div>
</section>
```

**Turquoise closing band.** Solid-Turquoise horizontal strip near the
bottom of the page (opposite the sidebar), used once, capped at roughly
15% of viewport height:

```html
<div style="background:var(--highlight); color:var(--highlight-foreground); padding:28px 0;">
  <div class="content-inner" style="text-align:center;">
    <p style="margin:0; font-family:var(--font-mono); text-transform:uppercase;
      letter-spacing:0.15em; font-size:0.8rem; font-weight:700;">Closing statement.</p>
  </div>
</div>
```

Guardrails for all three: never adjacent to Red's own Structural Block
(no shared edges, no touching the sidebar/hero-band); combined footprint
stays visibly smaller than Red's block wherever both appear on the same
page; ink/cream still dominate every surface outside these blocks; one
accent per component still holds.

**Destructive button hover, Default vs. Hover.** Demonstrates the hover
hierarchy (see `SKILL.md`'s "Hover states"): Red carries the real signal.

```html
<button style="border:1px solid var(--primary); background:none;
  color:var(--primary); padding:16px 8px;">Delete</button>
<!-- :hover (or a static .is-hover-demo modifier class for illustration) -->
<button style="border:1px solid var(--primary); background:var(--primary);
  color:var(--primary-foreground); padding:16px 8px;">Delete</button>
```

**Nav-link hover, Default vs. Hover.** The Turquoise flourish layered
alongside the link's real ink/red state feedback:

```html
<a style="color:var(--primary-foreground); opacity:0.75; border-bottom:2px
  solid transparent; padding-bottom:2px;">Section</a>
<!-- :hover (or a static .is-hover-demo modifier class for illustration) -->
<a style="color:var(--primary-foreground); opacity:1; border-bottom:2px
  solid var(--highlight); padding-bottom:2px;">Section</a>
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

The visible nod to Tri-Swiss's sibling system, Lux Swiss (formerly Duotone
Swiss). Reserved for inline
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
