# Electrical Behavior Model — GitHub Pages deployment

**Date:** 2026-04-15
**Status:** Design approved, pending spec review

## Goal

Publish the existing React component `~/repo/electrical_behavior/electrical_behavior_model.jsx` as a live web page under the `fortresspower` GitHub organization, accessible to internal staff and installers, with the absolute minimum ongoing maintenance.

Published URL: `https://fortresspower.github.io/electrical-behavior-model/`

## Non-goals

- Custom domain (`ebm.fortresspower.io`) — explicitly dropped. Users will be given the GH Pages URL directly.
- Build tooling (Vite, webpack, npm).
- CI workflow (`.github/workflows/`).
- Infrastructure-as-code. No AWS resources created.
- TypeScript, tests, linters.
- Analytics, tracking, feedback widgets.
- SEO. Site is intentionally `noindex`.

## Audience

Internal Fortress Power engineers and field installers. Link-shareable, reasonably official-looking, not marketing-discoverable.

## Architecture

GitHub Pages, "Deploy from a branch" mode, publishing `main` at repo root.

- **Private source repo:** `fortresspower/electrical-behavior-model`
- **Public published site:** GitHub Pages serves the repo root as static files
- **TLS:** Provisioned automatically by GitHub on `*.github.io`
- **Runtime:** React 18 + ReactDOM 18 + Babel standalone, all loaded from CDN (`unpkg` or `cdn.jsdelivr.net`). JSX is compiled in the browser at page load.
- **No DNS, no AWS, no CI, no build.**

## Repo contents

```
electrical-behavior-model/
├── index.html    (everything: CDN scripts + inline JSX + mount call)
└── README.md     (what it is, how to edit, where it's published)
```

Two files. That is the entire repository.

## Source conversion

The existing `electrical_behavior_model.jsx` (944 lines) is used as-is for all component logic, styles, and hooks. Three small edits adapt it to run without a bundler:

1. Replace `import React, { useMemo, useState } from "react";` with `const { useMemo, useState } = React;`
2. Remove `export default` from the component declaration — just declare `function ElectricalBehaviorModel() { ... }` as a top-level function in the inline script.
3. At the end of the inline script, add:
   ```js
   const root = ReactDOM.createRoot(document.getElementById("root"));
   root.render(React.createElement(ElectricalBehaviorModel));
   ```

Everything else (the inline `styles` object, `useMemo`/`useState` usage, JSX markup) stays unchanged.

## `index.html` structure

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="robots" content="noindex, nofollow" />
    <title>Electrical Behavior Model — Fortress Power</title>
    <script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  </head>
  <body>
    <div id="root"></div>
    <script type="text/babel" data-presets="react">
      // ... converted component code ...
      // ... ReactDOM.createRoot(...).render(...) ...
    </script>
  </body>
</html>
```

## Deployment

One-time setup:

1. Create private repo `fortresspower/electrical-behavior-model`.
2. Commit `index.html` and `README.md` to `main`.
3. Repo **Settings → Pages**: Source = "Deploy from a branch", Branch = `main`, Folder = `/` (root). Save.
4. Wait ~1 minute for first deploy. Visit `https://fortresspower.github.io/electrical-behavior-model/`.

Ongoing:

- Edit `index.html`, commit, push to `main`. GitHub rebuilds and redeploys in ~30 seconds.
- No secrets, no credentials, no state.

## Maintenance profile

- **Cert renewal:** handled by GitHub.
- **Dependency updates:** none — React/Babel are pinned to major versions via CDN URLs. Unpinning/updating is a manual `index.html` edit when desired; there is no dependency surface otherwise.
- **Cost:** $0 beyond the existing GitHub org plan.
- **Failure modes:** essentially "is github.io up".

## Trade-offs acknowledged

- **Babel standalone at page load.** ~400 KB download + sub-second JSX compile on first visit. Acceptable for internal/installer use; browser caches subsequent visits. If the tool ever feels sluggish, migrating to Vite + a CI deploy is a ~30-minute change with no impact on users.
- **Single-file monolith.** As the tool grows, the single `index.html` becomes awkward to edit. Splitting out CSS or breaking the component into sub-files will, at that point, justify adopting a build step.
- **Private repo + public site.** Anyone with the URL can view the site. The source stays private. This matches the "internal + installers" audience.

## Open questions

None. Design is complete as specified.
