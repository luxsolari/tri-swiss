# Changelog

All notable changes to this plugin are documented here. Format follows
[Keep a Changelog](https://keepachangelog.com/en/1.0.0/); this project adheres to
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- **`--primary-foreground-strong` on-token** — the `card` cream `#faf6ec`
  in light (4.8:1 on Swiss Red; the brand cream lands at 4.48:1), warm ink
  `#1a120e` in dark (4.8:1 on the lifted red), for small text on
  a Swiss Red fill: sidebar nav and footer, hero-band labels, the hovered
  Destructive button's text, Default / Hover captions on red. Added
  rather than re-tinting a brand color: cream on `#e2503f` is 3.4:1 and
  no light foreground can reach 4.5:1 on it, while darkening the red
  breaks red-on-black. `--primary-foreground` stays the brand cream for
  heading-size text on red (`assets/theme.css`, `SKILL.md`,
  `docs/assets/site.css`).
- **Documentation site** — the single showcase page is now a navigable
  site under `docs/` that is both showcase and documentation: Overview,
  Colors, Typography, Spacing, Components (nineteen patterns with live
  demos and guidelines, opening with an at-a-glance gallery), Structural
  Block, Highlight (the Turquoise token's own page), Charts and House
  Mark, all sharing `docs/assets/site.css` (tokens plus every pattern)
  and `docs/assets/site.js` (theme / flavor / Jost toggles, mobile nav,
  live palette values). Content and structure follow the extracted
  Tri-Swiss design-system artifact.
- **House banner** — `docs/banner.html`, the system's cover (red slab,
  ink block, a smaller turquoise block, three hairline rules, the
  tri-part segment stripe, the wordmark in Space Mono), rendered to
  `docs/assets/banner-light.png` and `banner-dark.png` by
  `scripts/capture/banner.sh` (headless Chrome). It heads the README
  (theme-aware `<picture>`) and the site's Overview.

### Changed
- `scripts/capture/capture.mjs` shoots from the per-page site and adds
  `plot.png`; `scripts/capture/verify-philosophy.mjs` checks every page
  plus the shared stylesheet and script. README and social screenshots
  regenerated.
- README "See it" now indexes the site's pages.

## [1.1.0] — 2026-07-10

### Added
- **Jost heading accent** — a governed, optional fourth typeface that
  gives **every heading** (`h1`–`h6`), plus the hero title/wordmark and
  long-form editorial chapter dividers (styled as headings), real
  protagonism when toggled — via a new `--font-display` role (defaults
  to `--font-mono`, no separate face, until the `.jost` class is
  applied). Labels, nav, tags, and data values stay on `--font-mono`
  directly — functional UI text, not headings. Unlike `.geist`, `.jost`
  is not a flavor — it never touches `--font-mono`/`--font-sans`
  directly, so it composes independently with either flavor and with
  `.dark`, exactly like `.dark` composes with `.geist`. This is
  Tri-Swiss's own original register, reintroduced and given its full
  intended reach. The showcase page gets a third, independent sidebar
  toggle ("Hero display": Mono · Jost) that reskins every heading on
  the page, including a live "02 — Chapter Title" example demonstrating
  the editorial-divider job, and `scripts/capture/capture.mjs` can
  screenshot any flavor/accent combination.

## [1.0.0] — 2026-07-10

### Changed
- **BREAKING: default typography flavor switched from Geist to Space** —
  `--font-mono`/`--font-sans` now default to Space Mono/Space Grotesk
  (matching sibling system Lux Swiss); Geist Mono/Geist Sans become the
  opt-in `.geist` flavor instead (`assets/theme.css`, `SKILL.md`,
  `docs/index.html`). Projects relying on Geist as the default need to
  add the `.geist` class to `<html>` to keep the old look.
- **Typography restructured from four registers to three** — the
  standalone `--font-annotation`/`font-annotation` role (Space Mono,
  italic only) is retired; annotations/captions now use
  `var(--font-mono)` + `font-style: italic` in whichever flavor is
  active, matching how Lux Swiss has always handled annotations
  (`assets/theme.css`, `SKILL.md`, `references/components.md`,
  `docs/index.html`).

### Added
- **Geist font flavor** — a `.geist` class (composes with `.dark`,
  exactly like the theme) that swaps `--font-mono`/`--font-sans` to
  Geist Mono/Geist Sans; `--font-serif` (Zilla Slab) is shared by both
  flavors and never overridden. `SKILL.md`'s setup steps ask which
  flavor to apply (default: Space), each with its own 3-family Google
  Fonts link. The showcase page (`docs/index.html`) gets a live
  Space·Geist toggle next to the Light·Dark one, and
  `scripts/capture/capture.mjs` can screenshot either flavor.

## [0.2.0] — 2026-07-08

### Added
- **Accent button and Accent card** — a fifth button variant
  (Red-bordered, general emphasis rather than destructive) and a new
  static Turquoise-bordered card with a background wash, addressing
  that only the Destructive button previously used an accent color on
  hover/border (`SKILL.md`, `references/components.md`,
  `docs/index.html`).
- **Interactive card and the dual-accent hover exception** — a new
  clickable card pattern, and a second named exception (this time to
  the hover-hierarchy rule): the Accent button and Interactive card
  swap Red for Turquoise on hover — a sequential state transition, not
  simultaneous adjacency, scoped to exactly these two patterns
  (`SKILL.md`, `references/components.md`, `docs/index.html`).
- **Hero turquoise** — a new turquoise tagline and icon accent beneath
  the hero title, and the hero's second CTA now uses the Accent button's
  dual-accent style, so Tri-Swiss's tri-color identity registers on
  first page load instead of only after scrolling (`docs/index.html`).
- **Lists and tables** — new component patterns: an unordered/ordered
  list treatment (thin top-border dividers, no bullet glyphs, tabular
  mono-font numbers) and a data table (bold mono header, tabular-nums,
  no zebra striping) — markers and borders stay ink/muted-foreground
  only, never an accent color (`SKILL.md`, `references/components.md`,
  `docs/index.html`).
- **Images in the grid** — new guidance for the first time: grid-aligned
  placement with a bordered container and mono-label caption, defaulting
  to a grayscale/duotone color treatment with full color permitted only
  as a scoped, named exception when the photograph itself is the
  primary content (`SKILL.md`, `references/components.md`,
  `docs/index.html`).
- **Richer typography demonstrations** — the `#registers` section's
  Body, Annotation, and Serif specimens now show full realistic examples
  instead of one-line placeholders, and a new "Content at any length"
  section demonstrates short/medium/long-form body copy at the same
  measure (`docs/index.html`).

### Fixed
- **Stray hardcoded font name in the Observable Plot reference snippet**
  — `references/components.md`'s chart example set `fontFamily` to a
  literal `'Geist Mono', monospace` instead of `var(--font-mono)`, the
  only place in the system that bypassed the font role variables (the
  shipped `docs/index.html` chart already used the token correctly).

## [0.1.0] — 2026-07-07

### Added
- Initial release of the **Tri-Swiss** design-system skill (`tri-swiss`).
- **`SKILL.md`** — philosophy (tri-tone, more colorful, Swiss-minimalist),
  full light/dark palette token tables including the governed `--highlight`
  token, typography rules across four registers (Geist Mono, Geist Sans,
  Space Mono italic, Zilla Slab), spacing/layout, and core component patterns
  (buttons, tags, section dividers, selective red card-border emphasis,
  decorative Turquoise accents, a Structural Block sidebar/hero-band/
  bold-word job for Swiss Red, the tri-part segment stripe, and a
  Turquoise hover-flourish on nav links).
- **`assets/theme.css`** — ready-to-paste Tailwind 4 theme with every CSS
  variable for light and dark mode, wired to Tailwind via `@theme inline`;
  also usable as plain CSS custom properties on non-Tailwind stacks.
- **`references/components.md`** — extended component catalogue: status
  pips, modal overlay, toggle controls, cards, inputs, hero/annotation
  type patterns, accent-expansion patterns (red dividers, red card
  borders, turquoise decoration), Structural Block patterns (sidebar/nav
  rail, hero band, bold word, tri-part segment stripe, turquoise
  hover-flourish), and the hand-rolled + Observable Plot chart patterns.
- **Showcase landing page** (`docs/index.html`) served on GitHub Pages —
  restructured around a persistent sidebar nav (collapsing to a red top
  band with a hamburger toggle on mobile) — plus README screenshots, a
  1200×630 social-preview card, and a reproducible Playwright capture
  script.
- **Philosophy-compliance verifier** (`scripts/capture/verify-philosophy.mjs`)
  — automated check that the showcase page never uses a rogue color, a
  shadow, an unrestyled icon cap, or leaks `--highlight` into a semantic
  UI role.
- **Turquoise Structural Block** — a third job for Pastel Turquoise,
  parallel to Red's own Structural Block but smaller in scale: a
  callout/note panel, a single second-moment panel, and a closing band
  (`SKILL.md`, `references/components.md`, `docs/index.html`).
- **Tri-part segment stripe reuse** — the stripe is no longer a single
  fixed instance; it's now a general decorative divider/spacer reusable
  at any length, demonstrated at three different lengths on the showcase
  page (`SKILL.md`, `references/components.md`, `docs/index.html`).
- **Hover-state color hierarchy** — a system-wide rule: Red is the only
  color permitted to carry real hover-state meaning; Turquoise may only
  layer in decoratively. Demonstrated via a new Destructive button
  variant and two "Default / Hover" static swatch pairs on the showcase
  page (`SKILL.md`, `references/components.md`, `docs/index.html`).
- **`--highlight-foreground` token** — constant black text/icon color for
  content placed on solid `--highlight` fills, needed now that Turquoise
  has real panel/band backgrounds (`assets/theme.css`, `SKILL.md`).

### Changed
- **Dual-licensed the repo.** The design system itself
  (`skills/tri-swiss/`, `docs/index.html`, `docs/assets/`,
  `HOUSE-MARK.md`) is now licensed under CC BY-SA 4.0 — free to use and
  adapt, including commercially, with attribution and share-alike
  required. Tooling and scripts remain MIT/X11. See `LICENSE-DESIGN` and the
  README's License section.
