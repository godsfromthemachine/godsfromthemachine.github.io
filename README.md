# godsfromthemachine.github.io

The official website for [Gods from the Machine](https://github.com/godsfromthemachine) — autonomous programs powered by local AI.

Live at: **https://godsfromthemachine.github.io**

## Pages

- **Home** — Vision, project overview, principles, quick start
- **Projects** — Gilgamesh, Zeus, Raijin, Garuda
- **Architecture** — Technical design, duality principle, agent loop, token budget
- **Roadmap** — Phase-based milestones from foundation to future
- **About** — Motivation, design values, hardware targets

## Built With

- [Hugo](https://gohugo.io/) — Static site generator
- Custom `machine-gods` theme — dark terminal aesthetic, no external dependencies
- SVG graphics — all logos and diagrams are hand-crafted SVGs

## Development

```bash
# Install Hugo
# https://gohugo.io/installation/

# Run local dev server
hugo server -D

# Build for production
hugo --minify
```

## Deployment

This site deploys to GitHub Pages via GitHub Actions. Pushing to `main` triggers a build and deploy.

## License

MIT
