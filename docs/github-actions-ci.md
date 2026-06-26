# GitHub Actions CI (model)

Use for serious single-file game repos that already have `tests/run_selfcheck_playwright.js`.

## Workflow sketch

File: **`.github/workflows/selfcheck.yml`**

- **Triggers:** `push` / `pull_request` to `main`, `workflow_dispatch`
- **Runner:** `ubuntu-latest`, timeout ~15 minutes
- **Steps:**
  1. `actions/checkout@v5`
  2. `actions/setup-node@v5` with **Node 22**
  3. `npm ci`
  4. `npx playwright install --with-deps chromium`
  5. `npm run test:selfcheck` → `node tests/run_selfcheck_playwright.js`
  6. `actions/upload-artifact@v5` with `if: always()` — `proof-*.json`, `proof-*.png`

## package.json

```json
{
  "private": true,
  "scripts": {
    "test:selfcheck": "node tests/run_selfcheck_playwright.js"
  },
  "devDependencies": {
    "playwright": "^1.52.0"
  }
}
```

## Rules

- **Do not** change gameplay `index.html` for CI-only wiring unless the card says so.
- **Fail the job** when harness exit code ≠ 0.
- Upload proofs even on failure for debugging.
- Verify first run with `gh run list --workflow selfcheck.yml`.

Production reference: public game repo **solidarity-not-charity-can-run** shipped this pattern (guard report under its `reports/guards/`).