---
name: tri-swiss
description: >
  Tri-Swiss — Lux Solari's Geist-based house design system, sibling to
  lux-swiss (Lux Swiss, formerly Duotone Swiss). A tri-tone visual language (ink + cream,
  a Swiss Red accent, and a governed non-semantic highlight)
  Swiss-minimalist layout, visible borders, no shadows, and Geist / Geist Mono
  typography. Use this skill whenever building, styling, or restyling ANY user
  interface: React/Next/Svelte/Vue components, HTML pages, landing pages,
  dashboards, buttons, cards, forms, navigation, modals, tags, charts, or
  Tailwind/CSS themes — even when the user does not name the design system
  explicitly. Apply it by default so every project shares the same aesthetic.
  Also trigger on phrases like "make this look good", "style this", "apply my
  design system", "tri-tone", "swiss", "geist", "give it a theme", or when
  starting a new frontend from scratch. When another design language is
  explicitly requested (Material, shadcn defaults untouched, a client's brand
  kit, or the sibling lux-swiss/Lux Swiss system specifically),
  defer to that instead.
---

# Tri-Swiss — Design System

A strict, minimalist visual language built around the Geist typeface family.
Two structural colors plus one strong accent plus a governed, non-semantic
highlight; hard borders, generous whitespace, monospace labels. The whole
point is restraint: every element earns its place or is removed, and
**most difference is still expressed through typography, spacing, and
contrast — Red and Turquoise are a deliberate, governed layer on top, never
a substitute for that discipline.**

Tri-Swiss is one of Lux Solari's two house-mark design systems — the
personal brand identity carried into every project built with them. Its
sibling, [Lux Swiss](https://github.com/luxsolari/lux-swiss) (formerly
Duotone Swiss), applies the same governance philosophy through a strict
two-color-plus-accent palette; see `HOUSE-MARK.md` for how the two relate.

## When you apply this

Whenever you build or restyle UI, reach for these tokens and patterns by
default instead of inventing ad-hoc colors or leaning on a component
library's stock look. Two setup moves come first on any new project:

1. **Install the theme.** Copy [`assets/theme.css`](assets/theme.css) into
   the project's global stylesheet (e.g. `app/globals.css`). It defines
   every CSS variable for light + dark mode, and wires them to Tailwind 4
   via `@theme inline`. For non-Tailwind stacks the same `:root` / `.dark`
   variables work as plain CSS custom properties.
2. **Load the fonts.** Add the Google Fonts link (below) or the framework
   equivalent (`next/font`, etc.).

Then compose UI from the patterns in this file. For the full component
library (status pips, modals, toggles, SVG charts) see
[`references/components.md`](references/components.md).

## Philosophy — two rules that govern everything

**Tri-tone, more colorful.** Ink and cream are still the two structural
colors. Swiss Red is the primary accent — primary action, destructive,
focus ring — and now also marks section-divider rules and a selectively
emphasized card/component border (one card in a set, never the whole
grid). It also has a third job: a Structural Block — a solid-color sidebar/nav
rail or hero band (pick one per layout, capped at ~25% of viewport
width/height), plus an independent bold-word accent inside a heading that
may combine with either. Outside that one block, ink/cream continue to
dominate every other surface exactly as before. Pastel Turquoise stays a **third, non-semantic** color — it never
carries meaning (no success/info/second-interactive-state use) — but is
no longer rationed to one touch per page: it recurs as pure decoration
(icon fills, underlines, background washes, dot accents, a chart's
second series, a brand/hero moment) anywhere its presence or absence
wouldn't change what the user understands about state. It still never
appears on a button, tag, or status pip, and never as a link's own
state-indicating color — though a link may carry it as a purely
decorative hover-flourish (see the `--highlight` section below). Three guardrails keep this
from tipping into loud: ink/cream still visually dominate any surface;
Red and Turquoise never touch or sit adjacent on the same element; one
accent per component, not both. The **tri-part segment stripe** — three equal solid blocks, ink/Red/
Turquoise in a row, used for a static decorative bar (e.g. beneath a hero
title) — is the one explicitly named exception to "Red and Turquoise
never touch": a single governed device, not a general loosening. Nowhere
else may the two sit adjacent.

Turquoise also gets a genuine layout job of its own — a **Turquoise
Structural Block**, parallel to Red's but kept clearly secondary: a
callout/note panel (content-sized, may recur a few times per page), a
single second-moment panel used once later in the page flow, or a
closing band used once near the page's end. These never touch or sit
adjacent to Red's own Structural Block, and their combined footprint
stays visibly smaller than Red's block wherever both appear on the same
page — Red keeps its ~25% viewport cap; Turquoise's forms are smaller
(the second-moment panel and closing band each capped around ~15% of
viewport height; the callout panel is content-sized, no viewport cap).
Ink/cream still dominate every surface outside these governed blocks,
and one accent per component still holds.

The tri-part segment stripe isn't limited to one instance either: it may
be reused at any length as a decorative divider or spacer — a small
marker before a heading, a wider closing flourish — anywhere a purely
decorative rule would otherwise go, as long as it stays three *equal*
segments in ink/Red/Turquoise order and is used selectively rather than
replacing the default `bg-border` divider throughout.

**Swiss-minimalist.** Borders are visible (1px solid, full ink or full
cream). No shadows — elevation comes from a background-color step (`--card`
vs `--background`). Whitespace is generous. Labels are uppercase monospace
with wide letter-spacing. Corners are mostly square.

## Palette

Use the semantic token, never a raw hex. `bg-background`, `text-foreground`,
`border-border`, `text-muted-foreground`, `bg-primary`, `text-highlight`, etc.

### Light mode
| Token | Hex | Role |
|-------|-----|------|
| `--background` | `#f5efe0` | Page background — warm cream |
| `--foreground` | `#000000` | Body text, active controls, borders |
| `--card` | `#faf6ec` | Elevated surface — card, popover |
| `--card-foreground` | `#000000` | Text on card surfaces |
| `--primary` | `#d3281b` | Swiss Red — accent, destructive, ring |
| `--primary-foreground` | `#f5efe0` | Text on primary |
| `--secondary` | `#000000` | Secondary action background |
| `--secondary-foreground` | `#f5efe0` | Text on secondary |
| `--muted` | `#ebe5d5` | Subtle backgrounds — hover, code blocks |
| `--muted-foreground` | `#4a4838` | Subdued labels, metadata, placeholders |
| `--border` | `#000000` | All borders — full ink for structural clarity |
| `--input` | `#faf6ec` | Input field background |
| `--ring` | `#d3281b` | Focus ring |
| `--highlight` | `#56bfa3` | Pastel Turquoise — governed, non-semantic (see below) |
| `--highlight-foreground` | `#000000` | Text/icon color on solid `--highlight` fills — constant black in both modes since Turquoise stays light/pastel even in dark mode |

### Dark mode
| Token | Hex | Role |
|-------|-----|------|
| `--background` | `#000000` | Near-black |
| `--foreground` | `#f5efe0` | Cream text |
| `--card` | `#161616` | Slightly lifted surface |
| `--primary` | `#e2503f` | Red lifted for dark contrast |
| `--secondary` | `#f5efe0` | Inverted |
| `--secondary-foreground` | `#000000` | — |
| `--muted` | `#1f1f1f` | Subtle dark surface |
| `--muted-foreground` | `#a8a696` | Warm grey — readable but recessed |
| `--border` | `#f5efe0` | Full cream — maintains structural clarity |
| `--input` | `#161616` | — |
| `--ring` | `#e2503f` | — |
| `--highlight` | `#63cbae` | Pastel Turquoise, lifted for dark mode |
| `--highlight-foreground` | `#000000` | Same constant black — Turquoise stays light/pastel even in dark mode |

Dark mode is the `.dark` class on `<html>`. Toggle with
`document.documentElement.classList.toggle("dark", isDark)` and persist under
a `theme` key in `localStorage`. In Tailwind 4 the variant is
`@custom-variant dark (&:is(.dark *));` (already in `theme.css`).

### The `--highlight` token — read this before using it

Pastel Turquoise is not a second accent. It carries **zero semantic
meaning** — never success, never info, never a second interactive state.
Unlike Swiss Red, it is never the answer when an element needs to signal
something. It IS sanctioned for open-ended **decorative** reuse:

1. A second data series/stroke in a hand-rolled SVG or Observable Plot chart.
2. A brand/hero moment (e.g. a hero accent or a logo mark).
3. An icon fill on a single icon, used as a flourish rather than a state cue.
4. An underline or rule beneath a heading or label.
5. A background wash (e.g. `bg-highlight/10`) behind a block that wants
   separation without a hard border.
6. A dot accent, matching the existing dot-indicator pattern.
7. A hover-triggered flourish on a nav link or label — an underline or dot
   that appears on `:hover`, purely ornamental and identical regardless of
   active/current/visited state, layered *alongside* the element's
   existing ink/muted-foreground hover color change (which still carries
   the real interactive feedback).

It can recur multiple times on the same page — the old "exactly one
brand moment" cap is gone — but the test that governs every use is
unchanged: if turquoise's presence or absence would change what the user
understands about *state*, it's wrong. If it's purely ornamental and
removable without changing meaning, it's fine.

**Do not** use it as a button's, tag's, status pip's, or link's own
state-indicating color. A link may show a decorative Turquoise
hover-flourish (item 7 above) *in addition to* its real ink/red state
feedback — Turquoise itself never signals the state (see "Hover states"
below for the system-wide rule this follows).

## Typography

Four registers, strictly separated by function — Geist is the primary
voice, the other three are governed extras with exactly one job each.

| Tier | Font | Role |
|------|------|------|
| Primary | **Geist Mono** (`font-mono`) | Headings, display, data values, tags, nav, labels, hero title/wordmark, and section/chapter dividers in long-form editorial content |
| Primary | **Geist Sans** (`font-sans`) | Body copy, prose, **and** dense-data/utility text (tables, fine print) at smaller size with tabular figures |
| Secondary | **Space Mono, italic only** (`font-annotation italic`) | Inline annotations and figure captions only — never emphasis |
| Tertiary | **Zilla Slab** (`font-serif`) | Long-form editorial body and pull-quotes — never UI |

```html
<link href="https://fonts.googleapis.com/css2?family=Geist:wght@100..900&family=Geist+Mono:wght@100..900&family=Space+Mono:ital,wght@0,400;1,400&family=Zilla+Slab:ital,wght@0,400;0,500;0,700;1,400&display=swap" rel="stylesheet" />
```

Geist Sans and Geist Mono load as **variable fonts** (100–900 axis); Space
Mono loads regular + italic; Zilla Slab loads its usual four cuts.

**Range comes from weight, not more typefaces.** Within the Primary
register, hierarchy is expressed through the weight axis, not a new face:

| Weight | Use |
|--------|-----|
| **300** Light | Large display sub-decks, lead paragraphs |
| **400** Regular | Body copy |
| **500** Medium | UI emphasis, active labels, small headings in prose |
| **700** Bold | Strong emphasis — sparing; prefer 500 |

**Heading scale** — Geist Mono, weight 700: `h1` 3rem/−0.02em/1.1 ·
`h2` 2.25rem/−0.02em/1.15 · `h3` 1.875rem/−0.01em/1.2 · `h4` 1.5rem/1.3 ·
`h5` 1.25rem/1.3 · `h6` 1.125rem/1.3.

**Body** — Geist Sans 400, 1rem base, line-height 1.65, `kern`/`liga`/`calt`
on, antialiased.

**Label pattern** (pervasive — nav, metadata, section headers): Geist Mono,
`text-xs`, `uppercase`, `tracking-[0.2em]`, weight 400 inactive / 700 active,
`text-muted-foreground` inactive → `text-foreground` active.

**Tabular figures for data.** Numerals in a column, table, chart axis, or
stat block get `font-variant-numeric: tabular-nums`
(`font-feature-settings: "tnum" 1`) so digits share one width and align to
the grid. Prose numerals stay proportional (the default).

**Space Mono italic** is reserved for one structural job: **inline
annotations and figure captions** (e.g. a `<figcaption>` or a marginal
note). It is never used for emphasis — emphasis is always weight. It is
also the visible nod tying Tri-Swiss to its sibling system, Lux Swiss
(formerly Duotone Swiss; it uses Space Mono/Grotesk as its own core pairing).

## Spacing & layout

- **Max content width:** 1000–1200px centered, `px-6` gutters.
- **Radius:** restrained — `0.5rem` base, rarely used. Most corners are square.
- **Borders:** 1px solid `--border` everywhere. **No shadows** — elevation is a
  background step (`--card` on `--background`).
- **Section header:** uppercase mono label with a full-width rule beside it.
  The rule is `bg-border` by default; swap to `bg-primary` for a section
  that earns emphasis (used selectively — one or two per page, never on
  every divider).

```jsx
<div className="mb-4 flex items-baseline gap-3">
  <h5>Section title</h5>
  <span className="h-px flex-1 bg-border" />
</div>
```

## Buttons

All buttons: `font-mono uppercase tracking-[0.2em] text-xs`. Four variants:

- **Ghost / nav** (most common): `text-muted-foreground hover:text-foreground
  transition-colors`, no border.
- **Outlined:** `border border-foreground px-4 py-2 hover:bg-foreground
  hover:text-background`.
- **Filled** (primary action, rare): `border border-foreground bg-foreground px-4
  py-2 text-background hover:bg-foreground/90`.
- **Destructive:** `border border-primary text-primary px-4 py-2
  hover:bg-primary hover:text-primary-foreground` — Red's already-named
  "destructive" job (see Philosophy), now with a documented variant.
  Demonstrates the hover hierarchy below: Red carries the real hover
  signal.

**Disabled** is always `opacity-40` — never a color change.

## Hover states

Wherever a hover state uses an *accent* color — not just an ink/muted-
foreground tone shift — to signal interactivity, **Red is the only color
permitted to carry that real signal.** Turquoise never signals on its own
in a hover state; it may only layer in as a purely decorative flourish
alongside Red's or ink's real feedback — as the Destructive button above
and the nav-link hover-flourish (see the `--highlight` token section)
both demonstrate. This doesn't require every hoverable element to use an
accent color: most buttons/tags/toggles hover via ink/muted-foreground
shifts only, unaffected by this rule.

## Tags / pills

Small inline badges: `font-mono text-[0.65rem] px-2 py-0.5 uppercase
tracking-[0.12em]`.

- **Neutral:** plain text, `text-muted-foreground`.
- **Signal** (latest / highlight): `bg-foreground text-background`.
- **Outlined:** `border border-dashed border-foreground/50 text-foreground/80`, 4px
  dot prefix.
- **Saved:** `border border-foreground/30 text-foreground/50`, 4px dot prefix.

`rounded-full` is reserved for dot indicators only — never on containers or pills.

## Iconography

**`geist-icons` is the single sanctioned icon set** — mirroring the
one-icon-library rule, no other icon library, no icon fonts, no emoji.
Restyle its three off-identity defaults; keep everything else:

| Attribute | geist-icons default | Tri-Swiss |
|-----------|---------------------|-----------|
| `stroke-width` | varies by icon | `1.5` |
| `stroke-linecap` | varies by icon | `square` |
| `stroke-linejoin` | varies by icon | `miter` |

Apply via one CSS rule (CSS overrides SVG presentation attributes, so the
raw icon markup needs no editing):

```css
.icon { width: 20px; height: 20px; fill: none; stroke: currentColor;
  stroke-width: 1.5; stroke-linecap: square; stroke-linejoin: miter; }
```

Icons are 16–20px, `currentColor` (inherit `foreground`/`muted-foreground`;
`primary` only for the destructive/accent cases already reserved for it —
never `highlight`), and **augment** the uppercase-mono labels — never
replace them.

## Charts

Hand-rolled SVG is the default — the simple charts (timeline strip, share
bars, single differential line) are ~20 lines of raw SVG. Colors are
foreground / muted / primary only, **except** when a chart genuinely has a
second data series worth distinguishing — that series, and only that one,
may use `--highlight`. `width="100%"`, fixed height, `viewBox` from the data
range.

When scales / axes / many-series / interaction genuinely warrant a library,
the **single sanctioned choice is [Observable Plot](https://observablehq.com/plot)**
(framework-agnostic SVG — no other chart library, no canvas libs). Restyle
it to the palette: set mark `fill`/`stroke` to `--foreground`/
`--muted-foreground`/`--primary` explicitly (never Plot's default color
scheme; `--highlight` only for a genuine second series), disable the color
legend, keep bars square, use explicit muted grid marks, and restyle axes
to the mono label pattern. See
[`references/components.md`](references/components.md) for the full pattern.

## Do not

- **No third semantic color.** `--highlight` is not success green or info
  blue in disguise — it carries no state meaning anywhere.
- **No accent-colored hover signal other than Red.** Turquoise may only
  decorate a hover state, never carry its meaning alone.
- **No shadows.** Depth is border presence + background steps.
- **No chart libraries except restyled Observable Plot.** Hand-rolled SVG is
  the default; reach for Plot only when complexity earns it, restyled to
  the palette.
- **No `rounded-full` on containers.** Dots only.
- **No raw hex in markup.** Always the semantic token.
- **Icons: restyled geist-icons only.** Monoline, `currentColor`, square
  caps. No icon fonts. **No emoji** in UI text unless explicitly requested.
- **No Turquoise on buttons, tags, status pips, or links.** Decorative
  reuse elsewhere is fine; semantic/interactive roles are not.
- **No red-and-turquoise on the same element.** Pick one accent per
  component, never both.
- **Ink/cream still dominate.** Accents are seasoning; a surface where
  Red or Turquoise out-covers ink/cream has gone too far.
