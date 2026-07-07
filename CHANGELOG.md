# Changelog

All notable changes to this plugin are documented here. Format follows
[Keep a Changelog](https://keepachangelog.com/en/1.0.0/); this project adheres to
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

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

### Changed
- **Dual-licensed the repo.** The design system itself
  (`skills/tri-swiss/`, `docs/index.html`, `docs/assets/`,
  `HOUSE-MARK.md`) is now licensed under CC BY-SA 4.0 — free to use and
  adapt, including commercially, with attribution and share-alike
  required. Tooling and scripts remain MIT. See `LICENSE-DESIGN` and the
  README's License section.
