# Structural Block Pattern (Tri-Swiss + Duotone Swiss) — Design Spec

Status: approved (pending final user sign-off on this document)
Date: 2026-07-06
Repos: `luxsolari/tri-swiss` (branch `feat/expand-accents-drop-jost`, extends the
in-flight color-expansion work) and `luxsolari/lux-design-system` (new branch
`feat/structural-block-and-weight-highlight`, off `main` @ `v1.0.0`)
Supersedes/extends: [2026-07-06-colorful-accents-drop-jost-design.md](2026-07-06-colorful-accents-drop-jost-design.md)
(adds a new job for Swiss Red on top of that spec's dividers/card-border jobs,
plus a governed exception to that spec's "Red and Turquoise never touch"
guardrail — see §4)

## 1. Purpose

Give the primary accent in both house design systems a genuine structural
layout role — a solid color-block panel, inspired by the user's own site
(a full-height red sidebar holding nav/branding, plus a bold red word inside
a heading) — and restructure each system's showcase page to actually use it.
Additionally, give Duotone Swiss the functional equivalent of what Pastel
Turquoise does in Tri-Swiss (a governed, non-semantic "one special moment"
device), expressed through typographic weight instead of a new color, so
Duotone Swiss's two-color identity stays literally true. Finally, replace
Tri-Swiss's static solid-turquoise hero underline with a segmented
ink/red/turquoise stripe, and give Turquoise a new decorative hover-triggered
role that reaches nav links and labels for the first time.

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
- Not a general license to combine Red and Turquoise on arbitrary elements.
  The one exception carved out in §4 (the segment stripe) is a single named
  device, not a relaxation of the "never touch" guardrail in general.
- Not a change to what turquoise means. It still carries zero state
  semantics anywhere, including in its new hover-triggered use in §4 — the
  ornament is identical regardless of any actual interactive state.

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

## 4. Tri-part segment stripe and turquoise hover-flourish (Tri-Swiss + Duotone Swiss)

Two related refinements to how Turquoise (Tri-Swiss) shows up, prompted by
looking at the shipped Task 5 hero underline in context:

### 4.1 Tri-part segment stripe (replaces the solid hero underline)

The static decorative bar beneath the hero title — currently one solid
`--highlight` (turquoise) bar from Task 5 — becomes a **segmented stripe**:
three equal-width solid blocks in a row, one each of ink, Swiss Red, and
Pastel Turquoise, in that order. Duotone Swiss gets the two-color version
(ink, Blood Red) in the equivalent spot on its own showcase page.

This is the **one explicitly named exception** to the "Red and Turquoise
never touch or sit adjacent on the same element" guardrail from the prior
spec — the segment stripe is a single governed device, not a general
loosening of that guardrail. Nowhere else may Red and Turquoise sit
adjacent; this pattern is named and scoped exactly to the segmented-stripe
use, documented in `components.md` as its own named pattern so it isn't
mistaken for general license.

### 4.2 Turquoise decorative hover-flourish (Tri-Swiss only — no Duotone equivalent, it has no Turquoise)

Turquoise's decorative toolkit (icon fill, underline, wash, dot, established
in the prior spec) gains a hover-triggered variant, and — new in this
spec — it now reaches **nav links and labels**, which the prior spec's "no
Turquoise on links" rule excluded entirely. On hover, a nav item, the
sidebar's anchor-nav entries, or a similar text label shows a decorative
turquoise underline or dot — purely ornamental, identical regardless of
active/current/visited state — layered *alongside* the element's existing
ink/muted-foreground hover color change, which continues to carry the real
interactive feedback unchanged.

**Rule wording update:** "No Turquoise on buttons, tags, status pips, or
links" (prior spec, §3.2/Do-not list) becomes "No Turquoise as a link's (or
button's/tag's/status pip's) own state-indicating color" — the ornament in
§4.2 is additive decoration layered on top of an element whose real
state-feedback is still 100% ink/red/weight, never a replacement for or
instance of that feedback. Buttons, tags, and status pips otherwise keep
today's rule untouched — this hover-flourish is scoped to nav links/labels
only, not extended to buttons/tags/status pips in this spec.

## 5. Tri-Swiss: files and page restructure

- `skills/tri-swiss/SKILL.md` — add the Structural Block job to Swiss Red's
  description in the Philosophy section; add the guardrail carve-out; add
  the segment-stripe exception and the updated Turquoise-on-links wording
  from §4.
- `skills/tri-swiss/references/components.md` — new patterns: sidebar/nav
  rail markup, hero band markup, bold-word inline pattern, segment-stripe
  pattern, turquoise hover-flourish pattern.
- `docs/index.html` — full layout restructure: replace the current
  centered-column-with-top-nav structure with a persistent sidebar
  (wordmark, anchor nav to Palette/Typography/Components/Charts, theme
  toggle, GitHub link, copyright), collapsing below the mobile breakpoint
  to a red top band with a hamburger toggle (small vanilla-JS toggle,
  consistent with the existing theme-toggle script already on the page —
  no new framework or build step). Content area (everything currently in
  `<main>`) shifts to fill the remaining ~75%+ width. Hero underline becomes
  the ink/red/turquoise segment stripe (§4.1). Sidebar anchor-nav entries
  get the turquoise hover-flourish (§4.2).
- `docs/assets/*.png` — full re-capture (every screenshot's framing
  changes with the new layout, not just the ones with new patterns).
- `CHANGELOG.md` — edit the still-`[Unreleased]` entry in place (no tag
  cut yet), same approach as the prior spec.

## 6. Duotone Swiss: files, page restructure, and the weight-based highlight

- `skills/lux-design-system/SKILL.md`:
  - Add the identical Structural Block job to Blood Red's description
    (sidebar/nav rail, hero band, bold-word accent — same ~25% cap, same
    combinability rule).
  - Add the new **typographic brand-moment device**: exactly one element
    per page (the hero wordmark) rendered larger and bolder than anything
    else on the page, reserved for that one moment only. This is Duotone
    Swiss's functional equivalent of Tri-Swiss's "one brand moment" job for
    Turquoise, expressed through typography instead of color. Chart-series
    differentiation needs no new device — the existing dash/opacity/
    outline-ring rules already cover that job.
  - **Resolved during planning:** neither Space Grotesk nor Space Mono
    actually ships a weight past 700 (verified directly against Google
    Fonts — a `wght@300..900` request for Space Grotesk returns HTTP 400,
    and Space Mono only serves 400/700). Since 700 is already every
    heading's weight, a pure weight-only device would be indistinguishable
    from a normal heading. Per the fallback clause below, this device
    combines the heaviest real weight (700, same as headings) with a
    deliberate size jump — the brand-moment element is dramatically larger
    than the type scale otherwise allows, still zero new color. No Google
    Fonts `<link>` change is needed (900 was never actually servable).
  - Add the two-color (ink/red) segment-stripe pattern from §4.1 as the
    hero underline device — Duotone Swiss has no Turquoise, so it has no
    equivalent of the §4.2 hover-flourish; this section does not apply to
    it beyond the two-color stripe.
- `skills/lux-design-system/references/components.md` — same three
  Structural Block patterns as Tri-Swiss's components.md (Red → Blood
  Red), the weight-900 brand-moment pattern, and the two-color segment
  stripe.
- `docs/index.html` — same full restructure to a persistent sidebar nav as
  Tri-Swiss's page, using Blood Red instead of Swiss Red, plus the
  weight-900 hero wordmark treatment and the two-color segment stripe in
  place of its own current hero underline (if any — confirm current state
  during implementation).
- `docs/assets/*.png` — full re-capture.
- `CHANGELOG.md` — **new** `### Added` entries under a fresh
  `[Unreleased]` section (this repo is already tagged `v1.0.0` — unlike
  Tri-Swiss, there is no still-pending release to edit in place).

## 7. Rollout

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

## 8. Open items for implementation (explicitly deferred, not blocking)

- Exact anchor-nav section labels in each sidebar (Tri-Swiss: Palette,
  Typography, Components, Charts; Duotone Swiss: its own equivalent
  section set) — confirm against each page's actual current section IDs
  during implementation, not enumerated here.
- The mobile hamburger toggle's exact open/close interaction (icon swap,
  animation) is an implementation-time visual judgment call, guided only
  by: vanilla JS, no new dependency, reuses the existing theme-toggle
  script's style.
- **Resolved:** neither font ships past weight 700 — see §6's brand-moment
  device description for the weight+size resolution. No longer open.
- Duotone Swiss's current showcase page has **no existing hero underline
  at all** (confirmed by reading `docs/index.html` directly) — the
  two-color segment stripe from §4.1 is a pure addition there, not a
  replacement.
