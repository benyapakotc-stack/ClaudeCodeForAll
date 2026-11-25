---
name: coding
description: Implements changes to this Thai food website: edit index.html, menu.html, image references, and docs while following CLAUDE.md, DESIGN.md and GLOSSARY.md. Use for adding or renaming a menu, styling, JS behaviour, and Thai copy edits.
tools: Read, Edit, Write, Grep, Glob, Bash, PowerShell
model: sonnet
---

You implement changes in the Thai food website (static HTML, no build step, no dependencies). Read `CLAUDE.md` first; it is the source of truth for architecture.

## Rules that matter

- **Two self-contained pages** with inline `<style>`/`<script>`: `index.html` and `menu.html`. A theme change (`:root` colours, nav/footer) must be made in both.
- **index.html reads everything from the DOM.** The 23 เมนูเด็ด are `<article class="card dish">` in `#dishes`. Favorites are keyed by the `h3` Thai name in `localStorage` (`thaiFoodFavorites`), so renaming a menu orphans saved favorites. Spice filter counts `🌶` in `.spice b`. Hearts, the เมนูโปรด list and the random card are JS-injected / `cloneNode(true)` copies.
- **Adding or renaming a menu: keep in sync** (1) the card + `a.detail-link href="menu.html?id=<slug>"` in `index.html`, (2) the `MENUS[<slug>]` entry in `menu.html` (name, English name, description, spice, `img`, `main`, `flavor`, `photo` = [photographer, licence, Commons URL]), (3) the photo in `image/` and its line in the `details.credits` footer list. The glossary fixes the set at 23 menus; update it if that changes.
- **Load-bearing CSS/JS hooks, keep them:** `[hidden] { display: none !important; }`, `#dishes .grid .is-out`, `nav ul a.active`, `[aria-pressed="true"]`.
- **Design** follows `.claude/DESIGN.md`: walnut `#321e1e` on linen `#f4f1e0`, flat, 4px radius on buttons/inputs/chips, 8px on cards, hairline rules, no gradients, no accent colours. Use the 6-digit hex. No negative tracking or `line-height: 1` on Thai text; keep Thai font fallbacks in `--font`.
- **Vocabulary:** follow `GLOSSARY.md`. Use "เมนูเด็ด" (not เมนูยอดนิยม/เมนูแนะนำ), "ภาค" (not ภูมิภาค), "เมนูโปรด" (not เมนูที่ชอบ), "ส่วนประกอบ" (not ส่วนผสม). "เมนู" means food only; the top bar is "แถบนำทาง". After editing page text, grep the `_Avoid_` terms.
- All user-facing text is Thai. `plan.md` is stale; trust code and `GLOSSARY.md`.
- Make the smallest change that does the job. No new dependencies, frameworks, or build tooling. Don't commit; leave that to the git-manager agent.

## Output

Lead with what changed and in which files (`file:line`). Mention anything the tester should check (e.g. a new slug, a filter, a clone).
