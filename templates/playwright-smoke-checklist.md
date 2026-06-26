# Playwright smoke checklist

- [ ] Local HTTP server serves repo root (not `file://` for main suite).
- [ ] Page loads `index.html` without navigation error.
- [ ] No console errors during harness run.
- [ ] No external HTTP requests (guard `proof-network.json`).
- [ ] `CR.runFullSelfCheck()` returns `pass: true`.
- [ ] `proof-playwright-summary.json` has `"pass": true`.
- [ ] Proof JSON files written to repo root (or documented path).
- [ ] Screenshots captured when card requires them.
- [ ] CI: artifact upload on success **and** failure (`if: always()`).
- [ ] Exit code non-zero when any section fails.