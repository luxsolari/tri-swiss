# Showcase Content Expansion Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add Lists, Tables, and Images-in-grid as new documented component patterns, and richen the showcase page's typography demonstration (fuller per-register examples plus a new text-length comparison) so the page and its reference docs serve as a genuine implementation reference.

**Architecture:** Pure documentation + static-page changes across `SKILL.md`, `references/components.md`, and `docs/index.html`, plus two new `scripts/capture/capture.mjs` job entries for the two new page sections. No new color tokens, no navigation restructuring — new content lands inside or alongside the existing single-page-scroll flow.

**Tech Stack:** Plain HTML/CSS (Tailwind 4 `@theme inline` tokens), Playwright (screenshot capture), Node.js (capture/verify scripts), Markdown.

## Global Constraints

- Ink, cream, Swiss Red, and Pastel Turquoise remain the only four color tokens — no fifth color introduced anywhere.
- List markers/dividers and table borders/headers use ink/muted-foreground only — **never** Red or Turquoise. This is a hard rule (see spec §3), not a style preference.
- Images default to a grayscale/duotone filter; full color is permitted **only** when the image itself is the primary content (blog photography, portfolio gallery, product photography) — a scoped, named exception, not a general license (see spec §4).
- No new rules for the four type registers — `SKILL.md`'s `## Typography` section is already complete and correct. The `#registers` section's richer examples and the new text-length section are content-only changes, no new prose rules.
- `scripts/capture/verify-philosophy.mjs` must continue to print `PASS: philosophy compliance OK` after every task that touches `docs/index.html`. It hard-fails on ANY hex color or `rgb()`/`rgba()` literal outside the exact token palette — the images-in-grid demo must use only `var(--primary)`, `var(--highlight)`, `var(--foreground)` etc. in `linear-gradient()` expressions, never a literal color or an `<img src>` pointing at a real photo.
- No page navigation/TOC restructuring — the existing sidebar nav list (`Palette`, `Typography`, `Components`, `Charts`) is unchanged; new sections (`#images`, `#text-length`) do NOT get their own nav link, consistent with `#registers` not having one either.
- Every commit follows this repo's `AGENTS.md` Conventional Commits + changelog-first discipline; `CHANGELOG.md` entries land under the existing `[Unreleased]` → new `### Added` section.

---

### Task 1: `SKILL.md` — Lists/tables and Images rules

**Files:**
- Modify: `skills/tri-swiss/SKILL.md`

**Interfaces:**
- Produces: the documented rules for Lists, Tables, and Images that Task 2 (components.md) and Task 3 (docs/index.html) implement.

- [ ] **Step 1: Insert `## Lists / tables` and `## Images` sections**

The file currently reads (around lines 304–317), ending with the `## Tags / pills` section right before `## Iconography`:

```
## Tags / pills

Small inline badges: `font-mono text-[0.65rem] px-2 py-0.5 uppercase
tracking-[0.12em]`.

- **Neutral:** plain text, `text-muted-foreground`.
- **Signal** (latest / highlight): `bg-foreground text-background`.
- **Outlined:** `border border-dashed border-foreground/50 text-foreground/80`, 4px
  dot prefix.
- **Saved:** `border border-foreground/30 text-foreground/50`, 4px dot prefix.

`rounded-full` is reserved for dot indicators only — never on containers or pills.

## Iconography
```

Replace with:

```
## Tags / pills

Small inline badges: `font-mono text-[0.65rem] px-2 py-0.5 uppercase
tracking-[0.12em]`.

- **Neutral:** plain text, `text-muted-foreground`.
- **Signal** (latest / highlight): `bg-foreground text-background`.
- **Outlined:** `border border-dashed border-foreground/50 text-foreground/80`, 4px
  dot prefix.
- **Saved:** `border border-foreground/30 text-foreground/50`, 4px dot prefix.

`rounded-full` is reserved for dot indicators only — never on containers or pills.

## Lists / tables

**Lists.** No default round bullet glyphs. Unordered list items get a
thin top-border divider between rows (`border-top:1px solid
var(--border)`) instead of a bullet mark. Ordered list items use
tabular mono-font numbers (`font-mono`, matching the existing "Tabular
figures" pattern) followed by body-font (`font-sans`) item text.

**Tables.** Bold mono-font (`font-mono`) header row with a 2px bottom
border (matching the `.rule`/divider style used throughout this page),
1px `border-border` between body rows, `tabular-nums` right-aligned for
numeric columns. No zebra striping — kept consistent with the existing
"no invented decoration" pattern.

**Guardrail: markers and borders stay neutral.** List dividers and
table borders/headers use ink/muted-foreground only — **never** Red or
Turquoise. Using an accent color as a decorative list/table marker
would be a new, unsanctioned use of colors reserved for their named
jobs (action, emphasis, hover, Structural Block). This is a hard rule,
not a style preference.

## Images

Neither the theme nor the component library had an opinion on images
until now.

**Grid placement.** An image sits inside a bordered container (1px
`border-border`, matching the existing card border style) spanning a
defined number of grid columns, at a consistent aspect ratio (e.g. 4:3
or 16:9) rather than an arbitrary crop, with a mono-label caption
beneath it (reusing the existing `.label`/`.annotation` caption
convention already used elsewhere on the page).

**Color treatment.** The **default and recommended** treatment is a
grayscale or duotone filter (`filter: grayscale(1)` or a duotone
technique mapped toward ink+cream) — this keeps the "only four color
tokens, ever" invariant airtight and matches the historical
Swiss/International Typographic Style tradition of black-and-white
photography. **Full color is permitted specifically when the image
itself is the primary content** — e.g. a blog post's photography, a
portfolio gallery, product photography — not as a general license for
decorative images sprinkled through UI chrome. This is a scoped, named
exception (like the tri-part stripe or the dual-accent hover pattern),
not an open "designer's choice."

## Iconography
```

- [ ] **Step 2: Add the two new `Do not` bullets**

The `## Do not` section currently has these two bullets right before the
closing "Ink/cream still dominate" bullet (around lines 380–385):

```
- **No red-and-turquoise simultaneously on the same element.** Pick one
  accent per component at any single moment — the tri-part stripe
  (spatial) and the dual-accent hover pattern (sequential) are the two
  named exceptions, not a general loosening.
- **Ink/cream still dominate.** Accents are seasoning; a surface where
  Red or Turquoise out-covers ink/cream has gone too far.
```

Replace with:

```
- **No red-and-turquoise simultaneously on the same element.** Pick one
  accent per component at any single moment — the tri-part stripe
  (spatial) and the dual-accent hover pattern (sequential) are the two
  named exceptions, not a general loosening.
- **No accent-colored list markers or table borders.** Dividers,
  numbers, and header rules stay ink/muted-foreground — Red and
  Turquoise are reserved for their named jobs, not decoration in a list
  or table.
- **No full-color images outside the named photography-content
  exception.** Default to grayscale/duotone; full color is only for
  images that are themselves the primary content (see "Images").
- **Ink/cream still dominate.** Accents are seasoning; a surface where
  Red or Turquoise out-covers ink/cream has gone too far.
```

- [ ] **Step 3: Verify**

Run: `node scripts/capture/verify-philosophy.mjs` from the repo root.
Expected: `PASS: philosophy compliance OK` (this task doesn't touch
`docs/index.html`, so it must still pass unchanged).

Run: `rtk grep -n "## Lists / tables\|## Images\|accent-colored list markers\|full-color images" skills/tri-swiss/SKILL.md`
Expected: matches for both new section headings and both new Do-not bullets.

- [ ] **Step 4: Commit**

```bash
git add skills/tri-swiss/SKILL.md
git commit -m "feat: add Lists/tables and Images rules to SKILL.md"
```

---

### Task 2: `references/components.md` — List, table, and image-in-grid patterns

**Files:**
- Modify: `skills/tri-swiss/references/components.md`

**Interfaces:**
- Consumes: the rule text from Task 1.
- Produces: the full HTML patterns Task 3 implements on the live page.

- [ ] **Step 1: Append new `## Lists / tables` and `## Images` sections after `## Inputs`**

The file currently ends (the last section) with:

```
## Inputs

```
border border-border bg-input px-3 py-2 font-mono text-sm
placeholder: text-muted-foreground
focus: outline-none ring-1 ring-ring
```

Labels above inputs use the uppercase mono label pattern from `SKILL.md`.
```

Replace with:

```
## Inputs

```
border border-border bg-input px-3 py-2 font-mono text-sm
placeholder: text-muted-foreground
focus: outline-none ring-1 ring-ring
```

Labels above inputs use the uppercase mono label pattern from `SKILL.md`.

## Lists / tables

**Unordered list.** A thin top-border divider between rows, no bullet
glyph, ink/muted-foreground only:

```jsx
<ul className="list-none m-0 p-0">
  <li className="border-t border-border py-3 first:border-t-0">First item</li>
  <li className="border-t border-border py-3">Second item</li>
  <li className="border-t border-border py-3">Third item</li>
</ul>
```

**Ordered list.** Tabular mono-font numbers followed by body-font item
text:

```jsx
<ol className="list-none m-0 p-0">
  <li className="border-t border-border py-3 first:border-t-0 flex gap-3">
    <span className="font-mono tabular-nums text-muted-foreground">01</span>
    <span>First step</span>
  </li>
  <li className="border-t border-border py-3 flex gap-3">
    <span className="font-mono tabular-nums text-muted-foreground">02</span>
    <span>Second step</span>
  </li>
</ol>
```

**Table.** Bold mono header row with a 2px bottom border, 1px rows,
tabular-nums right-aligned numeric columns, no zebra striping:

```jsx
<table className="w-full border-collapse text-sm">
  <thead>
    <tr className="font-mono font-bold uppercase text-xs tracking-[0.1em] border-b-2 border-border">
      <th className="text-left py-2">Metric</th>
      <th className="text-right py-2">Value</th>
    </tr>
  </thead>
  <tbody>
    <tr className="border-b border-border">
      <td className="py-2">Sessions</td>
      <td className="py-2 text-right tabular-nums">1,209,384</td>
    </tr>
    <tr className="border-b border-border">
      <td className="py-2">Conversions</td>
      <td className="py-2 text-right tabular-nums">18,050</td>
    </tr>
  </tbody>
</table>
```

Markers, dividers, and header rules stay ink/muted-foreground only —
never Red or Turquoise (see `SKILL.md`'s "Lists / tables" guardrail).

## Images

**Grid placement.** A bordered container at a consistent aspect ratio,
with a mono-label caption beneath:

```jsx
<figure className="m-0 border border-border" style={{ aspectRatio: "4 / 3" }}>
  <img src="/photo.jpg" alt=""
    style={{ width: "100%", height: "100%", objectFit: "cover", filter: "grayscale(1)" }} />
  <figcaption className="font-annotation italic text-sm text-muted-foreground p-2 border-t border-border">
    Fig. 1 — grayscale, the default treatment.
  </figcaption>
</figure>
```

**Color treatment.** Default is grayscale/duotone (above). Full color is
a named, scoped exception — only when the photograph itself is the
primary content (blog photography, portfolio gallery, product shots),
never as decoration:

```jsx
<figure className="m-0 border border-border" style={{ aspectRatio: "4 / 3" }}>
  <img src="/photo.jpg" alt=""
    style={{ width: "100%", height: "100%", objectFit: "cover" }} />
  <figcaption className="font-annotation italic text-sm text-muted-foreground p-2 border-t border-border">
    Fig. 2 — full color, because the photograph itself is the content.
  </figcaption>
</figure>
```
```

- [ ] **Step 2: Verify**

Run: `node scripts/capture/verify-philosophy.mjs` from the repo root.
Expected: `PASS: philosophy compliance OK`.

Run: `rtk grep -n "## Lists / tables\|## Images\|Unordered list\|primary content" skills/tri-swiss/references/components.md`
Expected: matches for both new section headings and the list/image patterns.

- [ ] **Step 3: Commit**

```bash
git add skills/tri-swiss/references/components.md
git commit -m "docs: add List, table, and image-in-grid patterns"
```

---

### Task 3: `docs/index.html` — CSS, Lists/Tables demo, and new Images section

**Files:**
- Modify: `docs/index.html`

**Interfaces:**
- Consumes: the patterns from Task 2.
- Produces: `.list-plain`, `table.data-table`, and `figure.img-grid` CSS classes, and a new `<section id="images">`.

- [ ] **Step 1: Add the list/table/image CSS rules**

The `<style>` block currently has this section (around lines 100–104), right before the `/* Structural Block */` comment:

```css
    .card-interactive { display:block; text-decoration:none; color:inherit;
      border:1px solid var(--primary); transition:border-color 0.15s, background-color 0.15s; }
    .card-interactive:hover, .card-interactive.is-hover-demo { border-color:var(--highlight);
      background:color-mix(in srgb, var(--highlight) 5%, var(--card)); }

    /* Structural Block — sidebar layout */
```

Insert the new rules between `.card-interactive:hover` and the
`/* Structural Block */` comment:

```css
    .card-interactive { display:block; text-decoration:none; color:inherit;
      border:1px solid var(--primary); transition:border-color 0.15s, background-color 0.15s; }
    .card-interactive:hover, .card-interactive.is-hover-demo { border-color:var(--highlight);
      background:color-mix(in srgb, var(--highlight) 5%, var(--card)); }

    /* Lists, tables, images — new content components. Markers/borders
       stay neutral (ink/muted), never an accent color. */
    .list-plain { list-style:none; margin:0; padding:0; }
    .list-plain li { border-top:1px solid var(--border); padding:10px 0; }
    .list-plain li:first-child { border-top:none; }
    table.data-table { width:100%; border-collapse:collapse; font-size:0.85rem; }
    table.data-table th { font-family:var(--font-mono); font-weight:700; text-transform:uppercase;
      letter-spacing:0.1em; font-size:0.7rem; text-align:left; padding:8px 0;
      border-bottom:2px solid var(--border); }
    table.data-table td { padding:8px 0; border-bottom:1px solid var(--border); }
    table.data-table td.num { text-align:right; font-variant-numeric:tabular-nums; }
    figure.img-grid { margin:0; border:1px solid var(--border); }
    figure.img-grid figcaption { border-top:1px solid var(--border); }

    /* Structural Block — sidebar layout */
```

- [ ] **Step 2: Add List and Table tiles to the `#components` grid, after Input and before Card**

The Components grid currently reads (around lines 438–453), showing the
Input tile immediately followed by the Card tile:

```html
          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Input</p>
            <label class="label" style="display:block; margin-bottom:8px;">Email</label>
            <input placeholder="you@example.com" style="width:100%; border:1px solid var(--border);
              background:var(--input); padding:8px 12px; font-family:var(--font-mono);
              font-size:0.85rem; color:var(--foreground);" />
          </div>

          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Card</p>
            <div style="border:1px solid var(--border); background:var(--card); padding:16px;">
              <p style="margin:0; font-family:var(--font-mono); font-weight:700;">Elevated surface</p>
              <p style="margin:8px 0 0; font-size:0.85rem; color:var(--muted-foreground);">
                Depth is a background step, never a shadow.</p>
            </div>
          </div>
```

Replace with:

```html
          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Input</p>
            <label class="label" style="display:block; margin-bottom:8px;">Email</label>
            <input placeholder="you@example.com" style="width:100%; border:1px solid var(--border);
              background:var(--input); padding:8px 12px; font-family:var(--font-mono);
              font-size:0.85rem; color:var(--foreground);" />
          </div>

          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">List <span style="text-transform:none; letter-spacing:normal; color:var(--muted-foreground);">— divider, not a bullet</span></p>
            <ul class="list-plain" style="font-size:0.9rem;">
              <li>First item</li>
              <li>Second item</li>
              <li>Third item</li>
            </ul>
          </div>

          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Table <span style="text-transform:none; letter-spacing:normal; color:var(--muted-foreground);">— tabular-nums, no zebra striping</span></p>
            <table class="data-table">
              <thead>
                <tr><th>Metric</th><th style="text-align:right;">Value</th></tr>
              </thead>
              <tbody>
                <tr><td>Sessions</td><td class="num">1,209,384</td></tr>
                <tr><td>Conversions</td><td class="num">18,050</td></tr>
                <tr><td>Rate</td><td class="num">1.4931%</td></tr>
              </tbody>
            </table>
          </div>

          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Card</p>
            <div style="border:1px solid var(--border); background:var(--card); padding:16px;">
              <p style="margin:0; font-family:var(--font-mono); font-weight:700;">Elevated surface</p>
              <p style="margin:8px 0 0; font-size:0.85rem; color:var(--muted-foreground);">
                Depth is a background step, never a shadow.</p>
            </div>
          </div>
```

- [ ] **Step 3: Add a new `#images` section after `#components` and before `#charts`**

The page currently reads (around lines 563–567), showing the end of the
Components grid immediately followed by `<section id="charts">`:

```html
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
      <section id="charts">
```

Replace with:

```html
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
      <section id="images">
        <div class="divider"><h3>Images in the grid</h3><span class="rule"></span></div>
        <p class="label" style="margin:-8px 0 24px;">Grayscale/duotone by default · full color only when the photo is the content</p>
        <div style="display:grid; grid-template-columns:1fr 1fr; gap:32px;">
          <figure class="img-grid" style="aspect-ratio:4/3;">
            <div style="width:100%; height:75%; background:linear-gradient(135deg, var(--primary) 0%, var(--highlight) 100%); filter:grayscale(1);"></div>
            <p class="annotation" style="font-size:0.85rem; color:var(--muted-foreground); margin:0; padding:10px 12px;">
              Fig. 1 — grayscale, the default treatment. Keeps the four-token palette intact.
            </p>
          </figure>
          <figure class="img-grid" style="aspect-ratio:4/3;">
            <div style="width:100%; height:75%; background:linear-gradient(135deg, var(--primary) 0%, var(--highlight) 100%);"></div>
            <p class="annotation" style="font-size:0.85rem; color:var(--muted-foreground); margin:0; padding:10px 12px;">
              Fig. 2 — full color, because the photograph itself is the content — a named, scoped exception.
            </p>
          </figure>
        </div>
      </section>
      <section id="charts">
```

- [ ] **Step 4: Verify**

Run: `node scripts/capture/verify-philosophy.mjs` from the repo root.
Expected: `PASS: philosophy compliance OK` — the images demo uses only
`var(--primary)`/`var(--highlight)` inside `linear-gradient()`, no
literal hex/rgb, no real `<img src>`.

Run: `rtk grep -n "list-plain\|data-table\|img-grid\|section id=\"images\"" docs/index.html`
Expected: matches for the CSS class definitions, the List/Table demo
tiles, and the new `#images` section.

- [ ] **Step 5: Commit**

```bash
git add docs/index.html
git commit -m "feat: add lists/tables demo and images-in-grid section"
```

---

### Task 4: `docs/index.html` — richer `#registers` specimens and new `#text-length` section

**Files:**
- Modify: `docs/index.html`

**Interfaces:**
- None beyond this task — purely content changes to existing/new sections.

- [ ] **Step 1: Replace the Body/Annotation/Serif one-liners in `#registers` with fuller examples**

The `#registers` section currently reads in full (around lines 356–384):

```html
      <section id="registers">
        <div class="divider"><h3>Four type registers</h3><span class="rule"></span></div>
        <p class="label" style="margin:-8px 0 24px;">Core Geist pairing + two sanctioned registers</p>
        <div style="display:grid; grid-template-columns:repeat(auto-fit,minmax(200px,1fr)); gap:32px;">
          <div>
            <p class="label" style="margin-bottom:12px;">Display / mono</p>
            <p style="font-family:var(--font-mono); font-size:1.7rem; font-weight:700; letter-spacing:-0.02em; margin:0;">Tri-Swiss</p>
            <p class="label" style="margin-top:10px;">Geist Mono</p>
          </div>
          <div>
            <p class="label" style="margin-bottom:12px;">Body &amp; utility / sans</p>
            <p style="font-family:var(--font-sans); font-size:1.05rem; margin:0;">The primary reading, UI, and dense-data voice.</p>
            <p class="label" style="margin-top:10px;">Geist Sans</p>
          </div>
          <div>
            <p class="label" style="margin-bottom:12px;">Annotation</p>
            <p class="annotation" style="font-size:1.05rem; margin:0;">Inline notes and figure captions only.</p>
            <p class="label" style="margin-top:10px;">Space Mono, italic</p>
          </div>
          <div>
            <p class="label" style="margin-bottom:12px;">Serif / long-form</p>
            <p style="font-family:var(--font-serif); font-size:1.15rem; line-height:1.5; margin:0;">For editorial body and pull-quotes — the comfortable reading register.</p>
            <p class="label" style="margin-top:10px;">Zilla Slab</p>
          </div>
        </div>
        <blockquote style="font-family:var(--font-serif); font-style:italic; font-size:1.5rem; line-height:1.45; border-left:2px solid var(--border); margin:40px 0 0; padding:2px 0 2px 24px; max-width:62ch;">
          &ldquo;Difference is expressed through weight, space, and contrast — almost never by adding a color.&rdquo;
        </blockquote>
      </section>
```

Replace with:

```html
      <section id="registers">
        <div class="divider"><h3>Four type registers</h3><span class="rule"></span></div>
        <p class="label" style="margin:-8px 0 24px;">Core Geist pairing + two sanctioned registers</p>
        <div style="display:grid; grid-template-columns:repeat(auto-fit,minmax(200px,1fr)); gap:32px;">
          <div>
            <p class="label" style="margin-bottom:12px;">Display / mono</p>
            <p style="font-family:var(--font-mono); font-size:1.7rem; font-weight:700; letter-spacing:-0.02em; margin:0;">Tri-Swiss</p>
            <p class="label" style="margin-top:10px;">Geist Mono</p>
          </div>
          <div>
            <p class="label" style="margin-bottom:12px;">Body &amp; utility / sans</p>
            <p style="font-family:var(--font-sans); font-size:1.05rem; margin:0;">Every element earns its place or is removed — the same body voice
              carries a landing-page lead, a dashboard's dense stat block, and a
              settings page's fine print, no separate voice for any of them, just
              the weight axis already established.</p>
            <p class="label" style="margin-top:10px;">Geist Sans</p>
          </div>
          <div>
            <p class="label" style="margin-bottom:12px;">Annotation</p>
            <p class="annotation" style="font-size:1.05rem; margin:0;">Fig. 1 — quarterly conversion rate, sampled daily across the
              reporting window.</p>
            <p class="label" style="margin-top:10px;">Space Mono, italic</p>
          </div>
          <div>
            <p class="label" style="margin-bottom:12px;">Serif / long-form</p>
            <p style="font-family:var(--font-serif); font-size:1.15rem; line-height:1.5; margin:0;">The Swiss grid didn't survive fifty years because it looked clean in
              a portfolio. It survived because a system with fewer decisions to
              make is a system that ships — for every designer who inherits it,
              not just the one who drew it first.</p>
            <p class="label" style="margin-top:10px;">Zilla Slab</p>
          </div>
        </div>
        <blockquote style="font-family:var(--font-serif); font-style:italic; font-size:1.5rem; line-height:1.45; border-left:2px solid var(--border); margin:40px 0 0; padding:2px 0 2px 24px; max-width:62ch;">
          &ldquo;Difference is expressed through weight, space, and contrast — almost never by adding a color.&rdquo;
        </blockquote>
      </section>
```

- [ ] **Step 2: Add a new `#text-length` section after `#registers` and before `#components`**

The page currently reads (around lines 383–385), showing the end of
`#registers` immediately followed by `<section id="components">`:

```html
        <blockquote style="font-family:var(--font-serif); font-style:italic; font-size:1.5rem; line-height:1.45; border-left:2px solid var(--border); margin:40px 0 0; padding:2px 0 2px 24px; max-width:62ch;">
          &ldquo;Difference is expressed through weight, space, and contrast — almost never by adding a color.&rdquo;
        </blockquote>
      </section>
      <section id="components">
```

Replace with:

```html
        <blockquote style="font-family:var(--font-serif); font-style:italic; font-size:1.5rem; line-height:1.45; border-left:2px solid var(--border); margin:40px 0 0; padding:2px 0 2px 24px; max-width:62ch;">
          &ldquo;Difference is expressed through weight, space, and contrast — almost never by adding a color.&rdquo;
        </blockquote>
      </section>
      <section id="text-length">
        <div class="divider"><h3>Content at any length</h3><span class="rule"></span></div>
        <p class="label" style="margin:-8px 0 24px;">Same body voice, same measure — spacing holds regardless of length</p>
        <div style="display:grid; grid-template-columns:repeat(auto-fit,minmax(220px,1fr)); gap:32px; align-items:start;">
          <div>
            <p class="label" style="margin-bottom:12px;">Short</p>
            <p style="font-size:0.85rem; color:var(--muted-foreground); margin:0;">Updated 3 hours ago.</p>
          </div>
          <div>
            <p class="label" style="margin-bottom:12px;">Medium</p>
            <p style="margin:0; max-width:42ch;">A single paragraph reads the same whether it's a card's supporting
              copy or a page's lead — same measure, same line-height, no
              separate rule for "short body text."</p>
          </div>
          <div>
            <p class="label" style="margin-bottom:12px;">Long-form</p>
            <div style="max-width:60ch;">
              <p style="margin:0;">Restraint scales. A single line of UI copy and a full article both
                draw from the same weight axis and the same measure — nothing
                about going longer asks for a new typographic idea.</p>
              <p style="margin:14px 0 0;">What changes is paragraph spacing, not the voice: each paragraph
                gets the same top margin as the body rhythm established
                elsewhere on this page, so a long-form block reads as more of
                the same system, not a different one.</p>
            </div>
          </div>
        </div>
      </section>
      <section id="components">
```

- [ ] **Step 3: Verify**

Run: `node scripts/capture/verify-philosophy.mjs` from the repo root.
Expected: `PASS: philosophy compliance OK`.

Run: `rtk grep -n "Every element earns its place or is removed\|quarterly conversion rate\|section id=\"text-length\"" docs/index.html`
Expected: matches for the richer Body specimen, the Annotation caption,
and the new text-length section.

- [ ] **Step 4: Commit**

```bash
git add docs/index.html
git commit -m "feat: richen registers specimens and add text-length section"
```

---

### Task 5: `scripts/capture/capture.mjs` and screenshot regeneration

**Files:**
- Modify: `scripts/capture/capture.mjs`
- Modify: `docs/assets/*.png` (regenerated, not hand-edited)

**Interfaces:**
- Consumes: the final page state from Tasks 3–4.
- Produces: two new capture jobs, `images.png` and `text-length.png`.

- [ ] **Step 1: Add two new capture job entries**

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
  { id: "#turquoise-moment", file: "turquoise-moment.png", dark: false },
  { id: "#closing-band", file: "closing-band.png", dark: false },
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
  { id: "#text-length", file: "text-length.png", dark: false },
  { id: "#images", file: "images.png", dark: false },
  { id: "#turquoise-moment", file: "turquoise-moment.png", dark: false },
  { id: "#closing-band", file: "closing-band.png", dark: false },
]);
```

- [ ] **Step 2: Regenerate all screenshots**

Run: `cd scripts/capture && node capture.mjs` (run `npm install` first
if `node_modules` isn't already present).

Expected output: 11 `wrote <file>.png` lines, no errors.

- [ ] **Step 3: Visually confirm the changes**

Read (view as an image) `docs/assets/components.png` and confirm it
shows: a "List" tile (three items separated by thin top-border dividers,
no bullet glyphs) and a "Table" tile (bold mono header row, tabular-nums
right-aligned values, no zebra striping) inserted between the Input and
Card tiles.

Read `docs/assets/images.png` and confirm it shows two figures side by
side: the left one visibly desaturated/grayscale, the right one showing
the same Red-to-Turquoise gradient in full color, each with a caption
below.

Read `docs/assets/type-registers.png` and confirm the Body, Annotation,
and Serif specimens now show full sentences (not the old one-line
placeholders), and the Display specimen and blockquote are unchanged.

Read `docs/assets/text-length.png` and confirm it shows three columns:
a short one-line label, a medium paragraph, and a longer two-paragraph
block, all in the same body typeface.

- [ ] **Step 4: Commit**

```bash
git add scripts/capture/capture.mjs docs/assets/
git commit -m "feat: add capture jobs and regenerate screenshots for showcase content expansion"
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

This repo's `CHANGELOG.md` `[Unreleased]` section currently already has
content from the just-merged accent-buttons/hero-turquoise work (the
last tagged release was `v0.1.0`), reading:

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

Replace with (appending three new bullets to the existing `### Added`
list — do NOT remove the three bullets already there):

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
decorative divider, not a one-off. A new Accent button and Accent card
put more color on hover/borders, and a second named exception — a
dual-accent hover swap on the Accent button and a new Interactive card —
lets Turquoise carry real hover meaning in exactly those two places. No
success green, no info blue, no second *semantic* accent otherwise — the
highlight never carries meaning outside these two named exceptions,
however often it recurs.
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
lets Turquoise carry real hover meaning in exactly those two places.
Lists, tables, and images are now documented patterns too — list/table
borders and markers stay ink/muted-foreground only, and images default
to a grayscale/duotone filter with full color as a scoped exception for
photography that is itself the content. No success green, no info blue,
no second *semantic* accent otherwise — the highlight never carries
meaning outside its two named hover exceptions, however often it recurs.
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
instead. Two named exceptions: the tri-part segment stripe (§ SKILL.md
Philosophy section) — Red and Turquoise sit spatially adjacent, reusable
at any length — and the dual-accent hover pattern on the Accent button
and Interactive card (§ SKILL.md "Hover states") — Red and Turquoise
swap sequentially on hover, never simultaneously. Neither is a general
loosening; don't extend either beyond its named pattern.
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
loosening; don't extend either beyond its named pattern. List/table
markers and borders stay ink/muted-foreground only; images default to
grayscale/duotone, with full color reserved for the one named exception
(§ SKILL.md "Images") where the photograph itself is the content.
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
git commit -m "docs: sync CHANGELOG, README, and CONTRIBUTING with the showcase content expansion"
```
