# Tri-Swiss — Design Spec

Status: approved (pending final user sign-off on this document)
Date: 2026-07-06
Repo: `luxsolari/tri-swiss` (not yet created — created during implementation)
Sibling of: [lux-design-system](https://github.com/luxsolari/lux-design-system) (Duotone Swiss)

## 1. Purpose

Tri-Swiss is a second Claude Code design-system plugin, part of the same house
family as Duotone Swiss (lux-design-system), but with its own typographic and
chromatic identity. It preserves every Swiss-minimalist structural rule from
the original system — visible 1px borders, no shadows, square corners,
generous whitespace, uppercase-mono labels — while replacing:

- **Typography**: Space Grotesk/Space Mono core → Geist Sans/Geist Mono core,
  plus two additional governed registers (Space Mono italic, Jost) not present
  in the original.
- **Palette model**: duotone-strict (two functional colors + one accent) →
  tri-tone (two functional colors + one accent + one narrowly-governed second
  "highlight" color).
- **Iconography**: Lucide → `geist-icons`.

The two systems are meant to be visibly related (same governance philosophy,
same file shape, same "sanctioned exception" pattern for optional registers/
icons/charts) but not identical — Tri-Swiss is its own design language, not a
theme variant of Duotone Swiss. (An earlier in-repo variant approach — a
`.alt` class toggling Duotone Swiss into a Geist flavor — was tried and
removed; see `lux-design-system` PR #5. Tri-Swiss supersedes that idea with
a proper sibling system.)

## 2. Non-goals

- Not a fork or a runtime-switchable variant of Duotone Swiss. No shared
  code; a new, independent plugin repo.
- Not a departure from Swiss-minimalist geometry — no shadows, no rounded
  corners, no change to spacing/layout/component patterns beyond what's
  listed in this spec.
- Not a redesign of the component catalogue's semantics — buttons, tags,
  status pips, cards, inputs keep the same variants and roles as Duotone
  Swiss, just restyled with the new tokens/fonts.

## 3. Palette

### Light mode

| Token | Hex | Role |
|-------|-----|------|
| `--background` | `#eae8d0` | Page background — warm cream |
| `--foreground` | `#000000` | Body text, active controls, borders |
| `--card` | `#f1efdb` | Elevated surface (small lift off background) |
| `--card-foreground` | `#000000` | Text on card surfaces |
| `--primary` | `#d3281b` | Swiss Red — accent, destructive, ring, focus |
| `--primary-foreground` | `#eae8d0` | Text on primary |
| `--secondary` | `#000000` | Secondary action background (ink, unchanged role) |
| `--secondary-foreground` | `#eae8d0` | Text on secondary |
| `--muted` | `#e2dfc7` | Subtle backgrounds — hover, code blocks |
| `--muted-foreground` | `#4a4838` | Subdued labels, metadata, placeholders |
| `--border` | `#000000` | All borders |
| `--input` | `#f1efdb` | Input field background (same as `--card`) |
| `--ring` | `#d3281b` | Focus ring |
| `--highlight` | `#56bfa3` | Pastel Turquoise — see §3.1, non-semantic |

`--card`/`--muted`/`--muted-foreground` were not given exact values by the
user; the hexes above were derived using the same small lightness/hue-step
method Duotone Swiss used off its two functional colors. Treated as a
reasonable first pass, visually adjustable during implementation without
needing to re-open this spec.

### Dark mode

| Token | Hex | Role |
|-------|-----|------|
| `--background` | `#000000` | Near-black |
| `--foreground` | `#eae8d0` | Cream text |
| `--card` | `#161616` | Slightly lifted surface |
| `--primary` | `#e2503f` | Swiss Red lifted for dark contrast (first-pass value) |
| `--secondary` | `#eae8d0` | Inverted |
| `--secondary-foreground` | `#000000` | — |
| `--muted` | `#1f1f1f` | Subtle dark surface |
| `--muted-foreground` | `#a8a696` | Warm grey, readable but recessed |
| `--border` | `#eae8d0` | Full cream |
| `--input` | `#161616` | Input field background (same as dark `--card`) |
| `--ring` | `#e2503f` | — |
| `--highlight` | `#63cbae` | Pastel Turquoise, slightly lifted for dark mode (first-pass value) |

Dark-mode `--primary`/`--highlight` values are first-pass, hand-tuned
approximations (same non-mathematical method the original system used) and
may be adjusted visually during implementation.

### 3.1 The `--highlight` token — governed, non-semantic

Pastel Turquoise is **not** a second accent in the semantic sense (not
success/info/a second interactive color). It is a rare structural highlight,
reserved for exactly two uses:

1. A second data series/stroke in hand-rolled SVG or Observable Plot charts.
2. One single brand moment (e.g. the demo page's hero accent, or a logo
   mark) — never repeated elsewhere on the same page.

**Do not** rule (added to Tri-Swiss's "Do not" list): Turquoise never appears
in buttons, tags, status pips, links, or any other UI state indicator. If a
new UI element wants a second color for meaning, the answer is still "no —
use weight, size, or whitespace instead," exactly as in Duotone Swiss.

## 4. Typography — four registers

| Tier | Font | License / source | Role |
|------|------|-------------------|------|
| Primary | **Geist Mono** | SIL OFL, Google Fonts / `geist` npm | Display, headings, data values, tags, nav, labels — same role mono has always had |
| Primary | **Geist Sans** | SIL OFL, Google Fonts / `geist` npm | Body copy **and** dense-data/utility (tables, fine print) at smaller size with tabular figures — absorbs the role Inter played in Duotone Swiss; no separate utility face |
| Secondary | **Space Mono** (italic only) | SIL OFL, Google Fonts | Inline annotations and figure captions only — never emphasis (emphasis is always weight). Always loaded as a real web font, not documentation-only. The deliberate "family lineage" nod tying Tri-Swiss to Duotone Swiss. |
| Tertiary | **Zilla Slab** | SIL OFL, Google Fonts | Long-form editorial body and pull-quotes. Never UI. Unchanged from Duotone Swiss. |
| 4th register | **Jost** | SIL OFL, Google Fonts | Hero title/wordmark, and section/chapter dividers in long-form editorial content. Never a UI heading, never a pull-quote, never running body text. Substitutes for Futura PT (a paid ParaType font) — Jost is a free geometric sans explicitly modeled on Futura/Kabel (round `O`, triangular `A`), keeping the "one public Google Fonts link, no licensing friction" pattern every other register uses. |

Weight scale for Geist Sans/Mono stays restrained — 300/400/500/700 drawn
from the fonts' full 100–900 variable axis — mirroring Duotone Swiss's
"range comes from weight, not more typefaces" rule. Jost and Zilla Slab each
get one or two weights as needed for their narrow roles; no variable-weight
scale needed for either.

Total families shipped: **4** (Geist Sans, Geist Mono, Space Mono, Zilla
Slab) plus Jost for the 4th register = **5** font files, loaded via one
combined Google Fonts link — matching Duotone Swiss's single-link pattern.

## 5. Iconography

`geist-icons` (MIT, npm) becomes the sole sanctioned icon set, replacing
Lucide. It ships monoline, single-color, zero-dependency SVG/React
components accepting standard SVG props (`color`, `size`, `title`).

Regardless of the library's own default styling, Tri-Swiss applies the same
restyle rule Duotone Swiss uses on Lucide, via one CSS rule:

```css
.icon { width: 20px; height: 20px; fill: none; stroke: currentColor;
  stroke-width: 1.5; stroke-linecap: square; stroke-linejoin: miter; }
```

Same governance as before: no other icon library, no icon fonts, no emoji.

## 6. Components & spacing

Unchanged from Duotone Swiss: max content width, 1px borders everywhere, no
shadows (elevation via background-color step), restrained radius, buttons
(ghost/outlined/filled), tags/pills, status pips, section-header pattern.
Labels switch from Space Mono to Geist Mono. Charts keep the hand-rolled SVG
default plus Observable Plot as the one sanctioned library when warranted,
restyled to foreground/muted/primary **and now `--highlight`** for a second
series (see §3.1) — the first time this system's chart guidance uses a
fourth color.

## 7. Repo scaffolding

Full parity with lux-design-system's shape, renamed:

- `.claude-plugin/plugin.json` — name `tri-swiss`, keywords updated
  (`tri-tone`, `geist`, `swiss-minimalist`, `tailwind`, `ui`, `frontend`,
  `theming`, `geist-sans`, `geist-mono`)
- `AGENTS.md`, `CONTRIBUTING.md`, `LICENSE` (MIT) — same whiting-based
  release discipline (Conventional Commits, semver-from-tags,
  changelog-first workflow, branch+PR only, no direct pushes to `main`)
- `skills/tri-swiss/SKILL.md` + `assets/theme.css` +
  `references/components.md`
- `docs/index.html` live demo (GitHub Pages) + `docs/assets/` +
  `scripts/capture/capture.mjs` (Playwright screenshot capture, same
  pattern as lux-design-system, no variant-toggle jobs)
- `README.md`, `CHANGELOG.md` (Keep a Changelog format)
- `.github/workflows/release.yml`, `scripts/hooks/` (commit-msg, pre-push),
  `scripts/suggest_version_bump.py`

GitHub repo `luxsolari/tri-swiss` is created during implementation (not
during this design phase), with the same branch → PR → review workflow used
throughout this session. Nothing lands on `main` without review.

## 8. Open items for implementation (explicitly deferred, not blocking)

- The dark-mode `--primary`/`--highlight` lift values (§3, marked
  "first-pass") are hand-tuned approximations, not measured conversions —
  fine to nudge visually against the demo page during implementation
  without re-opening this spec.
- `references/components.md` full component catalogue — port from
  lux-design-system 1:1 with token/font substitutions; no new components
  introduced by this spec.
