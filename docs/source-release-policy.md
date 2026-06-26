# Source release policy (template)

## Principles

- **Source-first is allowed** — optional `src/` scaffold, docs, and modular extraction for maintainability.
- **Release must be one HTML file** for GitHub Pages unless the user explicitly ships another contract.
- **Build pipeline is optional** until proven parity with root `index.html`.
- **Tests must target the release artifact** — default root `index.html`; `CR_RELEASE_ARTIFACT=dist` only when `dist/index.html` is canonical.
- **No runtime externals** unless approved (CDN scripts, fonts, images).
- **Root `index.html` stays canonical** until a build step passes the full harness against `dist/`.

## What not to do

- Do not point Pages at a dev-only file while players expect production `index.html`.
- Do not skip SAVE_VERSION migration when changing save shape.
- Do not commit secrets into the single-file artifact.