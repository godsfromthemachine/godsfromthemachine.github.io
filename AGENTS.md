# Agents Guide — godsfromthemachine.github.io

## Project Overview

Official website for [Gods from the Machine](https://github.com/godsfromthemachine), built with [Hugo](https://gohugo.io/) and a custom `machine-gods` theme. Deployed to GitHub Pages via GitHub Actions. Live at **https://godsfromthemachine.github.io**.

## Repository Structure

```
godsfromthemachine.github.io/
├── hugo.toml                          Site config (baseURL, title, menus, params)
├── content/
│   ├── _index.md                      Home page frontmatter
│   ├── projects/
│   │   ├── _index.md                  Projects index (pantheon overview, naming conventions)
│   │   ├── gilgamesh.md               Gilgamesh project page (features, architecture, token budget)
│   │   ├── zeus.md                    Zeus project page
│   │   ├── raijin.md                  Raijin project page
│   │   └── garuda.md                  Garuda project page
│   ├── architecture/
│   │   └── _index.md                  Architecture page (duality principle, agent loop, design decisions)
│   ├── roadmap/
│   │   └── _index.md                  Roadmap page (6 phases with visual timeline)
│   └── about/
│       └── _index.md                  About page (vision, motivation, values, hardware)
├── themes/machine-gods/
│   ├── theme.toml                     Theme metadata
│   ├── layouts/
│   │   ├── index.html                 Home page layout (hero, cards, principles, terminal)
│   │   ├── _default/
│   │   │   ├── baseof.html            Base template (head, nav, main, footer)
│   │   │   ├── single.html            Single content page layout
│   │   │   └── list.html              List/section page layout
│   │   └── partials/
│   │       ├── nav.html               Sticky navigation bar with hex logo
│   │       └── footer.html            Footer with license and tagline
│   └── static/
│       ├── css/style.css              Full site stylesheet (dark terminal aesthetic)
│       └── img/
│           ├── logo.svg               Organization logo (hexagonal icon)
│           ├── architecture.svg       CLI/MCP/API architecture diagram
│           └── favicon.svg            Browser favicon (hex icon)
├── .github/workflows/
│   └── hugo.yml                       GitHub Actions: build Hugo + deploy to Pages
├── .gitignore                         Ignores public/, resources/, .hugo_build.lock
├── LICENSE                            MIT, Baalateja Kataru
├── README.md                          Dev instructions
└── archetypes/default.md              Hugo default archetype
```

## Build & Preview

```bash
hugo server -D     # local dev server with drafts (http://localhost:1313)
hugo --minify       # production build → public/
```

Requires Hugo v0.123.7+ (extended edition).

## Deployment

Automatic via GitHub Actions on push to `main`. Workflow in `.github/workflows/hugo.yml`:
1. Installs Hugo 0.123.7 extended
2. Builds with `hugo --minify`
3. Uploads to GitHub Pages via `actions/deploy-pages@v4`

No manual deployment needed — just push to main.

## Theme: machine-gods

Custom theme with zero external dependencies (no Bootstrap, no Tailwind, no JS frameworks).

### Color Scheme
- Background: `#0a0a0f` (near-black)
- Card/code background: `#111118` / `#0d1117`
- Border: `#1e1e2e`
- Text: `#c9c9d4` (default), `#e8e8f0` (bright), `#666680` (muted)
- Cyan: `#00d4ff` — CLI, links, primary accent
- Purple: `#7b2ff7` — MCP, section headings, principle borders
- Pink: `#ff006e` — HTTP, tertiary accent
- Green: `#00ff88` — status indicators, terminal prompt

### Typography
- Sans: Inter (system fallback chain)
- Mono: JetBrains Mono, Fira Code, Cascadia Code, SF Mono (fallback chain)
- All headings use monospace font

### Components
- **Hero section** — centered logo, tagline, CTA buttons
- **Card grid** — responsive project cards with language badges and status indicators
- **Principles grid** — bordered blocks with alternating accent colors
- **Terminal widget** — faux macOS terminal with colored prompt/commands
- **Roadmap timeline** — vertical dot-line timeline with phase status (done/active/future)
- **Prose styles** — tables, code blocks, blockquotes, all dark-themed

### Responsive
- Single-column layout below 768px
- Nav stacks vertically on mobile
- Card grid collapses to single column

## Content Editing

### Adding a New Project Page
1. Create `content/projects/newgod.md` with frontmatter: `title`, `description`
2. Add status badge, language tag, and GitHub link at the top
3. Add a card entry in `themes/machine-gods/layouts/index.html` home page
4. Update the projects table in `content/projects/_index.md`

### Updating the Roadmap
Edit `content/roadmap/_index.md`. Use the existing HTML structure:
- `<div class="phase">` wrapper
- `<div class="phase-dot done|active|future">` for status indicator
- `<li class="done|todo">` for checklist items

### Updating Architecture
Edit `content/architecture/_index.md`. Contains markdown with embedded HTML for the SVG diagram. Keep code blocks and diagrams in sync with actual gilgamesh implementation.

## Keeping Content in Sync

This site documents the state of the Gods from the Machine project. When things change:
- **New tool added to gilgamesh** → update architecture page, gilgamesh project page, home page tool count
- **Phase completed** → update roadmap page
- **New project/god created** → add project page, update home page cards, update projects index
- **Version bump** → update gilgamesh project page status badge

## Commit Conventions

- Author: `bkataru <baalateja.k@gmail.com>`
- Run `hugo` locally to verify build before pushing
- Keep commits descriptive
