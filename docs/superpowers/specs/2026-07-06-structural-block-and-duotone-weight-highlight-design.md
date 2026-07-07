# Structural Block Pattern (Tri-Swiss + Duotone Swiss) — Design Spec

Status: approved (pending final user sign-off on this document)
Date: 2026-07-06
Repos: `luxsolari/tri-swiss` (branch `feat/expand-accents-drop-jost`, extends the
in-flight color-expansion work) and `luxsolari/lux-design-system` (new branch
`feat/structural-block-and-weight-highlight`, off `main` @ `v1.0.0`)
Supersedes/extends: [2026-07-06-colorful-accents-drop-jost-design.md](2026-07-06-colorful-accents-drop-jost-design.md)
(adds a new job for Swiss Red on top of that spec's dividers/card-border jobs)

## 1. Purpose

Give the primary accent in both house design systems a genuine structural
layout role — a solid color-block panel, inspired by the user's own site
(a full-height red sidebar holding nav/branding, plus a bold red word inside
a heading) — and restructure each system's showcase page to actually use it.
Additionally, give Duotone Swiss the functional equivalent of what Pastel
Turquoise does in Tri-Swiss (a governed, non-semantic "one special moment"
device), expressed through typographic weight instead of a new color, so
Duotone Swiss's two-color identity stays literally true.

## 2. Non-goals

- Not a new color token in Duotone Swiss. Ink, cream, and blood red remain
  the only three CSS custom properties for color; the new "brand moment"
  device is 100% typographic.
- Not a loosening of Duotone Swiss's "no second accent" rule for anything
  other than the one new Structural Block job defined here — tags, buttons,
  status pips, links are all unaffected and keep today's rules.
- Not a redesign of either system's component semantics (buttons, tags,
  status pips, cards) beyond what's needed to host the new sidebar nav.
- Not a rewrite of chart-series differentiation in either system — Tri-Swiss
  keeps `--highlight` for a genuine second series; Duotone Swiss keeps its
  existing dash/opacity/outline-ring approach (already weight/pattern-based,
  not color-based, and already sufficient — no changes needed there).

## 3. The Structural Block pattern (shared definition)

A new, third job for each system's primary accent (Swiss Red / Blood Red),
on top of the jobs each system already has (primary action, destructive,
ring, focus; plus, in Tri-Swiss, section dividers and selective card
borders from the prior spec). Three forms:

1. **Sidebar / nav rail** — a solid accent-colored panel running the full
   viewport height, capped at **~25% of viewport width**. Holds: wordmark,
   an in-page anchor nav (linking to each showcase section), a theme
   toggle, an external/GitHub link, and a copyright footer. Sticky/fixed —
   does not scroll away.
2. **Hero band** — a solid accent-colored horizontal block, used once, for
   a hero/intro moment. **Alternative to the sidebar, not combined with
   it** — a given layout picks one structural block, never both.
3. **Bold word/phrase accent** — one word or short phrase inside a heading
   rendered in the accent color at normal weight/size (no new device, just
   the existing primary color applied to inline text). **Independent** of
   the two block forms above and may combine with either — this mirrors
   the reference image, which uses a sidebar block AND a bold red word in
   the same layout.

### Guardrail update

Both systems' existing "ink/cream still dominate any surface" guardrail
(Tri-Swiss) / "duotone strict, no second accent" framing (Duotone Swiss)
gets one explicit carve-out: **at most one Structural Block (sidebar or
hero band) per layout, capped at ~25% of viewport width/height** — outside
that one block, ink/cream continue to dominate every other surface exactly
as before. The bold-word accent is inline text and does not count against
this cap.

**Tri-Swiss-specific interaction:** the Structural Block is a Red-only job.
It does not host Turquoise — a sidebar/hero-band block stays red/ink/cream
only, consistent with the existing "one accent per component, not both"
guardrail (the block itself is "one component").

## 4. Tri-Swiss: files and page restructure

- `skills/tri-swiss/SKILL.md` — add the Structural Block job to Swiss Red's
  description in the Philosophy section; add the guardrail carve-out.
- `skills/tri-swiss/references/components.md` — new patterns: sidebar/nav
  rail markup, hero band markup, bold-word inline pattern.
- `docs/index.html` — full layout restructure: replace the current
  centered-column-with-top-nav structure with a persistent sidebar
  (wordmark, anchor nav to Palette/Typography/Components/Charts, theme
  toggle, GitHub link, copyright), collapsing below the mobile breakpoint
  to a red top band with a hamburger toggle (small vanilla-JS toggle,
  consistent with the existing theme-toggle script already on the page —
  no new framework or build step). Content area (everything currently in
  `<main>`) shifts to fill the remaining ~75%+ width.
- `docs/assets/*.png` — full re-capture (every screenshot's framing
  changes with the new layout, not just the ones with new patterns).
- `CHANGELOG.md` — edit the still-`[Unreleased]` entry in place (no tag
  cut yet), same approach as the prior spec.

## 5. Duotone Swiss: files, page restructure, and the weight-based highlight

- `skills/lux-design-system/SKILL.md`:
  - Add the identical Structural Block job to Blood Red's description
    (sidebar/nav rail, hero band, bold-word accent — same ~25% cap, same
    combinability rule).
  - Add the new **weight-based brand-moment device**: exactly one element
    per page (the hero wordmark) rendered at an extreme weight — **900/
    Black** — a cut heavier than anything else on the page, reserved for
    that one moment only. This is Duotone Swiss's functional equivalent of
    Tri-Swiss's "one brand moment" job for Turquoise, expressed through
    weight instead of color. Chart-series differentiation needs no new
    device — the existing dash/opacity/outline-ring rules already cover
    that job.
  - Update the Google Fonts `<link>` to add weight `900` for whichever of
    Space Mono/Space Grotesk hosts the brand-moment element (confirm which
    family actually ships a 900 cut before implementation — if neither
    does, fall back to the heaviest weight either family actually
    supports, and note the substitution in the commit).
- `skills/lux-design-system/references/components.md` — same three
  Structural Block patterns as Tri-Swiss's components.md (Red → Blood
  Red), plus the weight-900 brand-moment pattern.
- `docs/index.html` — same full restructure to a persistent sidebar nav as
  Tri-Swiss's page, using Blood Red instead of Swiss Red, plus the
  weight-900 hero wordmark treatment.
- `docs/assets/*.png` — full re-capture.
- `CHANGELOG.md` — **new** `### Added` entries under a fresh
  `[Unreleased]` section (this repo is already tagged `v1.0.0` — unlike
  Tri-Swiss, there is no still-pending release to edit in place).

## 6. Rollout

Two separate branches, two separate PRs, executed one after the other
(not in parallel):

1. **Tri-Swiss** continues on `feat/expand-accents-drop-jost` — this spec's
   Tri-Swiss work becomes new tasks appended after the already-completed
   Tasks 1-8 (screenshot regeneration), before the existing Task 9
   (push + PR).
2. **Duotone Swiss** — branch `feat/structural-block-and-weight-highlight`
   in `luxsolari/lux-design-system`, following that repo's own AGENTS.md
   conventions (Conventional Commits, changelog-first, branch+PR only —
   confirmed present via its own `AGENTS.md`/`CONTRIBUTING.md`).

## 7. Open items for implementation (explicitly deferred, not blocking)

- Exact anchor-nav section labels in each sidebar (Tri-Swiss: Palette,
  Typography, Components, Charts; Duotone Swiss: its own equivalent
  section set) — confirm against each page's actual current section IDs
  during implementation, not enumerated here.
- The mobile hamburger toggle's exact open/close interaction (icon swap,
  animation) is an implementation-time visual judgment call, guided only
  by: vanilla JS, no new dependency, reuses the existing theme-toggle
  script's style.
- Whether Space Mono or Space Grotesk (or both) actually ship a static/
  variable 900 cut needs confirming against the real Google Fonts family
  metadata before the SKILL.md/theme.css edit lands — if unavailable,
  substitute the heaviest weight either family supports and note it.
