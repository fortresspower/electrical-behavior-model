# Electrical Behavior Model

Interactive battery + inverter wiring validator for Fortress Power installers and internal engineering.

**Published at:** https://fortresspower.github.io/electrical-behavior-model/

The published site is public; this source repo is private.

## What it does

Models inverter → battery wiring topologies for Envy 8kW / 10kW / Envy Duo 21 / Envy 12kW.
Validates port limits, conductor ratings, node current, Share Battery topology, and connection point (eWay Lite / eWay / eWay Pro) capacity. Highlights overloads, improper terminal usage, and inactive components.

For theoretical and educational use only — always consult a licensed electrician before installing anything.

## Editing

This is a **single-file, no-build** static site. The entire app is `index.html`:

- React 18 + ReactDOM 18 + Babel standalone loaded from unpkg
- Component code is an inline `<script type="text/babel">` block
- No `package.json`, no node, no bundler

To iterate locally:

```bash
# any static file server works; e.g.
python3 -m http.server 8080
# then open http://localhost:8080/
```

Edit `index.html`, refresh the browser. No build step.

## Deployment

GitHub Pages, "Deploy from a branch" mode, `main` / `/` (root).

Push to `main` → GitHub rebuilds and redeploys in ~30 seconds. Nothing else is automated — there is no workflow file, no CI secrets, no infrastructure.

## Design doc

See [`docs/2026-04-15-electrical-behavior-model-gh-pages-design.md`](docs/2026-04-15-electrical-behavior-model-gh-pages-design.md) for why it's built this way.

## Upgrading to a real build

If this ever outgrows the single-file pattern (dependency hell, dev-tooling needs, multi-page), migrate to Vite + a GitHub Actions deploy workflow. It's ~30 minutes of work. The single-file approach was chosen for minimum ongoing maintenance.
