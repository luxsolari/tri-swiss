# Turquoise Structural Block and Hover-State Hierarchy Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Give Pastel Turquoise a genuine Structural Block of its own (smaller in scale than Red's), make the tri-part segment stripe reusable at varying lengths, and formalize + demonstrate a hover-state color hierarchy (Red primary, Turquoise secondary) — so Tri-Swiss visibly differentiates from the strict-duotone Lux Swiss.

**Architecture:** Pure documentation + static-page changes across `SKILL.md`, `references/components.md`, `docs/index.html`, `assets/theme.css`, and the Playwright capture script. No build step, no JS framework — every change is either markdown prose, an HTML/CSS snippet, or a plain-object entry in the capture job list.

**Tech Stack:** Plain HTML/CSS (Tailwind 4 `@theme inline` tokens), Playwright (screenshot capture), Node.js (capture/verify scripts), Markdown.

## Global Constraints

- Ink, cream, Swiss Red, and Pastel Turquoise remain the only four color
  tokens — no fifth color introduced anywhere.
- Turquoise never carries semantic/state meaning — the new Structural
  Block and hover rule are additive, not a promotion to semantic status.
- Red's Structural Block keeps its ~25% viewport cap; Turquoise's new
  forms are each capped smaller (~15% of viewport height for the
  second-moment panel and closing band; content-sized, no viewport cap,
  for the callout panel).
- New Turquoise blocks must never be adjacent to or touch Red's own
  Structural Block (sidebar/hero-band) — the tri-part segment stripe
  remains the one named exception to "Red and Turquoise never touch."
  One accent per component still holds everywhere else.
- The existing Ghost/Outlined/Filled buttons are unchanged — they keep
  hovering via ink/muted-foreground shifts only.
- `scripts/capture/verify-philosophy.mjs` must continue to print
  `PASS: philosophy compliance OK` after every task that touches
  `docs/index.html`. Its `PALETTE` allow-list already contains `#000000`,
  so no new hex value needs to be added to it.
- Every commit follows this repo's `AGENTS.md` Conventional Commits +
  changelog-first discipline; this repo has no git tags yet, so
  `CHANGELOG.md` entries land under the existing `[Unreleased]` →
  `### Added` list, not a new version section.

---

### Task 1: `--highlight-foreground` token, Turquoise Structural Block philosophy, stripe-reuse rule, and hover-state hierarchy in `SKILL.md`/`theme.css`

**Files:**
- Modify: `skills/tri-swiss/assets/theme.css`
- Modify: `skills/tri-swiss/SKILL.md`

**Interfaces:**
- Produces: the CSS custom property `--highlight-foreground` (value
  `#000000` in both `:root` and `.dark`), and its Tailwind mapping
  `--color-highlight-foreground`. Task 3 uses this as `text-highlight-foreground`/`color:var(--highlight-foreground)` on every solid-Turquoise
  fill (callout panel, second-moment panel, closing band).
- Produces: the documented rule text for the Turquoise Structural Block,
  the stripe-reuse rule, and the hover-state hierarchy, which Task 2
  (components.md) and Task 4 (docs/index.html copy) reference.

- [ ] **Step 1: Add the `--highlight-foreground` token to `theme.css`**

In `skills/tri-swiss/assets/theme.css`, the `:root` block currently reads
(around line 24):

```css
  --highlight: #56bfa3;
  --radius: 0.5rem;
```

Change to:

```css
  --highlight: #56bfa3;
  --highlight-foreground: #000000;
  --radius: 0.5rem;
```

The `.dark` block currently ends with (around line 50):

```css
  --highlight: #63cbae;
}
```

Change to:

```css
  --highlight: #63cbae;
  --highlight-foreground: #000000;
}
```

`--highlight-foreground` is deliberately the same `#000000` in both
modes — Turquoise stays a light/pastel color even in dark mode
(`#63cbae`), so black text/icons read correctly on it in both modes
(measured contrast ratio ≈ 9.4:1 light mode, ≈ 10.7:1 dark mode; cream
text on Turquoise would be ≈ 1.7–1.9:1, well below WCAG AA).

The `@theme inline` block currently has (around line 73):

```css
  --color-highlight: var(--highlight);
```

Add directly below it:

```css
  --color-highlight: var(--highlight);
  --color-highlight-foreground: var(--highlight-foreground);
```

- [ ] **Step 2: Add `--highlight-foreground` to both palette tables in `SKILL.md`**

The light-mode table (around line 108) has this row:

```
| `--highlight` | `#56bfa3` | Pastel Turquoise — governed, non-semantic (see below) |
```

Add directly below it:

```
| `--highlight-foreground` | `#000000` | Text/icon color on solid `--highlight` fills — constant black in both modes since Turquoise stays light/pastel even in dark mode |
```

The dark-mode table (around line 124) has this row:

```
| `--highlight` | `#63cbae` | Pastel Turquoise, lifted for dark mode |
```

Add directly below it:

```
| `--highlight-foreground` | `#000000` | Same constant black — Turquoise stays light/pastel even in dark mode |
```

- [ ] **Step 3: Add the Turquoise Structural Block and stripe-reuse rule to the Philosophy section**

In `skills/tri-swiss/SKILL.md`, the Philosophy section currently ends
(around lines 76–80) with:

```
accent per component, not both. The **tri-part segment stripe** — three equal solid blocks, ink/Red/
Turquoise in a row, used for a static decorative bar (e.g. beneath a hero
title) — is the one explicitly named exception to "Red and Turquoise
never touch": a single governed device, not a general loosening. Nowhere
else may the two sit adjacent.

**Swiss-minimalist.** Borders are visible (1px solid, full ink or full
```

Replace with:

```
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
```

- [ ] **Step 4: Add the Destructive button variant to `## Buttons`**

The `## Buttons` section currently reads (around lines 231–242):

```
## Buttons

All buttons: `font-mono uppercase tracking-[0.2em] text-xs`. Three variants:

- **Ghost / nav** (most common): `text-muted-foreground hover:text-foreground
  transition-colors`, no border.
- **Outlined:** `border border-foreground px-4 py-2 hover:bg-foreground
  hover:text-background`.
- **Filled** (primary action, rare): `border border-foreground bg-foreground px-4
  py-2 text-background hover:bg-foreground/90`.

**Disabled** is always `opacity-40` — never a color change.
```

Replace with:

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
```

- [ ] **Step 5: Add the `## Hover states` section**

Directly after the `## Buttons` section (which now ends with "**Disabled**
is always `opacity-40` — never a color change.") and before `## Tags /
pills`, insert:

```
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
```

- [ ] **Step 6: Cross-reference the hover hierarchy from the `--highlight` token section**

The `--highlight` token section currently ends (around lines 157–160)
with:

```
**Do not** use it as a button's, tag's, status pip's, or link's own
state-indicating color. A link may show a decorative Turquoise
hover-flourish (item 7 above) *in addition to* its real ink/red state
feedback — Turquoise itself never signals the state.
```

Replace with:

```
**Do not** use it as a button's, tag's, status pip's, or link's own
state-indicating color. A link may show a decorative Turquoise
hover-flourish (item 7 above) *in addition to* its real ink/red state
feedback — Turquoise itself never signals the state (see "Hover states"
below for the system-wide rule this follows).
```

- [ ] **Step 7: Add a hover-hierarchy bullet to `## Do not`**

The `## Do not` section currently starts (around lines 301–304) with:

```
## Do not

- **No third semantic color.** `--highlight` is not success green or info
  blue in disguise — it carries no state meaning anywhere.
```

Replace with:

```
## Do not

- **No third semantic color.** `--highlight` is not success green or info
  blue in disguise — it carries no state meaning anywhere.
- **No accent-colored hover signal other than Red.** Turquoise may only
  decorate a hover state, never carry its meaning alone.
```

- [ ] **Step 8: Verify**

Run: `node scripts/capture/verify-philosophy.mjs` from the repo root.
Expected: `PASS: philosophy compliance OK` (this script only reads
`docs/index.html`, which this task doesn't touch, so it must still pass
unchanged).

Run: `rtk grep -n "highlight-foreground" skills/tri-swiss/assets/theme.css skills/tri-swiss/SKILL.md`
Expected: matches in both files (theme.css: 3 — `:root`, `.dark`,
`@theme inline`; SKILL.md: 2 — light table, dark table).

- [ ] **Step 9: Commit**

```bash
git add skills/tri-swiss/assets/theme.css skills/tri-swiss/SKILL.md
git commit -m "feat: add Turquoise Structural Block, stripe reuse, and hover hierarchy rules"
```

---

### Task 2: New patterns in `references/components.md`

**Files:**
- Modify: `skills/tri-swiss/references/components.md`

**Interfaces:**
- Consumes: the `--highlight-foreground` token and the rule text from
  Task 1.
- Produces: the documented HTML patterns for the three Turquoise
  Structural Block forms, the multi-length stripe, and the two
  Default/Hover swatch pairs, which Task 3/4 implement on the live page.

- [ ] **Step 1: Update the Structural Block section heading**

The section heading currently reads (line 171):

```
## Structural Block — sidebar, hero band, bold word
```

Replace with:

```
## Structural Block — sidebar, hero band, bold word, and Turquoise's own forms
```

- [ ] **Step 2: Update the Tri-part segment stripe entry for multi-length reuse**

The current entry reads (around lines 217–227):

```
**Tri-part segment stripe.** Three equal solid blocks — ink, Red,
Turquoise — used as a static decorative bar. The one named exception to
"Red and Turquoise never touch": scoped to this pattern only.

```html
<div style="display:flex; gap:4px; width:64px;">
  <div style="height:3px; flex:1; background:var(--foreground);"></div>
  <div style="height:3px; flex:1; background:var(--primary);"></div>
  <div style="height:3px; flex:1; background:var(--highlight);"></div>
</div>
```
```

Replace with:

```
**Tri-part segment stripe.** Three equal solid blocks — ink, Red,
Turquoise — used as a static decorative divider or spacer, at any
length: a small marker before a heading, a section divider, a wider
closing flourish. The one named exception to "Red and Turquoise never
touch": scoped to this pattern only. Always three *equal* segments in
this exact order; static and decorative only, never interactive or
meaningful; used selectively (a handful of times per page) rather than
replacing the default `bg-border` divider throughout.

```html
<!-- Small marker before a heading -->
<div style="display:flex; gap:3px; width:36px;">
  <div style="height:3px; flex:1; background:var(--foreground);"></div>
  <div style="height:3px; flex:1; background:var(--primary);"></div>
  <div style="height:3px; flex:1; background:var(--highlight);"></div>
</div>

<!-- Wider closing flourish -->
<div style="display:flex; gap:6px; width:140px;">
  <div style="height:3px; flex:1; background:var(--foreground);"></div>
  <div style="height:3px; flex:1; background:var(--primary);"></div>
  <div style="height:3px; flex:1; background:var(--highlight);"></div>
</div>
```
```

- [ ] **Step 3: Add the three Turquoise Structural Block patterns and the two hover swatch-pair patterns**

The file currently has this content right after the "Turquoise
hover-flourish on nav links" block, immediately before `## Iconography`
(around lines 229–245):

```
**Turquoise hover-flourish on nav links.** Purely decorative, identical
regardless of state — layered alongside the link's real ink/red hover
feedback, never replacing it:

```html
<a href="#section" style="color:var(--primary-foreground); text-decoration:none;
   opacity:0.75; border-bottom:2px solid transparent; padding-bottom:2px;"
   onmouseover="this.style.borderBottomColor='var(--highlight)'"
   onmouseout="this.style.borderBottomColor='transparent'">Section</a>
```

(In `docs/index.html` this is implemented via a CSS `:hover` rule rather
than inline `onmouseover`, shown in Task 3 — the inline-handler form above
is illustrative of the effect for consumers who copy this pattern into a
project without the page's own stylesheet.)

## Iconography (geist-icons, restyled)
```

Insert the new patterns between the parenthetical note and `## Iconography`:

```
**Turquoise hover-flourish on nav links.** Purely decorative, identical
regardless of state — layered alongside the link's real ink/red hover
feedback, never replacing it:

```html
<a href="#section" style="color:var(--primary-foreground); text-decoration:none;
   opacity:0.75; border-bottom:2px solid transparent; padding-bottom:2px;"
   onmouseover="this.style.borderBottomColor='var(--highlight)'"
   onmouseout="this.style.borderBottomColor='transparent'">Section</a>
```

(In `docs/index.html` this is implemented via a CSS `:hover` rule rather
than inline `onmouseover`, shown in Task 3 — the inline-handler form above
is illustrative of the effect for consumers who copy this pattern into a
project without the page's own stylesheet.)

**Turquoise callout/note panel.** Solid-Turquoise highlight box for tips,
notes, or dashboard callouts — content-sized, never full-bleed, capped at
a modest max-width matching prose/card width. May recur a few times per
page — a content-level device, not page chrome:

```html
<div style="background:var(--highlight); color:var(--highlight-foreground);
  padding:14px 16px; max-width:60ch;">
  <p style="margin:0; font-family:var(--font-mono); font-size:0.7rem;
    text-transform:uppercase; letter-spacing:0.12em; font-weight:700;">Tip</p>
  <p style="margin:8px 0 0; font-size:0.9rem;">Content-sized, never full-bleed.</p>
</div>
```

**Turquoise second-moment panel.** One larger solid-Turquoise section,
used once per page, appearing later in the page flow — never in the
hero, which stays Red's territory:

```html
<section>
  <div style="background:var(--highlight); color:var(--highlight-foreground); padding:40px;">
    <p style="margin:0; font-family:var(--font-mono); font-weight:700; font-size:1.5rem;">
      One genuine moment.
    </p>
  </div>
</section>
```

**Turquoise closing band.** Solid-Turquoise horizontal strip near the
bottom of the page (opposite the sidebar), used once, capped at roughly
15% of viewport height:

```html
<div style="background:var(--highlight); color:var(--highlight-foreground); padding:28px 0;">
  <div class="content-inner" style="text-align:center;">
    <p style="margin:0; font-family:var(--font-mono); text-transform:uppercase;
      letter-spacing:0.15em; font-size:0.8rem; font-weight:700;">Closing statement.</p>
  </div>
</div>
```

Guardrails for all three: never adjacent to Red's own Structural Block
(no shared edges, no touching the sidebar/hero-band); combined footprint
stays visibly smaller than Red's block wherever both appear on the same
page; ink/cream still dominate every surface outside these blocks; one
accent per component still holds.

**Destructive button hover, Default vs. Hover.** Demonstrates the hover
hierarchy (see `SKILL.md`'s "Hover states"): Red carries the real signal.

```html
<button style="border:1px solid var(--primary); background:none;
  color:var(--primary); padding:8px 14px;">Delete</button>
<!-- :hover (or a static .is-hover-demo modifier class for illustration) -->
<button style="border:1px solid var(--primary); background:var(--primary);
  color:var(--primary-foreground); padding:8px 14px;">Delete</button>
```

**Nav-link hover, Default vs. Hover.** The Turquoise flourish layered
alongside the link's real ink/red state feedback:

```html
<a style="color:var(--primary-foreground); opacity:0.75; border-bottom:2px
  solid transparent; padding-bottom:2px;">Section</a>
<!-- :hover (or a static .is-hover-demo modifier class for illustration) -->
<a style="color:var(--primary-foreground); opacity:1; border-bottom:2px
  solid var(--highlight); padding-bottom:2px;">Section</a>
```

## Iconography (geist-icons, restyled)
```

- [ ] **Step 4: Verify**

Run: `rtk grep -n "Turquoise callout\|Turquoise second-moment\|Turquoise closing band\|Nav-link hover, Default" skills/tri-swiss/references/components.md`
Expected: all four headings found.

- [ ] **Step 5: Commit**

```bash
git add skills/tri-swiss/references/components.md
git commit -m "docs: add Turquoise Structural Block and hover-state patterns to components.md"
```

---

### Task 3: `docs/index.html` — CSS additions and Components section demos

**Files:**
- Modify: `docs/index.html`

**Interfaces:**
- Consumes: `--highlight-foreground` (Task 1).
- Produces: the `.btn-destructive` and `.demo-nav-link` CSS classes
  (with their `:hover` and `.is-hover-demo` static-modifier rules), used
  only within this task's own markup. Produces the `id="components"`
  section's three new demo cards.

- [ ] **Step 1: Add `--highlight-foreground` to the page's inline `:root`/`.dark` blocks**

In `docs/index.html`, the inline `<style>` block's `:root` currently has
(around line 43):

```css
      --highlight:#56bfa3;
```

Change to:

```css
      --highlight:#56bfa3; --highlight-foreground:#000000;
```

The `.dark` block currently has (around line 52):

```css
      --highlight:#63cbae;
```

Change to:

```css
      --highlight:#63cbae; --highlight-foreground:#000000;
```

- [ ] **Step 2: Add the Destructive-button and nav-link-demo CSS rules**

The `<style>` block currently has this section (around lines 74–80):

```css
    .toggle button[aria-pressed="true"] { color:var(--foreground); }
    .toggle .mid { color:var(--muted-foreground); opacity:0.4; }

    /* Structural Block — sidebar layout */
```

Insert new rules between `.toggle .mid` and the `/* Structural Block —
sidebar layout */` comment:

```css
    .toggle button[aria-pressed="true"] { color:var(--foreground); }
    .toggle .mid { color:var(--muted-foreground); opacity:0.4; }

    /* Hover-state color hierarchy demos — Red carries the real signal,
       Turquoise only decorates. .is-hover-demo mirrors the :hover rule's
       exact declarations as a static class, so the state is visible in
       a screenshot as well as to a live pointer. */
    .btn-destructive { border:1px solid var(--primary); background:none; color:var(--primary);
      transition:background-color 0.15s, color 0.15s; }
    .btn-destructive:hover, .btn-destructive.is-hover-demo { background:var(--primary); color:var(--primary-foreground); }
    .demo-nav-link { color:var(--primary-foreground); text-decoration:none; opacity:0.75;
      border-bottom:2px solid transparent; padding-bottom:2px; font-family:var(--font-mono);
      font-size:0.8rem; text-transform:uppercase; letter-spacing:0.15em;
      transition:opacity 0.15s, border-color 0.15s; }
    .demo-nav-link:hover, .demo-nav-link.is-hover-demo { opacity:1; border-bottom-color:var(--highlight); }

    /* Structural Block — sidebar layout */
```

- [ ] **Step 3: Add the small tri-part stripe marker before the "Components" heading**

The Components section's divider currently reads (line 353):

```html
        <div class="divider"><h3>Components</h3><span class="rule"></span></div>
```

Replace with:

```html
        <div class="divider">
          <div style="display:flex; gap:3px; width:36px;" aria-hidden="true">
            <div style="height:3px; flex:1; background:var(--foreground);"></div>
            <div style="height:3px; flex:1; background:var(--primary);"></div>
            <div style="height:3px; flex:1; background:var(--highlight);"></div>
          </div>
          <h3>Components</h3><span class="rule"></span>
        </div>
```

- [ ] **Step 4: Add the Destructive button to the existing Buttons demo card**

The Buttons demo card currently reads (around lines 356–369):

```html
          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Buttons</p>
            <div style="display:flex; flex-wrap:wrap; gap:10px; font-family:var(--font-mono);
                        text-transform:uppercase; letter-spacing:0.2em; font-size:0.7rem;">
              <button style="border:1px solid var(--foreground); background:var(--foreground);
                color:var(--background); padding:8px 14px;">Filled</button>
              <button style="border:1px solid var(--foreground); background:none;
                color:var(--foreground); padding:8px 14px;">Outlined</button>
              <button style="border:none; background:none; color:var(--muted-foreground);
                padding:8px 14px;">Ghost</button>
              <button style="border:1px solid var(--foreground); background:none; padding:8px 14px;
                opacity:0.4;" disabled>Disabled</button>
            </div>
          </div>
```

Replace with:

```html
          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Buttons</p>
            <div style="display:flex; flex-wrap:wrap; gap:10px; font-family:var(--font-mono);
                        text-transform:uppercase; letter-spacing:0.2em; font-size:0.7rem;">
              <button style="border:1px solid var(--foreground); background:var(--foreground);
                color:var(--background); padding:8px 14px;">Filled</button>
              <button style="border:1px solid var(--foreground); background:none;
                color:var(--foreground); padding:8px 14px;">Outlined</button>
              <button style="border:none; background:none; color:var(--muted-foreground);
                padding:8px 14px;">Ghost</button>
              <button style="border:1px solid var(--foreground); background:none; padding:8px 14px;
                opacity:0.4;" disabled>Disabled</button>
              <button class="btn-destructive" style="padding:8px 14px; font:inherit;
                letter-spacing:inherit; text-transform:inherit;">Destructive</button>
            </div>
          </div>
```

- [ ] **Step 5: Add the Callout panel and two Default/Hover swatch-pair demo cards**

The Components grid currently ends (around lines 439–451) with:

```html
          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Icons · geist-icons — endorsed set</p>
            <div style="display:flex; flex-wrap:wrap; gap:18px; color:var(--foreground);">
              <svg class="icon" viewBox="0 0 24 24"><line x1="4" y1="6" x2="20" y2="6"/><line x1="4" y1="12" x2="20" y2="12"/><line x1="4" y1="18" x2="20" y2="18"/></svg><!-- menu -->
              <svg class="icon" viewBox="0 0 24 24"><path d="M18 6 6 18"/><path d="m6 6 12 12"/></svg><!-- x -->
              <svg class="icon" viewBox="0 0 24 24"><path d="M5 12h14"/><path d="m12 5 7 7-7 7"/></svg><!-- arrow-right -->
              <svg class="icon" viewBox="0 0 24 24"><path d="M15 3h6v6"/><path d="M10 14 21 3"/><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/></svg><!-- external-link -->
              <svg class="icon" viewBox="0 0 24 24"><circle cx="12" cy="12" r="4"/><path d="M12 2v2"/><path d="M12 20v2"/><path d="m4.9 4.9 1.4 1.4"/><path d="m17.7 17.7 1.4 1.4"/><path d="M2 12h2"/><path d="M20 12h2"/><path d="m6.3 17.7-1.4 1.4"/><path d="m19.1 4.9-1.4 1.4"/></svg><!-- sun -->
              <svg class="icon" viewBox="0 0 24 24"><path d="M12 3a6 6 0 0 0 9 9 9 9 0 1 1-9-9Z"/></svg><!-- moon -->
            </div>
          </div>

        </div>
      </section>
```

Replace with:

```html
          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Icons · geist-icons — endorsed set</p>
            <div style="display:flex; flex-wrap:wrap; gap:18px; color:var(--foreground);">
              <svg class="icon" viewBox="0 0 24 24"><line x1="4" y1="6" x2="20" y2="6"/><line x1="4" y1="12" x2="20" y2="12"/><line x1="4" y1="18" x2="20" y2="18"/></svg><!-- menu -->
              <svg class="icon" viewBox="0 0 24 24"><path d="M18 6 6 18"/><path d="m6 6 12 12"/></svg><!-- x -->
              <svg class="icon" viewBox="0 0 24 24"><path d="M5 12h14"/><path d="m12 5 7 7-7 7"/></svg><!-- arrow-right -->
              <svg class="icon" viewBox="0 0 24 24"><path d="M15 3h6v6"/><path d="M10 14 21 3"/><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/></svg><!-- external-link -->
              <svg class="icon" viewBox="0 0 24 24"><circle cx="12" cy="12" r="4"/><path d="M12 2v2"/><path d="M12 20v2"/><path d="m4.9 4.9 1.4 1.4"/><path d="m17.7 17.7 1.4 1.4"/><path d="M2 12h2"/><path d="M20 12h2"/><path d="m6.3 17.7-1.4 1.4"/><path d="m19.1 4.9-1.4 1.4"/></svg><!-- sun -->
              <svg class="icon" viewBox="0 0 24 24"><path d="M12 3a6 6 0 0 0 9 9 9 9 0 1 1-9-9Z"/></svg><!-- moon -->
            </div>
          </div>

          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Callout <span style="text-transform:none; letter-spacing:normal; color:var(--muted-foreground);">— Turquoise Structural Block</span></p>
            <div style="background:var(--highlight); color:var(--highlight-foreground); padding:14px 16px;">
              <p style="margin:0; font-family:var(--font-mono); font-size:0.7rem; text-transform:uppercase; letter-spacing:0.12em; font-weight:700;">Tip</p>
              <p style="margin:8px 0 0; font-size:0.82rem;">A solid-Turquoise panel for tips and notes — content-sized, never full-bleed.</p>
            </div>
          </div>

          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Hover — destructive button <span style="text-transform:none; letter-spacing:normal; color:var(--muted-foreground);">Red carries the real signal</span></p>
            <div style="display:flex; gap:24px; font-family:var(--font-mono);
                        text-transform:uppercase; letter-spacing:0.2em; font-size:0.7rem;">
              <div style="display:flex; flex-direction:column; gap:8px; align-items:flex-start;">
                <span style="font-size:0.65rem; letter-spacing:0.12em; color:var(--muted-foreground);">Default</span>
                <button class="btn-destructive" style="padding:8px 14px; font:inherit; letter-spacing:inherit; text-transform:inherit;">Delete</button>
              </div>
              <div style="display:flex; flex-direction:column; gap:8px; align-items:flex-start;">
                <span style="font-size:0.65rem; letter-spacing:0.12em; color:var(--muted-foreground);">Hover</span>
                <button class="btn-destructive is-hover-demo" style="padding:8px 14px; font:inherit; letter-spacing:inherit; text-transform:inherit;">Delete</button>
              </div>
            </div>
          </div>

          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Hover — nav link <span style="text-transform:none; letter-spacing:normal; color:var(--muted-foreground);">Turquoise flourish, layered alongside</span></p>
            <div style="display:flex; gap:0; background:var(--primary); padding:16px;">
              <div style="flex:1; display:flex; flex-direction:column; gap:8px; align-items:flex-start;">
                <span style="font-family:var(--font-mono); font-size:0.65rem; text-transform:uppercase; letter-spacing:0.12em; color:var(--primary-foreground); opacity:0.7;">Default</span>
                <a href="#components" class="demo-nav-link" onclick="return false;">Section</a>
              </div>
              <div style="flex:1; display:flex; flex-direction:column; gap:8px; align-items:flex-start;">
                <span style="font-family:var(--font-mono); font-size:0.65rem; text-transform:uppercase; letter-spacing:0.12em; color:var(--primary-foreground); opacity:0.7;">Hover</span>
                <a href="#components" class="demo-nav-link is-hover-demo" onclick="return false;">Section</a>
              </div>
            </div>
          </div>

        </div>
      </section>
```

- [ ] **Step 6: Verify**

Run: `node scripts/capture/verify-philosophy.mjs` from the repo root.
Expected: `PASS: philosophy compliance OK`.

Run: `rtk grep -n "btn-destructive\|demo-nav-link\|Turquoise Structural Block" docs/index.html`
Expected: matches for the CSS class definitions and the three new demo
cards' labels.

- [ ] **Step 7: Commit**

```bash
git add docs/index.html
git commit -m "feat: add destructive button, callout panel, and hover swatch demos to the showcase"
```

---

### Task 4: `docs/index.html` — second-moment panel, closing band, wide stripe divider, and rules-section copy

**Files:**
- Modify: `docs/index.html`

**Interfaces:**
- Consumes: `--highlight-foreground` (Task 1), `.content-inner` (existing
  class).
- Produces: `id="turquoise-moment"` (the second-moment panel section)
  and `id="closing-band"` (the closing band wrapper), which Task 5's
  capture script targets by element ID.

- [ ] **Step 1: Insert the second-moment panel, stripe divider, and closing band after the Charts section**

The page currently transitions from Charts straight to the off-canvas
social-card div (around lines 480–487):

```html
        <figure style="margin:32px 0 0;">
          <figcaption class="label" style="margin-bottom:12px;">Observable Plot · endorsed chart library — restyled to the palette</figcaption>
          <div id="plot-mount"></div>
        </figure>
      </section>
      <div id="social-card" aria-hidden="true" style="position:absolute; left:-9999px; top:0;
```

Replace with:

```html
        <figure style="margin:32px 0 0;">
          <figcaption class="label" style="margin-bottom:12px;">Observable Plot · endorsed chart library — restyled to the palette</figcaption>
          <div id="plot-mount"></div>
        </figure>
      </section>
      <section id="turquoise-moment">
        <div style="background:var(--highlight); color:var(--highlight-foreground); padding:40px;">
          <p class="label" style="color:var(--highlight-foreground); opacity:0.7; margin:0;">One more moment</p>
          <p style="margin:12px 0 0; font-family:var(--font-mono); font-weight:700; font-size:1.5rem; max-width:44ch;">
            A genuine third color — not an occasional accent.
          </p>
        </div>
      </section>
      <div style="display:flex; justify-content:center; padding:32px 0; border-top:1px solid var(--border);">
        <div style="display:flex; gap:6px; width:140px;" aria-hidden="true">
          <div style="height:3px; flex:1; background:var(--foreground);"></div>
          <div style="height:3px; flex:1; background:var(--primary);"></div>
          <div style="height:3px; flex:1; background:var(--highlight);"></div>
        </div>
      </div>
      <div id="closing-band" style="background:var(--highlight); color:var(--highlight-foreground); padding:28px 0;">
        <div class="content-inner" style="text-align:center;">
          <p style="margin:0; font-family:var(--font-mono); text-transform:uppercase; letter-spacing:0.15em; font-size:0.8rem; font-weight:700;">
            Two colors. One accent. One genuine third.
          </p>
        </div>
      </div>
      <div id="social-card" aria-hidden="true" style="position:absolute; left:-9999px; top:0;
```

- [ ] **Step 2: Update the "rules" section copy**

The `#rules` section's first rule card currently reads (around lines
205–214):

```html
          <div style="border:1px solid var(--border); background:var(--card); padding:24px;">
            <p class="label">01 — Tri-tone, more colorful</p>
            <p style="margin:12px 0 0;">Ink and cream still lead. Swiss Red is the primary
              accent — action, destructive, focus — and now also marks section
              dividers and a selectively emphasized card border. Pastel Turquoise
              stays non-semantic but recurs as pure decoration: icon fills,
              underlines, washes, dots. Neither ever carries state meaning, and
              the two never touch on the same element — except the one named
              tri-part segment stripe in the hero above.</p>
          </div>
```

Replace with:

```html
          <div style="border:1px solid var(--border); background:var(--card); padding:24px;">
            <p class="label">01 — Tri-tone, more colorful</p>
            <p style="margin:12px 0 0;">Ink and cream still lead. Swiss Red is the primary
              accent — action, destructive, focus, hover signal — and now also marks
              section dividers, a selectively emphasized card border, and a genuine
              Structural Block. Pastel Turquoise stays non-semantic but recurs as
              pure decoration and now has a Structural Block of its own, smaller in
              scale — icon fills, underlines, washes, dots, callout panels. Neither
              ever carries state meaning, and the two never touch on the same
              element — except the tri-part segment stripe, reused here as a
              divider at a few different lengths.</p>
          </div>
```

- [ ] **Step 3: Verify**

Run: `node scripts/capture/verify-philosophy.mjs` from the repo root.
Expected: `PASS: philosophy compliance OK`.

Run: `rtk grep -n 'id="turquoise-moment"\|id="closing-band"' docs/index.html`
Expected: one match each.

Manually confirm (read the surrounding markup) that `#turquoise-moment`
and `#closing-band` are not adjacent to `.sidebar` in the DOM (they
aren't — the sidebar is an `<aside>` sibling of `.content`, far from
these new blocks which live inside `.content-inner`).

- [ ] **Step 4: Commit**

```bash
git add docs/index.html
git commit -m "feat: add turquoise second-moment panel, closing band, and stripe divider"
```

---

### Task 5: Capture script and screenshot regeneration

**Files:**
- Modify: `scripts/capture/capture.mjs`
- Modify: `docs/assets/*.png` (regenerated, not hand-edited)

**Interfaces:**
- Consumes: `id="turquoise-moment"` and `id="closing-band"` (Task 4).
- Produces: `docs/assets/turquoise-moment.png` and
  `docs/assets/closing-band.png`, which Task 6's README changes embed.

- [ ] **Step 1: Add the two new capture jobs**

`scripts/capture/capture.mjs` currently has this job list:

```js
// Section shots — crisp 2x.
await shoot({ dsr: 2, viewport: { width: 1180, height: 1000 } }, [
  { fullViewport: true, file: "hero-light.png", dark: false },
  { fullViewport: true, file: "hero-dark.png", dark: true },
  { id: "#palette", file: "palette.png", dark: false },
  { id: "#components", file: "components.png", dark: false },
  { id: "#charts", file: "charts.png", dark: false },
  { id: "#registers", file: "type-registers.png", dark: false },
]);
```

Replace with:

```js
// Section shots — crisp 2x.
await shoot({ dsr: 2, viewport: { width: 1180, height: 1000 } }, [
  { fullViewport: true, file: "hero-light.png", dark: false },
  { fullViewport: true, file: "hero-dark.png", dark: true },
  { id: "#palette", file: "palette.png", dark: false },
  { id: "#components", file: "components.png", dark: false },
  { id: "#charts", file: "charts.png", dark: false },
  { id: "#registers", file: "type-registers.png", dark: false },
  { id: "#turquoise-moment", file: "turquoise-moment.png", dark: false },
  { id: "#closing-band", file: "closing-band.png", dark: false },
]);
```

- [ ] **Step 2: Regenerate all screenshots**

Run: `cd scripts/capture && npm install && node capture.mjs` (or, if
dependencies are already installed, just `node capture.mjs` from
`scripts/capture/`).

Expected output: `wrote hero-light.png`, `wrote hero-dark.png`,
`wrote palette.png`, `wrote components.png`, `wrote charts.png`,
`wrote type-registers.png`, `wrote turquoise-moment.png`,
`wrote closing-band.png`, `wrote social-card.png` — 9 lines, no errors.

- [ ] **Step 3: Visually confirm the new screenshots**

Read `docs/assets/components.png` and confirm it shows the Destructive
button, the Callout panel, and the two Default/Hover swatch pairs.

Read `docs/assets/turquoise-moment.png` and confirm it shows a
solid-Turquoise panel with black text.

Read `docs/assets/closing-band.png` and confirm it shows a
solid-Turquoise band with black centered text.

- [ ] **Step 4: Commit**

```bash
git add scripts/capture/capture.mjs docs/assets/
git commit -m "feat: regenerate screenshots to include the new turquoise blocks and hover demos"
```

---

### Task 6: `CHANGELOG.md`, `README.md`, `CONTRIBUTING.md` sync

**Files:**
- Modify: `CHANGELOG.md`
- Modify: `README.md`
- Modify: `CONTRIBUTING.md`

**Interfaces:**
- Consumes: `docs/assets/turquoise-moment.png` and
  `docs/assets/closing-band.png` (Task 5).

- [ ] **Step 1: Add the CHANGELOG entries**

`CHANGELOG.md`'s `[Unreleased]` → `### Added` list currently ends
(before the `### Changed` section) with:

```markdown
- **Philosophy-compliance verifier** (`scripts/capture/verify-philosophy.mjs`)
  — automated check that the showcase page never uses a rogue color, a
  shadow, an unrestyled icon cap, or leaks `--highlight` into a semantic
  UI role.

### Changed
```

Replace with:

```markdown
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
```

- [ ] **Step 2: Update the README aesthetic-summary paragraph**

`README.md`'s "The aesthetic" section currently reads:

```markdown
**Tri-tone, more colorful, still Swiss-minimalist.** Two structural colors
— ink (`#000000`) and warm cream (`#f5efe0`) — plus a Swiss Red accent
(`#d3281b`) that now also marks section dividers, selective card emphasis,
and a genuine Structural Block (a solid-color sidebar/hero band, capped at
~25% of viewport, or a bold word inside a heading), and a non-semantic
highlight, Pastel Turquoise (`#56bfa3`), used decoratively across icon
fills, underlines, washes, chart series, and a hover-triggered flourish on
nav links. No success green, no info blue, no second *semantic* accent —
the highlight never carries meaning, however often it recurs.
```

Replace with:

```markdown
**Tri-tone, more colorful, still Swiss-minimalist.** Two structural colors
— ink (`#000000`) and warm cream (`#f5efe0`) — plus a Swiss Red accent
(`#d3281b`) that now also marks section dividers, selective card emphasis,
a genuine Structural Block (a solid-color sidebar/hero band, capped at
~25% of viewport, or a bold word inside a heading), and hover-state
feedback wherever an accent signals interactivity, and a non-semantic
highlight, Pastel Turquoise (`#56bfa3`), used decoratively across icon
fills, underlines, washes, chart series, a hover-triggered flourish on
nav links, and now its own smaller Structural Block (a callout panel, a
second-moment panel, a closing band). The tri-part segment stripe — the
one place all three colors meet — is reusable at any length as a
decorative divider, not a one-off. No success green, no info blue, no
second *semantic* accent — the highlight never carries meaning, however
often it recurs.
```

- [ ] **Step 3: Add the two new screenshots to the "See it" gallery**

`README.md`'s "See it" section currently ends with:

```markdown
Four type registers and the component library:

![Type registers](docs/assets/type-registers.png)
![Component gallery](docs/assets/components.png)
![Charts](docs/assets/charts.png)

## What it does
```

Replace with:

```markdown
Four type registers and the component library:

![Type registers](docs/assets/type-registers.png)
![Component gallery](docs/assets/components.png)
![Charts](docs/assets/charts.png)

Turquoise's own Structural Block — a genuine third color, not an occasional accent:

![Turquoise second-moment panel](docs/assets/turquoise-moment.png)
![Turquoise closing band](docs/assets/closing-band.png)

## What it does
```

- [ ] **Step 4: Update the "Applying it to a project" highlight-usage sentence**

`README.md`'s step 3 currently reads:

```markdown
3. Build with the semantic tokens (`bg-background`, `text-foreground`,
   `border-border`, `bg-primary`, …) and the component patterns. Reach for
   `bg-highlight`/`text-highlight` decoratively — icon fills, underlines,
   washes, a chart's second series, a brand moment — never for a button,
   tag, or status color, and never to signal state.
```

Replace with:

```markdown
3. Build with the semantic tokens (`bg-background`, `text-foreground`,
   `border-border`, `bg-primary`, …) and the component patterns. Reach for
   `bg-highlight`/`text-highlight` decoratively — icon fills, underlines,
   washes, a chart's second series, a brand moment, a callout/panel/
   closing-band background (paired with `text-highlight-foreground`) —
   never for a button, tag, or status color, and never to signal state on
   its own in a hover.
```

- [ ] **Step 5: Update CONTRIBUTING.md's Design changes paragraph**

`CONTRIBUTING.md`'s "Design changes" section currently reads:

```markdown
The design language lives in `skills/tri-swiss/`. Keep the two governing
rules intact — **tri-tone, more colorful** (two structural colors + one
accent, Swiss Red, used more freely for action/divider/emphasis-border/
Structural-Block jobs + one non-semantic highlight, Pastel Turquoise, used
decoratively but never for state) and **Swiss-minimalist** (visible
borders, no shadows). Changes that add a *semantic* color (a third color
that means success/info/state) or a shadow contradict the system and won't
be accepted; express new states through weight, size, spacing, and
contrast instead. The one named exception is the tri-part segment stripe
(§ SKILL.md `--highlight` section) — a single governed device where Red and
Turquoise sit adjacent, not a general loosening.
```

Replace with:

```markdown
The design language lives in `skills/tri-swiss/`. Keep the two governing
rules intact — **tri-tone, more colorful** (two structural colors + one
accent, Swiss Red, used more freely for action/divider/emphasis-border/
Structural-Block/hover-signal jobs + one non-semantic highlight, Pastel
Turquoise, used decoratively and in its own smaller Structural Block, but
never for state) and **Swiss-minimalist** (visible borders, no shadows).
Changes that add a *semantic* color (a third color that means
success/info/state) or a shadow contradict the system and won't be
accepted; express new states through weight, size, spacing, and contrast
instead. The one named exception is the tri-part segment stripe (§
SKILL.md Philosophy section) — a single governed pattern where Red and
Turquoise sit adjacent, reusable at any length, not a general loosening.
```

- [ ] **Step 6: Verify**

Run: `node scripts/capture/verify-philosophy.mjs` from the repo root.
Expected: `PASS: philosophy compliance OK` (this task doesn't touch
`docs/index.html`, so it must already pass — this is a sanity check that
nothing upstream regressed).

Run: `claude plugin validate .` from the repo root.
Expected: passes (the same pre-existing, unrelated root-CLAUDE.md
warning from prior work is fine; no new errors).

- [ ] **Step 7: Commit**

```bash
git add CHANGELOG.md README.md CONTRIBUTING.md
git commit -m "docs: sync CHANGELOG, README, and CONTRIBUTING with the turquoise/hover work"
```
