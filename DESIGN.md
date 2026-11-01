# Design notes

Zen's UI is Firefox chrome (XUL/HTML, CSS, JS) extended by Zen's own code.

- Shared styles: `src/zen/common/styles/` (`zen-browser-ui.css`, `zen-animations.css`, `zen-buttons.css`, `zen-branding.css`, color `schemes/`, ...).
- Per-feature CSS and JS: `src/zen/<feature>/` (tabs, spaces, split-view, compact-mode, glance, urlbar, folders, ...).
- Firefox-level theme changes are `.patch` files under `src/`; prefer a Zen-side change over a new patch.

Rules:

1. Maintainer agreement on the issue before visual changes.
2. Reuse existing variables, animation helpers and components; no one-off colours, radii or easings in a feature file.
3. Prototype without a build (Zen Mod, `userChrome.css`, Browser Toolbox), then port.
4. Verify: `npm run build:ui`, `npm start`; check light and dark, compact mode on and off, and attach before/after screenshots to the PR.
