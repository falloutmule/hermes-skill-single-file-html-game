# GitHub Pages release checklist

- [ ] **Root `index.html`** is the file GitHub Pages serves (or documented `docs/` path if using that mode).
- [ ] **BUILD_ID** bumped for cache bust.
- [ ] **Play URL** recorded with `?v=<short-commit-hash>`.
- [ ] **Self-check URL** if supported: `?selfcheck=1&v=...`.
- [ ] Verified on **raw main** or Pages — not localhost-only.
- [ ] No claim of “deployed” from `127.0.0.1` alone.
- [ ] `SOURCE_RELEASE_POLICY.md` matches what you ship.
- [ ] Report includes **GITHUB PAGES URL** section.