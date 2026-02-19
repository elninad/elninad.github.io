# CLAUDE.md — AI Assistant Guide for elninad.github.io

This file provides context for AI assistants working on this repository.

## Project Overview

This is a **personal portfolio website** for Ninad Malvankar (Senior Principal Engineer at Upstox), hosted via GitHub Pages at [https://elninad.github.io/](https://elninad.github.io/).

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

All design tokens are CSS custom properties defined on `:root`:

```css
--bg:      #060b18       /* page background */
--surface: #0d1526       /* section backgrounds */
--card:    #111d35       /* card backgrounds */
--border:  rgba(99,102,241,.18)
--accent:  #6366f1       /* indigo — primary accent */
--accent2: #8b5cf6       /* violet — secondary accent */
--gold:    #f59e0b
--teal:    #22d3ee
--green:   #10b981
--text:    #e2e8f0
--muted:   #64748b
--font:    'Inter', sans-serif
--mono:    'JetBrains Mono', monospace
--radius:  16px
```

**Always use these variables** when modifying or adding styles. Do not hardcode color values.

### Responsive Breakpoint

Single breakpoint at `768px`. The site is mobile-first — the desktop layout is the enhancement.

### Animation Conventions

CSS keyframe names: `fade-in`, `slide-up`, `pulse`, `bounce`, `spin-slow`, `float-up`.

Elements that animate on scroll carry the class `.reveal`. The Intersection Observer adds `.visible` to trigger the animation. Skill bar widths are stored in `style="--w: XX%"` custom properties and animated via CSS when `.visible` is set on the parent.

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
In the Skills section, each `<div class="skill-bar">` has a child `<div class="bar" style="--w: XX%">`. Update the percentage there; the CSS animation reads it via the `--w` custom property.

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
