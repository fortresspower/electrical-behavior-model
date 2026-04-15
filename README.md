# Electrical Behavior Model

Browser-based tool for Fortress Power installers and internal engineering: model **Envy** inverter → battery wiring, run checks on current and connection limits, and see overloads and mis-wiring called out in the UI.

**Live site:** https://fortresspower.github.io/electrical-behavior-model/ (public). This repository is private.

## Scope and limitations

- **Inverters:** Envy 8kW, 10kW, Envy Duo 21 / Envy 12kW (shared model entry where the app groups them).
- **Checks:** Port counts, conductor rating assumptions, node current, Share Battery topology, and eWay Lite / eWay / eWay Pro connection-point capacity.
- **Not a substitute for field work or code compliance.** For education and planning only; follow product documentation and use a licensed electrician for real installations.

## Development

The app is a **single static file** (`index.html`): React 18 via CDN, inline Babel, no `package.json` or build step—by design, to keep maintenance minimal.

**Run locally** (any static server):

```bash
python3 -m http.server 8080
# open http://localhost:8080/
```

Edit `index.html` and refresh the browser.

**Deploy:** GitHub Pages, deploy from branch `main` at repository root. Pushing to `main` updates the live site; there is no separate CI workflow or build.
