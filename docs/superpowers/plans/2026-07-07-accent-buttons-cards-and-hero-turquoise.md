# Accent Buttons, Accent Cards, and Hero Turquoise Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add an Accent button and Accent card (more color on hover/borders, both systems' shared pattern), a Tri-Swiss-only Interactive card with a dual-accent hover pattern (a second named exception, this time to the hover-hierarchy rule), and three new turquoise touches in the hero so the tri-color identity registers on first page load.

**Architecture:** Pure documentation + static-page changes across `SKILL.md`, `references/components.md`, and `docs/index.html`. No new CSS tokens (Red and Turquoise already have their `-foreground` pairs), no new capture-job entries (every change lands inside the already-captured `#hero`/`#components` regions).

**Tech Stack:** Plain HTML/CSS (Tailwind 4 `@theme inline` tokens), Playwright (screenshot capture), Node.js (capture/verify scripts), Markdown.

## Global Constraints

- Ink, cream, Swiss Red, and Pastel Turquoise remain the only four color tokens — no fifth color introduced anywhere.
- Turquoise never carries semantic/state meaning outside the one named dual-accent hover exception (Accent button, Interactive card) — everywhere else, the existing rules (non-semantic, decorative-only) hold unchanged.
- The dual-accent hover pattern is **sequential** (Red at rest, Turquoise on hover) — the two colors are never visible on an element simultaneously. This does not touch the separate "Red and Turquoise never touch" adjacency guardrail (the tri-part stripe remains the only exception to *that* rule); it is a distinct, second named exception to the hover-hierarchy rule ("Red is the only color permitted to carry real hover-state meaning").
- The existing Ghost/Outlined/Filled/Destructive buttons and the existing plain Card/Emphasis card are unchanged — Accent button, Accent card, and Interactive card are additive new variants.
- `scripts/capture/verify-philosophy.mjs` must continue to print `PASS: philosophy compliance OK` after every task that touches `docs/index.html`. No new hex values are introduced (only `var(--primary)`, `var(--highlight)`, `var(--highlight-foreground)`, and `color-mix(in srgb, var(--highlight) N%, var(--card))` expressions, all already-sanctioned patterns in this file).
- No new capture-job entries are needed in `scripts/capture/capture.mjs` — confirm this repo's existing jobs (`hero-light`/`hero-dark` fullViewport, `#palette`, `#components`, `#charts`, `#registers`, `#turquoise-moment`, `#closing-band`, `#social-card`) already cover every section this plan touches before skipping that step.
- Every commit follows this repo's `AGENTS.md` Conventional Commits + changelog-first discipline; `CHANGELOG.md` entries land under the existing `[Unreleased]` → new `### Added` section (this repo tagged `v0.1.0` already, so `[Unreleased]` is currently empty — see Task 6).

---

### Task 1: `SKILL.md` — Accent button, dual-accent hover exception, Do-not fixes

**Files:**
- Modify: `skills/tri-swiss/SKILL.md`

**Interfaces:**
- Produces: the documented Accent button variant and the dual-accent hover exception rule text, which Task 2 (components.md) and Task 3 (docs/index.html) implement. The exact class names `.btn-accent` and `.card-interactive` are introduced in Task 3, not here — this task only documents the Tailwind-style utility strings (`border-primary`, `hover:bg-highlight`, etc.), consistent with how the existing Destructive button is documented.

- [ ] **Step 1: Add item 8 to the `--highlight` token's decorative-use list**

The numbered list in the `--highlight` token section currently ends
(around lines 167–171) with:

```
7. A hover-triggered flourish on a nav link or label — an underline or dot
   that appears on `:hover`, purely ornamental and identical regardless of
   active/current/visited state, layered *alongside* the element's
   existing ink/muted-foreground hover color change (which still carries
   the real interactive feedback).
```

Add item 8 directly after it:

```
7. A hover-triggered flourish on a nav link or label — an underline or dot
   that appears on `:hover`, purely ornamental and identical regardless of
   active/current/visited state, layered *alongside* the element's
   existing ink/muted-foreground hover color change (which still carries
   the real interactive feedback).
8. A card's border plus a subtle background wash (the Accent card) —
   still purely decorative, not a semantic "this card matters more" cue.
```

- [ ] **Step 2: Add the Accent button variant to `## Buttons`**

The `## Buttons` section currently reads (around lines 254–270):

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

Replace with:

```
## Buttons

All buttons: `font-mono uppercase tracking-[0.2em] text-xs`. Five variants:

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
- **Accent:** `border border-primary text-primary px-4 py-2
  hover:bg-highlight hover:text-highlight-foreground hover:border-highlight`
  — general emphasis, not destructive. Red border at rest, Turquoise on
  hover: the dual-accent hover exception (see "Hover states" below), not
  a decorative flourish.

**Disabled** is always `opacity-40` — never a color change.
```

- [ ] **Step 3: Add the dual-accent hover exception to `## Hover states`**

The `## Hover states` section currently reads in full (around lines
272–282):

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

Replace with:

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

**The one named exception: dual-accent hover.** The Accent button above
and the Interactive card (see `references/components.md`) swap Red for
Turquoise on hover — Red border at rest, Turquoise border/fill on hover.
This is a genuine, deliberate exception to "Red is the only color
permitted to carry real hover meaning," scoped to exactly these two
patterns. The two colors are never visible on the element simultaneously
(a state transition, not spatial adjacency), so it doesn't touch the
separate "Red and Turquoise never touch" guardrail — but it is a real
carve-out from this hover hierarchy, not a decorative layer, and must
not spread beyond these two named patterns.
```

- [ ] **Step 4: Fix the two contradicting `## Do not` bullets**

The `## Do not` section currently has these two bullets (around lines
355–358):

```
- **No Turquoise on buttons, tags, status pips, or links.** Decorative
  reuse elsewhere is fine; semantic/interactive roles are not.
- **No red-and-turquoise on the same element.** Pick one accent per
  component, never both.
```

Replace with:

```
- **No Turquoise on buttons, tags, status pips, or links as their
  default/rest color.** Decorative reuse elsewhere is fine;
  semantic/interactive roles are not — except the Accent button's and
  Interactive card's dual-accent hover state (see "Hover states"), the
  one named exception where Turquoise carries real hover meaning.
- **No red-and-turquoise simultaneously on the same element.** Pick one
  accent per component at any single moment — the tri-part stripe
  (spatial) and the dual-accent hover pattern (sequential) are the two
  named exceptions, not a general loosening.
```

- [ ] **Step 5: Verify**

Run: `node scripts/capture/verify-philosophy.mjs` from the repo root.
Expected: `PASS: philosophy compliance OK` (this task doesn't touch
`docs/index.html`, so it must still pass unchanged).

Run: `rtk grep -n "Accent:\|dual-accent\|Accent card" skills/tri-swiss/SKILL.md`
Expected: matches for the new Accent button bullet, the Hover states
exception paragraph, and the two updated Do-not bullets.

- [ ] **Step 6: Commit**

```bash
git add skills/tri-swiss/SKILL.md
git commit -m "feat: add Accent button and dual-accent hover exception to SKILL.md"
```

---

### Task 2: `references/components.md` — Accent button, Accent card, Interactive card

**Files:**
- Modify: `skills/tri-swiss/references/components.md`

**Interfaces:**
- Consumes: the rule text from Task 1.
- Produces: the full HTML patterns Task 3 implements on the live page.

- [ ] **Step 1: Update the "Turquoise decorative accents" cross-reference**

The current text reads (around lines 149–153):

```
**Turquoise decorative accents.** Non-semantic, ornamental only — never as
a button's, tag's, status pip's, or link's own state color (see the
"Structural Block" section below for the one sanctioned exception: a
purely decorative hover-flourish on nav links, layered alongside — never
replacing — the link's real state feedback):
```

Replace with:

```
**Turquoise decorative accents.** Non-semantic, ornamental only — never as
a button's, tag's, status pip's, or link's own default/rest state color
(see the "Structural Block" section below for the nav-link
hover-flourish, and "Cards" below for the Accent button / Interactive
card — the sanctioned exceptions where Turquoise carries real hover
meaning, not decoration):
```

- [ ] **Step 2: Add the Accent button and Interactive card Default/Hover swatch patterns**

The file currently has this content right after the "Nav-link hover,
Default vs. Hover" pattern, immediately before `## Iconography` (around
lines 317–328):

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

Insert a new pattern between the nav-link swatch and `## Iconography`:

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

**Accent button hover, Default vs. Hover — the dual-accent exception.**
Red border at rest, Turquoise fill on hover — the one named exception
where Turquoise carries real hover meaning:

```html
<button style="border:1px solid var(--primary); background:none;
  color:var(--primary); padding:8px 16px;">Learn more</button>
<!-- :hover (or a static .is-hover-demo modifier class for illustration) -->
<button style="border:1px solid var(--highlight); background:var(--highlight);
  color:var(--highlight-foreground); padding:8px 16px;">Learn more</button>
```

## Iconography (geist-icons, restyled)
```

- [ ] **Step 3: Add the Accent card and Interactive card patterns to `## Cards`**

The `## Cards` section currently reads in full (at the end of the file):

```
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

Replace with:

```
## Cards

Elevation is a background step, never a shadow.

```
border border-border bg-card text-card-foreground p-6
```

Nest a section divider (see `SKILL.md`) inside for titled regions. Keep corners
square unless a `rounded-md` (0.5rem) genuinely helps. For the one emphasized
card in a set, see the selective-border pattern under "Accent expansion"
above — swap `border-border` for `border-primary`.

**Accent card.** A static, Turquoise-bordered card with a subtle
background wash — stronger than the plain Emphasis card (border alone,
no wash), and non-semantic like every other Turquoise decorative reuse:

```jsx
<div className="border border-highlight p-6" style={{ background: "color-mix(in srgb, var(--highlight) 8%, var(--card))" }}>
  <p className="font-mono font-bold">Accent card</p>
  <p className="text-sm text-muted-foreground">
    Turquoise border + wash — decorative, not a state indicator.
  </p>
</div>
```

**Interactive card.** A clickable card (wrap in `<a>` or `<button>`) with
its own hover transition. Red border at rest, Turquoise border + wash on
hover — the dual-accent hover exception (see `SKILL.md`'s "Hover
states"), applied to a card instead of a button:

```html
<a href="#" style="display:block; text-decoration:none; color:inherit;
  border:1px solid var(--primary); padding:24px;
  transition:border-color 0.15s, background-color 0.15s;">
  <p style="margin:0; font-family:var(--font-mono); font-weight:700;">Interactive card</p>
  <p style="margin:8px 0 0; font-size:0.85rem; color:var(--muted-foreground);">
    Red border at rest, Turquoise on hover — click anywhere on the card.
  </p>
</a>
<!-- :hover (or a static .is-hover-demo modifier class for illustration) -->
<a href="#" style="display:block; text-decoration:none; color:inherit;
  border:1px solid var(--highlight);
  background:color-mix(in srgb, var(--highlight) 5%, var(--card)); padding:24px;">
  <p style="margin:0; font-family:var(--font-mono); font-weight:700;">Interactive card</p>
  <p style="margin:8px 0 0; font-size:0.85rem; color:var(--muted-foreground);">
    Red border at rest, Turquoise on hover — click anywhere on the card.
  </p>
</a>
```
```

- [ ] **Step 4: Verify**

Run: `node scripts/capture/verify-philosophy.mjs` from the repo root.
Expected: `PASS: philosophy compliance OK`.

Run: `rtk grep -n "Accent card\|Interactive card\|Accent button hover" skills/tri-swiss/references/components.md`
Expected: matches for all three new pattern headings.

- [ ] **Step 5: Commit**

```bash
git add skills/tri-swiss/references/components.md
git commit -m "docs: add Accent button, Accent card, and Interactive card patterns"
```

---

### Task 3: `docs/index.html` — CSS and Components-section demos

**Files:**
- Modify: `docs/index.html`

**Interfaces:**
- Consumes: the patterns from Task 2.
- Produces: the `.btn-accent` and `.card-interactive` CSS classes (with
  `:hover`/`.is-hover-demo` rules). Task 4 (hero) consumes `.btn-accent`
  on the hero's "View on GitHub" link — do not rename these classes.

- [ ] **Step 1: Add the `.btn-accent` and `.card-interactive` CSS rules**

The `<style>` block currently has this section (around lines 80–93):

```css
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

Insert the new rules between the `.demo-nav-link:hover` rule and the
`/* Structural Block */` comment:

```css
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

    /* Dual-accent hover exception — Red at rest, Turquoise on hover.
       The one named exception to "Turquoise never signals on its own." */
    .btn-accent { border:1px solid var(--primary); background:none; color:var(--primary);
      transition:background-color 0.15s, color 0.15s, border-color 0.15s; }
    .btn-accent:hover, .btn-accent.is-hover-demo { background:var(--highlight);
      color:var(--highlight-foreground); border-color:var(--highlight); }
    .card-interactive { display:block; text-decoration:none; color:inherit;
      border:1px solid var(--primary); transition:border-color 0.15s, background-color 0.15s; }
    .card-interactive:hover, .card-interactive.is-hover-demo { border-color:var(--highlight);
      background:color-mix(in srgb, var(--highlight) 5%, var(--card)); }

    /* Structural Block — sidebar layout */
```

- [ ] **Step 2: Add the Accent button to the existing Buttons demo card**

The Buttons demo card currently reads (around lines 380–395):

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
              <button class="btn-accent" style="padding:8px 14px; font:inherit;
                letter-spacing:inherit; text-transform:inherit;">Accent</button>
            </div>
          </div>
```

- [ ] **Step 3: Add the Accent card demo after the Emphasis card demo**

The Emphasis card demo currently reads (around lines 437–444):

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
```

Replace with:

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
            <p class="label" style="margin-bottom:16px;">Accent card <span style="text-transform:none; letter-spacing:normal; color:var(--muted-foreground);">— Turquoise border + wash</span></p>
            <div style="border:1px solid var(--highlight); background:color-mix(in srgb, var(--highlight) 8%, var(--card)); padding:16px;">
              <p style="margin:0; font-family:var(--font-mono); font-weight:700;">Accent card</p>
              <p style="margin:8px 0 0; font-size:0.85rem; color:var(--muted-foreground);">
                Turquoise border + wash — decorative, not a state indicator.</p>
            </div>
          </div>

          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Decorative accent <span style="text-transform:none; letter-spacing:normal; color:var(--muted-foreground);">— non-semantic</span></p>
```

- [ ] **Step 4: Add the Interactive card demo and the Accent-button Default/Hover swatch pair**

The Components grid currently ends (around lines 500–514) with:

```html
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

Replace with:

```html
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

          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Hover — accent button <span style="text-transform:none; letter-spacing:normal; color:var(--muted-foreground);">Red at rest, Turquoise on hover</span></p>
            <div style="display:flex; gap:24px; font-family:var(--font-mono);
                        text-transform:uppercase; letter-spacing:0.2em; font-size:0.7rem;">
              <div style="display:flex; flex-direction:column; gap:8px; align-items:flex-start;">
                <span style="font-size:0.65rem; letter-spacing:0.12em; color:var(--muted-foreground);">Default</span>
                <button class="btn-accent" style="padding:8px 16px; font:inherit; letter-spacing:inherit; text-transform:inherit;">Learn more</button>
              </div>
              <div style="display:flex; flex-direction:column; gap:8px; align-items:flex-start;">
                <span style="font-size:0.65rem; letter-spacing:0.12em; color:var(--muted-foreground);">Hover</span>
                <button class="btn-accent is-hover-demo" style="padding:8px 16px; font:inherit; letter-spacing:inherit; text-transform:inherit;">Learn more</button>
              </div>
            </div>
          </div>

          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Interactive card <span style="text-transform:none; letter-spacing:normal; color:var(--muted-foreground);">— dual-accent hover, hover to try</span></p>
            <a href="#components" class="card-interactive" style="padding:16px;" onclick="return false;">
              <p style="margin:0; font-family:var(--font-mono); font-weight:700;">Click anywhere</p>
              <p style="margin:8px 0 0; font-size:0.85rem; color:var(--muted-foreground);">
                Red border at rest, Turquoise on hover.</p>
            </a>
          </div>

        </div>
      </section>
```

- [ ] **Step 5: Verify**

Run: `node scripts/capture/verify-philosophy.mjs` from the repo root.
Expected: `PASS: philosophy compliance OK`.

Run: `rtk grep -n "btn-accent\|card-interactive" docs/index.html`
Expected: matches for the CSS class definitions plus 3 usage sites
(Buttons demo, hover swatch pair ×2 for `.btn-accent`; Interactive card
demo for `.card-interactive`).

- [ ] **Step 6: Commit**

```bash
git add docs/index.html
git commit -m "feat: add accent button, accent card, and interactive card demos"
```

---

### Task 4: `docs/index.html` — Hero turquoise additions and rules-section accuracy fix

**Files:**
- Modify: `docs/index.html`

**Interfaces:**
- Consumes: `.btn-accent` (Task 3) for the hero's "View on GitHub" CTA.

- [ ] **Step 1: Add the turquoise tagline and icon accent, and swap the hero's second CTA to `.btn-accent`**

The `#hero` section currently reads in full (around lines 175–201):

```html
      <section id="hero" style="border-top:none;">
        <p class="label">Claude Code plugin</p>
        <h1 class="hero-title">Tri-Swiss.</h1>
        <div style="display:flex; gap:4px; width:64px; margin-top:16px;" aria-hidden="true">
          <div style="height:3px; flex:1; background:var(--foreground);"></div>
          <div style="height:3px; flex:1; background:var(--primary);"></div>
          <div style="height:3px; flex:1; background:var(--highlight);"></div>
        </div>
        <p style="font-size:1.15rem; max-width:60ch; margin:24px 0 0; color:var(--foreground);">
          Lux Solari's Geist-based house design language — Swiss-minimalist,
          tri-tone, more colorful. Two structural colors, one strong
          <span style="color:var(--primary);">accent</span>, one governed
          highlight; everything else is weight, space, and contrast.
        </p>
        <div style="display:flex; gap:16px; margin-top:32px; font-family:var(--font-mono);
                    text-transform:uppercase; letter-spacing:0.2em; font-size:0.75rem;">
          <a href="#components" style="border:1px solid var(--foreground); background:var(--foreground);
             color:var(--background); padding:12px 20px; text-decoration:none;">See the components</a>
          <a href="https://github.com/luxsolari/tri-swiss"
             style="border:1px solid var(--foreground); padding:12px 20px; text-decoration:none;
             display:inline-flex; align-items:center; gap:8px;">
            View on GitHub
            <svg class="icon" viewBox="0 0 24 24"><!-- geist-icons arrow-up-right -->
              <path d="M7 17 17 7"/><path d="M7 7h10v10"/></svg>
          </a>
        </div>
      </section>
```

Replace with:

```html
      <section id="hero" style="border-top:none;">
        <p class="label">Claude Code plugin</p>
        <h1 class="hero-title">Tri-Swiss.</h1>
        <div style="display:flex; align-items:center; gap:8px; margin-top:12px;">
          <svg class="icon" viewBox="0 0 24 24" width="18" height="18" style="color:var(--highlight);" aria-hidden="true"><circle cx="12" cy="12" r="9"/></svg>
          <p style="font-family:var(--font-mono); font-weight:700; font-size:1.5rem; margin:0; color:var(--highlight);">A genuine third color.</p>
        </div>
        <div style="display:flex; gap:4px; width:64px; margin-top:16px;" aria-hidden="true">
          <div style="height:3px; flex:1; background:var(--foreground);"></div>
          <div style="height:3px; flex:1; background:var(--primary);"></div>
          <div style="height:3px; flex:1; background:var(--highlight);"></div>
        </div>
        <p style="font-size:1.15rem; max-width:60ch; margin:24px 0 0; color:var(--foreground);">
          Lux Solari's Geist-based house design language — Swiss-minimalist,
          tri-tone, more colorful. Two structural colors, one strong
          <span style="color:var(--primary);">accent</span>, one governed
          highlight; everything else is weight, space, and contrast.
        </p>
        <div style="display:flex; gap:16px; margin-top:32px; font-family:var(--font-mono);
                    text-transform:uppercase; letter-spacing:0.2em; font-size:0.75rem;">
          <a href="#components" style="border:1px solid var(--foreground); background:var(--foreground);
             color:var(--background); padding:12px 20px; text-decoration:none;">See the components</a>
          <a href="https://github.com/luxsolari/tri-swiss" class="btn-accent"
             style="padding:12px 20px; text-decoration:none; display:inline-flex; align-items:center;
             gap:8px; font:inherit; letter-spacing:inherit; text-transform:inherit;">
            View on GitHub
            <svg class="icon" viewBox="0 0 24 24"><!-- geist-icons arrow-up-right -->
              <path d="M7 17 17 7"/><path d="M7 7h10v10"/></svg>
          </a>
        </div>
      </section>
```

- [ ] **Step 2: Fix the rules-section "01" paragraph's now-inaccurate claim**

The `#rules` section's first rule card currently reads (around lines
220–231):

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

Replace with:

```html
          <div style="border:1px solid var(--border); background:var(--card); padding:24px;">
            <p class="label">01 — Tri-tone, more colorful</p>
            <p style="margin:12px 0 0;">Ink and cream still lead. Swiss Red is the primary
              accent — action, destructive, focus, hover signal — and now also marks
              section dividers, a selectively emphasized card border, and a genuine
              Structural Block. Pastel Turquoise stays non-semantic but recurs as
              pure decoration and now has a Structural Block of its own, smaller in
              scale — icon fills, underlines, washes, dots, callout panels, an
              accent card's border. Two named exceptions aside — the tri-part
              segment stripe (spatial) and a dual-accent hover swap on the Accent
              button and Interactive card (sequential, never simultaneous) —
              neither color carries state meaning nor touches the other.</p>
          </div>
```

- [ ] **Step 3: Verify**

Run: `node scripts/capture/verify-philosophy.mjs` from the repo root.
Expected: `PASS: philosophy compliance OK`.

Run: `rtk grep -n "A genuine third color\|dual-accent hover swap" docs/index.html`
Expected: one match each.

- [ ] **Step 4: Commit**

```bash
git add docs/index.html
git commit -m "feat: add hero turquoise tagline/icon, swap hero CTA to accent button"
```

---

### Task 5: Screenshot regeneration

**Files:**
- Modify: `docs/assets/*.png` (regenerated, not hand-edited)

**Interfaces:**
- Consumes: the final page state from Tasks 3–4.

- [ ] **Step 1: Confirm no new capture-job entries are needed**

Read `scripts/capture/capture.mjs` and confirm its existing jobs
(`hero-light`/`hero-dark` fullViewport, `#palette`, `#components`,
`#charts`, `#registers`, `#turquoise-moment`, `#closing-band`,
`#social-card`) already cover every element this plan changed — they
do: the hero changes are inside the fullViewport hero jobs, and the new
Components-section demos are inside the existing `#components` job.

- [ ] **Step 2: Regenerate all screenshots**

Run: `cd scripts/capture && node capture.mjs` (run `npm install` first
if `node_modules` isn't already present).

Expected output: 9 `wrote <file>.png` lines, no errors.

- [ ] **Step 3: Visually confirm the changes**

Read (view as an image) `docs/assets/hero-light.png` and confirm it
shows: the turquoise tagline "A genuine third color." with its icon
beneath the hero title, and the "View on GitHub" button now
Red-bordered (its default/rest state — hover isn't captured in a static
screenshot, that's expected).

Read `docs/assets/components.png` and confirm it shows: an "Accent"
button in the Buttons card (Red border, matching Destructive's rest
style), a new "Accent card" (Turquoise border + a subtle tinted
background, visually distinct from the plain Emphasis card next to it),
a new "Interactive card" demo, and a new "Hover — accent button"
Default/Hover swatch pair (Default: Red border; Hover: solid Turquoise
fill with dark text).

- [ ] **Step 4: Commit**

```bash
git add docs/assets/
git commit -m "feat: regenerate screenshots for accent buttons/cards and hero turquoise"
```

---

### Task 6: `CHANGELOG.md`, `README.md`, `CONTRIBUTING.md` sync

**Files:**
- Modify: `CHANGELOG.md`
- Modify: `README.md`
- Modify: `CONTRIBUTING.md`

**Interfaces:**
- None — this is the final, documentation-only task.

- [ ] **Step 1: Add the CHANGELOG entry**

This repo is tagged `v0.1.0`, so `CHANGELOG.md`'s `[Unreleased]` section
is currently empty. It reads:

```markdown
## [Unreleased]

## [0.1.0] — 2026-07-07
```

Replace with:

```markdown
## [Unreleased]

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

## [0.1.0] — 2026-07-07
```

- [ ] **Step 2: Update README.md's aesthetic-summary paragraph**

`README.md`'s "## The aesthetic" section currently reads:

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
decorative divider, not a one-off. A new Accent button and Accent card
put more color on hover/borders, and a second named exception — a
dual-accent hover swap on the Accent button and a new Interactive card —
lets Turquoise carry real hover meaning in exactly those two places. No
success green, no info blue, no second *semantic* accent otherwise — the
highlight never carries meaning outside these two named exceptions,
however often it recurs.
```

- [ ] **Step 3: Update CONTRIBUTING.md's Design-changes paragraph**

`CONTRIBUTING.md`'s "Design changes" section currently reads:

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
instead. Two named exceptions: the tri-part segment stripe (§ SKILL.md
Philosophy section) — Red and Turquoise sit spatially adjacent, reusable
at any length — and the dual-accent hover pattern on the Accent button
and Interactive card (§ SKILL.md "Hover states") — Red and Turquoise
swap sequentially on hover, never simultaneously. Neither is a general
loosening; don't extend either beyond its named pattern.
```

- [ ] **Step 4: Verify**

Run: `node scripts/capture/verify-philosophy.mjs` from the repo root.
Expected: `PASS: philosophy compliance OK` (this task doesn't touch
`docs/index.html`, so this is a sanity check that nothing upstream
regressed).

Run: `claude plugin validate .` from the repo root.
Expected: `✔ Validation passed with warnings`, with exactly one
pre-existing, unrelated warning about `CLAUDE.md` at the plugin root
not being loaded as project context — confirm no *new* errors or
warnings appear.

- [ ] **Step 5: Commit**

```bash
git add CHANGELOG.md README.md CONTRIBUTING.md
git commit -m "docs: sync CHANGELOG, README, and CONTRIBUTING with the accent-buttons work"
```
