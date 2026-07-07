# Turquoise Structural Block and Hover-State Hierarchy — Design Spec

Status: approved
Date: 2026-07-07
Repo: `luxsolari/tri-swiss` (branch `feat/turquoise-structural-block-and-hovers`)

## 1. Purpose

Tri-Swiss and Lux Swiss currently read as nearly identical at a glance:
Tri-Swiss's third color, Pastel Turquoise, is confined to small, easy-to-miss
touches (a hover-only flourish, one segment of the tri-part stripe, a small
components-section demo cluster, a chart line). The system doesn't visibly
prove it's a tri-color system rather than "duotone plus an occasional red."

This spec gives Turquoise genuine structural presence — a Structural Block
of its own, parallel to Red's — makes the tri-part segment stripe (the
one place all three colors already appear together) reusable at varying
lengths as a general decorative divider/spacer instead of one fixed
instance, and formalizes the color hierarchy for hover states. All three
changes are made visible on the showcase page.

## 2. Non-goals

- Not a fourth color. Ink, cream, Swiss Red, and Pastel Turquoise remain the
  only four CSS custom properties for color.
- Not a promotion of Turquoise to semantic status — it still never carries
  state meaning (success/info/second-interactive-state). This spec adds
  *structural* and *hover-adjacent decorative* presence, not meaning.
- Not a redesign of the existing Ghost/Outlined/Filled button defaults —
  they keep hovering via ink/muted-foreground shifts, unaffected.
- Not a change to the tri-part segment stripe's structure (still three
  *equal* solid blocks, ink/Red/Turquoise, in that order, still the one
  named exception to "Red and Turquoise never touch") — only its length
  and where it may appear become flexible.
- Not a change to the other existing purely decorative Turquoise jobs
  (icon fills, washes, chart lines, underlines, dot accents) — unchanged,
  this is additive.

## 3. Turquoise Structural Block

A new, third job for Pastel Turquoise (alongside its existing chart/brand-
moment job and its purely decorative reuse), giving it real layout
presence — but kept clearly secondary to Red's own Structural Block.

Three forms, independently usable and combinable:

1. **Callout/note panel** — a solid-turquoise highlight box for UI content
   (tip/info banners, form annotations, dashboard callouts). Content-sized,
   never full-bleed, capped at a modest max-width matching prose/card
   width. May recur a few times per page — it's a content-level device, not
   page chrome.
2. **Second-moment panel** — one larger solid-turquoise section, used once
   per page, appearing later in the page flow (never in the hero, which
   stays Red's territory).
3. **Closing band** — a solid-turquoise horizontal strip near the bottom of
   the page (opposite the sidebar), used once, capped at roughly 15% of
   viewport height.

**Guardrails:**

- **Never adjacent to Red's Structural Block.** The existing "Red and
  Turquoise never touch" rule extends explicitly to these new blocks — no
  shared edges, no touching the sidebar/hero-band. (The tri-part segment
  stripe remains the one named exception, unchanged.)
- **Smaller scale than Red's block.** Red keeps its ~25% viewport cap.
  Turquoise's forms are individually capped smaller: the callout panel is
  content-sized (no viewport-relative cap), and the second-moment panel and
  closing band are each capped at roughly 15% of viewport height. Wherever
  both Red's and Turquoise's blocks appear on the same page, Turquoise's
  combined footprint stays visibly smaller than Red's.
- **Ink/cream still dominate every surface outside governed color
  blocks** — unchanged from the existing rule.
- **One accent per component still holds** — a single panel is never both
  Red's Structural Block and Turquoise's Structural Block (the tri-part
  stripe is the one named exception, unchanged).

## 4. Tri-part segment stripe as a flexible decorative device

Today the tri-part stripe (ink/Red/Turquoise, three equal solid blocks) is
documented as a single fixed-width (64px) bar used once, beneath the hero.
It's the one place all three colors already appear together, which makes
it the clearest visual proof that this is a tri-color system — so it
should be reusable, not a one-off.

**Rule:** the stripe may be used at any length as a decorative divider or
spacer — a small marker before a heading, a section divider between two
blocks of content, a wider closing flourish — anywhere a purely decorative
horizontal (or vertical) rule would otherwise go. Height/thickness stays
thin and consistent with the existing convention (the three segments
always equal width to each other, always in ink/Red/Turquoise order); only
the overall length varies by context.

**Guardrails (unchanged from today, now stated explicitly for reuse):**

- Always three *equal* segments in ink/Red/Turquoise order — never
  reweighted, reordered, or reduced to two colors.
- Static and decorative only — never interactive, never a progress/status
  indicator, never carrying meaning.
- Stays a deliberate, recognizable signature mark — used selectively (a
  handful of times per page) rather than replacing the default
  `bg-border` divider throughout. Overuse would dilute it into wallpaper
  and undercut the "one named exception to Red-and-Turquoise-never-touch"
  status it holds.

## 5. Hover-state color hierarchy

A new, system-wide rule governing how the two accents behave in hover
states, generalizing the existing nav-link-specific footnote:

> Wherever a hover state uses an *accent* color (not just an ink/muted-
> foreground tone shift) to signal interactivity, **Red is the only color
> permitted to carry that real signal**. Turquoise never signals on its own
> in a hover state — it may only layer in as a purely decorative flourish
> alongside Red's or ink's real feedback.

This does not require every hoverable element to use an accent color — the
majority of buttons/tags/toggles hover via ink/muted-foreground shifts only
and are unaffected. It governs only the cases where an accent participates.

**New button variant demonstrating the rule:** a **Destructive** button —
`border-primary text-primary`, hover fills with `bg-primary`/
`text-primary-foreground`. Red's "destructive" job was already named in the
philosophy (`Red: primary action/destructive/ring`) but had no documented
button variant until now.

## 6. Making hover states visible on the showcase

Static screenshots can't capture a live `:hover`, and the existing
Turquoise hover-flourish on nav links is invisible without a mouse. Add two
"Default / Hover" static swatch pairs — a resting-state element beside a
second element carrying the exact same hover CSS applied via a static
modifier class, so both a live visitor *and* a screenshot see the state:

1. **Destructive button** — default vs. red hover fill.
2. **Nav link** — default vs. ink color-shift + Turquoise underline
   flourish.

The real interactive elements elsewhere on the page (the actual sidebar
nav, the actual destructive button if placed in a live context) keep
genuine `:hover` CSS — the static swatches are documentation, layered
alongside, not a replacement for real interaction.

## 7. Files and page changes

- `skills/tri-swiss/SKILL.md` — add the Turquoise Structural Block job and
  its guardrails (Philosophy section, near the existing Structural Block/
  `--highlight` discussion); state the tri-part stripe's reuse-as-divider
  rule explicitly (same area of the Philosophy section); add the
  hover-state color hierarchy rule (new subsection, likely near "The
  `--highlight` token" or "Buttons").
- `skills/tri-swiss/references/components.md` — new patterns: Turquoise
  callout/note panel, second-moment panel, closing band (under the
  existing "Structural Block" section); update the existing "Tri-part
  segment stripe" entry to show it at more than one length and state the
  reuse guardrails; new Destructive button variant (under "Buttons" —
  currently only Ghost/Outlined/Filled exist); new "Default / Hover"
  swatch-pair pattern for both the destructive button and the nav-link
  flourish.
- `docs/index.html` — implement all three Turquoise Structural Block forms
  (callout panel in Components, second-moment panel after Charts, closing
  band near the footer); add at least one more tri-part stripe instance at
  a different length in a new location (in addition to the existing hero
  instance); add a Destructive button demo; add the two Default/Hover
  static swatch pairs.
- `docs/assets/*.png` — re-capture to show the new blocks, the additional
  stripe instance, and the swatches.
- `CHANGELOG.md` — new entries under `[Unreleased]` documenting the
  Turquoise Structural Block, the tri-part stripe's reuse-as-divider rule,
  the hover-state hierarchy rule, and the Destructive button variant.
- `README.md` / `CONTRIBUTING.md` — reflect the new patterns in the
  aesthetic-summary / Design-changes paragraphs.

## 8. Rollout

Branch `feat/turquoise-structural-block-and-hovers` off `main`, following
this repo's own `AGENTS.md` conventions (Conventional Commits,
changelog-first, branch+PR only, no direct pushes to `main`).
