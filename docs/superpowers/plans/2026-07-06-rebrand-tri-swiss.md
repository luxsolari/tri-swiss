# Rebrand (Tri-Swiss half) Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Unify Tri-Swiss's cream hue with Lux Swiss's (formerly Duotone Swiss's) existing cream family, and add house-mark framing identifying Tri-Swiss and Lux Swiss as Lux Solari's two sibling personal-brand design systems.

**Architecture:** New tasks on the existing branch `feat/expand-accents-drop-jost`, executed **before** that branch's already-written Structural Block plan (`docs/superpowers/plans/2026-07-06-structural-block-tri-swiss.md`) — same branch, one continuous commit sequence, one eventual PR covering both pieces of work. A mechanical global hex substitution (not a redesign) plus new prose.

**Tech Stack:** Same as the rest of this repo — plain CSS custom properties, Markdown, Node.js verify script.

## Global Constraints

- Branch: `feat/expand-accents-drop-jost`. Continue committing here.
- Exact hex substitutions (light mode): `#eae8d0` → `#f5efe0`, `#f1efdb` → `#faf6ec`, `#e2dfc7` → `#ebe5d5`. These same three pairs apply everywhere the old hex appears — light mode, dark mode, and every semantic role that happens to reuse one of these values (`--primary-foreground`, `--secondary`, `--secondary-foreground`, `--card-foreground` in dark mode, `--border` in dark mode) — confirmed by grep that all of Tri-Swiss's cream-family occurrences are exactly these three hex strings, nothing else.
- `--muted-foreground` (`#4a4838` light, `#a8a696` dark) is **not** part of this change — it's a grey/label tone, not the cream hue itself.
- No screenshot regeneration in this plan — screenshots stay stale until the Structural Block plan's own capture step (its Task 8) runs afterward, since both the rebrand and Structural Block changes will be present in the file by then. Regenerating twice would be redundant.
- `HOUSE-MARK.md` lives at the **repo root**, not under `docs/` — `docs/` is this repo's GitHub Pages source directory (confirmed: Pages serves from `main:/docs`), so a root-level placement matches where `README.md`/`CONTRIBUTING.md`/`CHANGELOG.md`/`LICENSE` already live, rather than being served (unstyled) as a Pages asset. This is a placement adjustment from the spec's literal `docs/HOUSE-MARK.md` path — the content and intent are unchanged.
- Conventional Commits on every commit subject.

---

### Task 1: `theme.css` — cream hue substitution

**Files:**
- Modify: `skills/tri-swiss/assets/theme.css`

**Interfaces:** N/A — CSS custom properties only.

- [ ] **Step 1: Replace all three cream hex pairs**

Using the Edit tool with `replace_all: true`, make these three replacements in `skills/tri-swiss/assets/theme.css`:

1. Replace all occurrences of `#eae8d0` with `#f5efe0`
2. Replace all occurrences of `#f1efdb` with `#faf6ec`
3. Replace all occurrences of `#e2dfc7` with `#ebe5d5`

- [ ] **Step 2: Verify no old cream hex remains and the new ones are present**

Run: `rtk grep -in "eae8d0\|f1efdb\|e2dfc7" skills/tri-swiss/assets/theme.css`
Expected: no output (exit code 1 — no matches).

Run: `rtk grep -c "f5efe0\|faf6ec\|ebe5d5" skills/tri-swiss/assets/theme.css`
Expected: a count greater than 0.

- [ ] **Step 3: Commit**

```bash
rtk git add skills/tri-swiss/assets/theme.css
rtk git commit -m "feat(theme): unify cream hue with Lux Swiss's palette"
```

---

### Task 2: `SKILL.md` — cream hue substitution in palette tables

**Files:**
- Modify: `skills/tri-swiss/SKILL.md`

**Interfaces:** N/A.

- [ ] **Step 1: Replace all three cream hex pairs**

Using the Edit tool with `replace_all: true`, make these three replacements in `skills/tri-swiss/SKILL.md`:

1. Replace all occurrences of `#eae8d0` with `#f5efe0`
2. Replace all occurrences of `#f1efdb` with `#faf6ec`
3. Replace all occurrences of `#e2dfc7` with `#ebe5d5`

- [ ] **Step 2: Verify no old cream hex remains**

Run: `rtk grep -in "eae8d0\|f1efdb\|e2dfc7" skills/tri-swiss/SKILL.md`
Expected: no output (exit code 1 — no matches).

- [ ] **Step 3: Commit**

```bash
rtk git add skills/tri-swiss/SKILL.md
rtk git commit -m "docs(skill): unify cream hue with Lux Swiss's palette in SKILL.md tables"
```

---

### Task 3: `docs/index.html` — cream hue substitution

**Files:**
- Modify: `docs/index.html`

**Interfaces:** N/A.

- [ ] **Step 1: Replace all three cream hex pairs**

Using the Edit tool with `replace_all: true`, make these three replacements in `docs/index.html`:

1. Replace all occurrences of `#eae8d0` with `#f5efe0`
2. Replace all occurrences of `#f1efdb` with `#faf6ec`
3. Replace all occurrences of `#e2dfc7` with `#ebe5d5`

- [ ] **Step 2: Verify no old cream hex remains**

Run: `rtk grep -in "eae8d0\|f1efdb\|e2dfc7" docs/index.html`
Expected: no output (exit code 1 — no matches).

- [ ] **Step 3: Commit**

```bash
rtk git add docs/index.html
rtk git commit -m "feat(showcase): unify cream hue with Lux Swiss's palette"
```

---

### Task 4: `scripts/capture/verify-philosophy.mjs` — update the palette allowlist

**Files:**
- Modify: `scripts/capture/verify-philosophy.mjs:8-11`

**Interfaces:** N/A.

- [ ] **Step 1: Update the PALETTE set**

Find this exact block:

```js
const PALETTE = new Set([
  "#eae8d0","#000000","#f1efdb","#d3281b","#e2dfc7","#4a4838","#56bfa3",
  "#161616","#e2503f","#1f1f1f","#a8a696","#63cbae",
]);
```

Replace it with:

```js
const PALETTE = new Set([
  "#f5efe0","#000000","#faf6ec","#d3281b","#ebe5d5","#4a4838","#56bfa3",
  "#161616","#e2503f","#1f1f1f","#a8a696","#63cbae",
]);
```

- [ ] **Step 2: Run the philosophy verifier**

Run: `cd scripts/capture && npm run verify`
Expected: `PASS: philosophy compliance OK` — confirms `docs/index.html` (already updated in Task 3) contains only allowlisted hex values, and that the three old cream hexes are correctly no longer in the allowlist.

- [ ] **Step 3: Commit**

```bash
rtk git add scripts/capture/verify-philosophy.mjs
rtk git commit -m "test(verify): update palette allowlist for the unified cream hue"
```

---

### Task 5: House-mark framing — `README.md`, `SKILL.md`, and `HOUSE-MARK.md`

**Files:**
- Modify: `README.md`
- Modify: `skills/tri-swiss/SKILL.md`
- Create: `HOUSE-MARK.md`

**Interfaces:** N/A.

- [ ] **Step 1: Add the house-mark paragraph to README.md**

Find this exact paragraph (the intro, ending right before `## The aesthetic`):

```markdown
A Claude Code plugin that teaches Claude **Tri-Swiss** — Lux Solari's
Geist-based house design language, sibling to
[lux-design-system](https://github.com/luxsolari/lux-design-system) (Duotone
Swiss) — so every project you build shares one consistent, opinionated
aesthetic.
```

Replace it with:

```markdown
A Claude Code plugin that teaches Claude **Tri-Swiss** — Lux Solari's
Geist-based house design language, sibling to
[Lux Swiss](https://github.com/luxsolari/lux-swiss) (formerly Duotone
Swiss) — so every project you build shares one consistent, opinionated
aesthetic.

Tri-Swiss and Lux Swiss are the two house-mark design systems that carry
Lux Solari's personal brand identity into every project built with them —
related governance, distinct palettes. See [HOUSE-MARK.md](HOUSE-MARK.md)
for how the two relate.
```

- [ ] **Step 2: Add the house-mark sentence to SKILL.md's opening overview**

Find this exact paragraph (the top-level overview, right after the `# Tri-Swiss — Design System` heading):

```markdown
A strict, minimalist visual language built around the Geist typeface family.
Two structural colors plus one strong accent plus a governed, non-semantic
highlight; hard borders, generous whitespace, monospace labels. The whole
point is restraint: every element earns its place or is removed, and
**most difference is still expressed through typography, spacing, and
contrast — Red and Turquoise are a deliberate, governed layer on top, never
a substitute for that discipline.**
```

Immediately after it, add:

```markdown

Tri-Swiss is one of Lux Solari's two house-mark design systems — the
personal brand identity carried into every project built with them. Its
sibling, [Lux Swiss](https://github.com/luxsolari/lux-swiss) (formerly
Duotone Swiss), applies the same governance philosophy through a strict
two-color-plus-accent palette; see `HOUSE-MARK.md` for how the two relate.
```

- [ ] **Step 3: Create `HOUSE-MARK.md`**

Create `HOUSE-MARK.md` at the repo root with this exact content:

```markdown
# House Mark

Tri-Swiss and Lux Swiss are Lux Solari's two house-mark design systems —
not two variations of one system, but two related, independently governed
languages that both carry the same personal brand identity into whatever
project they're applied to. A house mark, in the traditional sense, is the
imprint a publisher or maker stamps across everything they produce so it
reads as theirs regardless of the specific title — that's the role these
two systems play here.

## What they share

- The same two structural pillars: visible 1px borders, no shadows
  (elevation is a background-color step), generous whitespace, uppercase
  monospace labels, square corners.
- The same governance discipline: every color, every optional type
  register, every icon set is explicitly sanctioned and scoped — nothing
  ad hoc.
- The same warm cream base (`#f5efe0` light / near-black dark, ink)
  underneath everything.

## What's different

| | Tri-Swiss | Lux Swiss |
|---|---|---|
| Palette | Tri-tone — ink/cream + Swiss Red + a governed Pastel Turquoise highlight | Strict duotone — ink/cream + Blood Red, no third color |
| Typography | Geist family (Sans/Mono) + Space Mono italic + Zilla Slab | Space Grotesk/Mono + Zilla Slab |
| Icons | `geist-icons` | Lucide |
| Structural Block | Sidebar/hero-band/bold-word for Swiss Red, plus a tri-part ink/red/turquoise segment stripe | Sidebar/hero-band/bold-word for Blood Red, plus a two-color ink/red segment stripe and a typographic brand-moment device (since there's no third color to spare) |

## Picking one

Use Tri-Swiss when a project wants a bit more chromatic range (the
governed Turquoise highlight, used decoratively). Use Lux Swiss when a
project wants the starkest possible two-color statement. Both apply the
same Swiss-minimalist structural rules underneath — picking one is a
palette decision, not a governance one.
```

- [ ] **Step 4: Verify all three edits landed**

Run: `rtk grep -n "Lux Swiss" README.md skills/tri-swiss/SKILL.md HOUSE-MARK.md`
Expected: at least one match in each of the three files.

- [ ] **Step 5: Commit**

```bash
rtk git add README.md skills/tri-swiss/SKILL.md HOUSE-MARK.md
rtk git commit -m "docs: add house-mark framing and HOUSE-MARK.md"
```

---

### Task 6: Sanity check before moving on to the Structural Block plan

**Files:** None (verification only).

**Interfaces:** N/A.

- [ ] **Step 1: Confirm no residual references to the old Duotone Swiss name in Tri-Swiss's own files**

Run: `rtk grep -rn "Duotone Swiss" README.md skills/tri-swiss/ HOUSE-MARK.md`
Expected: any matches found should describe Lux Swiss's *former* name in passing (e.g. "formerly Duotone Swiss") — not a live/current reference. Read each match and confirm this by eye.

- [ ] **Step 2: Re-run the philosophy verifier one more time**

Run: `cd scripts/capture && npm run verify`
Expected: `PASS: philosophy compliance OK`.

- [ ] **Step 3: No commit needed — this task is verification only**

Proceed to the already-written Structural Block plan
(`docs/superpowers/plans/2026-07-06-structural-block-tri-swiss.md`) next,
after first patching it per that plan's own note (any prose naming
"Duotone Swiss" should say "Lux Swiss" — a mechanical find-replace against
the plan document itself, not its already-shipped Tasks 1-8).
