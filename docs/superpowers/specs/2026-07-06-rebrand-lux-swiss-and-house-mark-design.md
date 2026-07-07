# Rebrand: Duotone Swiss → Lux Swiss, Cream Unification, House-Mark Framing — Design Spec

Status: approved
Date: 2026-07-06
Repos: `luxsolari/tri-swiss` (this repo) and `luxsolari/lux-design-system`
(to be renamed `luxsolari/lux-swiss`), plus a small update to
`luxsolari/lux-solari-plugins` (the marketplace repo).
Sequencing: **executes before** the two not-yet-started Structural Block
plans (`2026-07-06-structural-block-tri-swiss.md` and
`2026-07-06-structural-block-duotone-swiss.md`), since those reference the
old name and `lux-design-system` URLs repeatedly. Both Structural Block
plans get patched to the new name as part of this work (see §5).

## 1. Purpose

Three related changes, all landing before any further Structural Block
work:

1. **Cream-hue unification.** Tri-Swiss's cream family is a slightly
   different hue from Duotone Swiss's. Unify by adopting Duotone Swiss's
   cream everywhere in Tri-Swiss.
2. **Rename Duotone Swiss → Lux Swiss**, including the GitHub repo, the
   plugin slug, and every reference across both repos and the marketplace
   — a fuller, more personally-branded name than a purely technical
   color-count descriptor, parallel in structure to "Tri-Swiss."
3. **House-mark framing.** Both systems get explicit copy identifying
   them as siblings — the two design systems that carry Lux Solari's
   personal house mark — in each repo's README, SKILL.md, and a new
   shared `docs/HOUSE-MARK.md` present in both.

## 2. Non-goals

- Not a change to Tri-Swiss's Swiss Red, Pastel Turquoise, or any other
  non-cream token.
- Not a change to Lux Swiss's (current Duotone Swiss's) own cream values —
  Tri-Swiss adopts them; Lux Swiss's theme.css/SKILL.md palette tables are
  untouched by §1.
- Not a change to either system's component semantics, typography, or the
  Structural Block work already spec'd/planned — this rebrand is a
  prerequisite, not a replacement.
- Not a silent breaking change. The rename is documented as a `BREAKING
  CHANGE` in Lux Swiss's `CHANGELOG.md` and bumps that plugin to `v2.0.0`
  per whiting's semver discipline (existing `/plugin install
  lux-design-system@lux-solari-plugins` commands break).

## 3. Cream-hue unification (Tri-Swiss only)

Adopt Duotone Swiss's cream family exactly:

| Token | Tri-Swiss today | New value (= Lux Swiss's existing value) |
|-------|------------------|-------------------------------------------|
| `--background` (light) | `#eae8d0` | `#f5efe0` |
| `--card` (light) | `#f1efdb` | `#faf6ec` |
| `--muted` (light) | `#e2dfc7` | `#ebe5d5` |
| `--input` (light) | `#f1efdb` | `#faf6ec` (mirrors `--card`, as today) |
| `--foreground` (dark) | `#eae8d0` | `#f5efe0` |
| `--border` (dark) | `#eae8d0` | `#f5efe0` (mirrors `--foreground`, as today) |

`--muted-foreground` (both modes) is a grey/label tone blended toward ink,
not the cream hue itself — **left unchanged** (Tri-Swiss `#4a4838`/
`#a8a696` are close to but distinct from Lux Swiss's `#4a4a48`/`#a8a8a0`,
and the user's request was specifically about the cream hue).

Files touched: `skills/tri-swiss/assets/theme.css`, `skills/tri-swiss/SKILL.md`
(palette tables), `docs/index.html` (inline `:root`/`.dark` CSS vars),
`scripts/capture/verify-philosophy.mjs` (palette allowlist — remove the 3
old hexes, add the 3 new ones; this is the one legitimate case where this
plan touches that file, since the change is exactly what the allowlist
exists to gate).

## 4. Rename: Duotone Swiss → Lux Swiss

"Duotone" survives as a tagline/descriptor, not the name itself — e.g.
"Lux Swiss — a duotone design system," mirroring how "Tri-Swiss" doesn't
spell out "tri-tone" in its bare name either.

**Cascades to (in `lux-design-system`, to be renamed `lux-swiss`):**

- GitHub repo: `luxsolari/lux-design-system` → `luxsolari/lux-swiss` (via
  `gh repo rename`, which also updates the local clone's `origin` remote
  URL automatically and GitHub Pages continues serving from the renamed
  repo — GitHub sets up an automatic redirect from the old URL).
- `.claude-plugin/plugin.json` — `name` field: `lux-design-system` →
  `lux-swiss`.
- Skill directory: `skills/lux-design-system/` → `skills/lux-swiss/` (via
  `git mv`, preserving history), including its `SKILL.md` frontmatter
  `name:` field.
- Every "Duotone Swiss" / "Duotone" / "lux-design-system" string across:
  `SKILL.md`, `references/components.md`, `README.md`, `CONTRIBUTING.md`,
  `CHANGELOG.md` (new `BREAKING CHANGE` entry, version bump to `2.0.0`),
  `docs/index.html` (`<title>`, meta description, og/twitter tags
  including the `luxsolari.github.io/lux-design-system` URLs, nav
  wordmark, hero copy, social-card text, GitHub links).
- `scripts/capture/verify-philosophy.mjs`'s hardcoded `og:image` URL check
  (currently asserts the `lux-design-system` Pages path) updates to the
  new `lux-swiss` path.

**Cascades to (in `lux-solari-plugins`, the marketplace repo):**

- `.claude-plugin/marketplace.json`'s `lux-design-system` entry: `name` →
  `lux-swiss`, `homepage`/`repository`/`source.repo` → the renamed repo
  URL, `description` updated to the new name/tagline.
- `README.md`'s corresponding plugin section and install command
  (`claude plugin install lux-design-system@lux-solari-plugins` →
  `claude plugin install lux-swiss@lux-solari-plugins`).

**Not renamed:** the marketplace repo itself, and Tri-Swiss's own name/repo
(unaffected by this rename beyond the house-mark copy in §6 referencing
its sibling's new name).

## 5. Patching the Structural Block plans

Both already-written Structural Block plans reference the old name/repo
path and must be patched (not re-brainstormed — this is a mechanical
find-replace against the plans' own text) before their tasks are
dispatched:

- `docs/superpowers/plans/2026-07-06-structural-block-tri-swiss.md`
  (Tri-Swiss repo): no renames needed for Tri-Swiss itself, but any prose
  that names its sibling ("Duotone Swiss") should say "Lux Swiss" instead.
- `docs/superpowers/plans/2026-07-06-structural-block-duotone-swiss.md`
  (in the `lux-design-system`/`lux-swiss` repo): update every "Duotone
  Swiss"/wordmark/GitHub-link/URL reference in its concrete HTML/CSS code
  blocks (the sidebar wordmark currently reads "Duotone," the GitHub link
  points at `luxsolari/lux-design-system`, etc.) to the new name and repo
  path. The plan's own filename can stay as-is (historical record of when
  it was written) or be renamed for clarity — implementer's call, not
  load-bearing.

## 6. House-mark framing

Each repo's `README.md` gets a short paragraph near the top (after the
title/badges, before or as part of the existing intro paragraph) naming
the sibling system and framing both as the two house-mark design systems
that carry Lux Solari's personal brand identity. Each `SKILL.md` gets the
equivalent framing folded into its own opening overview paragraph (the one
`SKILL.md`'s frontmatter/`# <Name> — Design System` intro right after the
title), so the skill itself carries this context, not just human-facing
docs.

A new `docs/HOUSE-MARK.md` (identical content, present in both repos)
explains the concept in more depth: what a house mark is, why there are
two related-but-distinct systems (Tri-Swiss's tri-tone-plus-Structural-
Block palette vs. Lux Swiss's strict duotone-plus-Structural-Block
palette), and cross-links to the sibling repo. Exact prose is an
implementation-time writing task guided by this framing, not enumerated
verbatim here — the two systems' own README/SKILL.md content already
establishes each one's voice and level of formality to match.

## 7. Rollout

Two implementation plans (mirroring the Structural Block split), each
landing as **new, earlier tasks on the same already-created branch** as
that repo's Structural Block plan — not a separate branch. Neither branch
has been pushed or opened as a PR yet, so there's no shared/external state
to reconcile, and executing rebrand-then-Structural-Block as one
continuous commit sequence avoids a real merge-conflict risk a separate
branch would create (both touch `docs/index.html`, `SKILL.md`,
`theme.css`). One PR per repo at the end covers both pieces of work.

1. **Tri-Swiss** — new tasks on `feat/expand-accents-drop-jost` (this
   repo's existing branch), executed **before** that plan's own
   Structural Block tasks. Covers §3 (cream) and §6 (house-mark copy,
   Tri-Swiss's half).
2. **Lux Swiss** (`lux-design-system` → `lux-swiss` repo) — new tasks on
   `feat/structural-block-and-weight-highlight` (already created, only the
   spec+plan committed so far, no implementation started), executed
   **before** that plan's own Structural Block tasks. Covers §4 (the
   rename, including the `gh repo rename` operation) and §6 (house-mark
   copy, Lux Swiss's half).
3. **Marketplace repo** — a small, direct, controller-executed update (no
   full SDD plan needed — same treatment as the original marketplace
   registration PR earlier this session): branch, edit
   `marketplace.json`/`README.md`, PR.

After all three land, §5's plan-patching happens on each repo's existing
Structural Block plan document before dispatching any of its implementers
(the plan text itself needs the name/URL fixes even though its tasks now
execute after, not instead of, the rebrand tasks).

## 8. Open items for implementation (explicitly deferred, not blocking)

- Exact house-mark prose (§6) is a writing task at implementation time,
  guided by this spec's framing but not pre-written here — each system's
  existing README/SKILL.md voice should carry through.
- Whether `gh repo rename` requires re-authenticating GitHub Pages'
  custom-domain/HTTPS settings (unlikely, since Pages here uses the
  default `github.io` domain, not a custom domain) — confirm during
  implementation; if anything needs re-enabling post-rename, do so as
  part of that task.
- The exact wording of the `BREAKING CHANGE` footer/commit and the
  `2.0.0` version bump in `lux-design-system`'s `plugin.json` — derive the
  new version with `scripts/suggest_version_bump.py` at implementation
  time rather than hand-picking it here, per that repo's own whiting
  convention (semver is derived from commits, never hand-edited).
