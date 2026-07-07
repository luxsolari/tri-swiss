# Accent Buttons, Accent Cards, and Hero Turquoise — Design Spec

> This is the Tri-Swiss half of a two-repo design. Lux Swiss's counterpart
> spec lives in `luxsolari/lux-swiss`
> (`docs/superpowers/specs/2026-07-07-accent-buttons-and-cards-design.md`)
> and covers the shared button/card patterns without the dual-accent
> hover pattern or hero changes, which are Tri-Swiss-only (Lux Swiss has
> only one accent color).

Status: approved
Date: 2026-07-07
Repo: `luxsolari/tri-swiss` (branch `feat/accent-buttons-cards-and-hero-turquoise`)

## 1. Purpose

Two related gaps, both about color richness:

1. **Very little hover-color use on buttons.** Of the four button
   variants, only Destructive (added last round) uses an accent color
   for its border/hover — Ghost/Outlined/Filled all hover via
   ink/muted-foreground only. Cards have no hover state at all.
2. **Tri-Swiss's tertiary color doesn't register on first page load.**
   Turquoise's existing touches (hover-only flourish, the thin tri-part
   stripe, a components-section demo cluster, a chart line) are all
   either invisible without scrolling or invisible without a pointer.
   The system doesn't visibly read as tri-color until well after the
   fold.

This spec adds two new component patterns (an Accent button, an Accent
card) usable by both house systems, a Tri-Swiss-only Interactive card
with a dual-accent hover transition, and three new turquoise touches in
the hero itself.

## 2. Non-goals

- Not a redesign of the existing Ghost/Outlined/Filled buttons or the
  existing plain Card / Emphasis card — all unchanged, this is additive.
- Not a loosening of "Turquoise never carries semantic/state meaning" —
  the new Accent card's Turquoise border and the dual-accent hover
  pattern are both still non-semantic; they don't indicate success,
  error, or any application state.
- Not a general loosening of "Red and Turquoise never touch" (spatial
  adjacency) — the dual-accent pattern is a *sequential* state
  transition (never both colors visible on the element at once), so it
  doesn't touch that guardrail at all. It's a new, separate exception to
  a different rule (see §4).
- Not a fourth color. Ink, cream, Swiss Red, and Pastel Turquoise remain
  the only four color tokens.

## 3. Accent button and Accent card (shared pattern, both systems)

**Accent button** — a new, fifth button variant: `border-primary`,
background none at rest; hover fills solid (`bg-primary`,
`text-primary-foreground`). Same swap mechanism as the existing
Destructive button, but for general emphasis/non-destructive use — e.g.
a secondary call-to-action that wants more visual weight than Ghost but
isn't a destructive action. Named "Accent" (not "Primary") to avoid
confusion with the existing `--primary` token name and the Filled
button's already-established "primary action, rare" role.

**Accent card** — a new static card variant: accent-colored border plus
a subtle accent-tinted background wash (e.g. `bg-primary/5`), stronger
than the existing plain-bordered "Emphasis card" (which uses a red
border alone, no wash). In Tri-Swiss this uses **Turquoise**
(`border-highlight`, `bg-highlight/5`, using the `--highlight-foreground`
text color from last round) — reinforcing Turquoise's existing
non-semantic decorative role, and giving it a second static-card
touchpoint distinct from Red's Emphasis card.

## 4. Interactive card and the dual-accent hover exception (Tri-Swiss only)

**Interactive card** — a new clickable card pattern (an `<a>` or
`<button>` wrapping card content): ink border at rest, transitioning on
hover. In Tri-Swiss, the hover transition uses the **dual-accent**
pattern below rather than plain Red.

**Dual-accent hover pattern.** The existing hover-state color hierarchy
(established last round) says: "Red is the only color permitted to
carry real hover-state meaning; Turquoise may only layer in
decoratively." The Accent button's Tri-Swiss variant and the Interactive
card both introduce one deliberate, named exception to that rule: the
element's border is Red at rest and swaps to Turquoise on hover (or vice
versa) — a genuine state-carrying color change using Turquoise, not a
decorative layer alongside Red.

This is a **sequential** transition, not simultaneous — the two colors
are never visible on the element at the same time, so it does not
touch the separate "Red and Turquoise never touch" adjacency guardrail
(§2). It is, however, a real exception to "Turquoise only decorates in
hover states," and must be named as such: **scoped to exactly these two
patterns (the Accent button and the Interactive card), not a general
loosening.** Everywhere else, the existing hover hierarchy holds
unchanged — Turquoise still never signals on its own in any other hover
context.

## 5. Hero turquoise (Tri-Swiss only)

Three additions to the hero section, on top of the existing tri-part
stripe:

1. **One hero CTA button switches to the Accent button's dual-accent
   style.** The hero currently has two CTAs ("See the components",
   filled ink; "View on GitHub", outlined ink). One of them (the
   non-primary one — "View on GitHub") becomes the new dual-accent
   Accent button: Red-bordered at rest, Turquoise-bordered/filled on
   hover.
2. **A new turquoise tagline beneath the hero title** — a short new
   phrase, sized between the title and the existing body paragraph,
   rendered in Turquoise. Independent of the existing hero paragraph
   text (not a recolor of existing words).
3. **A small turquoise icon accent near the title/wordmark** —
   lightweight, purely decorative (consistent with Turquoise's existing
   "icon fill used as a flourish" sanctioned use), smallest footprint of
   the three additions.

**Guardrail note:** this adds four turquoise touches to the hero
(the three above, plus the existing tri-part stripe). Each individual
addition must stay modest in scale — one small icon, one short tagline,
one CTA's border/hover, not new full-width blocks — so that "ink/cream
still visually dominate any surface" continues to hold even with more
turquoise touches than before.

## 6. Files and page changes

- `skills/tri-swiss/SKILL.md` — add the Accent button variant (Buttons
  section), the Accent card and Interactive card patterns (Philosophy or
  a new subsection), and the dual-accent hover exception (Hover states
  section, alongside the existing hierarchy rule).
- `skills/tri-swiss/references/components.md` — full HTML patterns for
  Accent button (including its Tri-Swiss dual-accent hover variant),
  Turquoise Accent card, and Interactive card; new Default/Hover swatch
  pairs demonstrating the dual-accent transition.
- `docs/index.html` — new Accent button + Accent card + Interactive card
  demos in the Components section (with Default/Hover swatches showing
  the dual-accent transition); the three hero additions (CTA swap,
  tagline, icon).
- `docs/assets/*.png` — re-capture to show the new hero state and the
  new Components-section demos.
- `CHANGELOG.md` — new entries under `[Unreleased]` → `### Added`
  documenting the Accent button, Accent card, Interactive card, the
  dual-accent hover exception, and the hero additions.
- `README.md` / `CONTRIBUTING.md` — reflect the new patterns in the
  aesthetic-summary / Design-changes paragraphs.

## 7. Rollout

Branch `feat/accent-buttons-cards-and-hero-turquoise` off `main`,
following this repo's own `AGENTS.md` conventions (Conventional Commits,
changelog-first, branch+PR only, no direct pushes to `main`).
