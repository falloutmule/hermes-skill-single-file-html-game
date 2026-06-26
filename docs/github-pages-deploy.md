# GitHub Pages deploy

- **Default:** project site serves **root `index.html`**.
- **Cache-busting:** `?v=<git-short-hash>` on every verification link after push.
- **Self-check:** `?selfcheck=1` when game supports one-shot overlay check.
- **Pages vs localhost:** Local `127.0.0.1` proves harness; Pages CDN may lag minutes behind `main` — check raw GitHub `main` before declaring deploy failure.
- **Do not** claim production deploy from local server only.

See `templates/github-pages-release-checklist.md`.