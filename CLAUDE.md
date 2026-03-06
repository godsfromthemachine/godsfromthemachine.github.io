# CLAUDE.md — godsfromthemachine.github.io

## What is this?

Official website for [Gods from the Machine](https://github.com/godsfromthemachine). Hugo static site with custom `machine-gods` theme. Deployed to GitHub Pages via Actions. Live at https://godsfromthemachine.github.io.

## Build & Preview

```bash
hugo server -D     # dev server at localhost:1313
hugo --minify       # production build
```

Requires Hugo v0.123.7+ extended. Deploys automatically on push to `main`.

## Critical Rules

- **No external CSS/JS dependencies.** The theme is fully custom — no Bootstrap, Tailwind, or JS frameworks.
- **Color scheme**: cyan (#00d4ff), purple (#7b2ff7), pink (#ff006e), green (#00ff88), dark bg (#0a0a0f).
- **Fonts**: monospace for headings/code (JetBrains Mono, Fira Code fallback), sans for body (Inter, system fallback).
- **Keep content in sync** with actual project state. When gilgamesh changes, update the site.
- **Run `hugo` before pushing** to verify the build succeeds.
- **Author**: `bkataru <baalateja.k@gmail.com>`.

## Site Structure

```
content/
├── _index.md              Home (hero, project cards, principles, terminal)
├── projects/              Per-project pages (gilgamesh, zeus, raijin, garuda)
├── architecture/          Duality principle, agent loop, design decisions
├── roadmap/               6-phase timeline with status indicators
└── about/                 Vision, motivation, values, hardware targets
```

## Theme Location

`themes/machine-gods/` — layouts in `layouts/`, CSS in `static/css/style.css`, SVGs in `static/img/`.

## Navigation

Defined in `hugo.toml` under `[menu]`: Home, Projects, Architecture, Roadmap, About, GitHub (external).

## Adding Content

- New project: add `content/projects/newgod.md`, add card to `layouts/index.html`, update `content/projects/_index.md`
- Roadmap update: edit `content/roadmap/_index.md`, use `phase-dot done|active|future` and `li done|todo` classes
- New page: create `content/section/_index.md`, add menu entry in `hugo.toml`
