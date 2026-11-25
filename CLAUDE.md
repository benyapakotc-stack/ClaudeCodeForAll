# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static Thai-language website introducing Thai food. No build step, no dependencies, no tests, no linter: open `index.html` directly in a browser (`file://` works, including `menu.html?id=...`). All user-facing text is Thai.

## Architecture

Two HTML pages, each self-contained (inline `<style>` and `<script>`), plus food photos (JPG, from Wikimedia Commons) in `image/`.

**`index.html`** (main page). The 23 menus ("เมนูเด็ด") are plain `<article class="card dish">` elements in `#dishes`. There is no separate data source; the JS reads everything from the DOM:
- Menu name = the card's `h3` text. Favorites are stored in `localStorage` key `thaiFoodFavorites` as a JSON array of these Thai names, so renaming a menu orphans any saved favorite (unknown names are dropped on load).
- Spice level for filtering = the count of `🌶` characters inside `.spice b` (0 for "ไม่เผ็ด"). Search matches `h3`, `.en` and `p`.
- The favorite hearts are injected by JS, and the "เมนูโปรด" list and the random "เมนูวันนี้" card are `cloneNode(true)` copies of these cards. Any markup added to a card (like the `.detail-link`) shows up in all three places automatically. The search/spice filter hides cards only via `#dishes .grid .is-out`, so the clones are deliberately unaffected.

**`menu.html`** (ingredient page for one menu). A single page driven by `?id=<slug>` and a `MENUS` object in its script. Keep these in sync when adding or renaming a menu:
1. the card in `index.html` and its `<a class="detail-link" href="menu.html?id=<slug>">`
2. the `MENUS[<slug>]` entry (name, English name, description, spice, `img` filename, `main` and `flavor` lists; a third `true` element marks an optional ingredient)
3. the photo in `image/` (the card's `<img>`, `MENUS[].img`, and `MENUS[].photo` = [photographer, licence, Commons page URL]) plus its line in the `details.credits` list in the `index.html` footer; CC BY / BY-SA licences require that attribution

`menu.html` copies the `:root` colour variables and nav/footer styles from `index.html` rather than sharing a stylesheet, so a theme change needs both files.

Each card's `.emoji` div holds an `<img>` (styled by `.dish .emoji img`, 4:3 `object-fit: cover`); the four flavor cards use `.flavor .photo img` (4:3, `image/flavor-*.jpg`, credited in the same `details.credits` list). The old hand-drawn SVGs remain in `image/` but are unused.

## Visual design

Styling follows `.claude/DESIGN.md` (Houseplant: walnut `#321e1e` on linen `#f4f1e0`, flat surfaces, 4px radius on buttons/inputs/chips, 8px on cards, hairline rules, no gradients, no accent colours; the food photos supply all the colour). DESIGN.md contains typos (`#321e1`, `80-120px` as a value); use the 6-digit hex. Deliberate deviations for Thai text: no `-0.05em` tracking or `line-height: 1` on Thai headings (tone marks collide), reserved for the Latin "THAI FOOD" footer wordmark, and the Thai font fallbacks stay in `--font` because the Latin display fonts have no Thai glyphs. Several rules are load-bearing for the JS: keep `[hidden] { display: none !important; }`, `#dishes .grid .is-out`, and the `nav ul a.active` / `[aria-pressed="true"]` styles.

## Vocabulary rules

`GLOSSARY.md` is the source of truth for terminology and lists forbidden synonyms (`_Avoid_`). Examples: use "เมนูเด็ด" not "เมนูยอดนิยม/เมนูแนะนำ", "ภาค" not "ภูมิภาค", "เมนูโปรด" not "เมนูที่ชอบ", "ส่วนประกอบ" not "ส่วนผสม". "เมนู" means food only, never the top navigation bar (call that "แถบนำทาง"). After editing page text, grep the `_Avoid_` terms. The glossary also fixes the เมนูเด็ด at 23 menus, and that set is the only pool for the random pick.

## Docs state

`plan.md` is stale: it still describes a single-file, emoji-only, local-only site. Trust the code and `GLOSSARY.md` over it. `task.md` is the checklist of what is done and open.

## Git / tooling

- Remote: `https://github.com/benyapakotc-stack/ClaudeCodeForAll` (public), branch `main`.
- `.claude/settings.local.json` is gitignored (machine-local hooks); don't commit it.
- The repo-local git identity uses a GitHub noreply email so the real address isn't published. Don't change it.
- `gh` is installed but may not be on `PATH` in an already-open shell; the full path is `C:\Program Files\GitHub CLI\gh.exe`. `gh auth setup-git` has been run, so plain `git push` works.
- Skills live in `.agents/skills/` (locked in `skills-lock.json`); `.claude/skills/` mirrors them.
