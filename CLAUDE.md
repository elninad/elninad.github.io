# CLAUDE.md — AI Assistant Guide for elninad.github.io

This file provides context for AI assistants working on this repository.

## Project Overview

This is a **personal portfolio website** for Ninad Malvankar (Architect at Upstox), hosted via GitHub Pages at [https://elninad.github.io/](https://elninad.github.io/).

The site is a **single-page, zero-build static website** — all content lives in one HTML file with embedded CSS and JavaScript. There is no framework, no package manager, no build step, and no test suite.

## Repository Structure

```
elninad.github.io/
├── index.html      # Entire website: HTML + embedded CSS + embedded JS (~1,226 lines)
├── robots.txt      # Allows all crawlers, references sitemap
├── sitemap.xml     # Single-entry sitemap for the home page
└── CLAUDE.md       # This file
```

## Technology Stack

| Layer | Technology |
|---|---|
| Markup | HTML5 (semantic elements) |
| Styling | CSS3 with custom properties (variables), no preprocessor |
| Scripting | Vanilla JavaScript (ES6+), no frameworks |
| Icons | Font Awesome 6.5.0 (CDN) |
| Fonts | Google Fonts — Inter (body) + JetBrains Mono (code/badges) |
| Hosting | GitHub Pages (auto-deployed from `main` branch root) |
| SEO | JSON-LD structured data, Open Graph, Twitter Cards, sitemap |

## Development Workflow

### No Build Step Required

Changes to `index.html`, `robots.txt`, or `sitemap.xml` are deployed automatically by GitHub Pages when pushed to the `main` branch. There is no `npm install`, no compilation, and no CI/CD pipeline.

### Local Preview

Open `index.html` directly in a browser, or use any static file server:

```bash
# Python
python3 -m http.server 8080

# Node.js (if available)
npx serve .
```

### Deployment

Push to `main`. GitHub Pages handles the rest.

```bash
git add .
git commit -m "feat: <description>"
git push origin main
```

## index.html — Internal Structure

The file is divided into well-commented sections using ASCII dividers:

```
/* ─── Section Name ─────────────────────────────────────────── */
```

### Head (lines ~1–644)
- Meta tags: charset, viewport, description, keywords, robots, author
- Open Graph and Twitter Card tags
- Google Fonts and Font Awesome CDN links
- Canonical URL
- Embedded `<style>` block (all CSS)
- JSON-LD structured data (`Person`, `WebSite`, `Article`, `FAQPage` schemas)

### Body Sections (lines ~645–1170)
| Section | ID | Content |
|---|---|---|
| Navigation | `#navbar` | Fixed header with logo `<NM />` and nav links |
| Hero | `#hero` | Title, badge, stats, CTA buttons, particle animation |
| About | `#about` | Bio, avatar with orbit animation, skill pills |
| Skills | `#skills` | Animated progress bars by category |
| Timeline | `#journey` | Career and education history (alternating layout) |
| Certifications | `#certs` | AWS + academic credentials |
| Writing | `#writing` | Medium article cards |
| Contact | `#contact` | LinkedIn, Medium, GitHub buttons |
| Footer | — | Attribution and location |

### Scripts (lines ~1171–1224)
All vanilla JavaScript, no external libraries:
- **Parallax**: translates `#parallaxBg` at 35% scroll speed (passive listener)
- **Sticky nav**: toggles `.scrolled` class on `#navbar` after 40px scroll
- **Particles**: creates 28 floating `<div>` particles with randomized positions, colors, sizes, and animation durations
- **Intersection Observer**: adds `.visible` to `.reveal` elements at 12% viewport threshold; triggers skill bar width animations; unobserves after first trigger for performance
- **Timeline stagger**: applies incremental `transition-delay` (6ms steps) to timeline items

## CSS Design System

The site uses a retro terminal + arcade ("Press Start") theme: CRT phosphor palette, Ghostty-style terminal panels, pixel/display fonts, and custom 8-bit SVG sprites. All design tokens are CSS custom properties defined on `:root`:

```css
--bg:      #060a07       /* CRT black, green-tinted page background */
--surface: #0a120c       /* section backgrounds */
--card:    #0f1a12       /* card / terminal-panel backgrounds */
--border:  rgba(51,255,124,.16)
--accent:  #33ff7c       /* phosphor green — primary accent */
--accent2: #ff5fd7       /* neon magenta — secondary accent */
--gold:    #ffb000       /* amber phosphor / coin gold */
--teal:    #3fd9ff       /* neon cyan */
--green:   #00d68a
--text:    #d9f2dd       /* phosphor white */
--muted:   #8db096
--font:    'Inter', sans-serif          /* body copy */
--mono:    'JetBrains Mono', monospace  /* dates, tags, prompts */
--pixel:   'Press Start 2P', monospace  /* short uppercase labels ONLY (.5-.8rem) */
--display: 'VT323', monospace           /* section h2 headings + terminal text */
--radius:  10px
```

Each accent also has an RGB-triplet twin for translucent tints — `--accent-rgb`, `--accent2-rgb`, `--gold-rgb`, `--teal-rgb`, `--green-rgb`, `--red-rgb`, plus `--ink-rgb` (overlay ink), `--nav-rgb` (nav backdrop) and `--accent-shadow` (3D button base). Write tints as `rgba(var(--accent-rgb), .12)` — never as literal channel values — so the alternate-universe palette swap recolours them.

**Always use these variables** when modifying or adding styles. Do not hardcode color values.

**No smooth gradients.** The theme bans color fades (`linear-gradient` blends, radial glows, gradient text). Allowed lookalikes: hard-edged `repeating-linear-gradient` stripes (scanlines, dashed tracks, bar notches) and hard-stop `conic-gradient` segments (avatar ring). For glow, use `text-shadow`/`box-shadow` in an accent tint; for atmosphere, use the pixel starfield (`.px-twinkle`), not gradient blobs.

### Alternate Universe (`html.alt-univ`)

The Konami code performs a **dimension shift**: `html.alt-univ` swaps every `:root` token to a modern light 2026 palette (indigo `#4f46e5` accent, white cards, Inter replacing the pixel/display fonts) and hides the CRT scanlines. The reality-rift cursor lens previews this universe through `backdrop-filter: invert(1) hue-rotate(...)`. When adding styles, check they read correctly in both universes — anything tokenized via the variables above adapts automatically.

### Theme Conventions

- `.term` + `data-title="..."` turns any panel into a terminal window (title bar and traffic-light dots are drawn by `::before`/`::after`). Cards that set their own `padding` shorthand need a `.term.<class>` padding-top override (see `.term.skill-card`).
- Custom pixel art lives in an inline `<svg><defs><symbol id="px-*">` sprite sheet right after `<body>`; instance it with `<use href="#px-...">` or the `spriteSvg()` JS helper. Do not use emoji anywhere; use sprites instead.
- Do not use em dashes in new decorative/UI text; use `::`, `>`, hyphens, or pipes as separators.
- A static CRT scanline overlay is drawn by `body::after` (disabled under `prefers-reduced-motion`, hidden in the alt universe).
- Decorative game labels (`.section-tag`, `.level-marker`, `.tl-badge`, `.tl-xp`, lore HUD, score HUD, GAME OVER footer line, final score line) are `aria-hidden` where they carry no real content; never move entity facts into them.
- **Arcade engine (`GAME`)**: a script-block module that owns the ghost companion (`#ghostPal` chases the cursor on fine pointers, docks bottom-right under `html.no-pointer`), the score HUD (`#scoreHud`), the dynamic canvas favicon (ghost recoloured per level), and the nav-logo ghost (`#navGhost`). Points: +15 per section explored, +5 per FAQ lore entry (+100 lore complete), +10 per unique terminal command, +2 per unique interactive click, +500 Konami secret, plus the shown value for each timeline XP orb (`.tl-xp` is click-to-collect once; it gains `.claimed` and an `:: EATEN` suffix). Levels at 60/300/1200/6000 change ghost size/colour — thresholds are tuned so exploration reaches lv3-4 and orb feasting is the fast lane to lv5. Award points via `GAME.add(n, x, y)` — every earn runs the visible pipeline: a `.fly-coin` flies from the earn spot to the ghost, the ghost chomps and shows `+N`, then a `.fly-spark` carries it into the HUD, which bumps (`hud-bump`) while the displayed score rolls up to the real total. Orb clicks instead run `feast(n, x, y)`: a staggered coin shower, a longer `ghost-feast` gulp, a `.score-pop.big` banner (`pop-rise-big`), and a 3-spark volley into the HUD.
- **Pixel cursor & reality rift**: on fine pointers without reduced motion, `html.custom-cursor` hides the native cursor behind `#pxCursor` (crosshair, rotates over links) and `#riftLens` — a `backdrop-filter` lens masked by a pixelated glitch silhouette (`rift-glitch` snaps between blocky staircase clip-path polygons via `step-end`, with RGB interference lines inside) that previews the alternate universe, squashes/stretches along its direction of travel, and drags two tinted `.rift-echo` ectoplasm trails whose opacity follows pointer speed. All disabled on touch (`@media (hover: none)`) and under reduced motion.
- The companion ghost uses the dedicated `#px-ghost-pal` sprite: `currentColor` body plus a contrast rim and eye colors exposed as CSS vars (`--ghost-rim`, `--ghost-eye`, `--ghost-pupil`) set inline via `style="fill:var(...)"` so they pierce the `<use>` shadow DOM. The rim stays dark (outline on light surfaces, incl. inside the inverted rift); the colored drop-shadow glow defines it on dark surfaces.

### Responsive Breakpoint

Single breakpoint at `768px`. The site is mobile-first — the desktop layout is the enhancement.

### Animation Conventions

CSS keyframe names: `fade-in`, `slide-up`, `pulse`, `bounce`, `spin-slow`, `float-up`, `blink`, `glint`, `t-print`, `dialogue-in`, `lore-flash`, `twinkle`, `ghost-bob`, `ghost-chomp`, `pop-rise`, `pop-rise-big`, `ghost-feast`, `rift-glitch`, `ring-flick`, `hud-bump`, `dim-flash`.

Elements that animate on scroll carry the class `.reveal`. The Intersection Observer adds `.visible` to trigger the animation. Skill bar fills read their target width from `data-width` attributes when `.visible` is set on the parent.

All new animations (typed hero intro, blinking cursors/labels, scanlines, starfield, ghost companion, rift cursor, dimension shift) are disabled or skipped under `prefers-reduced-motion`; the typing JS checks `matchMedia` and shows everything instantly.

### Interactive Terminal

The hero is a terminal window (`#heroTerm`). The first prompt line (`.t-cmd.t-first`) is visible with a blinking cursor from first paint; the typed intro then fills it in (lines get `t-done` when finished, which hides their cursor). After the intro it reveals a live prompt (`#termInput`) whose caret is a permanently blinking block (`#termCursor`): the native caret is transparent and a hidden `.t-mirror` span measures the typed text so the block sits right after it — keep mirror and input fonts in sync. Commands are defined in the `COMMANDS` map in the script block (help, whoami, skills, quests, certs, writing, beyond, contact, talk, ask, konami, score, warp, sudo, clear). Output is rendered with `textContent`/`createElement` only — never `innerHTML`. The `ask <question>` command keyword-matches FAQ summaries in the DOM and opens the matching entry; the FAQ "LORE" tracker counts opened entries and feeds the score. The Konami code (up up down down left right left right B A) triggers the **dimension shift** — toggling `html.alt-univ` (and the +500 secret, first time only); `warp` re-triggers it once the secret is found.

## HTML Conventions

- Use semantic HTML5 elements: `<main>`, `<nav>`, `<section>`, `<article>`, `<address>`, `<time>`
- External links must use `rel="noopener"` (and `target="_blank"` where appropriate)
- Use `var(--mono)` font for dates, badges, and code-like labels
- Heading hierarchy: `<h1>` in hero only; sections use `<h2>`; cards use `<h3>`
- Icons come from Font Awesome classes, e.g. `<i class="fab fa-aws"></i>`

## SEO Conventions

- Update `<meta name="description">` and OG/Twitter description tags if page content changes significantly
- Update `sitemap.xml` `<lastmod>` date on any substantive content change
- JSON-LD schemas (`Person`, `WebSite`, `Article`, `FAQPage`) are in `<script type="application/ld+json">` blocks in `<head>` — keep them in sync with actual content

## Commit Message Convention

```
type: short description (#PR)
```

- **feat**: new feature or section
- **fix**: bug or content correction
- **style**: CSS/visual changes
- **chore**: dependency, config, or meta updates
- **docs**: documentation only

Example: `feat: add open source projects section (#7)`

Include a Claude Code session URL in the commit body when relevant (this is standard practice in this repo based on git history).

## Common Tasks

### Add a new timeline entry
Find the `<div class="timeline" id="journey">` section. Copy an existing `<div class="timeline-item">` block and update the content. Choose the correct dot color:
- Education: `teal`
- Past work: `green`
- Leadership: `gold`
- Current role: `accent2` (violet)

### Update skill percentages
In the Skills section, each `.skill-bar-row` has a `.skill-bar-fill` with a `data-width="XX"` attribute plus matching `aria-valuenow` and visible label text. Update all three together; the Intersection Observer JS reads `data-width` to animate the bar, and the label renders as `LV XX%` via CSS.

### Add a new section
1. Add the `<section id="new-section" class="section reveal">` block in `<body>`
2. Add a nav link `<a href="#new-section">Label</a>` in `<nav>`
3. Add corresponding CSS in the embedded `<style>` block, following the existing section pattern
4. If the section has scroll-triggered animations, add `.reveal` to the root element and `.reveal` to child elements that should animate

### Modify SEO metadata
All meta tags are in `<head>`. JSON-LD structured data blocks follow the embedded CSS. Update both the `<meta>` tags and the JSON-LD when changing job titles, company, or skills.

## What NOT to Do

- Do not introduce a build system, bundler, or package manager unless explicitly requested
- Do not add JavaScript frameworks (React, Vue, etc.) without explicit instruction — the site intentionally uses vanilla JS
- Do not hardcode colors — always use CSS custom properties
- Do not remove `rel="noopener"` from external links
- Do not break the single-file architecture without explicit discussion
- Do not modify `robots.txt` to disallow crawlers
