# AGENTS.md

Fork of [zen-browser/desktop](https://github.com/zen-browser/desktop) (Zen Browser, a Firefox-based browser; MPL-2.0). `dev` is the default branch. Zen's own code is in `src/zen/` (tabs, spaces, split-view, compact-mode, glance, urlbar, ...); Firefox changes are `.patch` files under `src/`; the Firefox source itself is downloaded into the git-ignored `engine/`.

## Onboarding essentials

- **Access:** none needed to read or fork (public). To open PRs you need a GitHub account and `gh auth login`. No maintainer grant is required for upstream PRs; maintainers review at `zen-browser/desktop`.
- **Flow:** find or open the issue first (`gh issue list -R zen-browser/desktop`); for features or design changes get a maintainer's agreement on the issue before coding. Branch from upstream: `git fetch upstream dev && git switch -c fix/<short-name> upstream/dev` (add the remote with `git remote add upstream https://github.com/zen-browser/desktop.git`). Commit titles look like `gh-<issue>: Short description`. Push to your fork, open a PR against `zen-browser/desktop:dev`. Read `docs/contribute.md` and `CODE_OF_CONDUCT.md`.
- **Design rules:** (1) reuse Zen's existing CSS and components in `src/zen/common/styles/` and the feature folders; don't add colours or animations outside them; (2) prototype in an installed Zen with a Zen Mod, `userChrome.css` or the Browser Toolbox, then port to `src/zen/`; (3) test light and dark and compact mode, attach before/after screenshots to the PR. See DESIGN.md.
- **Fact sources:** upstream docs (https://docs.zen-browser.app, the "building" guide first), `docs/contribute.md`, and the issue. Don't state browser behaviour you haven't reproduced.
- **Sibling repos:** none required.
- **Machine-specific:** none. Needs ~40 GB free disk and a first full build of about an hour.

## Setup (macOS, verified)

Toolchain pins: Node from `.nvmrc` (22), Python from `.python-version` (3.11), Rust from `rust-toolchain.toml` (via rustup).

```bash
brew install sccache fnm gnu-tar          # plus Xcode, rustup, uv
fnm install 22 && uv python install 3.11
uv venv --python 3.11 .venv && source .venv/bin/activate
npm i
npm run init          # downloads + bootstraps Firefox source (long)
python3 ./scripts/update_en_US_packs.py
npm run build         # first build ~1h; later: npm run build:ui (JS/CSS only)
npm start
```

Pitfalls: `npm run init` on macOS needs GNU tar (`gnu-tar`). `Unexpected end of input` while unpacking means a truncated tarball in `.surfer/engine/`: delete it and `engine/`, rerun init.

## Check before pushing

`npm run lint` (and `npm run lint:fix`), `npm test`; `npm run lc` checks license headers (new files need the MPL header). Husky runs `lint-staged` on commit.

## Don'ts

Don't edit `engine/` directly (it is generated from Firefox plus patches); change `src/` and rebuild. Don't commit `.venv/`, `node_modules/` or `.surfer/`. Don't open large unsolicited refactors.
