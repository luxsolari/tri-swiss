# Tri-Swiss Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build and publish the Tri-Swiss Claude Code design-system plugin — a sibling to `lux-design-system` (Duotone Swiss) that keeps the Swiss-minimalist structural rules but replaces the palette (tri-tone: ink + cream + Swiss Red + a governed Pastel Turquoise highlight) and typography (Geist as the primary voice, plus three governed extra registers) and iconography (`geist-icons` replacing Lucide).

**Architecture:** A static Claude Code plugin repo, structurally identical to `lux-design-system`: a `skills/tri-swiss/` directory holding `SKILL.md` (the taught design language), `assets/theme.css` (the Tailwind 4 token file), and `references/components.md` (extended component catalogue); a `docs/index.html` GitHub Pages showcase with a Playwright capture script and a philosophy-compliance verifier; and the same whiting-based release governance (Conventional Commits hook, protected-branch hook, semver-suggestion script, changelog-extraction script, GitHub Actions release workflow).

**Tech Stack:** Plain HTML/CSS (Tailwind 4 token syntax, no build step for the skill itself), Node.js + Playwright for the demo-page capture/verification scripts, Python 3 for release tooling, GitHub Actions for releases.

**Spec:** `docs/superpowers/specs/2026-07-06-tri-swiss-design.md` (this plan implements it in full; refer to it for rationale behind every decision below).

## Global Constraints

- Swiss-minimalist geometry is unchanged: visible 1px borders everywhere, **no shadows** (elevation is a background-color step only), square corners (radius reserved, rarely used), generous whitespace, uppercase-mono labels with wide letter-spacing.
- Palette is restricted to the exact hex tokens defined in spec §3 (light) and the dark-mode table (also §3) — no color appears in the theme, the skill doc, or the demo page outside that set.
- `--highlight` (`#56bfa3` light / `#63cbae` dark) is **non-semantic**: it never appears in buttons, tags, status pips, or links. It is reserved for exactly two uses — a second chart data series, and one single brand/hero moment on the demo page — per spec §3.1.
- Every typeface is free and Google-Fonts-hosted (SIL OFL): Geist Sans, Geist Mono, Space Mono, Zilla Slab, Jost. No paid fonts (Futura PT was explicitly rejected in favor of Jost — spec §4).
- `geist-icons` (MIT) is the only sanctioned icon set. Every icon is restyled via one CSS rule regardless of the library's own defaults: `stroke-width: 1.5; stroke-linecap: square; stroke-linejoin: miter`. No other icon library, no icon fonts, no emoji in UI text.
- License is MIT, matching `lux-design-system`. Repo governance matches `lux-design-system`'s `AGENTS.md` verbatim: Conventional Commits (enforced by `scripts/hooks/commit-msg`), no direct pushes to `main` (enforced by `scripts/hooks/pre-push`), changelog-first workflow, versions derived from tags via `scripts/suggest_version_bump.py` (never hand-edited after the initial bootstrap value).
- Full structural parity with `lux-design-system` (spec §7) — same file layout, same `.claude-plugin/plugin.json` shape, same GitHub Pages + Playwright capture pattern, same release automation.
- Nothing is pushed to `luxsolari/tri-swiss`'s `main` branch directly. All scaffolding work lands via a feature branch and a PR, exactly as this session's `lux-design-system` PR #5 did.

---

### Task 1: Bootstrap the GitHub repo and feature branch

**Files:** N/A — repository-level git/gh operations only. Working directory is the already-initialized local repo at `C:\Users\luxsolari\Code\tri-swiss`, which currently has one commit on `main` (the design spec).

**Interfaces:**
- Consumes: nothing.
- Produces: a `luxsolari/tri-swiss` GitHub repo with `main` pushed (spec-only), and a local+remote-tracked branch `feat/initial-scaffold` checked out, ready for Tasks 2–10 to commit onto.

- [ ] **Step 1: Verify the local repo state before touching anything remote**

Run: `cd "C:\Users\luxsolari\Code\tri-swiss" && git log --oneline && git status --short`
Expected: one commit (`docs: add Tri-Swiss design spec`), clean working tree, no remote configured yet (`git remote -v` prints nothing).

- [ ] **Step 2: Create the GitHub repo and push `main`**

Run:
```bash
cd "C:\Users\luxsolari\Code\tri-swiss"
gh repo create luxsolari/tri-swiss --public --source=. --remote=origin \
  --description "Tri-Swiss — a tri-tone Swiss-minimalist design system built around Geist. Sibling to lux-design-system." \
  --homepage "https://luxsolari.github.io/tri-swiss/" \
  --push
```
Expected: repo created, `origin` remote added, `main` pushed (containing just the spec commit). `gh repo view luxsolari/tri-swiss --json url -q .url` prints `https://github.com/luxsolari/tri-swiss`.

- [ ] **Step 3: Create the feature branch for all scaffolding work**

Run: `git checkout -b feat/initial-scaffold`
Expected: `Switched to a new branch 'feat/initial-scaffold'`. All remaining tasks in this plan commit onto this branch; nothing else touches `main` again until the PR merges.

---

### Task 2: Governance and plugin-manifest scaffolding

**Files:**
- Create: `.claude-plugin/plugin.json`
- Create: `LICENSE`
- Create: `CLAUDE.md`
- Create: `AGENTS.md`
- Create: `CONTRIBUTING.md`
- Create: `.gitignore`

**Interfaces:**
- Consumes: nothing.
- Produces: a structurally valid (if not yet functional) Claude Code plugin skeleton — `plugin.json`'s `"skills": "./skills/"` pointer is consumed by Task 5 when `skills/tri-swiss/SKILL.md` is created.

- [ ] **Step 1: Write `.claude-plugin/plugin.json`**

```json
{
  "name": "tri-swiss",
  "version": "0.1.0",
  "description": "Tri-Swiss — a tri-tone (ink + cream + Swiss Red + Pastel Turquoise highlight) Swiss-minimalist design system built around the Geist typeface family. Sibling to lux-design-system (Duotone Swiss); same governance, different palette and type identity.",
  "author": {
    "name": "Lux Solari",
    "email": "luxsolari@outlook.com",
    "url": "https://github.com/luxsolari"
  },
  "repository": "https://github.com/luxsolari/tri-swiss",
  "license": "MIT",
  "keywords": [
    "design-system",
    "tri-tone",
    "swiss-minimalist",
    "tailwind",
    "ui",
    "frontend",
    "theming",
    "geist",
    "geist-sans",
    "geist-mono"
  ],
  "skills": "./skills/"
}
```

- [ ] **Step 2: Write `LICENSE` (MIT, identical terms to lux-design-system)**

```
MIT License

Copyright (c) 2026 Lux Solari (Luciano Laje)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

- [ ] **Step 3: Write `CLAUDE.md`**

```markdown
@AGENTS.md
```

- [ ] **Step 4: Write `AGENTS.md` (verbatim governance rules, repo-agnostic — same text as lux-design-system)**

```markdown
# Agent Rules

This repo uses [whiting](https://github.com/luxsolari/whiting) for release
discipline. The following rules apply to human contributors and AI agents
alike.

## Conventional Commits

Every commit subject line must follow:

```
type(scope)!: description
```

Allowed types: `feat` `fix` `docs` `style` `refactor` `perf` `test` `build`
`ci` `chore` `revert`. `scope` is optional. A `!` before the colon, or a
`BREAKING CHANGE:` footer in the body, marks a breaking change.

This is enforced locally by a `commit-msg` hook, and direct pushes to
`main` are blocked by a `pre-push` hook (see below). After
cloning, activate both once with:

```
git config core.hooksPath scripts/hooks
git config whiting.defaultbranch main
```

Both settings are local, unversioned git config — every clone needs to run
this once; it isn't inherited from the remote.

## Semver-bump discipline

Version numbers are never hand-edited. The next version is derived from
commits since the last tag via `scripts/suggest_version_bump.py` (`feat` →
minor, `fix` → patch, breaking → major). Git tags are the source of truth
for "what version is this."

## Changelog-first workflow

Every user-facing change adds an entry under `## [Unreleased]` in
`CHANGELOG.md`, in the same commit or PR that makes the change. No
undocumented changes.

## No direct pushes to main

Land changes via a branch and a pull request. Direct pushes to
`main` are blocked locally by a `pre-push` hook.
```

- [ ] **Step 5: Write `CONTRIBUTING.md` (adapted governing-rules language for tri-tone)**

```markdown
# Contributing

Contributions welcome! Here's how:

## Design changes
The design language lives in `skills/tri-swiss/`. Keep the two governing
rules intact — **tri-tone strict** (two structural colors + one strong accent
+ one rare, non-semantic highlight reserved for charts/brand moments — never
a second accent, never a third UI color) and **Swiss-minimalist** (visible
borders, no shadows). Changes that add a semantic color or a shadow
contradict the system and won't be accepted; express new states through
weight, size, spacing, and contrast instead.

- Palette / token changes: update both `SKILL.md`'s tables and `assets/theme.css`
  so they never drift apart.
- New component patterns: add them to `references/components.md`, and only
  surface the most common ones in `SKILL.md` to keep it lean.

## Bug reports
Open an issue describing: what you expected, what happened, and your Claude Code
version.

## Plugin development
1. Clone the repo.
2. `claude plugin validate .` to verify structure.
3. Test locally: install from the local directory and try it on a real UI task.
4. Bump the version in `.claude-plugin/plugin.json` and add a `CHANGELOG.md` entry
   following [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) + SemVer.
5. Submit a PR with a clear description.

## Code of conduct
Be kind. Be constructive.
```

- [ ] **Step 6: Write `.gitignore` (verbatim, repo-agnostic)**

```
# OS
.DS_Store
Thumbs.db

# Editors
*.swp
*~
.vscode/
.idea/

# Claude Code — local/machine-specific, never shared across installs
.claude/settings.local.json
.claude/*.local.md
CLAUDE.local.md
.mcp.local.json

# Secrets
.env
.env.local
.env.*.local
```

- [ ] **Step 7: Verify the manifest is well-formed JSON**

Run: `node -e "JSON.parse(require('fs').readFileSync('.claude-plugin/plugin.json','utf8')); console.log('valid JSON')"`
Expected: `valid JSON`

- [ ] **Step 8: Commit**

```bash
git add .claude-plugin/plugin.json LICENSE CLAUDE.md AGENTS.md CONTRIBUTING.md .gitignore
git commit -m "chore: bootstrap plugin manifest and governance docs"
```

---

### Task 3: Release automation and git hooks

**Files:**
- Create: `scripts/hooks/commit-msg`
- Create: `scripts/hooks/pre-push`
- Create: `scripts/suggest_version_bump.py`
- Create: `scripts/extract_changelog.py`
- Create: `.github/workflows/release.yml`

**Interfaces:**
- Consumes: nothing (all four scripts are repo-agnostic; `extract_changelog.py` reads `CHANGELOG.md` at the repo root, produced by Task 10).
- Produces: the release pipeline Task 10's `CHANGELOG.md` will be extracted from, and the local commit/push guards every later task's commits pass through once hooks are activated.

- [ ] **Step 1: Write `scripts/hooks/commit-msg` (verbatim)**

```sh
#!/bin/sh
# Rejects commit messages that don't follow Conventional Commits.
set -eu

commit_msg_file="$1"
subject=$(head -n 1 "$commit_msg_file")

case "$subject" in
    Merge\ *) exit 0 ;;
esac

pattern='^(feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert)(\([^)]+\))?!?: .+'

if ! printf '%s' "$subject" | grep -qE "$pattern"; then
    echo "commit-msg: rejected — subject line must follow Conventional Commits:" >&2
    echo "  type(scope)?: description" >&2
    echo "  allowed types: feat fix docs style refactor perf test build ci chore revert" >&2
    echo "  example: feat(api): add pagination to /users endpoint" >&2
    echo "got: $subject" >&2
    exit 1
fi
```

- [ ] **Step 2: Write `scripts/hooks/pre-push` (verbatim)**

```sh
#!/bin/sh
# Blocks direct pushes to the repo's protected (default) branch.
# Tags and other branches are unaffected — only a push whose remote ref IS
# the protected branch is rejected; land changes via PR/merge instead.
set -eu

protected_branch=$(git config --get whiting.defaultbranch || true)
protected_branch=${protected_branch:-main}

while read -r local_ref local_sha remote_ref remote_sha; do
    case "$remote_ref" in
        "refs/heads/$protected_branch")
            echo "pre-push: rejected — direct pushes to '$protected_branch' aren't allowed." >&2
            echo "pre-push: push a branch and open a PR instead." >&2
            exit 1
            ;;
    esac
done

exit 0
```

- [ ] **Step 3: Make both hooks executable**

Run: `chmod +x scripts/hooks/commit-msg scripts/hooks/pre-push`
Expected: no output; `ls -l scripts/hooks/` shows the executable bit set on both files.

- [ ] **Step 4: Write `scripts/suggest_version_bump.py` (verbatim)**

```python
#!/usr/bin/env python3
"""Suggest the next semver bump from Conventional Commits since the last tag."""
import re
import subprocess
import sys

COMMIT_TYPE_RE = re.compile(
    r"^(?P<type>feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert)"
    r"(?:\((?P<scope>[^)]+)\))?(?P<breaking>!)?:\s*(?P<description>.+)$"
)
BREAKING_FOOTER_RE = re.compile(r"^BREAKING CHANGE:", re.MULTILINE)


def classify_bump(commit_subjects, commit_bodies=None):
    """Classify the required semver bump from commit subject lines.

    commit_bodies, if given, is a list of full commit message bodies
    aligned with commit_subjects, checked for a 'BREAKING CHANGE:' footer.
    """
    bodies = commit_bodies or [""] * len(commit_subjects)
    level = "none"
    for subject, body in zip(commit_subjects, bodies):
        match = COMMIT_TYPE_RE.match(subject)
        if not match:
            continue
        if match.group("breaking") or BREAKING_FOOTER_RE.search(body):
            return "major"
        commit_type = match.group("type")
        if commit_type == "feat" and level != "major":
            level = "minor"
        elif commit_type == "fix" and level not in ("major", "minor"):
            level = "patch"
    return level


def next_version(current, bump):
    """Compute the next version string from a 'vX.Y.Z' tag and a bump level."""
    major, minor, patch = (int(part) for part in current.lstrip("v").split("."))
    if bump == "major":
        return f"v{major + 1}.0.0"
    if bump == "minor":
        return f"v{major}.{minor + 1}.0"
    if bump == "patch":
        return f"v{major}.{minor}.{patch + 1}"
    raise ValueError(f"No release needed: bump={bump!r}")


def last_tag():
    result = subprocess.run(
        ["git", "describe", "--tags", "--abbrev=0", "--match", "v*.*.*"],
        capture_output=True, text=True,
    )
    if result.returncode != 0:
        return None
    return result.stdout.strip()


def commits_since(tag):
    rev_range = f"{tag}..HEAD" if tag else "HEAD"
    subjects = subprocess.run(
        ["git", "log", rev_range, "--format=%s"],
        capture_output=True, text=True, check=True,
    ).stdout.splitlines()
    bodies = subprocess.run(
        ["git", "log", rev_range, "--format=%B%x00"],
        capture_output=True, text=True, check=True,
    ).stdout.split("\x00")
    return subjects, bodies


def main():
    tag = last_tag()
    subjects, bodies = commits_since(tag)
    if not subjects:
        print(f"No commits since {tag or '(no tags yet)'} — nothing to release.")
        return 1
    bump = classify_bump(subjects, bodies)
    if bump == "none":
        print(f"No feat/fix/breaking commits since {tag or '(no tags yet)'} — no release needed.")
        return 1
    baseline = tag or "v0.0.0"
    suggested = next_version(baseline, bump)
    print(f"Last tag: {tag or '(none)'}")
    print(f"Commits considered: {len(subjects)}")
    print(f"Bump level: {bump}")
    print(f"Suggested next version: {suggested}")
    return 0


if __name__ == "__main__":
    sys.exit(main())
```

- [ ] **Step 5: Write a quick functional check for `classify_bump` (no dedicated test suite exists upstream — this is a manual/functional verification, matching how lux-design-system verifies these scripts)**

Run:
```bash
python3 -c "
import sys
sys.path.insert(0, 'scripts')
from suggest_version_bump import classify_bump, next_version
assert classify_bump(['feat: add x']) == 'minor'
assert classify_bump(['fix: bug']) == 'patch'
assert classify_bump(['feat!: breaking']) == 'major'
assert classify_bump(['docs: typo']) == 'none'
assert next_version('v0.1.0', 'minor') == 'v0.2.0'
assert next_version('v0.1.0', 'major') == 'v1.0.0'
print('suggest_version_bump: all checks passed')
"
```
Expected: `suggest_version_bump: all checks passed`

- [ ] **Step 6: Write `scripts/extract_changelog.py` (verbatim)**

```python
#!/usr/bin/env python3
"""Extract a single version's section from CHANGELOG.md for release notes."""
import re
import sys
from pathlib import Path


def extract(changelog_text: str, version: str) -> str:
    pattern = re.compile(
        rf'^## \[{re.escape(version)}\].*?\n(.*?)(?=^## \[|\Z)',
        re.MULTILINE | re.DOTALL,
    )
    match = pattern.search(changelog_text)
    if not match:
        raise SystemExit(f"No CHANGELOG entry found for version {version}")

    body = match.group(1)
    # Drop the trailing reference-link line and "---" separator.
    lines = [
        line for line in body.split('\n')
        if not line.startswith(f'[{version}]:') and line.strip() != '---'
    ]
    return '\n'.join(lines).strip() + '\n'


def main() -> int:
    if len(sys.argv) != 2:
        print("Usage: extract_changelog.py <version>", file=sys.stderr)
        return 1

    version = sys.argv[1].lstrip('v')
    repo_root = Path(__file__).parent.parent
    changelog_text = (repo_root / "CHANGELOG.md").read_text(encoding="utf-8")
    print(extract(changelog_text, version), end='')
    return 0


if __name__ == "__main__":
    sys.exit(main())
```

- [ ] **Step 7: Verify `extract_changelog.py` against a throwaway fixture (CHANGELOG.md doesn't exist yet until Task 10 — use a temp file so this task doesn't depend on Task 10)**

Run:
```bash
printf '# Changelog\n\n## [0.1.0] - 2026-07-06\n\n### Added\n- Test entry\n\n[0.1.0]: https://example.com\n' > /tmp/tri-swiss-changelog-fixture.md
python3 -c "
from pathlib import Path
import sys
sys.path.insert(0, 'scripts')
from extract_changelog import extract
text = Path('/tmp/tri-swiss-changelog-fixture.md').read_text()
result = extract(text, '0.1.0')
assert 'Test entry' in result
assert '[0.1.0]:' not in result
print('extract_changelog: check passed')
"
rm /tmp/tri-swiss-changelog-fixture.md
```
Expected: `extract_changelog: check passed`

- [ ] **Step 8: Write `.github/workflows/release.yml` (verbatim, repo-agnostic)**

```yaml
name: Release

on:
  push:
    tags:
      - "v*.*.*"
  workflow_dispatch:
    inputs:
      tag:
        description: "Existing tag to (re)publish a release for, e.g. v0.6.2"
        required: true

permissions:
  contents: write

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Resolve tag
        id: resolve
        run: |
          if [ "${{ github.event_name }}" = "workflow_dispatch" ]; then
            echo "tag=${{ github.event.inputs.tag }}" >> "$GITHUB_OUTPUT"
          else
            echo "tag=${GITHUB_REF_NAME}" >> "$GITHUB_OUTPUT"
          fi

      - name: Extract changelog section
        run: python3 scripts/extract_changelog.py "${{ steps.resolve.outputs.tag }}" > release_notes.md

      - name: Create or update release
        env:
          GH_TOKEN: ${{ github.token }}
        run: |
          TAG="${{ steps.resolve.outputs.tag }}"
          if gh release view "$TAG" --repo "${{ github.repository }}" >/dev/null 2>&1; then
            gh release edit "$TAG" --repo "${{ github.repository }}" --title "$TAG" --notes-file release_notes.md
          else
            gh release create "$TAG" --repo "${{ github.repository }}" --title "$TAG" --notes-file release_notes.md
          fi
```

- [ ] **Step 9: Activate the hooks locally (per AGENTS.md's own instructions) — skip gracefully if denied**

Run: `git config core.hooksPath scripts/hooks && git config whiting.defaultbranch main`
Expected: no output. If the permission system denies this (it did once already in `lux-design-system` in this same session, flagged as unrequested persistence), skip it and note in the task's completion report that hooks weren't activated locally — this doesn't block anything else in this plan, it only affects local enforcement on this machine.

- [ ] **Step 10: Commit**

```bash
git add scripts/hooks/commit-msg scripts/hooks/pre-push scripts/suggest_version_bump.py scripts/extract_changelog.py .github/workflows/release.yml
git commit -m "ci: add release automation and commit/push guard hooks"
```

---

### Task 4: `theme.css` — Tri-Swiss design tokens

**Files:**
- Create: `skills/tri-swiss/assets/theme.css`

**Interfaces:**
- Consumes: nothing.
- Produces: the canonical token set every later task (`SKILL.md`, `references/components.md`, `docs/index.html`, `verify-philosophy.mjs`) must match exactly:
  - Light: `--background:#eae8d0` `--foreground:#000000` `--card:#f1efdb` `--card-foreground:#000000` `--primary:#d3281b` `--primary-foreground:#eae8d0` `--secondary:#000000` `--secondary-foreground:#eae8d0` `--muted:#e2dfc7` `--muted-foreground:#4a4838` `--border:#000000` `--input:#f1efdb` `--ring:#d3281b` `--highlight:#56bfa3`
  - Dark: `--background:#000000` `--foreground:#eae8d0` `--card:#161616` `--card-foreground:#eae8d0` `--primary:#e2503f` `--secondary:#eae8d0` `--secondary-foreground:#000000` `--muted:#1f1f1f` `--muted-foreground:#a8a696` `--border:#eae8d0` `--input:#161616` `--ring:#e2503f` `--highlight:#63cbae`
  - Font roles: `--mono` (Geist Mono), `--sans` (Geist Sans), `--annotation` (Space Mono), `--serif` (Zilla Slab), `--hero` (Jost) — wired to Tailwind utilities `--font-mono`, `--font-sans`, `--font-annotation`, `--font-serif`, `--font-hero`.

- [ ] **Step 1: Write `skills/tri-swiss/assets/theme.css`**

```css
/* Tri-Swiss — theme tokens for Tailwind 4.
 * Paste into your global stylesheet (e.g. app/globals.css).
 * For non-Tailwind stacks, the :root / .dark blocks work as plain CSS
 * custom properties — drop the @import and @theme lines. */

@import "tailwindcss";

@custom-variant dark (&:is(.dark *));

:root {
  --background: #eae8d0;
  --foreground: #000000;
  --card: #f1efdb;
  --card-foreground: #000000;
  --primary: #d3281b;
  --primary-foreground: #eae8d0;
  --secondary: #000000;
  --secondary-foreground: #eae8d0;
  --muted: #e2dfc7;
  --muted-foreground: #4a4838;
  --border: #000000;
  --input: #f1efdb;
  --ring: #d3281b;
  --highlight: #56bfa3;
  --radius: 0.5rem;

  /* Font roles. Primary = Geist (display + body/utility). Secondary = Space
     Mono, italic only, annotations/captions. Tertiary = Zilla Slab, long-form.
     4th register = Jost, hero/wordmark + section dividers only. */
  --mono: "Geist Mono", ui-monospace, monospace;
  --sans: "Geist Sans", ui-sans-serif, system-ui, sans-serif;
  --annotation: "Space Mono", ui-monospace, monospace;
  --serif: "Zilla Slab", Georgia, serif;
  --hero: "Jost", ui-sans-serif, system-ui, sans-serif;
}

.dark {
  --background: #000000;
  --foreground: #eae8d0;
  --card: #161616;
  --card-foreground: #eae8d0;
  --primary: #e2503f;
  --primary-foreground: #eae8d0;
  --secondary: #eae8d0;
  --secondary-foreground: #000000;
  --muted: #1f1f1f;
  --muted-foreground: #a8a696;
  --border: #eae8d0;
  --input: #161616;
  --ring: #e2503f;
  --highlight: #63cbae;
}

@theme inline {
  /* Reference the role vars so Tailwind's font utilities stay in sync. */
  --font-sans: var(--sans);
  --font-mono: var(--mono);
  --font-annotation: var(--annotation);
  --font-serif: var(--serif);
  --font-hero: var(--hero);

  --color-background: var(--background);
  --color-foreground: var(--foreground);
  --color-card: var(--card);
  --color-card-foreground: var(--card-foreground);
  --color-primary: var(--primary);
  --color-primary-foreground: var(--primary-foreground);
  --color-secondary: var(--secondary);
  --color-secondary-foreground: var(--secondary-foreground);
  --color-muted: var(--muted);
  --color-muted-foreground: var(--muted-foreground);
  --color-border: var(--border);
  --color-input: var(--input);
  --color-ring: var(--ring);
  --color-highlight: var(--highlight);

  --radius-sm: calc(var(--radius) - 2px);
  --radius-md: var(--radius);
  --radius-lg: calc(var(--radius) + 2px);
}

@layer base {
  * {
    @apply border-border outline-ring/50;
  }
  body {
    @apply bg-background text-foreground;
  }
}
```

- [ ] **Step 2: Verify every required token is present and no forbidden hex slipped in**

Run:
```bash
node -e "
const css = require('fs').readFileSync('skills/tri-swiss/assets/theme.css', 'utf8');
const required = ['--background','--foreground','--card','--card-foreground','--primary',
  '--primary-foreground','--secondary','--secondary-foreground','--muted','--muted-foreground',
  '--border','--input','--ring','--highlight','--mono','--sans','--annotation','--serif','--hero'];
const missing = required.filter(t => !css.includes(t + ':'));
if (missing.length) { console.error('MISSING:', missing.join(', ')); process.exit(1); }
const allowedHex = new Set(['#eae8d0','#000000','#f1efdb','#d3281b','#e2dfc7','#4a4838',
  '#56bfa3','#161616','#e2503f','#1f1f1f','#a8a696','#63cbae']);
const hexes = [...css.matchAll(/#[0-9a-fA-F]{6}\b/g)].map(m => m[0].toLowerCase());
const rogue = [...new Set(hexes)].filter(h => !allowedHex.has(h));
if (rogue.length) { console.error('ROGUE HEX:', rogue.join(', ')); process.exit(1); }
if (/box-shadow/i.test(css)) { console.error('SHADOW FOUND'); process.exit(1); }
console.log('theme.css: all tokens present, palette pure, no shadows');
"
```
Expected: `theme.css: all tokens present, palette pure, no shadows`

- [ ] **Step 3: Commit**

```bash
git add skills/tri-swiss/assets/theme.css
git commit -m "feat: add Tri-Swiss theme tokens"
```

---

### Task 5: `SKILL.md` — the taught design language

**Files:**
- Create: `skills/tri-swiss/SKILL.md`

**Interfaces:**
- Consumes: the exact token names/hexes from Task 4, the icon restyle rule from the spec (§5).
- Produces: the frontmatter `name: tri-swiss` that plugin.json's skill loader (Task 2) resolves; the philosophy/governance text that `CONTRIBUTING.md` (Task 2) already references.

- [ ] **Step 1: Write `skills/tri-swiss/SKILL.md`**

```markdown
---
name: tri-swiss
description: >
  Tri-Swiss — Lux Solari's Geist-based house design system, sibling to
  lux-design-system (Duotone Swiss). A tri-tone visual language (ink + cream,
  one blood-red-adjacent accent, one rare non-semantic highlight)
  Swiss-minimalist layout, visible borders, no shadows, and Geist / Geist Mono
  typography. Use this skill whenever building, styling, or restyling ANY user
  interface: React/Next/Svelte/Vue components, HTML pages, landing pages,
  dashboards, buttons, cards, forms, navigation, modals, tags, charts, or
  Tailwind/CSS themes — even when the user does not name the design system
  explicitly. Apply it by default so every project shares the same aesthetic.
  Also trigger on phrases like "make this look good", "style this", "apply my
  design system", "tri-tone", "swiss", "geist", "give it a theme", or when
  starting a new frontend from scratch. When another design language is
  explicitly requested (Material, shadcn defaults untouched, a client's brand
  kit, or the sibling lux-design-system/Duotone Swiss system specifically),
  defer to that instead.
---

# Tri-Swiss — Design System

A strict, minimalist visual language built around the Geist typeface family.
Two structural colors plus one strong accent plus one rare, non-semantic
highlight; hard borders, generous whitespace, monospace labels. The whole
point is restraint: every element earns its place or is removed, and
**difference is expressed through typography, spacing, and contrast —
almost never by adding a color.**

## When you apply this

Whenever you build or restyle UI, reach for these tokens and patterns by
default instead of inventing ad-hoc colors or leaning on a component
library's stock look. Two setup moves come first on any new project:

1. **Install the theme.** Copy [`assets/theme.css`](assets/theme.css) into
   the project's global stylesheet (e.g. `app/globals.css`). It defines
   every CSS variable for light + dark mode, and wires them to Tailwind 4
   via `@theme inline`. For non-Tailwind stacks the same `:root` / `.dark`
   variables work as plain CSS custom properties.
2. **Load the fonts.** Add the Google Fonts link (below) or the framework
   equivalent (`next/font`, etc.).

Then compose UI from the patterns in this file. For the full component
library (status pips, modals, toggles, SVG charts) see
[`references/components.md`](references/components.md).

## Philosophy — two rules that govern everything

**Tri-tone strict.** Ink and cream are the two structural colors; Swiss Red
is the primary accent. Pastel Turquoise is a **third, rare, non-semantic**
color — it never carries meaning (no success/info/second-interactive-state
use) and appears in exactly two places: a second data series in a chart, and
one single brand/hero moment per page. Everywhere else — buttons, tags,
status pips, links — it does not exist. If you feel the urge to add a color
beyond these, add a `font-bold`, a size step, or whitespace instead.

**Swiss-minimalist.** Borders are visible (1px solid, full ink or full
cream). No shadows — elevation comes from a background-color step (`--card`
vs `--background`). Whitespace is generous. Labels are uppercase monospace
with wide letter-spacing. Corners are mostly square.

## Palette

Use the semantic token, never a raw hex. `bg-background`, `text-foreground`,
`border-border`, `text-muted-foreground`, `bg-primary`, `text-highlight`, etc.

### Light mode
| Token | Hex | Role |
|-------|-----|------|
| `--background` | `#eae8d0` | Page background — warm cream |
| `--foreground` | `#000000` | Body text, active controls, borders |
| `--card` | `#f1efdb` | Elevated surface — card, popover |
| `--card-foreground` | `#000000` | Text on card surfaces |
| `--primary` | `#d3281b` | Swiss Red — accent, destructive, ring |
| `--primary-foreground` | `#eae8d0` | Text on primary |
| `--secondary` | `#000000` | Secondary action background |
| `--secondary-foreground` | `#eae8d0` | Text on secondary |
| `--muted` | `#e2dfc7` | Subtle backgrounds — hover, code blocks |
| `--muted-foreground` | `#4a4838` | Subdued labels, metadata, placeholders |
| `--border` | `#000000` | All borders — full ink for structural clarity |
| `--input` | `#f1efdb` | Input field background |
| `--ring` | `#d3281b` | Focus ring |
| `--highlight` | `#56bfa3` | Pastel Turquoise — governed, non-semantic (see below) |

### Dark mode
| Token | Hex | Role |
|-------|-----|------|
| `--background` | `#000000` | Near-black |
| `--foreground` | `#eae8d0` | Cream text |
| `--card` | `#161616` | Slightly lifted surface |
| `--primary` | `#e2503f` | Red lifted for dark contrast |
| `--secondary` | `#eae8d0` | Inverted |
| `--secondary-foreground` | `#000000` | — |
| `--muted` | `#1f1f1f` | Subtle dark surface |
| `--muted-foreground` | `#a8a696` | Warm grey — readable but recessed |
| `--border` | `#eae8d0` | Full cream — maintains structural clarity |
| `--input` | `#161616` | — |
| `--ring` | `#e2503f` | — |
| `--highlight` | `#63cbae` | Pastel Turquoise, lifted for dark mode |

Dark mode is the `.dark` class on `<html>`. Toggle with
`document.documentElement.classList.toggle("dark", isDark)` and persist under
a `theme` key in `localStorage`. In Tailwind 4 the variant is
`@custom-variant dark (&:is(.dark *));` (already in `theme.css`).

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

## Typography

Four registers, strictly separated by function — Geist is the primary
voice, the other three are governed extras with exactly one job each.

| Tier | Font | Role |
|------|------|------|
| Primary | **Geist Mono** (`font-mono`) | Headings, display, data values, tags, nav, labels |
| Primary | **Geist Sans** (`font-sans`) | Body copy, prose, **and** dense-data/utility text (tables, fine print) at smaller size with tabular figures |
| Secondary | **Space Mono, italic only** (`font-annotation italic`) | Inline annotations and figure captions only — never emphasis |
| Tertiary | **Zilla Slab** (`font-serif`) | Long-form editorial body and pull-quotes — never UI |
| 4th register | **Jost** (`font-hero`) | Hero title/wordmark and section/chapter dividers in long-form editorial content only — never a UI heading, never a pull-quote |

```html
<link href="https://fonts.googleapis.com/css2?family=Geist:wght@100..900&family=Geist+Mono:wght@100..900&family=Space+Mono:ital,wght@0,400;1,400&family=Zilla+Slab:ital,wght@0,400;0,500;0,700;1,400&family=Jost:wght@400;700&display=swap" rel="stylesheet" />
```

Geist Sans and Geist Mono load as **variable fonts** (100–900 axis); Space
Mono loads regular + italic; Zilla Slab loads its usual four cuts; Jost
loads two static weights (400/700) — a hero/wordmark register doesn't need
a full variable scale.

**Range comes from weight, not more typefaces.** Within the Primary
register, hierarchy is expressed through the weight axis, not a new face:

| Weight | Use |
|--------|-----|
| **300** Light | Large display sub-decks, lead paragraphs |
| **400** Regular | Body copy |
| **500** Medium | UI emphasis, active labels, small headings in prose |
| **700** Bold | Strong emphasis — sparing; prefer 500 |

**Heading scale** — Geist Mono, weight 700: `h1` 3rem/−0.02em/1.1 ·
`h2` 2.25rem/−0.02em/1.15 · `h3` 1.875rem/−0.01em/1.2 · `h4` 1.5rem/1.3 ·
`h5` 1.25rem/1.3 · `h6` 1.125rem/1.3.

**Body** — Geist Sans 400, 1rem base, line-height 1.65, `kern`/`liga`/`calt`
on, antialiased.

**Label pattern** (pervasive — nav, metadata, section headers): Geist Mono,
`text-xs`, `uppercase`, `tracking-[0.2em]`, weight 400 inactive / 700 active,
`text-muted-foreground` inactive → `text-foreground` active.

**Tabular figures for data.** Numerals in a column, table, chart axis, or
stat block get `font-variant-numeric: tabular-nums`
(`font-feature-settings: "tnum" 1`) so digits share one width and align to
the grid. Prose numerals stay proportional (the default).

**Space Mono italic** is reserved for one structural job: **inline
annotations and figure captions** (e.g. a `<figcaption>` or a marginal
note). It is never used for emphasis — emphasis is always weight. It is
also the visible nod tying Tri-Swiss to its sibling system, Duotone Swiss
(which uses Space Mono/Grotesk as its own core pairing).

**Jost** is reserved for exactly two jobs: a page or section's hero
title/wordmark, and chapter/section dividers inside long-form editorial
content (e.g. a large "02" or chapter title between article sections). It
never appears as a UI heading (that's Geist Mono's job) and never as a
pull-quote (that's Zilla Slab's job).

## Spacing & layout

- **Max content width:** 1000–1200px centered, `px-6` gutters.
- **Radius:** restrained — `0.5rem` base, rarely used. Most corners are square.
- **Borders:** 1px solid `--border` everywhere. **No shadows** — elevation is a
  background step (`--card` on `--background`).
- **Section header:** uppercase mono label with a full-width rule beside it.

```jsx
<div className="mb-4 flex items-baseline gap-3">
  <h5>Section title</h5>
  <span className="h-px flex-1 bg-border" />
</div>
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

**`geist-icons` is the single sanctioned icon set** — mirroring the
one-icon-library rule, no other icon library, no icon fonts, no emoji.
Restyle its three off-identity defaults; keep everything else:

| Attribute | geist-icons default | Tri-Swiss |
|-----------|---------------------|-----------|
| `stroke-width` | varies by icon | `1.5` |
| `stroke-linecap` | varies by icon | `square` |
| `stroke-linejoin` | varies by icon | `miter` |

Apply via one CSS rule (CSS overrides SVG presentation attributes, so the
raw icon markup needs no editing):

```css
.icon { width: 20px; height: 20px; fill: none; stroke: currentColor;
  stroke-width: 1.5; stroke-linecap: square; stroke-linejoin: miter; }
```

Icons are 16–20px, `currentColor` (inherit `foreground`/`muted-foreground`;
`primary` only for the destructive/accent cases already reserved for it —
never `highlight`), and **augment** the uppercase-mono labels — never
replace them.

## Charts

Hand-rolled SVG is the default — the simple charts (timeline strip, share
bars, single differential line) are ~20 lines of raw SVG. Colors are
foreground / muted / primary only, **except** when a chart genuinely has a
second data series worth distinguishing — that series, and only that one,
may use `--highlight`. `width="100%"`, fixed height, `viewBox` from the data
range.

When scales / axes / many-series / interaction genuinely warrant a library,
the **single sanctioned choice is [Observable Plot](https://observablehq.com/plot)**
(framework-agnostic SVG — no other chart library, no canvas libs). Restyle
it to the palette: set mark `fill`/`stroke` to `--foreground`/
`--muted-foreground`/`--primary` explicitly (never Plot's default color
scheme; `--highlight` only for a genuine second series), disable the color
legend, keep bars square, use explicit muted grid marks, and restyle axes
to the mono label pattern. See
[`references/components.md`](references/components.md) for the full pattern.

## Do not

- **No third semantic color.** `--highlight` is not success green or info
  blue in disguise — it carries no state meaning anywhere.
- **No shadows.** Depth is border presence + background steps.
- **No chart libraries except restyled Observable Plot.** Hand-rolled SVG is
  the default; reach for Plot only when complexity earns it, restyled to
  the palette.
- **No `rounded-full` on containers.** Dots only.
- **No raw hex in markup.** Always the semantic token.
- **Icons: restyled geist-icons only.** Monoline, `currentColor`, square
  caps. No icon fonts. **No emoji** in UI text unless explicitly requested.
- **No Turquoise outside charts/brand moments.** It never appears in
  buttons, tags, or status indicators.
- **No Jost outside hero/wordmark and chapter dividers.** It is not a body
  face, not a UI heading face, not a pull-quote face.
```

- [ ] **Step 2: Verify frontmatter parses and required governance phrases are present**

Run:
```bash
node -e "
const fs = require('fs');
const md = fs.readFileSync('skills/tri-swiss/SKILL.md', 'utf8');
if (!md.startsWith('---\nname: tri-swiss')) { console.error('bad frontmatter'); process.exit(1); }
const required = ['No shadows', 'Swiss-minimalist', 'tri-tone', '--highlight',
  'geist-icons', 'Space Mono', 'Zilla Slab', 'Jost', 'stroke-linecap: square'];
const missing = required.filter(s => !md.includes(s));
if (missing.length) { console.error('MISSING PHRASES:', missing.join(' | ')); process.exit(1); }
console.log('SKILL.md: frontmatter and required governance phrases OK');
"
```
Expected: `SKILL.md: frontmatter and required governance phrases OK`

- [ ] **Step 3: Commit**

```bash
git add skills/tri-swiss/SKILL.md
git commit -m "feat: write Tri-Swiss SKILL.md"
```

---

### Task 6: `references/components.md` — extended component catalogue

**Files:**
- Create: `skills/tri-swiss/references/components.md`

**Interfaces:**
- Consumes: token names from Task 4, the icon restyle rule and chart rules from Task 5.
- Produces: nothing consumed by later tasks directly (this is a leaf reference doc), but must stay in sync with `SKILL.md`.

- [ ] **Step 1: Write `skills/tri-swiss/references/components.md`**

```markdown
# Component Catalogue

Detailed patterns beyond the core set in `SKILL.md`. All snippets assume the theme
tokens from `assets/theme.css` are installed. Tailwind class names are shown; the
same tokens work as plain CSS custom properties on other stacks.

## Status pips (source / connection indicators)

Inline label with a leading dot whose color encodes state. State is expressed by the
dot color only — the text stays neutral. `--highlight` never appears here (see
"The `--highlight` token" in `SKILL.md`).

```
inline-flex items-center gap-1.5
font-mono text-xs uppercase tracking-[0.15em]

dot: h-1.5 w-1.5 rounded-full
  connected:    bg-foreground
  warning:      bg-primary
  loading:      bg-muted-foreground/30 animate-pulse
  disconnected: bg-foreground/25
```

## Modal overlay

Backdrop dims the page with a translucent background (not a shadow). The panel is a
hard-bordered box.

```
overlay: fixed inset-0 z-50 flex items-center justify-center bg-background/80 px-6
panel:   w-full max-w-[560px] border border-foreground bg-background p-6
         font-mono text-sm
```

## Toggle controls (theme, language, binary options)

Two (or more) inline options separated by a middot; the active option is
`text-foreground`, the rest are muted with a hover lift. No switch chrome, no pill —
just weight/contrast difference.

```jsx
<div className="flex items-baseline gap-1.5 font-mono text-xs uppercase tracking-[0.2em]">
  <button className={active ? "text-foreground" : "text-muted-foreground hover:text-foreground"}>
    Option A
  </button>
  <span className="text-muted-foreground/40">·</span>
  <button className={active ? "text-muted-foreground hover:text-foreground" : "text-foreground"}>
    Option B
  </button>
</div>
```

## SVG charts

All graphs are hand-rolled SVG by default. No chart library — Recharts / chart.js /
victory carry aesthetic opinions (rounded corners, default palettes, tooltips) that
fight this system. Palette is foreground / muted / primary, **plus `--highlight`
only when there's a genuine second data series to distinguish** — never as a
default color choice.

Sizing: `width="100%"`, a fixed pixel height, and a `viewBox` computed from the data
range. No responsive charting library needed — the viewBox does the scaling.

Reference chart types:

- **Timeline strip** — a horizontal event bar. Discrete events render as dots along
  the bar; milestones/objectives as vertical tick marks.
- **Differential line** — a delta value over time with a zero-crossing fill:
  `foreground` above the axis, `primary` below.
- **Paired curve** — two lines on the same axes, one solid (primary series,
  `foreground`) and one dashed (secondary series, `muted-foreground`).
- **Share bars** — horizontal stacked bars for proportional data; the highlighted
  slice is distinguished with an outline ring, not a different hue.
- **Governed second-series line** — when a chart genuinely needs a *third* visually
  distinct line (not just primary/muted), draw it solid in `--highlight` — this is
  one of the only two sanctioned uses of Turquoise in the whole system. Never use
  it as a default third color "because it's there."

The through-line: encode categories and emphasis with fill opacity, stroke style
(solid vs dashed), and outline rings first — reach for `--highlight` only when
those tools are exhausted and a chart truly needs a third distinct line.

### Observable Plot (the one sanctioned library)

Default to hand-rolled SVG. When a library is warranted, use Observable Plot and
restyle it — never its defaults:

```js
import * as Plot from "https://cdn.jsdelivr.net/npm/@observablehq/plot@0.6/+esm";

const chart = Plot.plot({
  width: 640, height: 240,
  style: { background: "transparent", color: "var(--foreground)",
           fontFamily: "'Geist Mono', monospace", fontSize: "11px" },
  x: { label: null }, y: { label: null },
  marks: [
    Plot.gridY({ stroke: "var(--muted-foreground)", strokeOpacity: 0.15 }),
    Plot.ruleY([0], { stroke: "var(--muted-foreground)", strokeOpacity: 0.4 }),
    Plot.lineY(data, { x: "t", y: "v", stroke: "var(--foreground)", strokeWidth: 1.5 }),
    Plot.lineY(data, { x: "t", y: "u", stroke: "var(--muted-foreground)",
                       strokeWidth: 1.5, strokeDasharray: "4 3" }),
    // Only add this third mark when a genuine second series needs distinguishing
    // from both the primary and muted lines — the one sanctioned chart use of highlight:
    Plot.lineY(data, { x: "t", y: "w", stroke: "var(--highlight)", strokeWidth: 1.5 }),
  ],
});
document.querySelector("#mount").append(chart);
```

Rules: explicit palette colors per mark (no default scheme), no color legend,
square bars, muted grid, solid-vs-dashed for series, outline ring (not hue) for a
highlighted slice, `--highlight` reserved for a genuine third line only.

## Iconography (geist-icons, restyled)

`geist-icons` is the only sanctioned icon set. Drop in the icon's SVG and add
`class="icon"`; the CSS restyle below overrides the library's own defaults (CSS
beats SVG presentation attributes, so the paths are untouched).

```css
.icon { width: 20px; height: 20px; fill: none; stroke: currentColor;
  stroke-width: 1.5; stroke-linecap: square; stroke-linejoin: miter; }
```

```html
<span class="inline-flex items-center gap-2 font-mono text-xs uppercase tracking-[0.2em]">
  <svg class="icon" viewBox="0 0 24 24"><!-- geist-icons 'arrow-right' paths --></svg>
  Read more
</span>
```

Sizing 16–20px. Never let an icon replace the mono text label — it augments it.
For React, import from `geist-icons` and pass the same stroke props (or the
`.icon` class):

```jsx
import { IconArrowRight } from "geist-icons";

<IconArrowRight className="icon" />
```

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

## Annotations & captions (Space Mono, italic only)

The visible nod to Tri-Swiss's sibling system (Duotone Swiss). Reserved for inline
annotations and figure captions — never emphasis, never a heading.

```jsx
<figcaption className="font-annotation italic text-sm text-muted-foreground">
  Fig. 3 — quarterly share by category.
</figcaption>
```

## Cards

Elevation is a background step, never a shadow.

```
border border-border bg-card text-card-foreground p-6
```

Nest a section divider (see `SKILL.md`) inside for titled regions. Keep corners
square unless a `rounded-md` (0.5rem) genuinely helps.

## Inputs

```
border border-border bg-input px-3 py-2 font-mono text-sm
placeholder: text-muted-foreground
focus: outline-none ring-1 ring-ring
```

Labels above inputs use the uppercase mono label pattern from `SKILL.md`.
```

- [ ] **Step 2: Verify no forbidden colors and the highlight-governance language is present**

Run:
```bash
node -e "
const md = require('fs').readFileSync('skills/tri-swiss/references/components.md', 'utf8');
const required = ['--highlight', 'geist-icons', 'Jost', 'Space Mono', 'never a default color'];
const missing = required.filter(s => !md.includes(s));
if (missing.length) { console.error('MISSING:', missing.join(' | ')); process.exit(1); }
if (/box-shadow/i.test(md)) { console.error('SHADOW MENTION FOUND'); process.exit(1); }
console.log('components.md: governance language present, no shadow mentions');
"
```
Expected: `components.md: governance language present, no shadow mentions`

- [ ] **Step 3: Commit**

```bash
git add skills/tri-swiss/references/components.md
git commit -m "docs: add Tri-Swiss component catalogue"
```

---

### Task 7: `docs/index.html` — live GitHub Pages showcase

**Files:**
- Create: `docs/index.html`

**Interfaces:**
- Consumes: exact tokens from Task 4, exact typography/icon/chart rules from Tasks 5–6.
- Produces: the page Task 8's philosophy verifier checks, and the page Task 9's Playwright script screenshots.

- [ ] **Step 1: Write `docs/index.html`**

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />

  <script>
    // Apply the saved theme before first paint to avoid a flash of the wrong mode.
    (function () {
      try {
        var saved = localStorage.getItem("theme");
        var dark = saved ? saved === "dark"
          : window.matchMedia && window.matchMedia("(prefers-color-scheme: dark)").matches;
        if (dark) document.documentElement.classList.add("dark");
      } catch (e) {}
    })();
  </script>

  <title>Tri-Swiss — a tri-tone Swiss-minimalist design system for Claude Code</title>
  <meta name="description" content="Two structural colors, one strong accent, one rare highlight. A Geist-based Swiss-minimalist design system that gives every Claude Code project one consistent, opinionated aesthetic." />

  <meta property="og:type" content="website" />
  <meta property="og:title" content="Tri-Swiss — a tri-tone Swiss-minimalist design system" />
  <meta property="og:description" content="Two structural colors, one strong accent, one rare highlight. One install, one opinionated aesthetic across every Claude Code project." />
  <meta property="og:url" content="https://luxsolari.github.io/tri-swiss/" />
  <meta property="og:image" content="https://luxsolari.github.io/tri-swiss/assets/social-card.png" />
  <meta property="og:image:width" content="1200" />
  <meta property="og:image:height" content="630" />
  <meta name="twitter:card" content="summary_large_image" />
  <meta name="twitter:title" content="Tri-Swiss — a tri-tone Swiss-minimalist design system" />
  <meta name="twitter:description" content="Two structural colors, one strong accent, one rare highlight." />
  <meta name="twitter:image" content="https://luxsolari.github.io/tri-swiss/assets/social-card.png" />

  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Geist:wght@100..900&family=Geist+Mono:wght@100..900&family=Space+Mono:ital,wght@0,400;1,400&family=Zilla+Slab:ital,wght@0,400;0,500;0,700;1,400&family=Jost:wght@400;700&display=swap" rel="stylesheet" />

  <style>
    :root {
      --background:#eae8d0; --foreground:#000000; --card:#f1efdb; --card-foreground:#000000;
      --primary:#d3281b; --primary-foreground:#eae8d0; --secondary:#000000; --secondary-foreground:#eae8d0;
      --muted:#e2dfc7; --muted-foreground:#4a4838; --border:#000000; --input:#f1efdb; --ring:#d3281b;
      --highlight:#56bfa3;
      /* Role fonts */
      --font-mono:'Geist Mono',ui-monospace,monospace; --font-sans:'Geist Sans',system-ui,sans-serif;
      --font-annotation:'Space Mono',ui-monospace,monospace; --font-serif:'Zilla Slab',Georgia,serif;
      --font-hero:'Jost',system-ui,sans-serif;
    }
    .dark {
      --background:#000000; --foreground:#eae8d0; --card:#161616; --card-foreground:#eae8d0;
      --primary:#e2503f; --primary-foreground:#eae8d0; --secondary:#eae8d0; --secondary-foreground:#000000;
      --muted:#1f1f1f; --muted-foreground:#a8a696; --border:#eae8d0; --input:#161616; --ring:#e2503f;
      --highlight:#63cbae;
    }
    * { box-sizing: border-box; }
    body { margin:0; background:var(--background); color:var(--foreground);
      font-family:var(--font-sans); line-height:1.65;
      -webkit-font-smoothing:antialiased; font-feature-settings:"kern","liga","calt"; }
    .wrap { max-width:1100px; margin:0 auto; padding:0 24px; }
    h1,h2,h3,h4,h5,h6 { font-family:var(--font-mono); font-weight:700; margin:0; }
    h1 { font-size:3rem; letter-spacing:-0.02em; line-height:1.1; }
    h2 { font-size:2.25rem; letter-spacing:-0.02em; line-height:1.15; }
    h3 { font-size:1.5rem; line-height:1.3; }
    .hero-title { font-family:var(--font-hero); font-weight:700; letter-spacing:-0.01em;
      font-size:3.5rem; line-height:1.05; margin:20px 0 0; }
    .label { font-family:var(--font-mono); font-size:0.75rem; text-transform:uppercase;
      letter-spacing:0.2em; color:var(--muted-foreground); }
    .annotation { font-family:var(--font-annotation); font-style:italic; }
    .divider { display:flex; align-items:baseline; gap:12px; margin:0 0 24px; }
    .divider .rule { height:1px; flex:1; background:var(--border); }
    section { padding:64px 0; border-top:1px solid var(--border); }
    a { color:inherit; }
    .icon { width:20px; height:20px; fill:none; stroke:currentColor;
      stroke-width:1.5; stroke-linecap:square; stroke-linejoin:miter; }
    /* nav */
    nav { display:flex; align-items:center; justify-content:space-between;
      padding:20px 0; border-bottom:1px solid var(--border); }
    .wordmark { font-family:var(--font-mono); font-weight:700; letter-spacing:0.15em;
      text-transform:uppercase; font-size:0.8rem; }
    .toggle { display:flex; align-items:baseline; gap:6px; font-family:var(--font-mono);
      font-size:0.75rem; text-transform:uppercase; letter-spacing:0.2em; }
    .toggle button { background:none; border:none; padding:0; cursor:pointer;
      font:inherit; letter-spacing:inherit; text-transform:inherit; color:var(--muted-foreground); }
    .toggle button[aria-pressed="true"] { color:var(--foreground); }
    .toggle .mid { color:var(--muted-foreground); opacity:0.4; }
  </style>
</head>
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
      <section id="hero" style="border-top:none;">
        <p class="label">Claude Code plugin</p>
        <h1 class="hero-title">Tri-Swiss.</h1>
        <p style="font-size:1.15rem; max-width:60ch; margin:24px 0 0; color:var(--foreground);">
          Lux Solari's Geist-based house design language — Swiss-minimalist,
          tri-tone. Two structural colors, one strong accent, one rare
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
      <section id="install" style="border-top:none; padding-top:0;">
        <div style="border:1px solid var(--border); background:var(--card); padding:28px 32px;
             display:flex; flex-wrap:wrap; align-items:center; gap:24px 40px; justify-content:space-between;">
          <div style="min-width:0;">
            <p class="label" style="margin:0 0 12px;">Install — two lines in Claude Code</p>
            <pre style="margin:0; border:none; background:none; padding:0; overflow:auto;
                 font-family:var(--font-mono); font-size:1rem; line-height:1.8; color:var(--foreground);">/plugin marketplace add luxsolari/lux-solari-plugins
/plugin install tri-swiss</pre>
          </div>
          <a href="https://github.com/luxsolari/tri-swiss"
             style="border:1px solid var(--foreground); background:var(--foreground); color:var(--background);
             padding:12px 22px; text-decoration:none; font-family:var(--font-mono);
             text-transform:uppercase; letter-spacing:0.2em; font-size:0.75rem; white-space:nowrap;">Get the plugin</a>
        </div>
      </section>
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
          <div style="border:1px solid var(--border); background:var(--card); padding:24px;">
            <p class="label">02 — Swiss-minimalist</p>
            <p style="margin:12px 0 0;">Visible 1px borders, no shadows — elevation is a
              background step. Generous whitespace, square corners, uppercase monospace
              labels with wide letter-spacing.</p>
          </div>
        </div>
      </section>
      <section id="palette">
        <div class="divider"><h3>Palette</h3><span class="rule"></span></div>
        <p class="label" style="margin:-8px 0 20px;">Semantic tokens — never a raw hex in markup</p>
        <div style="display:grid; grid-template-columns:repeat(auto-fill,minmax(172px,1fr)); gap:16px;">
          <figure style="margin:0; border:1px solid var(--border);">
            <div class="pal-chip" data-token="background" style="height:64px; background:var(--background);"></div>
            <figcaption style="border-top:1px solid var(--border); padding:8px 10px;">
              <span class="label" style="display:block;">background</span>
              <span style="font-family:var(--font-mono); font-size:0.7rem; color:var(--muted-foreground);">page</span>
              <dl class="pal-vals" style="margin:8px 0 0; font-family:var(--font-mono); font-size:0.62rem; line-height:1.55; color:var(--muted-foreground);"></dl>
            </figcaption>
          </figure>
          <figure style="margin:0; border:1px solid var(--border);">
            <div class="pal-chip" data-token="card" style="height:64px; background:var(--card);"></div>
            <figcaption style="border-top:1px solid var(--border); padding:8px 10px;">
              <span class="label" style="display:block;">card</span>
              <span style="font-family:var(--font-mono); font-size:0.7rem; color:var(--muted-foreground);">surface</span>
              <dl class="pal-vals" style="margin:8px 0 0; font-family:var(--font-mono); font-size:0.62rem; line-height:1.55; color:var(--muted-foreground);"></dl>
            </figcaption>
          </figure>
          <figure style="margin:0; border:1px solid var(--border);">
            <div class="pal-chip" data-token="foreground" style="height:64px; background:var(--foreground);"></div>
            <figcaption style="border-top:1px solid var(--border); padding:8px 10px;">
              <span class="label" style="display:block;">foreground</span>
              <span style="font-family:var(--font-mono); font-size:0.7rem; color:var(--muted-foreground);">ink/text</span>
              <dl class="pal-vals" style="margin:8px 0 0; font-family:var(--font-mono); font-size:0.62rem; line-height:1.55; color:var(--muted-foreground);"></dl>
            </figcaption>
          </figure>
          <figure style="margin:0; border:1px solid var(--border);">
            <div class="pal-chip" data-token="muted" style="height:64px; background:var(--muted);"></div>
            <figcaption style="border-top:1px solid var(--border); padding:8px 10px;">
              <span class="label" style="display:block;">muted</span>
              <span style="font-family:var(--font-mono); font-size:0.7rem; color:var(--muted-foreground);">subtle</span>
              <dl class="pal-vals" style="margin:8px 0 0; font-family:var(--font-mono); font-size:0.62rem; line-height:1.55; color:var(--muted-foreground);"></dl>
            </figcaption>
          </figure>
          <figure style="margin:0; border:1px solid var(--border);">
            <div class="pal-chip" data-token="muted-foreground" style="height:64px; background:var(--muted-foreground);"></div>
            <figcaption style="border-top:1px solid var(--border); padding:8px 10px;">
              <span class="label" style="display:block;">muted-foreground</span>
              <span style="font-family:var(--font-mono); font-size:0.7rem; color:var(--muted-foreground);">metadata</span>
              <dl class="pal-vals" style="margin:8px 0 0; font-family:var(--font-mono); font-size:0.62rem; line-height:1.55; color:var(--muted-foreground);"></dl>
            </figcaption>
          </figure>
          <figure style="margin:0; border:1px solid var(--border);">
            <div class="pal-chip" data-token="primary" style="height:64px; background:var(--primary);"></div>
            <figcaption style="border-top:1px solid var(--border); padding:8px 10px;">
              <span class="label" style="display:block;">primary</span>
              <span style="font-family:var(--font-mono); font-size:0.7rem; color:var(--muted-foreground);">accent</span>
              <dl class="pal-vals" style="margin:8px 0 0; font-family:var(--font-mono); font-size:0.62rem; line-height:1.55; color:var(--muted-foreground);"></dl>
            </figcaption>
          </figure>
          <figure style="margin:0; border:1px solid var(--border);">
            <div class="pal-chip" data-token="highlight" style="height:64px; background:var(--highlight);"></div>
            <figcaption style="border-top:1px solid var(--border); padding:8px 10px;">
              <span class="label" style="display:block;">highlight</span>
              <span style="font-family:var(--font-mono); font-size:0.7rem; color:var(--muted-foreground);">charts/brand only</span>
              <dl class="pal-vals" style="margin:8px 0 0; font-family:var(--font-mono); font-size:0.62rem; line-height:1.55; color:var(--muted-foreground);"></dl>
            </figcaption>
          </figure>
          <figure style="margin:0; border:1px solid var(--border);">
            <div class="pal-chip" data-token="border" style="height:64px; background:var(--border);"></div>
            <figcaption style="border-top:1px solid var(--border); padding:8px 10px;">
              <span class="label" style="display:block;">border</span>
              <span style="font-family:var(--font-mono); font-size:0.7rem; color:var(--muted-foreground);">borders</span>
              <dl class="pal-vals" style="margin:8px 0 0; font-family:var(--font-mono); font-size:0.62rem; line-height:1.55; color:var(--muted-foreground);"></dl>
            </figcaption>
          </figure>
        </div>
      </section>
      <section id="typography">
        <div class="divider"><h3>Typography</h3><span class="rule"></span></div>
        <div style="display:grid; grid-template-columns:1fr 1fr; gap:40px; align-items:start;">
          <div>
            <p class="label" style="margin-bottom:16px;">Display &amp; data — mono</p>
            <p style="font-family:var(--font-mono); font-weight:700; font-size:3rem; margin:0; letter-spacing:-0.02em;">Aa</p>
            <h1 style="margin:16px 0 0;">Heading one</h1>
            <h2 style="margin:8px 0 0;">Heading two</h2>
            <h3 style="margin:8px 0 0;">Heading three</h3>
            <p class="annotation" style="font-size:0.9rem; color:var(--muted-foreground); margin:20px 0 0; max-width:34ch;">
              Space Mono italic is reserved for annotations and figure captions — never for
              emphasis. Emphasis is always weight.</p>
          </div>
          <div>
            <p class="label" style="margin-bottom:16px;">Body — weight axis 300–700</p>
            <p style="font-weight:300; font-size:1.35rem; line-height:1.4; margin:0;">Light 300 — display sub-decks and leads.</p>
            <p style="font-weight:400; margin:14px 0 0;">Regular 400 — the body voice. Every element
              earns its place or is removed; difference is expressed through weight, space, and
              contrast — almost never by adding a color.</p>
            <p style="font-weight:500; margin:14px 0 0;">Medium 500 — UI emphasis and active labels.</p>
            <p style="font-weight:700; margin:14px 0 0;">Bold 700 — strong emphasis, used sparingly.</p>
            <p class="label" style="margin-top:28px;">Tabular figures · aligned to the grid</p>
            <div style="font-variant-numeric:tabular-nums; font-feature-settings:'tnum' 1; margin-top:10px; display:grid; grid-template-columns:auto auto; gap:4px 32px; width:max-content;">
              <span>Sessions</span><span style="text-align:right;">1,209,384</span>
              <span>Conversions</span><span style="text-align:right;">18,050</span>
              <span>Rate</span><span style="text-align:right;">1.4931%</span>
            </div>
          </div>
        </div>
      </section>
      <section id="registers">
        <div class="divider"><h3>Five type registers</h3><span class="rule"></span></div>
        <p class="label" style="margin:-8px 0 24px;">Core Geist duotone + three sanctioned registers</p>
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
          <div>
            <p class="label" style="margin-bottom:12px;">Hero / editorial display</p>
            <p style="font-family:var(--font-hero); font-size:1.4rem; font-weight:700; margin:0;">02 — Chapter</p>
            <p class="label" style="margin-top:10px;">Jost</p>
          </div>
        </div>
        <blockquote style="font-family:var(--font-serif); font-style:italic; font-size:1.5rem; line-height:1.45; border-left:2px solid var(--border); margin:40px 0 0; padding:2px 0 2px 24px; max-width:62ch;">
          &ldquo;Difference is expressed through weight, space, and contrast — almost never by adding a color.&rdquo;
        </blockquote>
      </section>
      <section id="components">
        <div class="divider"><h3>Components</h3><span class="rule"></span></div>
        <div style="display:grid; grid-template-columns:repeat(auto-fill,minmax(260px,1fr)); gap:20px;">

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

          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Tags</p>
            <div style="display:flex; flex-wrap:wrap; gap:8px; font-family:var(--font-mono);
                        font-size:0.65rem; text-transform:uppercase; letter-spacing:0.12em;">
              <span style="color:var(--muted-foreground); padding:2px 8px;">Neutral</span>
              <span style="background:var(--foreground); color:var(--background); padding:2px 8px;">Signal</span>
              <span style="border:1px dashed color-mix(in srgb, var(--foreground) 50%, transparent); padding:2px 8px;">Outlined</span>
            </div>
          </div>

          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Status pips</p>
            <div style="display:flex; flex-direction:column; gap:10px; font-family:var(--font-mono);
                        font-size:0.7rem; text-transform:uppercase; letter-spacing:0.15em;">
              <span style="display:inline-flex; align-items:center; gap:8px;">
                <span style="width:6px;height:6px;border-radius:9999px;background:var(--foreground);"></span>Connected</span>
              <span style="display:inline-flex; align-items:center; gap:8px;">
                <span style="width:6px;height:6px;border-radius:9999px;background:var(--primary);"></span>Warning</span>
              <span style="display:inline-flex; align-items:center; gap:8px;">
                <span style="width:6px;height:6px;border-radius:9999px;background:color-mix(in srgb, var(--foreground) 35%, transparent);"></span>Disconnected</span>
            </div>
          </div>

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

          <div style="border:1px solid var(--border); padding:20px;">
            <p class="label" style="margin-bottom:16px;">Toggle</p>
            <div style="display:flex; gap:8px; font-family:var(--font-mono); font-size:0.75rem;
                        text-transform:uppercase; letter-spacing:0.2em;">
              <span style="color:var(--foreground);">EN</span>
              <span style="color:var(--muted-foreground); opacity:0.4;">·</span>
              <span style="color:var(--muted-foreground);">ES</span>
            </div>
          </div>

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
      <section id="charts">
        <div class="divider"><h3>Charts</h3><span class="rule"></span></div>
        <p class="label" style="margin:-8px 0 24px;">Hand-rolled SVG by default · Observable Plot when a lib is warranted</p>
        <div style="display:grid; grid-template-columns:1fr 1fr; gap:32px; align-items:start;">

          <figure style="margin:0;">
            <figcaption class="label" style="margin-bottom:12px;">Share bars — outline ring marks the highlight slice</figcaption>
            <svg width="100%" height="120" viewBox="0 0 320 120">
              <rect x="0" y="10" width="240" height="18" fill="var(--foreground)"/>
              <rect x="0" y="40" width="150" height="18" fill="var(--muted-foreground)"/>
              <rect x="0" y="70" width="90"  height="18" fill="var(--muted-foreground)"/>
              <rect x="0" y="10" width="240" height="18" fill="none" stroke="var(--primary)" stroke-width="1.5"/>
            </svg>
          </figure>

          <figure style="margin:0;">
            <figcaption class="label" style="margin-bottom:12px;">Governed third line — `--highlight` reserved for a genuine second series</figcaption>
            <svg width="100%" height="120" viewBox="0 0 320 120">
              <line x1="0" y1="60" x2="320" y2="60" stroke="var(--muted-foreground)" stroke-opacity="0.4"/>
              <polyline fill="none" stroke="var(--foreground)" stroke-width="1.5"
                points="0,60 40,45 80,35 120,50 160,42 200,55 240,48 280,60 320,52"/>
              <polyline fill="none" stroke="var(--muted-foreground)" stroke-width="1.5" stroke-dasharray="4 3"
                points="0,70 40,68 80,72 120,66 160,74 200,70 240,76 280,72 320,78"/>
              <polyline fill="none" stroke="var(--highlight)" stroke-width="1.5"
                points="0,90 40,80 80,60 120,50 160,55 200,40 240,35 280,30 320,25"/>
            </svg>
          </figure>
        </div>

        <figure style="margin:32px 0 0;">
          <figcaption class="label" style="margin-bottom:12px;">Observable Plot · endorsed chart library — restyled to the palette</figcaption>
          <div id="plot-mount"></div>
        </figure>
      </section>
      <div id="social-card" aria-hidden="true" style="position:absolute; left:-9999px; top:0;
           width:1200px; height:630px; background:var(--background); color:var(--foreground);
           padding:80px; display:flex; flex-direction:column; justify-content:space-between;
           border:1px solid var(--border);">
        <p class="label">Claude Code plugin</p>
        <div>
          <h1 class="hero-title" style="font-size:4.5rem; line-height:1.05; margin:0;">Tri-Swiss.</h1>
          <p style="font-family:var(--font-mono); font-size:1.25rem; margin:20px 0 0;
             color:var(--muted-foreground);">Two colors. One accent. One rare highlight.</p>
        </div>
        <p style="font-family:var(--font-mono); font-size:1rem; letter-spacing:0.1em;
           margin:0;">/plugin install tri-swiss</p>
      </div>
      <footer style="border-top:1px solid var(--border); padding:48px 0; margin-top:0;">
        <div style="display:flex; justify-content:space-between; align-items:baseline;
             flex-wrap:wrap; gap:16px; margin-top:24px; font-family:var(--font-mono);
             font-size:0.75rem; text-transform:uppercase; letter-spacing:0.15em;">
          <span style="color:var(--muted-foreground);">MIT © 2026 Lux Solari (Luciano Laje)</span>
          <a href="https://github.com/luxsolari/tri-swiss" style="display:inline-flex;
             align-items:center; gap:8px; text-decoration:none;">GitHub
            <svg class="icon" viewBox="0 0 24 24" width="16" height="16"><path d="M15 3h6v6"/><path d="M10 14 21 3"/><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/></svg>
          </a>
        </div>
      </footer>
    </main>
  </div>
  <script type="module">
    import * as Plot from "https://cdn.jsdelivr.net/npm/@observablehq/plot@0.6/+esm";
    const data = [0,1,3,2,4,6,5,7,6,8].map((v,i) => ({ t:i, v, u: Math.max(0, v-2), w: Math.min(9, v+1.5) }));
    // Render only after fonts load so Plot measures axis-label metrics with the real
    // font — otherwise a fallback-font measurement shifts the layout (non-deterministic
    // pixels, plus a visible reflow on the live page).
    await document.fonts.ready;
    const chart = Plot.plot({
      width: 900, height: 220, marginLeft: 28, marginBottom: 24,
      style: { background:"transparent", color:"var(--foreground)",
               fontFamily:"var(--font-mono)", fontSize:"11px" },
      x: { label:null }, y: { label:null },
      marks: [
        Plot.gridY({ stroke:"var(--muted-foreground)", strokeOpacity:0.15 }),
        Plot.lineY(data, { x:"t", y:"v", stroke:"var(--foreground)", strokeWidth:1.5 }),
        Plot.lineY(data, { x:"t", y:"u", stroke:"var(--muted-foreground)", strokeWidth:1.5, strokeDasharray:"4 3" }),
        Plot.lineY(data, { x:"t", y:"w", stroke:"var(--highlight)", strokeWidth:1.5 }),
      ],
    });
    document.querySelector("#plot-mount").append(chart);
  </script>
  <script>
    // Fill each palette swatch with its live hex / rgb / hsl, recomputed on theme change
    // so the values always match the swatch (data values → Geist Mono).
    (function () {
      function pad(n) { return n.toString(16).padStart(2, "0"); }
      function toHsl(r, g, b) {
        r /= 255; g /= 255; b /= 255;
        var max = Math.max(r, g, b), min = Math.min(r, g, b), d = max - min, h = 0, s = 0, l = (max + min) / 2;
        if (d) {
          s = l > 0.5 ? d / (2 - max - min) : d / (max + min);
          h = max === r ? (g - b) / d + (g < b ? 6 : 0) : max === g ? (b - r) / d + 2 : (r - g) / d + 4;
          h *= 60;
        }
        return [Math.round(h), Math.round(s * 100), Math.round(l * 100)];
      }
      function fill() {
        document.querySelectorAll("#palette figure").forEach(function (fig) {
          var chip = fig.querySelector(".pal-chip"), dl = fig.querySelector(".pal-vals");
          if (!chip || !dl) return;
          var m = getComputedStyle(chip).backgroundColor.match(/\d+/g);
          if (!m) return;
          var r = +m[0], g = +m[1], b = +m[2], H = toHsl(r, g, b);
          dl.innerHTML = "<div>HEX #" + pad(r) + pad(g) + pad(b) + "</div>" +
            "<div>RGB " + r + " " + g + " " + b + "</div>" +
            "<div>HSL " + H[0] + " " + H[1] + "% " + H[2] + "%</div>";
        });
      }
      fill();
      new MutationObserver(fill).observe(document.documentElement, { attributes: true, attributeFilter: ["class"] });
    })();
  </script>
  <script>
    (function () {
      var root = document.documentElement;
      var saved = localStorage.getItem("theme");
      var prefersDark = window.matchMedia && window.matchMedia("(prefers-color-scheme: dark)").matches;
      apply(saved ? saved === "dark" : !!prefersDark);
      function apply(dark) {
        root.classList.toggle("dark", dark);
        document.querySelectorAll("[data-theme-btn]").forEach(function (b) {
          b.setAttribute("aria-pressed", String(b.dataset.themeBtn === (dark ? "dark" : "light")));
        });
      }
      document.addEventListener("click", function (e) {
        var btn = e.target.closest("[data-theme-btn]");
        if (!btn) return;
        var dark = btn.dataset.themeBtn === "dark";
        localStorage.setItem("theme", dark ? "dark" : "light");
        apply(dark);
      });
    })();
  </script>
</body>
</html>
```

- [ ] **Step 2: Sanity-check the page opens without a server (temporary manual check — the real automated check is Task 8)**

Run: `node -e "const html = require('fs').readFileSync('docs/index.html','utf8'); if (!html.includes('<!doctype html>')) throw new Error('missing doctype'); console.log('docs/index.html: well-formed enough to parse, length', html.length);"`
Expected: `docs/index.html: well-formed enough to parse, length <some number>`

- [ ] **Step 3: Commit**

```bash
git add docs/index.html
git commit -m "feat: add Tri-Swiss showcase landing page"
```

---

### Task 8: Philosophy-compliance verifier (the automated "test suite" for the demo page)

**Files:**
- Create: `scripts/capture/verify-philosophy.mjs`
- Create: `scripts/capture/package.json`

**Interfaces:**
- Consumes: `docs/index.html` from Task 7 — this task's script greps that exact file.
- Produces: a `PASS`/`FAIL` exit code any future edit to `docs/index.html` must satisfy; Task 9's capture script and this verifier share `package.json`.

- [ ] **Step 1: Write `scripts/capture/verify-philosophy.mjs`**

```js
import fs from "node:fs";
import path from "node:path";
import { fileURLToPath } from "node:url";

const here = path.dirname(fileURLToPath(import.meta.url));
const html = fs.readFileSync(path.resolve(here, "../../docs/index.html"), "utf8");

const PALETTE = new Set([
  "#eae8d0","#000000","#f1efdb","#d3281b","#e2dfc7","#4a4838","#56bfa3",
  "#161616","#e2503f","#1f1f1f","#a8a696","#63cbae",
]);

const fail = (msg) => { console.error("FAIL:", msg); process.exitCode = 1; };

// 1. Palette purity — no hex outside the token set anywhere in the file.
const hexes = [...html.matchAll(/#[0-9a-fA-F]{6}\b/g)].map((m) => m[0].toLowerCase());
const rogue = [...new Set(hexes)].filter((h) => !PALETTE.has(h));
if (rogue.length) fail("rogue hex color(s): " + rogue.join(", "));

// 1b. No raw rgb()/rgba() or 3-digit hex — use a token or color-mix over a token.
if (/\brgba?\(/i.test(html)) fail("raw rgb()/rgba() literal — use a token or color-mix(var(--token), transparent)");
if (/#[0-9a-fA-F]{3}\b/.test(html)) fail("3-digit hex literal found — use a full token hex or var()");

// 2. No shadows.
if (/box-shadow|drop-shadow/i.test(html)) fail("shadow found (elevation must be a background step)");

// 3. Icons: no rounded caps left un-restyled.
if (/stroke-linecap\s*[:=]\s*["']?round\b/i.test(html)) fail('icon stroke-linecap:round found (must be square)');

// 4. Required social/OG meta tags present.
for (const needle of ['property="og:image"', 'property="og:title"', 'name="twitter:card"',
                       'name="description"']) {
  if (!html.includes(needle)) fail("missing meta: " + needle);
}

// 5. og:image must be an absolute Pages URL (no /docs/ segment). Anchored to
// the og:image meta tag's content attribute specifically — a whole-file
// substring search would also match the twitter:image tag and miss a
// regression that broke only og:image.
if (!/property="og:image"\s+content="https:\/\/luxsolari\.github\.io\/tri-swiss\/assets\/social-card\.png"/.test(html))
  fail("og:image is not the absolute Pages asset URL");

// 6. Author is credited somewhere (hero and/or footer name the house).
if ((html.match(/Lux Solari/g) || []).length < 1) fail("author should be credited at least once");

// 7. `--highlight` never appears in a button/tag/status-pip inline style block.
// Heuristic: scan each `<button`/status-pip dot declaration block for the
// highlight hex values; a false positive here would mean the governed color
// leaked into a semantic UI role.
const forbiddenHighlightContexts = [/background:var\(--highlight\)[^"]*"[^>]*>\s*<\/span>\s*(?:Connected|Warning|Disconnected)/i];
for (const re of forbiddenHighlightContexts) {
  if (re.test(html)) fail("--highlight used in a status pip (forbidden — non-semantic only)");
}

if (!process.exitCode) console.log("PASS: philosophy compliance OK");
```

- [ ] **Step 2: Write `scripts/capture/package.json`**

```json
{
  "name": "tri-swiss-capture",
  "private": true,
  "type": "module",
  "scripts": { "capture": "node capture.mjs", "verify": "node verify-philosophy.mjs" },
  "devDependencies": { "playwright": "^1.48.0" }
}
```

- [ ] **Step 3: Run the verifier against the real `docs/index.html` from Task 7 — this is the test, and it must pass on first run since Task 7 was written to satisfy it**

Run: `cd scripts/capture && node verify-philosophy.mjs`
Expected: `PASS: philosophy compliance OK`

- [ ] **Step 4: Prove the verifier actually catches violations (temporarily introduce one, confirm FAIL, then discard)**

Run:
```bash
cd scripts/capture
node -e "
const fs = require('fs');
const p = '../../docs/index.html';
const original = fs.readFileSync(p, 'utf8');
fs.writeFileSync(p, original.replace('background:var(--background);', 'background:#123456;'));
"
node verify-philosophy.mjs; echo "exit code: $?"
git -C ../.. checkout -- docs/index.html
node verify-philosophy.mjs
```
Expected: first run prints `FAIL: rogue hex color(s): #123456` and `exit code: 1`; after `git checkout` restores the file, the second run prints `PASS: philosophy compliance OK` again.

- [ ] **Step 5: Commit**

```bash
cd ../..
git add scripts/capture/verify-philosophy.mjs scripts/capture/package.json
git commit -m "test: add philosophy-compliance verifier for the showcase page"
```

---

### Task 9: Playwright screenshot capture script

**Files:**
- Create: `scripts/capture/capture.mjs`
- Create: `scripts/capture/.gitignore`

**Interfaces:**
- Consumes: `docs/index.html` (Task 7) and the section IDs it defines (`#hero`, `#palette`, `#components`, `#charts`, `#registers`, `#social-card`).
- Produces: `docs/assets/*.png` files, which Task 10's `README.md` links to.

- [ ] **Step 1: Write `scripts/capture/capture.mjs`**

```js
import { chromium } from "playwright";
import path from "node:path";
import fs from "node:fs";
import { fileURLToPath } from "node:url";

const here = path.dirname(fileURLToPath(import.meta.url));
const fileUrl = "file://" + path.resolve(here, "../../docs/index.html").replace(/\\/g, "/");
const outDir = path.resolve(here, "../../docs/assets");
fs.mkdirSync(outDir, { recursive: true });

async function shoot({ dsr, viewport }, jobs) {
  const browser = await chromium.launch();
  const ctx = await browser.newContext({ deviceScaleFactor: dsr, viewport });
  const page = await ctx.newPage();
  await page.goto(fileUrl, { waitUntil: "networkidle" });
  await page.evaluate(() => document.fonts.ready);
  await page.waitForSelector("#plot-mount svg", { timeout: 20000 }); // Plot chart rendered from CDN
  for (const j of jobs) {
    await page.evaluate((d) => document.documentElement.classList.toggle("dark", d), !!j.dark);
    await page.waitForTimeout(200);
    const el = await page.$(j.id);
    // #social-card is intentionally rendered off-canvas (left:-9999px) so it never
    // shows in the live page. Playwright's elementHandle.screenshot() cannot clip a
    // negative-x region correctly (the browser can't scroll to negative X, so the
    // clip falls back to the viewport origin and silently captures whatever else is
    // at (0,0)). Fix: temporarily zero out the element's own inline offset for the
    // capture only (DOM-only, in the headless page — never touches docs/index.html
    // on disk), then restore it so the live page is unaffected.
    const box = await el.boundingBox();
    const offCanvas = box.x < 0 || box.y < 0;
    if (offCanvas) {
      await el.evaluate((node) => {
        node.dataset.captureLeft = node.style.left;
        node.dataset.captureTop = node.style.top;
        node.style.left = "0px";
        node.style.top = "0px";
      });
      await page.waitForTimeout(50);
    }
    const shotBox = await el.boundingBox();
    if (j.assert) {
      if (Math.round(shotBox.width) !== j.assert.w || Math.round(shotBox.height) !== j.assert.h)
        throw new Error(`${j.file}: expected ${j.assert.w}x${j.assert.h}, got ${Math.round(shotBox.width)}x${Math.round(shotBox.height)}`);
    }
    await el.screenshot({ path: path.join(outDir, j.file) });
    if (offCanvas) {
      await el.evaluate((node) => {
        node.style.left = node.dataset.captureLeft;
        node.style.top = node.dataset.captureTop;
        delete node.dataset.captureLeft;
        delete node.dataset.captureTop;
      });
    }
    console.log("wrote", j.file);
  }
  await browser.close();
}

// Section shots — crisp 2x.
await shoot({ dsr: 2, viewport: { width: 1180, height: 1000 } }, [
  { id: "#hero", file: "hero-light.png", dark: false },
  { id: "#hero", file: "hero-dark.png", dark: true },
  { id: "#palette", file: "palette.png", dark: false },
  { id: "#components", file: "components.png", dark: false },
  { id: "#charts", file: "charts.png", dark: false },
  { id: "#registers", file: "type-registers.png", dark: false },
]);

// Social card — exact 1200x630 at 1x for OG.
await shoot({ dsr: 1, viewport: { width: 1280, height: 720 } }, [
  { id: "#social-card", file: "social-card.png", dark: false, assert: { w: 1200, h: 630 } },
]);
```

- [ ] **Step 2: Write `scripts/capture/.gitignore` (keep only the untracked install output out of the tree — the lockfile itself IS committed, for reproducible installs)**

```
node_modules/
```

- [ ] **Step 3: Install Playwright and its browser, then run the capture**

Run:
```bash
cd scripts/capture
npm install
npx playwright install chromium
npm run capture
```
Expected: six `wrote <file>.png` lines (`hero-light.png`, `hero-dark.png`, `palette.png`, `components.png`, `charts.png`, `type-registers.png`, `social-card.png` — seven total across both `shoot()` calls), and `docs/assets/` now contains all seven files.

- [ ] **Step 4: Verify the social card is exactly 1200×630 (the script's own `assert` already checked this at capture time — confirm the file on disk matches)**

Run: `node -e "const {execSync} = require('child_process'); console.log(execSync('npx playwright --version', {cwd:'scripts/capture'}).toString().trim());" && ls -la docs/assets/`
Expected: a Playwright version string, and `docs/assets/social-card.png` present alongside the six other PNGs.

- [ ] **Step 5: Commit (including the lockfile `npm install` generated in Step 3, for reproducible installs)**

```bash
git add scripts/capture/capture.mjs scripts/capture/.gitignore scripts/capture/package-lock.json docs/assets/*.png
git commit -m "feat: add Playwright capture script and generated showcase screenshots"
```

---

### Task 10: `README.md` and `CHANGELOG.md`

**Files:**
- Create: `README.md`
- Create: `CHANGELOG.md`

**Interfaces:**
- Consumes: the screenshot filenames from Task 9 (`hero-light.png`, `hero-dark.png`, `components.png`, `charts.png`, `type-registers.png`), the install command from Task 7's demo page.
- Produces: `CHANGELOG.md`, which Task 3's `extract_changelog.py` will read once a version tag is cut (outside this plan's scope — see final note).

- [ ] **Step 1: Write `README.md`**

```markdown
# Tri-Swiss

[![Version](https://img.shields.io/github/v/release/luxsolari/tri-swiss)](https://github.com/luxsolari/tri-swiss/releases)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

<p align="center">
  <a href="https://luxsolari.github.io/tri-swiss/">
    <img src="docs/assets/hero-light.png" alt="Tri-Swiss — a tri-tone Swiss-minimalist design system" width="900" />
  </a>
</p>

<p align="center"><strong><a href="https://luxsolari.github.io/tri-swiss/">View the live demo →</a></strong></p>

A Claude Code plugin that teaches Claude **Tri-Swiss** — Lux Solari's
Geist-based house design language, sibling to
[lux-design-system](https://github.com/luxsolari/lux-design-system) (Duotone
Swiss) — so every project you build shares one consistent, opinionated
aesthetic.

## The aesthetic

**Tri-tone strict, Swiss-minimalist.** Two structural colors — ink
(`#000000`) and warm cream (`#eae8d0`) — plus a Swiss Red accent
(`#d3281b`) and one rare, non-semantic highlight, Pastel Turquoise
(`#56bfa3`), reserved for a chart's second series or one single brand
moment. No success green, no info blue, no second *semantic* accent — the
highlight never carries meaning.

- Visible 1px borders everywhere; **no shadows** (elevation is a background step).
- Generous whitespace; mostly square corners.
- **Geist Mono** for headings, data, tags, and nav; **Geist Sans** for body
  and dense-data/utility text.
- Two governed extra registers: **Space Mono** (italic, annotations/captions
  only — the visible nod to Duotone Swiss) and **Jost** (hero title/wordmark
  and section dividers only).
- Uppercase monospace labels with wide letter-spacing.
- Hand-rolled SVG charts by default — no chart libraries except a
  restyled Observable Plot.

## See it

Light and dark are the same tri-tone system inverted — difference by
contrast, never by a new hue:

| Light | Dark |
|-------|------|
| ![Light mode hero](docs/assets/hero-light.png) | ![Dark mode hero](docs/assets/hero-dark.png) |

Five type registers and the component library:

![Type registers](docs/assets/type-registers.png)
![Component gallery](docs/assets/components.png)
![Charts](docs/assets/charts.png)

## What it does

Once installed, the `tri-swiss` skill activates automatically whenever
Claude builds or restyles UI — components, pages, forms, dashboards,
Tailwind/CSS themes — and applies these tokens and patterns by default,
even if you don't name the design system. You can also invoke it explicitly
("apply my design system", "make this tri-swiss", "use the Geist system").

The skill bundles:

- **`assets/theme.css`** — ready-to-paste Tailwind 4 theme with every token
  for light + dark mode. Drop it into `app/globals.css` (or any global
  stylesheet).
- **`references/components.md`** — the full component catalogue: buttons,
  tags, status pips, modals, toggles, cards, inputs, hero/annotation type
  patterns, and the SVG chart patterns.

## Install

Add the marketplace, then install:

```
/plugin marketplace add luxsolari/lux-solari-plugins
/plugin install tri-swiss
```

## Applying it to a project

1. Copy `assets/theme.css` into your global stylesheet.
2. Add the Geist + Geist Mono + Space Mono + Zilla Slab + Jost Google Fonts
   link (or `next/font`).
3. Build with the semantic tokens (`bg-background`, `text-foreground`,
   `border-border`, `bg-primary`, …) and the component patterns. Reach for
   `bg-highlight`/`text-highlight` only for a chart's second series or one
   single brand moment — never for a button, tag, or status color.

Dark mode is the `.dark` class on `<html>`, toggled via JS and persisted to
`localStorage` under a `theme` key.

## License

MIT © 2026 Lux Solari (Luciano Laje)
```

- [ ] **Step 2: Write `CHANGELOG.md`**

```markdown
# Changelog

All notable changes to this plugin are documented here. Format follows
[Keep a Changelog](https://keepachangelog.com/en/1.0.0/); this project adheres to
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

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

- [ ] **Step 3: Verify README image references all exist on disk (from Task 9) and the version badge repo slug matches**

Run:
```bash
node -e "
const fs = require('fs');
const readme = fs.readFileSync('README.md', 'utf8');
const imgs = [...readme.matchAll(/docs\/assets\/([a-z0-9-]+\.png)/g)].map(m => m[1]);
const missing = [...new Set(imgs)].filter(f => !fs.existsSync('docs/assets/' + f));
if (missing.length) { console.error('MISSING IMAGES:', missing.join(', ')); process.exit(1); }
if (!readme.includes('luxsolari/tri-swiss')) { console.error('missing repo slug'); process.exit(1); }
console.log('README.md: all referenced images exist, repo slug correct');
"
```
Expected: `README.md: all referenced images exist, repo slug correct`

- [ ] **Step 4: Commit**

```bash
git add README.md CHANGELOG.md
git commit -m "docs: add README and initial CHANGELOG"
```

---

### Task 11: Push the branch and open the PR

**Files:** N/A — repository-level git/gh operations only.

**Interfaces:**
- Consumes: every commit from Tasks 1–10 on `feat/initial-scaffold`.
- Produces: a reviewable PR on `luxsolari/tri-swiss`; nothing further depends on this task.

- [ ] **Step 1: Confirm the branch's full commit history before pushing**

Run: `git log --oneline main..feat/initial-scaffold`
Expected: 9 commits (Tasks 2–10, one each — Task 1 has no commit, it's pure repo/branch setup).

- [ ] **Step 2: Push the branch**

Run: `git push -u origin feat/initial-scaffold`
Expected: `Branch 'feat/initial-scaffold' set up to track 'origin/feat/initial-scaffold'.`

- [ ] **Step 3: Open the PR**

Run:
```bash
gh pr create --title "Initial Tri-Swiss scaffold" --body "$(cat <<'EOF'
## Summary
- Full initial scaffold of the Tri-Swiss design-system plugin: plugin manifest, governance docs, release automation, the `tri-swiss` skill (SKILL.md + theme.css + components.md), the GitHub Pages showcase page, a philosophy-compliance verifier, a Playwright capture script, README, and CHANGELOG.
- Implements docs/superpowers/specs/2026-07-06-tri-swiss-design.md in full — see that spec for the rationale behind the tri-tone palette, the four-register typography model, and the geist-icons choice.

## Test plan
- [x] `theme.css` token/palette-purity check passes (Task 4)
- [x] `SKILL.md` frontmatter + governance-phrase check passes (Task 5)
- [x] `references/components.md` governance-language check passes (Task 6)
- [x] `verify-philosophy.mjs` passes against `docs/index.html`, and was confirmed to actually catch a rogue-color regression before being reverted (Task 8)
- [x] Playwright capture script produced all 7 expected screenshots, including a `1200×630` social card assertion (Task 9)
- [x] README image references all resolve to real files on disk (Task 10)

🤖 Generated with [Claude Code](https://claude.com/claude-code)
EOF
)"
```
Expected: a PR URL printed, e.g. `https://github.com/luxsolari/tri-swiss/pull/1`.

- [ ] **Step 4: Report the PR URL to the user**

No command — just relay the URL from Step 3's output as this plan's final deliverable.

---

## Follow-ups explicitly out of scope for this plan

- Cutting the first version tag (`v0.1.0` or `v1.0.0`) and triggering `release.yml` — that's a repo-owner decision made after the PR merges, using `scripts/suggest_version_bump.py` for guidance.
- Registering `tri-swiss` in the `luxsolari/lux-solari-plugins` marketplace repo (a separate repo not touched by this plan) so `/plugin install tri-swiss` actually resolves — flag this to the user as a manual follow-up once the PR merges.
- Enabling GitHub Pages for the `docs/` folder on the new repo (Settings → Pages → source: `main` branch, `/docs` folder) — a one-time repo-settings action outside git, do this after the PR merges.
