# Tri-Swiss — Colorful Accents & Jost Removal — Design Spec

Status: approved (pending final user sign-off on this document)
Date: 2026-07-06
Repo: `luxsolari/tri-swiss`
Supersedes (partially): [2026-07-06-tri-swiss-design.md](2026-07-06-tri-swiss-design.md) §3.1 (`--highlight` governance) and §4 (typography, 4th register)
Branch: `feat/expand-accents-drop-jost`

## 1. Purpose

Two changes to the shipped-but-unreleased Tri-Swiss system (no tag cut yet,
so this amends the pending initial release rather than adding a v2):

1. **More colorful accents.** Swiss Red and Pastel Turquoise become more
   visually present across the system, without turning it into a
   multi-color UI kit. The tri-tone *frame* survives; the rationing rules
   around the two accents loosen.
2. **Drop Jost.** The 4th typographic register (hero title/wordmark +
   chapter dividers) is removed. Its two jobs fold into Geist Mono, which
   already handles all other display/heading duty. Four font files ship
   instead of five; three registers instead of four.

## 2. Non-goals

- Not a departure from Swiss-minimalist geometry — still no shadows, no
  rounded corners beyond dot indicators, still visible 1px borders.
- Not a semantic promotion of Turquoise. It still never means
  success/info/state/a second interactive color. Its cap loosens; its
  meaning does not change.
- Not a new token surface. No new CSS custom properties are introduced for
  color — the expanded usage reuses `--primary`/`--highlight` with
  Tailwind opacity modifiers (`/40`, `/10`, etc.), the same pattern already
  used for `border-foreground/50` elsewhere in the system.
- Not a rewrite of the automated verifier. `verify-philosophy.mjs`'s
  palette-purity check and its "`--highlight` never in a status pip" check
  both still hold under the new rules; no test changes required.

## 3. Color philosophy — revised

### 3.1 Swiss Red (`--primary`) — same jobs, plus two new ones

Existing jobs unchanged: primary action / CTA, destructive action, focus
ring. **New jobs added:**

- **Section-divider rules.** The hairline beside a section-header label
  (`<span class="h-px flex-1 bg-border" />` in the existing pattern) may
  use `bg-primary` instead of `bg-border` to give a section break a color
  anchor. Not mandatory on every divider — used where a section deserves
  emphasis (e.g. the opening section of a page, a "new"/"featured"
  callout), not uniformly on all of them.
- **Selective card/component borders.** A card that is the emphasized one
  in a set (featured, current, first-in-list, the one the page wants you
  to look at first) may use `border-primary` instead of `border-foreground`.
  This is an emphasis device applied to *one* card in a group, never the
  default border for a whole card grid.

Red is still rationed relative to ink — it marks the thing that matters
most on a given surface, not a decorative wash.

### 3.2 Pastel Turquoise (`--highlight`) — still non-semantic, less rationed

Unchanged: **never** success/info/a second interactive state; never on a
button, tag, status pip, or link. The old cap ("exactly one brand moment
per page, plus one chart series") is replaced with **decorative reuse**:
turquoise may recur multiple times per page as pure ornament —

- an icon fill (a single icon accented `text-highlight` instead of
  `currentColor`, as a decorative flourish, not a state indicator)
- an underline or rule beneath a heading or label
- a background wash (`bg-highlight/10`) behind a block that wants visual
  separation without a hard border
- a dot accent (matching the existing dot-indicator pattern, just in
  turquoise instead of foreground/primary)
- its existing jobs: a chart's second data series, a hero/brand moment

The distinguishing test stays the same as before: if the color's presence
or absence would change what the user understands about *state*, it's
wrong. If it's purely decorative and could be removed without changing
meaning, it's fine.

### 3.3 Restraint guardrails (replace the old hard caps)

Qualitative, matching the system's existing prose style rather than
mechanically-enforced numbers:

- Ink/cream must still visually dominate any given surface — accents are
  seasoning, not the base palette.
- Red and Turquoise never touch or sit directly adjacent on the same
  element (e.g. a turquoise-washed card never also gets a red border).
- One accent per component at a time, not both — a card gets *either* a
  red border *or* a turquoise wash, never both.

These are guidance for whoever (human or Claude) applies the skill, not
something `verify-philosophy.mjs` can practically check — no new
automated check is added for them.

## 4. Typography — three registers (was four)

| Tier | Font | Role |
|------|------|------|
| Primary | **Geist Mono** | Headings, display, data values, tags, nav, labels, **and now the hero title/wordmark + chapter/section dividers in long-form content** (absorbing Jost's two jobs) |
| Primary | **Geist Sans** | Body copy, prose, dense-data/utility text |
| Secondary | **Space Mono, italic only** | Inline annotations and figure captions only |
| Tertiary | **Zilla Slab** | Long-form editorial body and pull-quotes — never UI |

Jost is removed entirely: dropped from the Google Fonts `<link>`, from
`theme.css` (`--hero`/`--font-hero` custom properties removed), from the
typography table and its "4th register" paragraph in `SKILL.md`, from the
"Hero / display type (Jost)" pattern in `components.md` (rewritten to show
the same hero pattern using large/bold Geist Mono instead), and from the
font-loading step in `README.md`. Font count drops from 5 files to 4;
register count from 4 to 3.

The hero title becomes: `font-mono font-bold` at the existing hero size
scale, letter-spacing tightened as headings already are — no new sizing
rules needed since Geist Mono's heading scale already covers this range.

## 5. Files touched

- `skills/tri-swiss/SKILL.md` — typography table, Philosophy section,
  `--highlight` section, "Do not" list, Google Fonts `<link>`.
- `skills/tri-swiss/assets/theme.css` — remove `--hero`/`--font-hero` and
  their comment. No new tokens added.
- `skills/tri-swiss/references/components.md` — rewrite the hero pattern
  to Geist Mono; add new patterns for red section dividers, red-bordered
  emphasis cards, and turquoise decorative accents (icon fill, underline,
  wash, dot).
- `docs/index.html` — drop Jost import and `.hero-title` font-family
  override (falls through to Geist Mono); "type registers" showcase
  section goes from 4 samples to 3; restyled throughout to demonstrate the
  new red/turquoise patterns.
- `README.md` — "Five type registers" → "Four type registers"; drop Jost
  from the install step's font-loading instruction.
- `CHANGELOG.md` — edit the existing `[Unreleased]` bullets in place
  (three registers instead of four; expanded color-philosophy wording)
  rather than adding `Removed`/`Changed` entries for a feature that was
  never in a tagged release. No tag exists yet (`git tag -l` is empty), so
  the "initial release" description simply becomes accurate.
- `docs/assets/*.png` — regenerated via the existing Playwright capture
  script (`scripts/capture/`); visuals change materially (hero, type
  registers, components, charts, social card).
- `scripts/capture/verify-philosophy.mjs` — **no changes.** Palette set is
  unchanged (no new hex values introduced); the highlight-never-in-a-
  status-pip check is still exactly what §3.2 requires.

## 6. Rollout

Same workflow as the original scaffold: branch `feat/expand-accents-drop-jost`
off `main` (the initial scaffold's branch, `feat/initial-scaffold`, is
already merged and is not reused), Conventional Commits throughout,
philosophy verifier + Playwright capture re-run before PR, PR opened
against `main` for review — no direct pushes.

## 7. Open items for implementation (explicitly deferred, not blocking)

- Exact Tailwind opacity values for turquoise washes (`/10` vs `/15`, etc.)
  are a first pass, adjustable visually against the demo page during
  implementation without reopening this spec.
- Which specific cards/dividers in the existing `docs/index.html` layout
  get the new red/turquoise treatment is an implementation-time visual
  judgment call, guided by §3.3's guardrails, not enumerated here.
