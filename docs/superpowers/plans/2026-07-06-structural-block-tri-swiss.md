# Structural Block Pattern (Tri-Swiss) Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Give Swiss Red a new "Structural Block" job (sidebar/hero-band/bold-word), restructure the Tri-Swiss showcase page around a persistent sidebar nav, replace the solid-turquoise hero underline with a segmented ink/red/turquoise stripe, and give Turquoise a decorative hover-flourish that now reaches nav links.

**Architecture:** Continues on the existing branch `feat/expand-accents-drop-jost` (currently at commit `4fdf2b2`, clean tree — Tasks 1-8 of the prior plan are already merged into this branch's history). This plan's Task 1 picks up immediately after that commit. Plain HTML/CSS/vanilla-JS, no build step, matching the existing showcase page's stack.

**Tech Stack:** Same as the rest of this repo — Tailwind 4 `@theme inline` tokens in `theme.css` (not touched by this plan), plain CSS custom properties + inline styles in `docs/index.html`, Node.js capture/verify scripts.

## Global Constraints

- Branch: `feat/expand-accents-drop-jost`. Continue committing here; do not create a new branch.
- Structural Block cap: sidebar ≤ ~25% viewport width (min 220px, max 280px).
- Sidebar/hero-band forms are mutually exclusive per layout; the bold-word accent is independent and may combine with either. This plan's showcase page uses the **sidebar** form.
- The Structural Block (sidebar) itself is Red/ink/cream only — no Turquoise on its own background/shell.
- **Nesting clarification (resolves an apparent tension in the spec):** the sidebar's anchor-nav links are their own component, nested inside the sidebar shell. They may independently carry the Turquoise hover-flourish (§4.2 of the spec) as their own single accent — this does not violate "one accent per component," since the nav link is the component in question, not the red shell it sits inside.
- The tri-part segment stripe (ink/red/turquoise) is the **one** named exception to "Red and Turquoise never touch on the same element" — scoped to that one pattern only, not a general loosening.
- Turquoise still never carries state meaning anywhere, including in the new hover-flourish — the ornament is identical regardless of any actual interactive state.
- No new CSS custom properties for color. Reuse `--primary`, `--highlight`, `--foreground`, `--background`, `--primary-foreground`.
- `scripts/capture/verify-philosophy.mjs` is not modified by this plan.
- Conventional Commits on every commit subject; changelog-first (this plan's Task 6 covers `CHANGELOG.md`).

---

### Task 1: `SKILL.md` — Structural Block job, guardrail carve-out, segment-stripe exception, updated Turquoise-on-links wording

**Files:**
- Modify: `skills/tri-swiss/SKILL.md` (Philosophy section, `--highlight` section, Do-not list — exact locations found by content match, since line numbers have shifted since the prior plan)

**Interfaces:** N/A — documentation only.

- [ ] **Step 1: Add the Structural Block job to the Philosophy section**

Find the paragraph starting "**Tri-tone, more colorful.**" (added by the prior plan's Task 3). Immediately after the sentence ending "...and now also marks section-divider rules and a selectively emphasized card/component border (one card in a set, never the whole grid).", insert this new sentence before the next sentence about Pastel Turquoise:

```markdown
It also has a third job: a Structural Block — a solid-color sidebar/nav
rail or hero band (pick one per layout, capped at ~25% of viewport
width/height), plus an independent bold-word accent inside a heading that
may combine with either. Outside that one block, ink/cream continue to
dominate every other surface exactly as before.
```

- [ ] **Step 2: Add the tri-part segment stripe as a named guardrail exception**

Find the sentence "Three guardrails keep this from tipping into loud: ink/cream still visually dominate any surface; Red and Turquoise never touch or sit adjacent on the same element; one accent per component, not both." Immediately after it, add:

```markdown
The **tri-part segment stripe** — three equal solid blocks, ink/Red/
Turquoise in a row, used for a static decorative bar (e.g. beneath a hero
title) — is the one explicitly named exception to "Red and Turquoise
never touch": a single governed device, not a general loosening. Nowhere
else may the two sit adjacent.
```

- [ ] **Step 3: Update the `--highlight` section's decorative-reuse list and Do-not wording**

Find the numbered list in the `--highlight` section (items 1-6, ending with "A dot accent, matching the existing dot-indicator pattern."). Add a 7th item:

```markdown
7. A hover-triggered flourish on a nav link or label — an underline or dot
   that appears on `:hover`, purely ornamental and identical regardless of
   active/current/visited state, layered *alongside* the element's
   existing ink/muted-foreground hover color change (which still carries
   the real interactive feedback).
```

Then find this sentence in the same section: "**Do not** use it in buttons, tags, status pips, links, or any other UI state indicator." Replace it with:

```markdown
**Do not** use it as a button's, tag's, status pip's, or link's own
state-indicating color. A link may show a decorative Turquoise
hover-flourish (item 7 above) *in addition to* its real ink/red state
feedback — Turquoise itself never signals the state.
```

- [ ] **Step 4: Verify the edits landed and nothing else was disturbed**

Run: `rtk grep -n "Structural Block\|segment stripe\|hover-triggered flourish" skills/tri-swiss/SKILL.md`
Expected: three matches, one per edit above, all inside the Philosophy/`--highlight` sections (not inside Typography or the Do-not list's other bullets).

- [ ] **Step 5: Commit**

```bash
rtk git add skills/tri-swiss/SKILL.md
rtk git commit -m "feat(skill): add Structural Block job and segment-stripe exception"
```

---

### Task 2: `components.md` — new patterns for the sidebar, hero band, bold word, segment stripe, and hover-flourish

**Files:**
- Modify: `skills/tri-swiss/references/components.md` — insert a new section

**Interfaces:** N/A.

- [ ] **Step 1: Insert the new "Structural Block" section**

Insert this new section immediately after the existing "## Accent expansion — Red dividers, emphasis borders, Turquoise decoration" section (added by the prior plan) and before "## Iconography (geist-icons, restyled)":

```markdown
## Structural Block — sidebar, hero band, bold word

A third job for Swiss Red, on top of dividers and card borders: a genuine
layout element, not just a border or divider. Three forms — pick one
block form per layout; the bold-word form is independent and may combine
with either.

**Sidebar / nav rail.** Full-height solid Red panel, capped at ~25% of
viewport width (min 220px, max 280px), sticky/fixed. Holds wordmark,
in-page anchor nav, theme toggle, external link, copyright:

```html
<aside class="sidebar" style="width:22%; min-width:220px; max-width:280px;
  background:var(--primary); color:var(--primary-foreground);
  position:sticky; top:0; height:100vh; display:flex; flex-direction:column;
  justify-content:space-between; padding:32px 28px; box-sizing:border-box;">
  <div>
    <span style="font-family:var(--font-mono); font-weight:700; font-size:1.5rem;">Wordmark</span>
    <nav style="margin-top:40px; display:flex; flex-direction:column; gap:16px;
      font-family:var(--font-mono); font-size:0.8rem; text-transform:uppercase; letter-spacing:0.15em;">
      <a href="#section-one" style="color:var(--primary-foreground); text-decoration:none;
         opacity:0.75; border-bottom:2px solid transparent; padding-bottom:2px;">Section One</a>
    </nav>
  </div>
  <div style="font-family:var(--font-mono); font-size:0.7rem; text-transform:uppercase;
    letter-spacing:0.12em; opacity:0.75;">© Year Author</div>
</aside>
```

**Hero band.** Solid Red horizontal block, used once, for a hero/intro
moment — an alternative to the sidebar, never combined with it:

```html
<div style="background:var(--primary); color:var(--primary-foreground); padding:64px 48px;">
  <h1 style="margin:0;">Hero title.</h1>
</div>
```

**Bold word/phrase accent.** One word inside a heading or paragraph in
Red, at normal weight/size — independent of the two block forms, may
combine with either:

```html
<p>Two structural colors, one strong <span style="color:var(--primary);">accent</span>.</p>
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
```

- [ ] **Step 2: Verify the section was inserted in the right place**

Run: `rtk grep -n "^## " skills/tri-swiss/references/components.md`
Expected: "## Structural Block — sidebar, hero band, bold word" appears
directly between "## Accent expansion — Red dividers, emphasis borders,
Turquoise decoration" and "## Iconography (geist-icons, restyled)".

- [ ] **Step 3: Commit**

```bash
rtk git add skills/tri-swiss/references/components.md
rtk git commit -m "feat(components): add Structural Block, segment-stripe, and hover-flourish patterns"
```

---

### Task 3: `docs/index.html` — sidebar layout CSS and hamburger toggle script

**Files:**
- Modify: `docs/index.html` (the `<style>` block and the scripts near the end of `<body>`)

**Interfaces:**
- Produces: CSS classes `.layout`, `.sidebar`, `.sidebar-top`, `.sidebar-wordmark`,
  `.sidebar-nav`, `.sidebar-bottom`, `.content`, `.content-inner`, `.hamburger` —
  Task 4 (HTML restructure) consumes these exact class names.
- Produces: a `data-nav-toggle` button attribute and `data-nav-list` nav
  attribute, wired by this task's new script — Task 4 uses these same
  attribute names on its markup.

- [ ] **Step 1: Add the sidebar/content layout CSS**

Find this existing rule in the `<style>` block:

```css
    .toggle .mid { color:var(--muted-foreground); opacity:0.4; }
```

Immediately after it (still inside the `<style>` block, before the closing
`</style>`), add:

```css
    /* Structural Block — sidebar layout */
    .layout { display:flex; min-height:100vh; }
    .sidebar { width:22%; min-width:220px; max-width:280px; flex-shrink:0;
      background:var(--primary); color:var(--primary-foreground);
      position:sticky; top:0; height:100vh; overflow-y:auto;
      display:flex; flex-direction:column; justify-content:space-between;
      padding:32px 28px; box-sizing:border-box; }
    .sidebar-wordmark { font-family:var(--font-mono); font-weight:700;
      font-size:1.4rem; line-height:1.2; }
    .sidebar-nav { margin-top:40px; display:flex; flex-direction:column; gap:16px;
      font-family:var(--font-mono); font-size:0.8rem; text-transform:uppercase;
      letter-spacing:0.15em; }
    .sidebar-nav a { color:var(--primary-foreground); text-decoration:none;
      opacity:0.75; border-bottom:2px solid transparent; padding-bottom:2px;
      width:max-content; transition:opacity 0.15s, border-color 0.15s; }
    .sidebar-nav a:hover { opacity:1; border-bottom-color:var(--highlight); }
    .sidebar-bottom { display:flex; flex-direction:column; gap:16px;
      font-family:var(--font-mono); font-size:0.7rem; text-transform:uppercase;
      letter-spacing:0.12em; }
    .sidebar-bottom a { color:var(--primary-foreground); text-decoration:none;
      opacity:0.85; display:inline-flex; align-items:center; gap:6px; }
    .sidebar-bottom .toggle button { color:var(--primary-foreground); opacity:0.6; }
    .sidebar-bottom .toggle button[aria-pressed="true"] { opacity:1; }
    .sidebar-bottom .toggle .mid { color:var(--primary-foreground); opacity:0.4; }
    .hamburger { display:none; background:none; border:none;
      color:var(--primary-foreground); cursor:pointer; padding:4px; }
    .content { flex:1; min-width:0; }
    .content-inner { max-width:900px; margin:0 auto; padding:0 32px; }
    @media (max-width: 860px) {
      .layout { flex-direction:column; }
      .sidebar { width:100%; max-width:none; height:auto; position:sticky;
        top:0; z-index:10; flex-direction:row; align-items:center;
        justify-content:space-between; padding:16px 20px; }
      .sidebar-nav { display:none; margin-top:0; position:absolute; top:100%;
        left:0; right:0; background:var(--primary); padding:20px;
        flex-direction:column; gap:16px; }
      .sidebar-nav.open { display:flex; }
      .sidebar-bottom { display:none; }
      .hamburger { display:block; }
    }
```

- [ ] **Step 2: Add the hamburger toggle script**

Find the last `<script>` block in the file (the theme-toggle script, ending
with `})();` right before `</script></body></html>`). Immediately after
that script's closing `</script>` tag, add a new script block:

```html
  <script>
    (function () {
      var btn = document.querySelector("[data-nav-toggle]");
      var list = document.querySelector("[data-nav-list]");
      if (!btn || !list) return;
      btn.addEventListener("click", function () {
        var open = list.classList.toggle("open");
        btn.setAttribute("aria-expanded", String(open));
      });
    })();
  </script>
```

- [ ] **Step 3: Verify the CSS and script were added without syntax errors**

Run: `rtk grep -n "\.sidebar \{" docs/index.html`
Expected: exactly one match, inside the `<style>` block.

Run: `rtk grep -n "data-nav-toggle" docs/index.html`
Expected: one match here (the script's `querySelector` call) — Task 4 will
add the matching HTML attribute.

- [ ] **Step 4: Commit**

```bash
rtk git add docs/index.html
rtk git commit -m "feat(showcase): add sidebar layout CSS and mobile nav toggle script"
```

---

### Task 4: `docs/index.html` — restructure the page around the sidebar

**Files:**
- Modify: `docs/index.html` (the `<body>` markup)

**Interfaces:**
- Consumes: `.layout`/`.sidebar`/`.content`/`.content-inner`/`.hamburger`
  classes and `data-nav-toggle`/`data-nav-list` attributes from Task 3.

- [ ] **Step 1: Replace the old nav + wrap opening with the sidebar + content structure**

Find this exact block:

```html
<body>
  <div class="wrap">
    <nav>
      <span class="wordmark">Lux / Tri-Swiss</span>
      <div class="toggle" role="group" aria-label="Color theme">
        <button data-theme-btn="light" aria-pressed="true">Light</button>
        <span class="mid">·</span>
        <button data-theme-btn="dark" aria-pressed="false">Dark</button>
      </div>
    </nav>
    <main>
```

Replace it with:

```html
<body>
  <div class="layout">
    <aside class="sidebar">
      <div>
        <div style="display:flex; align-items:center; justify-content:space-between;">
          <span class="sidebar-wordmark">Tri-Swiss</span>
          <button class="hamburger" aria-label="Toggle navigation" aria-expanded="false" data-nav-toggle>
            <svg class="icon" viewBox="0 0 24 24" style="color:var(--primary-foreground);">
              <line x1="4" y1="6" x2="20" y2="6"/><line x1="4" y1="12" x2="20" y2="12"/><line x1="4" y1="18" x2="20" y2="18"/>
            </svg>
          </button>
        </div>
        <nav class="sidebar-nav" data-nav-list>
          <a href="#palette">Palette</a>
          <a href="#typography">Typography</a>
          <a href="#components">Components</a>
          <a href="#charts">Charts</a>
        </nav>
      </div>
      <div class="sidebar-bottom">
        <div class="toggle" role="group" aria-label="Color theme">
          <button data-theme-btn="light" aria-pressed="true">Light</button>
          <span class="mid">·</span>
          <button data-theme-btn="dark" aria-pressed="false">Dark</button>
        </div>
        <a href="https://github.com/luxsolari/tri-swiss">
          GitHub
          <svg class="icon" viewBox="0 0 24 24" width="14" height="14" style="color:var(--primary-foreground);">
            <path d="M15 3h6v6"/><path d="M10 14 21 3"/><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/>
          </svg>
        </a>
        <span>© 2026 Lux Solari</span>
      </div>
    </aside>
    <div class="content">
      <div class="content-inner">
    <main>
```

Note the indentation shift: everything from the original `<main>` onward
(all existing `<section>` elements, the `#social-card` div, and the
`<footer>`) stays **completely unchanged** — only the opening wrapper
above it changes. Do not edit any section content in this step.

- [ ] **Step 2: Replace the old closing `</main></div>` with the new closing tags**

Find this exact block, near the end of `<body>` (immediately after the
`</footer>` closing tag and before the first `<script type="module">`):

```html
    </main>
  </div>
  <script type="module">
```

Replace it with:

```html
    </main>
      </div>
    </div>
  </div>
  <script type="module">
```

- [ ] **Step 3: Remove the now-unused top-level `.wrap`/`nav` CSS rules that only applied to the old layout**

Find and delete this rule (the old top-nav bar, now replaced by the
sidebar — the `.wrap` rule itself stays, since `.content-inner` reuses its
max-width value, but the plain `nav` selector rule is dead code now):

```css
    /* nav */
    nav { display:flex; align-items:center; justify-content:space-between;
      padding:20px 0; border-bottom:1px solid var(--border); }
    .wordmark { font-family:var(--font-mono); font-weight:700; letter-spacing:0.15em;
      text-transform:uppercase; font-size:0.8rem; }
```

Delete this whole rule. Leave `.toggle`, `.toggle button`, `.toggle
button[aria-pressed="true"]`, `.toggle .mid` in place — they're reused by
`.sidebar-bottom .toggle` in Task 3's CSS.

- [ ] **Step 4: Visually verify the page loads without console errors**

Open `docs/index.html` directly in a browser (or via a local file:// URL)
and confirm: a red sidebar renders on the left holding the wordmark, nav
links, theme toggle, GitHub link, and copyright; the rest of the page's
existing sections render unchanged in the remaining width; no JS console
errors. Resize the viewport below ~860px and confirm the sidebar collapses
to a top red band with a working hamburger toggle.

- [ ] **Step 5: Run the philosophy verifier**

Run: `cd scripts/capture && npm run verify`
Expected: `PASS: philosophy compliance OK`.

- [ ] **Step 6: Commit**

```bash
rtk git add docs/index.html
rtk git commit -m "feat(showcase): restructure page around a persistent sidebar nav"
```

---

### Task 5: `docs/index.html` — segment-stripe hero underline and bold-word accent

**Files:**
- Modify: `docs/index.html` (hero section)

**Interfaces:** N/A.

- [ ] **Step 1: Replace the solid-turquoise hero underline with the tri-part segment stripe**

Find this exact line (added by the prior plan, in the `#hero` section):

```html
        <span style="display:block; width:64px; height:3px; background:var(--highlight); margin-top:16px;" aria-hidden="true"></span>
```

Replace it with:

```html
        <div style="display:flex; gap:4px; width:64px; margin-top:16px;" aria-hidden="true">
          <div style="height:3px; flex:1; background:var(--foreground);"></div>
          <div style="height:3px; flex:1; background:var(--primary);"></div>
          <div style="height:3px; flex:1; background:var(--highlight);"></div>
        </div>
```

- [ ] **Step 2: Add the bold-word accent to the hero subhead**

Find this exact paragraph in the `#hero` section:

```html
          Lux Solari's Geist-based house design language — Swiss-minimalist,
          tri-tone, more colorful. Two structural colors, one strong accent,
          one governed highlight; everything else is weight, space, and contrast.
```

Replace it with:

```html
          Lux Solari's Geist-based house design language — Swiss-minimalist,
          tri-tone, more colorful. Two structural colors, one strong
          <span style="color:var(--primary);">accent</span>, one governed
          highlight; everything else is weight, space, and contrast.
```

- [ ] **Step 3: Run the philosophy verifier**

Run: `cd scripts/capture && npm run verify`
Expected: `PASS: philosophy compliance OK`.

- [ ] **Step 4: Verify no stray solid-turquoise underline remains**

Run: `rtk grep -n "height:3px; background:var(--highlight); margin-top:16px" docs/index.html`
Expected: no output (exit code 1) — confirms the old single-color underline was fully replaced, not left duplicated alongside the new stripe.

- [ ] **Step 5: Commit**

```bash
rtk git add docs/index.html
rtk git commit -m "feat(showcase): segment-stripe hero underline and bold-word accent"
```

---

### Task 6: `README.md` and `CONTRIBUTING.md` — reflect the Structural Block pattern

**Files:**
- Modify: `README.md` (aesthetic-summary paragraph)
- Modify: `CONTRIBUTING.md` (Design changes paragraph)

**Interfaces:** N/A.

- [ ] **Step 1: Extend README's aesthetic-summary paragraph**

Find this paragraph (added by the prior plan):

```markdown
**Tri-tone, more colorful, still Swiss-minimalist.** Two structural colors
— ink (`#000000`) and warm cream (`#eae8d0`) — plus a Swiss Red accent
(`#d3281b`) that now also marks section dividers and selective card
emphasis, and a non-semantic highlight, Pastel Turquoise (`#56bfa3`), used
decoratively across icon fills, underlines, washes, and chart series. No
success green, no info blue, no second *semantic* accent — the highlight
never carries meaning, however often it recurs.
```

Replace it with:

```markdown
**Tri-tone, more colorful, still Swiss-minimalist.** Two structural colors
— ink (`#000000`) and warm cream (`#eae8d0`) — plus a Swiss Red accent
(`#d3281b`) that now also marks section dividers, selective card emphasis,
and a genuine Structural Block (a solid-color sidebar/hero band, capped at
~25% of viewport, or a bold word inside a heading), and a non-semantic
highlight, Pastel Turquoise (`#56bfa3`), used decoratively across icon
fills, underlines, washes, chart series, and a hover-triggered flourish on
nav links. No success green, no info blue, no second *semantic* accent —
the highlight never carries meaning, however often it recurs.
```

- [ ] **Step 2: Update CONTRIBUTING.md's Design changes paragraph**

Find this paragraph (already updated by the prior plan):

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

Replace it with:

```markdown
## Design changes
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

- [ ] **Step 3: Verify both edits landed**

Run: `rtk grep -n "Structural Block\|Structural-Block" README.md CONTRIBUTING.md`
Expected: at least one match in each file.

- [ ] **Step 4: Commit**

```bash
rtk git add README.md CONTRIBUTING.md
rtk git commit -m "docs: reflect Structural Block pattern in README and CONTRIBUTING"
```

---

### Task 7: `CHANGELOG.md` — reflect the Structural Block pattern

**Files:**
- Modify: `CHANGELOG.md` (the `[Unreleased]` → `### Added` bullets)

**Interfaces:** N/A.

- [ ] **Step 1: Extend the SKILL.md bullet**

Find this bullet (from the prior plan):

```markdown
- **`SKILL.md`** — philosophy (tri-tone, more colorful, Swiss-minimalist),
  full light/dark palette token tables including the governed `--highlight`
  token, typography rules across three registers (Geist Mono/Sans, Space
  Mono italic, Zilla Slab), spacing/layout, and core component patterns
  (buttons, tags, section dividers, selective red card-border emphasis,
  decorative Turquoise accents).
```

Replace it with:

```markdown
- **`SKILL.md`** — philosophy (tri-tone, more colorful, Swiss-minimalist),
  full light/dark palette token tables including the governed `--highlight`
  token, typography rules across three registers (Geist Mono/Sans, Space
  Mono italic, Zilla Slab), spacing/layout, and core component patterns
  (buttons, tags, section dividers, selective red card-border emphasis,
  decorative Turquoise accents, a Structural Block sidebar/hero-band/
  bold-word job for Swiss Red, the tri-part segment stripe, and a
  Turquoise hover-flourish on nav links).
```

- [ ] **Step 2: Extend the components.md bullet**

Find this bullet:

```markdown
- **`references/components.md`** — extended component catalogue: status
  pips, modal overlay, toggle controls, cards, inputs, hero/annotation
  type patterns, accent-expansion patterns (red dividers, red card
  borders, turquoise decoration), and the hand-rolled + Observable Plot
  chart patterns.
```

Replace it with:

```markdown
- **`references/components.md`** — extended component catalogue: status
  pips, modal overlay, toggle controls, cards, inputs, hero/annotation
  type patterns, accent-expansion patterns (red dividers, red card
  borders, turquoise decoration), Structural Block patterns (sidebar/nav
  rail, hero band, bold word, tri-part segment stripe, turquoise
  hover-flourish), and the hand-rolled + Observable Plot chart patterns.
```

- [ ] **Step 3: Extend the showcase-page bullet**

Find this bullet:

```markdown
- **Showcase landing page** (`docs/index.html`) served on GitHub Pages,
  plus README screenshots, a 1200×630 social-preview card, and a
  reproducible Playwright capture script.
```

Replace it with:

```markdown
- **Showcase landing page** (`docs/index.html`) served on GitHub Pages —
  restructured around a persistent sidebar nav (collapsing to a red top
  band with a hamburger toggle on mobile) — plus README screenshots, a
  1200×630 social-preview card, and a reproducible Playwright capture
  script.
```

- [ ] **Step 4: Verify the edits landed**

Run: `rtk grep -n "Structural Block" CHANGELOG.md`
Expected: at least one match.

- [ ] **Step 5: Commit**

```bash
rtk git add CHANGELOG.md
rtk git commit -m "docs: reflect Structural Block pattern in changelog"
```

---

### Task 8: Regenerate screenshots

**Files:**
- Modify (binary, regenerated): all 7 files in `docs/assets/`

**Interfaces:** N/A.

- [ ] **Step 1: Run the capture script**

Run: `cd scripts/capture && npm run capture`
Expected: 7 lines of `wrote <filename>.png`, no thrown errors.

- [ ] **Step 2: Visually confirm the sidebar layout and new patterns render correctly**

Open `docs/assets/hero-light.png` and confirm: the red sidebar appears on
the left, the segment stripe (ink/red/turquoise) appears beneath the hero
title instead of the old solid turquoise bar, and the word "accent" in the
hero subhead renders in red. Open `docs/assets/components.png` and confirm
the existing "Emphasis card" and "Decorative accent" tiles from the prior
plan still render correctly within the narrower content column.

- [ ] **Step 3: Re-run the philosophy verifier**

Run: `cd scripts/capture && npm run verify`
Expected: `PASS: philosophy compliance OK`.

- [ ] **Step 4: Commit**

```bash
rtk git add docs/assets/
rtk git commit -m "chore(showcase): regenerate screenshots for the Structural Block pattern"
```

---

### Task 9: Push branch and open the PR

**Files:** None (git/gh operations only).

**Interfaces:** N/A.

**Note:** this supersedes the prior plan's own Task 9 (never executed) —
that plan's branch now carries all of this plan's commits too, so there is
only one push and one PR for the whole branch.

- [ ] **Step 1: Confirm the full commit history since `main`**

Run: `rtk git log main..HEAD --oneline`
Expected: the prior plan's commits plus this plan's 8 new commits, all
Conventional Commits, no unexpected entries.

- [ ] **Step 2: Push the branch**

Run: `rtk git push -u origin feat/expand-accents-drop-jost`
Expected: branch pushed/updated on `origin`.

- [ ] **Step 3: Open the PR**

```bash
gh pr create --title "feat: more colorful accents, Structural Block, drop Jost" --body "$(cat <<'EOF'
## Summary
- Loosens Swiss Red / Pastel Turquoise rationing (docs/superpowers/specs/2026-07-06-colorful-accents-drop-jost-design.md): Red gains section-divider and selective-card-border jobs; Turquoise stays non-semantic but is no longer capped at one touch per page.
- Drops the Jost 4th type register; hero title/wordmark and chapter dividers fold into Geist Mono.
- Adds the Structural Block pattern (docs/superpowers/specs/2026-07-06-structural-block-and-duotone-weight-highlight-design.md): a sidebar/hero-band/bold-word job for Swiss Red, restructuring the showcase page around a persistent sidebar nav; replaces the solid-turquoise hero underline with a tri-part ink/red/turquoise segment stripe (one named exception to "Red and Turquoise never touch"); adds a Turquoise hover-flourish reaching nav links for the first time.
- Showcase page, component catalogue, README, CONTRIBUTING, and CHANGELOG all updated to match; screenshots regenerated.

## Test plan
- [x] `scripts/capture/verify-philosophy.mjs` passes
- [x] No residual `Jost`/`font-hero` references anywhere in the repo
- [ ] Visual check of the sidebar layout (desktop + mobile collapse) and regenerated `docs/assets/*.png` on the PR itself
EOF
)"
```

Expected: PR URL printed; note it for the user.
