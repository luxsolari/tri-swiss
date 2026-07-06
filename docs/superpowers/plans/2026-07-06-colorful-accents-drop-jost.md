# Colorful Accents & Jost Removal Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Loosen the Tri-Swiss design system's accent-color rationing (Swiss Red gains section-divider and selective-card-border jobs; Pastel Turquoise stays non-semantic but sheds its one-touch-per-page cap) and remove the Jost 4th type register, folding its two jobs into Geist Mono — across the skill itself, its showcase page, and all supporting docs.

**Architecture:** This is a documentation/CSS/HTML system, not application code — there is no compiler or unit-test suite for `SKILL.md`/`components.md`/`README.md`/`CONTRIBUTING.md`/`CHANGELOG.md`. Each task's "test cycle" is either (a) `rtk grep` for residual references that must no longer exist, or (b) for `docs/index.html`, the repo's one real automated check, `scripts/capture/verify-philosophy.mjs`, plus a Playwright screenshot regeneration as the final visual check.

**Tech Stack:** Plain HTML/CSS (Tailwind 4 `@theme inline` tokens), Node.js scripts (`scripts/capture/capture.mjs`, `verify-philosophy.mjs`), Playwright (already installed in `scripts/capture/node_modules`).

## Global Constraints

- No new CSS custom properties for color — reuse `--primary`/`--highlight` with Tailwind opacity modifiers (e.g. `bg-highlight/10`), matching the existing `/50`, `/30` pattern already in the buttons/tags spec.
- Pastel Turquoise (`--highlight`) never appears on a button, tag, status pip, or link — this is unchanged and is what `verify-philosophy.mjs`'s status-pip check enforces.
- Guardrails to reflect in prose everywhere accent usage is described: ink/cream still visually dominate any surface; Red and Turquoise never touch/sit adjacent on the same element; one accent per component, not both.
- No tag has been cut yet (`git tag -l` is empty) — `CHANGELOG.md`'s `[Unreleased]` bullets describing the initial release get edited in place, not layered with new `Removed`/`Changed` entries.
- Conventional Commits on every commit subject; branch is `feat/expand-accents-drop-jost` (the original scaffold branch, `feat/initial-scaffold`, is already merged — do not reuse it); PR against `main`, no direct pushes.
- `scripts/capture/verify-philosophy.mjs` itself is **not modified** — its palette-purity check and highlight-never-in-a-status-pip check both still hold under the new rules.

---

### Task 1: Remove Jost from `theme.css`

**Files:**
- Modify: `skills/tri-swiss/assets/theme.css:27-34`, `:60`

**Interfaces:** N/A — CSS custom properties only; no other task depends on `--hero`/`--font-hero` existing.

- [ ] **Step 1: Remove the `--hero` token and update the font-roles comment**

In `skills/tri-swiss/assets/theme.css`, replace:

```css
  /* Font roles. Primary = Geist (display + body/utility). Secondary = Space
     Mono, italic only, annotations/captions. Tertiary = Zilla Slab, long-form.
     4th register = Jost, hero/wordmark + section dividers only. */
  --mono: "Geist Mono", ui-monospace, monospace;
  --sans: "Geist", ui-sans-serif, system-ui, sans-serif;
  --annotation: "Space Mono", ui-monospace, monospace;
  --serif: "Zilla Slab", Georgia, serif;
  --hero: "Jost", ui-sans-serif, system-ui, sans-serif;
```

with:

```css
  /* Font roles. Primary = Geist (display + body/utility, and now also
     hero/wordmark + section dividers). Secondary = Space Mono, italic
     only, annotations/captions. Tertiary = Zilla Slab, long-form. */
  --mono: "Geist Mono", ui-monospace, monospace;
  --sans: "Geist", ui-sans-serif, system-ui, sans-serif;
  --annotation: "Space Mono", ui-monospace, monospace;
  --serif: "Zilla Slab", Georgia, serif;
```

- [ ] **Step 2: Remove the `--font-hero` mapping in `@theme inline`**

Replace:

```css
  --font-sans: var(--sans);
  --font-mono: var(--mono);
  --font-annotation: var(--annotation);
  --font-serif: var(--serif);
  --font-hero: var(--hero);
```

with:

```css
  --font-sans: var(--sans);
  --font-mono: var(--mono);
  --font-annotation: var(--annotation);
  --font-serif: var(--serif);
```

- [ ] **Step 3: Verify no Jost/hero references remain**

Run: `rtk grep -i "jost\|font-hero\|--hero" skills/tri-swiss/assets/theme.css`
Expected: no output (exit code 1 — no matches).

- [ ] **Step 4: Commit**

```bash
rtk git add skills/tri-swiss/assets/theme.css
rtk git commit -m "feat(theme): drop the Jost hero font token"
```

---

### Task 2: Remove Jost from `SKILL.md` typography section

**Files:**
- Modify: `skills/tri-swiss/SKILL.md:127-142`, `:176-180`

**Interfaces:** N/A.

- [ ] **Step 1: Drop the 4th-register row from the typography table**

Replace:

```markdown
| Tier | Font | Role |
|------|------|------|
| Primary | **Geist Mono** (`font-mono`) | Headings, display, data values, tags, nav, labels |
| Primary | **Geist Sans** (`font-sans`) | Body copy, prose, **and** dense-data/utility text (tables, fine print) at smaller size with tabular figures |
| Secondary | **Space Mono, italic only** (`font-annotation italic`) | Inline annotations and figure captions only — never emphasis |
| Tertiary | **Zilla Slab** (`font-serif`) | Long-form editorial body and pull-quotes — never UI |
| 4th register | **Jost** (`font-hero`) | Hero title/wordmark and section/chapter dividers in long-form editorial content only — never a UI heading, never a pull-quote |
```

with:

```markdown
| Tier | Font | Role |
|------|------|------|
| Primary | **Geist Mono** (`font-mono`) | Headings, display, data values, tags, nav, labels, hero title/wordmark, and section/chapter dividers in long-form editorial content |
| Primary | **Geist Sans** (`font-sans`) | Body copy, prose, **and** dense-data/utility text (tables, fine print) at smaller size with tabular figures |
| Secondary | **Space Mono, italic only** (`font-annotation italic`) | Inline annotations and figure captions only — never emphasis |
| Tertiary | **Zilla Slab** (`font-serif`) | Long-form editorial body and pull-quotes — never UI |
```

- [ ] **Step 2: Drop Jost from the Google Fonts link**

Replace:

```html
<link href="https://fonts.googleapis.com/css2?family=Geist:wght@100..900&family=Geist+Mono:wght@100..900&family=Space+Mono:ital,wght@0,400;1,400&family=Zilla+Slab:ital,wght@0,400;0,500;0,700;1,400&family=Jost:wght@400;700&display=swap" rel="stylesheet" />
```

with:

```html
<link href="https://fonts.googleapis.com/css2?family=Geist:wght@100..900&family=Geist+Mono:wght@100..900&family=Space+Mono:ital,wght@0,400;1,400&family=Zilla+Slab:ital,wght@0,400;0,500;0,700;1,400&display=swap" rel="stylesheet" />
```

- [ ] **Step 3: Update the font-loading paragraph**

Replace:

```markdown
Geist Sans and Geist Mono load as **variable fonts** (100–900 axis); Space
Mono loads regular + italic; Zilla Slab loads its usual four cuts; Jost
loads two static weights (400/700) — a hero/wordmark register doesn't need
a full variable scale.
```

with:

```markdown
Geist Sans and Geist Mono load as **variable fonts** (100–900 axis); Space
Mono loads regular + italic; Zilla Slab loads its usual four cuts.
```

- [ ] **Step 4: Delete the Jost paragraph**

Delete this whole paragraph entirely (it directly follows the Space Mono italic paragraph, near the end of the Typography section):

```markdown
**Jost** is reserved for exactly two jobs: a page or section's hero
title/wordmark, and chapter/section dividers inside long-form editorial
content (e.g. a large "02" or chapter title between article sections). It
never appears as a UI heading (that's Geist Mono's job) and never as a
pull-quote (that's Zilla Slab's job).
```

- [ ] **Step 5: Verify no Jost references remain in the typography section**

Run: `rtk grep -n -i "jost\|font-hero" skills/tri-swiss/SKILL.md`
Expected: no matches yet from Typography — matches may still appear from the "Do not" list and frontmatter, which Task 3 removes. If Task 2's edits are correct, none of the remaining hits should be inside the Typography table/paragraphs you just edited.

- [ ] **Step 6: Commit**

```bash
rtk git add skills/tri-swiss/SKILL.md
rtk git commit -m "feat(skill): drop Jost from Tri-Swiss typography"
```

---

### Task 3: Rewrite color philosophy in `SKILL.md`

**Files:**
- Modify: `skills/tri-swiss/SKILL.md:1-19` (frontmatter description), `:48-56` (Philosophy), `:107-121` (`--highlight` section), `:188-195` (section-header example), `:267-283` (Do not list)

**Interfaces:** N/A.

- [ ] **Step 1: Soften the frontmatter's color description**

In the YAML frontmatter `description`, replace:

```yaml
  Tri-Swiss — Lux Solari's Geist-based house design system, sibling to
  lux-design-system (Duotone Swiss). A tri-tone visual language (ink + cream,
  one blood-red-adjacent accent, one rare non-semantic highlight)
```

with:

```yaml
  Tri-Swiss — Lux Solari's Geist-based house design system, sibling to
  lux-design-system (Duotone Swiss). A tri-tone visual language (ink + cream,
  a Swiss Red accent, and a governed non-semantic highlight)
```

- [ ] **Step 2: Rewrite the "Tri-tone strict" philosophy paragraph**

Replace:

```markdown
**Tri-tone strict.** Ink and cream are the two structural colors; Swiss Red
is the primary accent. Pastel Turquoise is a **third, rare, non-semantic**
color — it never carries meaning (no success/info/second-interactive-state
use) and appears in exactly two places: a second data series in a chart, and
one single brand/hero moment per page. Everywhere else — buttons, tags,
status pips, links — it does not exist. If you feel the urge to add a color
beyond these, add a `font-bold`, a size step, or whitespace instead.
```

with:

```markdown
**Tri-tone, more colorful.** Ink and cream are still the two structural
colors. Swiss Red is the primary accent — primary action, destructive,
focus ring — and now also marks section-divider rules and a selectively
emphasized card/component border (one card in a set, never the whole
grid). Pastel Turquoise stays a **third, non-semantic** color — it never
carries meaning (no success/info/second-interactive-state use) — but is
no longer rationed to one touch per page: it recurs as pure decoration
(icon fills, underlines, background washes, dot accents, a chart's
second series, a brand/hero moment) anywhere its presence or absence
wouldn't change what the user understands about state. It still never
appears on a button, tag, status pip, or link. Three guardrails keep this
from tipping into loud: ink/cream still visually dominate any surface;
Red and Turquoise never touch or sit adjacent on the same element; one
accent per component, not both.
```

- [ ] **Step 3: Rewrite the `--highlight` token section**

Replace:

```markdown
### The `--highlight` token — read this before using it

Pastel Turquoise is not a second accent. It carries **zero semantic
meaning** — never success, never info, never a second interactive state.
It is sanctioned for exactly two jobs:

1. A second data series/stroke in a hand-rolled SVG or Observable Plot chart.
2. One single brand moment per surface (e.g. a hero accent or a logo mark) —
   never repeated elsewhere on the same page.

**Do not** use it in buttons, tags, status pips, links, or any other UI
state. If a new element wants a second color for meaning, the answer is
still "no — use weight, size, or whitespace," exactly as in the two-color
core.
```

with:

```markdown
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

It can recur multiple times on the same page — the old "exactly one
brand moment" cap is gone — but the test that governs every use is
unchanged: if turquoise's presence or absence would change what the user
understands about *state*, it's wrong. If it's purely ornamental and
removable without changing meaning, it's fine.

**Do not** use it in buttons, tags, status pips, links, or any other UI
state indicator. If a new element wants a second color for *meaning*, the
answer is still "no — use weight, size, or whitespace."
```

- [ ] **Step 4: Add the red-emphasis-divider option to the section-header pattern**

Replace:

```markdown
- **Section header:** uppercase mono label with a full-width rule beside it.

```jsx
<div className="mb-4 flex items-baseline gap-3">
  <h5>Section title</h5>
  <span className="h-px flex-1 bg-border" />
</div>
```
```

with:

```markdown
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
```

- [ ] **Step 5: Replace the old absolute bans in the "Do not" list**

Replace:

```markdown
- **No Turquoise outside charts/brand moments.** It never appears in
  buttons, tags, or status indicators.
- **No Jost outside hero/wordmark and chapter dividers.** It is not a body
  face, not a UI heading face, not a pull-quote face.
```

with:

```markdown
- **No Turquoise on buttons, tags, status pips, or links.** Decorative
  reuse elsewhere is fine; semantic/interactive roles are not.
- **No red-and-turquoise on the same element.** Pick one accent per
  component, never both.
- **Ink/cream still dominate.** Accents are seasoning; a surface where
  Red or Turquoise out-covers ink/cream has gone too far.
```

- [ ] **Step 6: Verify no Jost references remain anywhere in `SKILL.md`**

Run: `rtk grep -i "jost\|font-hero\|--hero" skills/tri-swiss/SKILL.md`
Expected: no output (exit code 1 — no matches). This confirms Task 2 + this task removed every reference.

- [ ] **Step 7: Commit**

```bash
rtk git add skills/tri-swiss/SKILL.md
rtk git commit -m "feat(skill): loosen Tri-Swiss accent-color rationing"
```

---

### Task 4: Rewrite `references/components.md`

**Files:**
- Modify: `skills/tri-swiss/references/components.md:142-156` (hero pattern), `:169-178` (Cards)
- Add: new "Accent expansion" section (insert after the Observable Plot subsection, before "## Iconography", i.e. after line 112 / before line 114 in the current file)

**Interfaces:** N/A.

- [ ] **Step 1: Rewrite the hero pattern from Jost to Geist Mono**

Replace:

```markdown
## Hero / display type (Jost)

Reserved for exactly two jobs: a page's hero title/wordmark, and section/chapter
dividers inside long-form editorial content. Never a UI heading, never a
pull-quote, never running body text.

```jsx
<h1 className="font-hero" style={{ fontSize: "3.5rem", fontWeight: 700, letterSpacing: "-0.01em" }}>
  Page Title.
</h1>

<div className="font-hero" style={{ fontSize: "1.5rem", fontWeight: 700 }}>
  02 — Chapter Title
</div>
```
```

with:

```markdown
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
```

- [ ] **Step 2: Insert the new "Accent expansion" section**

Insert this new section immediately after the Observable Plot code block ends (after the line `document.querySelector("#mount").append(chart);` and its closing code fence) and before the `## Iconography (geist-icons, restyled)` heading:

```markdown
## Accent expansion — Red dividers, emphasis borders, Turquoise decoration

Beyond their original jobs (Red: primary action/destructive/ring. Turquoise:
chart second series/brand moment), both accents now recur more often across
a page — Red picks up two new structural jobs; Turquoise gets decorative
reuse. Guardrails: ink/cream still dominate any surface, Red and Turquoise
never touch on the same element, and each component gets at most one accent.

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

**Turquoise decorative accents.** Non-semantic, ornamental only — never on
a button, tag, status pip, or link:

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
```

- [ ] **Step 3: Cross-link the Cards section to the new emphasis-border pattern**

Replace:

```markdown
## Cards

Elevation is a background step, never a shadow.

```
border border-border bg-card text-card-foreground p-6
```

Nest a section divider (see `SKILL.md`) inside for titled regions. Keep corners
square unless a `rounded-md` (0.5rem) genuinely helps.
```

with:

```markdown
## Cards

Elevation is a background step, never a shadow.

```
border border-border bg-card text-card-foreground p-6
```

Nest a section divider (see `SKILL.md`) inside for titled regions. Keep corners
square unless a `rounded-md` (0.5rem) genuinely helps. For the one emphasized
card in a set, see the selective-border pattern under "Accent expansion"
above — swap `border-border` for `border-primary`.
```

- [ ] **Step 4: Verify no Jost references remain**

Run: `rtk grep -i "jost\|font-hero" skills/tri-swiss/references/components.md`
Expected: no output (exit code 1 — no matches).

- [ ] **Step 5: Commit**

```bash
rtk git add skills/tri-swiss/references/components.md
rtk git commit -m "feat(components): add accent-expansion patterns, drop Jost hero"
```

---

### Task 5: Update `docs/index.html` — remove Jost, demonstrate the new accent patterns

**Files:**
- Modify: `docs/index.html` (multiple locations, listed below)

**Interfaces:** N/A. This task's test is the repo's real automated check, `scripts/capture/verify-philosophy.mjs`.

- [ ] **Step 1: Drop Jost from the Google Fonts link (line 36)**

Replace:

```html
  <link href="https://fonts.googleapis.com/css2?family=Geist:wght@100..900&family=Geist+Mono:wght@100..900&family=Space+Mono:ital,wght@0,400;1,400&family=Zilla+Slab:ital,wght@0,400;0,500;0,700;1,400&family=Jost:wght@400;700&display=swap" rel="stylesheet" />
```

with:

```html
  <link href="https://fonts.googleapis.com/css2?family=Geist:wght@100..900&family=Geist+Mono:wght@100..900&family=Space+Mono:ital,wght@0,400;1,400&family=Zilla+Slab:ital,wght@0,400;0,500;0,700;1,400&display=swap" rel="stylesheet" />
```

- [ ] **Step 2: Remove the `--font-hero` custom property (lines 44-47)**

Replace:

```css
      /* Role fonts */
      --font-mono:'Geist Mono',ui-monospace,monospace; --font-sans:'Geist',system-ui,sans-serif;
      --font-annotation:'Space Mono',ui-monospace,monospace; --font-serif:'Zilla Slab',Georgia,serif;
      --font-hero:'Jost',system-ui,sans-serif;
```

with:

```css
      /* Role fonts */
      --font-mono:'Geist Mono',ui-monospace,monospace; --font-sans:'Geist',system-ui,sans-serif;
      --font-annotation:'Space Mono',ui-monospace,monospace; --font-serif:'Zilla Slab',Georgia,serif;
```

- [ ] **Step 3: Point `.hero-title` at Geist Mono (line 64)**

Replace:

```css
    .hero-title { font-family:var(--font-hero); font-weight:700; letter-spacing:-0.01em;
      font-size:3.5rem; line-height:1.05; margin:20px 0 0; }
```

with:

```css
    .hero-title { font-family:var(--font-mono); font-weight:700; letter-spacing:-0.01em;
      font-size:3.5rem; line-height:1.05; margin:20px 0 0; }
```

- [ ] **Step 4: Add a decorative turquoise brand accent under the hero title**

In the `#hero` section, immediately after the `<h1 class="hero-title">Tri-Swiss.</h1>` line, insert:

```html
        <span style="display:block; width:64px; height:3px; background:var(--highlight); margin-top:16px;" aria-hidden="true"></span>
```

- [ ] **Step 5: Give the "rules" section a red divider and rewrite rule 01's copy**

Replace:

```html
      <section id="rules">
        <div class="divider"><h3>Two rules govern everything</h3><span class="rule"></span></div>
        <div style="display:grid; grid-template-columns:1fr 1fr; gap:24px;">
          <div style="border:1px solid var(--border); background:var(--card); padding:24px;">
            <p class="label">01 — Tri-tone strict</p>
            <p style="margin:12px 0 0;">Ink and cream are the two structural colors; Swiss
              Red is the primary accent. Pastel Turquoise is a rare, non-semantic
              highlight reserved for a chart's second series or one single brand
              moment — never a button, tag, or status color.</p>
          </div>
```

with:

```html
      <section id="rules">
        <div class="divider"><h3>Two rules govern everything</h3><span class="rule" style="background:var(--primary);"></span></div>
        <div style="display:grid; grid-template-columns:1fr 1fr; gap:24px;">
          <div style="border:1px solid var(--border); background:var(--card); padding:24px;">
            <p class="label">01 — Tri-tone, more colorful</p>
            <p style="margin:12px 0 0;">Ink and cream still lead. Swiss Red is the primary
              accent — action, destructive, focus — and now also marks section
              dividers and a selectively emphasized card border. Pastel Turquoise
              stays non-semantic but recurs as pure decoration: icon fills,
              underlines, washes, dots. Neither ever carries state meaning, and
              the two never touch on the same element.</p>
          </div>
```

(The second grid cell, "02 — Swiss-minimalist," is unchanged.)

- [ ] **Step 6: Update the palette section's `highlight` swatch caption**

Replace:

```html
              <span style="font-family:var(--font-mono); font-size:0.7rem; color:var(--muted-foreground);">charts/brand only</span>
```

with:

```html
              <span style="font-family:var(--font-mono); font-size:0.7rem; color:var(--muted-foreground);">decorative, non-semantic</span>
```

- [ ] **Step 7: Update the registers section heading/label and remove the Jost tile**

Replace:

```html
        <div class="divider"><h3>Five type registers</h3><span class="rule"></span></div>
        <p class="label" style="margin:-8px 0 24px;">Core Geist duotone + three sanctioned registers</p>
```

with:

```html
        <div class="divider"><h3>Four type registers</h3><span class="rule"></span></div>
        <p class="label" style="margin:-8px 0 24px;">Core Geist duotone + two sanctioned registers</p>
```

Then delete this whole block (the last tile in the registers grid, immediately before the closing `</div>` of that grid and before the `<blockquote>`):

```html
          <div>
            <p class="label" style="margin-bottom:12px;">Hero / editorial display</p>
            <p style="font-family:var(--font-hero); font-size:1.4rem; font-weight:700; margin:0;">02 — Chapter</p>
            <p class="label" style="margin-top:10px;">Jost</p>
          </div>
```

- [ ] **Step 8: Add two new component tiles — selective red card border, turquoise decorative accents**

In the `#components` grid, immediately after the existing "Card" tile (the tile containing `<p class="label" style="margin-bottom:16px;">Card</p>`) and before the "Toggle" tile, insert:

```html
          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Emphasis card <span style="text-transform:none; letter-spacing:normal; color:var(--muted-foreground);">— selective red border</span></p>
            <div style="border:1px solid var(--primary); background:var(--card); padding:16px;">
              <p style="margin:0; font-family:var(--font-mono); font-weight:700;">Featured</p>
              <p style="margin:8px 0 0; font-size:0.85rem; color:var(--muted-foreground);">
                One card in a set — never the default for a whole grid.</p>
            </div>
          </div>

          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Decorative accent <span style="text-transform:none; letter-spacing:normal; color:var(--muted-foreground);">— non-semantic</span></p>
            <div style="display:flex; align-items:center; gap:14px;">
              <svg class="icon" viewBox="0 0 24 24" style="color:var(--highlight);"><path d="M12 2v20"/><path d="M2 12h20"/></svg>
              <span style="width:6px; height:6px; border-radius:9999px; background:var(--highlight);"></span>
              <span style="border-bottom:2px solid var(--highlight); font-family:var(--font-mono); font-size:0.75rem; text-transform:uppercase; letter-spacing:0.12em; padding-bottom:2px;">Underline</span>
            </div>
          </div>
```

- [ ] **Step 9: Run the philosophy verifier**

Run: `cd scripts/capture && npm run verify`
Expected: `PASS: philosophy compliance OK` with exit code 0. If it fails, the error names the specific rule broken (rogue hex, shadow, unrestyled icon cap, or highlight leaking into a status pip) — fix the offending markup from the steps above and re-run.

- [ ] **Step 10: Verify no Jost references remain**

Run: `rtk grep -i "jost\|font-hero" docs/index.html`
Expected: no output (exit code 1 — no matches).

- [ ] **Step 11: Commit**

```bash
rtk git add docs/index.html
rtk git commit -m "feat(showcase): demonstrate expanded accents, drop Jost hero"
```

---

### Task 6: Sync `README.md` and `CONTRIBUTING.md`

**Files:**
- Modify: `README.md:22-28`, `:34-35`, `:49`, `:84`
- Modify: `CONTRIBUTING.md:6-12`

**Interfaces:** N/A.

- [ ] **Step 1: Rewrite README's aesthetic-summary paragraph**

Replace:

```markdown
**Tri-tone strict, Swiss-minimalist.** Two structural colors — ink
(`#000000`) and warm cream (`#eae8d0`) — plus a Swiss Red accent
(`#d3281b`) and one rare, non-semantic highlight, Pastel Turquoise
(`#56bfa3`), reserved for a chart's second series or one single brand
moment. No success green, no info blue, no second *semantic* accent — the
highlight never carries meaning.
```

with:

```markdown
**Tri-tone, more colorful, still Swiss-minimalist.** Two structural colors
— ink (`#000000`) and warm cream (`#eae8d0`) — plus a Swiss Red accent
(`#d3281b`) that now also marks section dividers and selective card
emphasis, and a non-semantic highlight, Pastel Turquoise (`#56bfa3`), used
decoratively across icon fills, underlines, washes, and chart series. No
success green, no info blue, no second *semantic* accent — the highlight
never carries meaning, however often it recurs.
```

- [ ] **Step 2: Update the governed-registers bullet**

Replace:

```markdown
- Two governed extra registers: **Space Mono** (italic, annotations/captions
  only — the visible nod to Duotone Swiss) and **Jost** (hero title/wordmark
  and section dividers only).
```

with:

```markdown
- One governed extra register: **Space Mono** (italic, annotations/captions
  only — the visible nod to Duotone Swiss).
```

- [ ] **Step 3: Update the type-register count above the screenshots**

Replace:

```markdown
Five type registers and the component library:
```

with:

```markdown
Four type registers and the component library:
```

- [ ] **Step 4: Drop Jost from the font-loading install step**

Replace:

```markdown
2. Add the Geist + Geist Mono + Space Mono + Zilla Slab + Jost Google Fonts
   link (or `next/font`).
```

with:

```markdown
2. Add the Geist + Geist Mono + Space Mono + Zilla Slab Google Fonts
   link (or `next/font`).
```

- [ ] **Step 5: Fix `CONTRIBUTING.md`'s outdated rule statement**

Replace:

```markdown
## Design changes
The design language lives in `skills/tri-swiss/`. Keep the two governing
rules intact — **tri-tone strict** (two structural colors + one strong accent
+ one rare, non-semantic highlight reserved for charts/brand moments — never
a second accent, never a third UI color) and **Swiss-minimalist** (visible
borders, no shadows). Changes that add a semantic color or a shadow
contradict the system and won't be accepted; express new states through
weight, size, spacing, and contrast instead.
```

with:

```markdown
## Design changes
The design language lives in `skills/tri-swiss/`. Keep the two governing
rules intact — **tri-tone, more colorful** (two structural colors + one
accent, Swiss Red, used more freely for action/divider/emphasis-border jobs
+ one non-semantic highlight, Pastel Turquoise, used decoratively but never
for state) and **Swiss-minimalist** (visible borders, no shadows). Changes
that add a *semantic* color (a third color that means success/info/state)
or a shadow contradict the system and won't be accepted; express new states
through weight, size, spacing, and contrast instead.
```

- [ ] **Step 6: Verify no Jost/stale-rule references remain**

Run: `rtk grep -i "jost\|five type registers\|reserved for charts/brand moments" README.md CONTRIBUTING.md`
Expected: no output (exit code 1 — no matches).

- [ ] **Step 7: Commit**

```bash
rtk git add README.md CONTRIBUTING.md
rtk git commit -m "docs: sync README and CONTRIBUTING with expanded accents, drop Jost"
```

---

### Task 7: Update `CHANGELOG.md`

**Files:**
- Modify: `CHANGELOG.md:9-24` (the `[Unreleased]` → `### Added` bullets)

**Interfaces:** N/A.

- [ ] **Step 1: Edit the pending-release bullets in place**

Replace:

```markdown
### Added
- Initial release of the **Tri-Swiss** design-system skill (`tri-swiss`).
- **`SKILL.md`** — philosophy (tri-tone strict, Swiss-minimalist), full
  light/dark palette token tables including the governed `--highlight`
  token, typography rules across four registers (Geist Mono/Sans, Space
  Mono italic, Zilla Slab, Jost), spacing/layout, and core component
  patterns (buttons, tags, section dividers).
- **`assets/theme.css`** — ready-to-paste Tailwind 4 theme with every CSS
  variable for light and dark mode, wired to Tailwind via `@theme inline`;
  also usable as plain CSS custom properties on non-Tailwind stacks.
- **`references/components.md`** — extended component catalogue: status
  pips, modal overlay, toggle controls, cards, inputs, hero/annotation
  type patterns, and the hand-rolled + Observable Plot chart patterns.
- **Showcase landing page** (`docs/index.html`) served on GitHub Pages,
  plus README screenshots, a 1200×630 social-preview card, and a
  reproducible Playwright capture script.
- **Philosophy-compliance verifier** (`scripts/capture/verify-philosophy.mjs`)
  — automated check that the showcase page never uses a rogue color, a
  shadow, an unrestyled icon cap, or leaks `--highlight` into a semantic
  UI role.
```

with:

```markdown
### Added
- Initial release of the **Tri-Swiss** design-system skill (`tri-swiss`).
- **`SKILL.md`** — philosophy (tri-tone, more colorful, Swiss-minimalist),
  full light/dark palette token tables including the governed `--highlight`
  token, typography rules across three registers (Geist Mono/Sans, Space
  Mono italic, Zilla Slab), spacing/layout, and core component patterns
  (buttons, tags, section dividers, selective red card-border emphasis,
  decorative Turquoise accents).
- **`assets/theme.css`** — ready-to-paste Tailwind 4 theme with every CSS
  variable for light and dark mode, wired to Tailwind via `@theme inline`;
  also usable as plain CSS custom properties on non-Tailwind stacks.
- **`references/components.md`** — extended component catalogue: status
  pips, modal overlay, toggle controls, cards, inputs, hero/annotation
  type patterns, accent-expansion patterns (red dividers, red card
  borders, turquoise decoration), and the hand-rolled + Observable Plot
  chart patterns.
- **Showcase landing page** (`docs/index.html`) served on GitHub Pages,
  plus README screenshots, a 1200×630 social-preview card, and a
  reproducible Playwright capture script.
- **Philosophy-compliance verifier** (`scripts/capture/verify-philosophy.mjs`)
  — automated check that the showcase page never uses a rogue color, a
  shadow, an unrestyled icon cap, or leaks `--highlight` into a semantic
  UI role.
```

- [ ] **Step 2: Verify the register count is now three everywhere in the changelog**

Run: `rtk grep -i "four registers\|jost" CHANGELOG.md`
Expected: no output (exit code 1 — no matches).

- [ ] **Step 3: Commit**

```bash
rtk git add CHANGELOG.md
rtk git commit -m "docs: update changelog for expanded accents and Jost removal"
```

---

### Task 8: Regenerate screenshots

**Files:**
- Modify (binary, regenerated): `docs/assets/hero-light.png`, `docs/assets/hero-dark.png`, `docs/assets/palette.png`, `docs/assets/components.png`, `docs/assets/charts.png`, `docs/assets/type-registers.png`, `docs/assets/social-card.png`

**Interfaces:** N/A.

- [ ] **Step 1: Run the capture script**

Run: `cd scripts/capture && npm run capture`
Expected: seven lines of `wrote <filename>.png`, one per file listed above, no thrown errors. (`node_modules` is already present in `scripts/capture/`; if it's missing, run `npm install` first.)

- [ ] **Step 2: Visually confirm the new patterns render correctly**

Open `docs/assets/hero-light.png` and `docs/assets/components.png` and confirm: the turquoise underline appears beneath the hero title, the "rules" section divider is red, the new "Emphasis card" tile has a red border, and the new "Decorative accent" tile shows a turquoise icon/dot/underline. Re-run Step 1 after any further HTML tweaks.

- [ ] **Step 3: Re-run the philosophy verifier one more time post-capture**

Run: `cd scripts/capture && npm run verify`
Expected: `PASS: philosophy compliance OK`.

- [ ] **Step 4: Commit the regenerated screenshots**

```bash
rtk git add docs/assets/
rtk git commit -m "chore(showcase): regenerate screenshots for expanded accents"
```

---

### Task 9: Push branch and open PR

**Files:** None (git/gh operations only).

**Interfaces:** N/A.

- [ ] **Step 1: Confirm the branch and full commit history**

Run: `rtk git log main..HEAD --oneline`
Expected: 8 commits, one per Task 1–8 above, all Conventional Commits.

- [ ] **Step 2: Push the branch**

Run: `rtk git push -u origin feat/expand-accents-drop-jost`
Expected: branch created on `origin`.

- [ ] **Step 3: Open the PR**

```bash
gh pr create --title "feat: more colorful accents, drop Jost" --body "$(cat <<'EOF'
## Summary
- Loosens Swiss Red / Pastel Turquoise rationing per docs/superpowers/specs/2026-07-06-colorful-accents-drop-jost-design.md — Red gains section-divider and selective-card-border jobs; Turquoise stays non-semantic but is no longer capped at one touch per page.
- Drops the Jost 4th type register; hero title/wordmark and chapter dividers fold into Geist Mono. Three registers instead of four, four font files instead of five.
- Showcase page, component catalogue, README, CONTRIBUTING, and CHANGELOG all updated to match; screenshots regenerated.

## Test plan
- [x] `scripts/capture/verify-philosophy.mjs` passes
- [x] No residual `Jost`/`font-hero` references anywhere in the repo (grepped per-task)
- [ ] Visual check of regenerated `docs/assets/*.png` on the PR itself
EOF
)"
```

Expected: PR URL printed; note it for the user.
