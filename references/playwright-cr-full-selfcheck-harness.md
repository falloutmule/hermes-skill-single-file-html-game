# Playwright + CR full self-check harness

Use when a game ships **root `index.html`** and needs **objective proof before push**: in-game `CR.run*SelfCheck()` plus Playwright over **local HTTP** (not `file://` for the main suite).

## Shipped vs harness files

| Shipped | Beside game (OK) |
|---------|------------------|
| `index.html` only | `tests/run_selfcheck_playwright.js`, `proof-*.json`, `proof-*.png`, guard reports |

## In-game API

Export on **`window.CR`**:

- **`runFullSelfCheck()`** — aggregate; return `{ pass, checks, errors, buildId }`.
- Section checks as the project matures: layout, input, levels, render, save/load, mobile reliability, etc.

**URL:** `?selfcheck=1` — one-shot check, then restore title/menu (no benchmark leak).

## Playwright runner (`tests/run_selfcheck_playwright.js`)

**Run:** `node tests/run_selfcheck_playwright.js` from repo root.

- Embedded **`http.createServer`** on `127.0.0.1` (e.g. port 4173).
- **Network guard:** fail requests outside localhost, `data:`, `blob:`.
- Hooks: `console`, `pageerror`, `request`.
- Core: `page.evaluate(() => CR.runFullSelfCheck())` → `proof-full-selfcheck.json`.
- Summary: **`proof-playwright-summary.json`** with aggregate `pass`.

**Release artifact:** `resolveReleaseArtifactPath()` — default root `index.html`; env `CR_RELEASE_ARTIFACT=dist` when build parity exists.

## CI compatibility

Same script via `npm run test:selfcheck` in GitHub Actions — see `docs/github-actions-ci.md`.

## Pitfalls

- Invalid JS inside `page.evaluate` strings breaks the whole section.
- Joy release needs **`pointerup`** on the joystick element so handlers run.
- Hermes `terminal` foreground **timeout max 600s** — use background + notify for long suites.
- `.gitignore` may hide proofs — `git add -f` when publishing evidence.

## Report format

WHAT WAS DONE / VERIFIED / FAILED / STATE / BLOCKERS / NEXT / EVIDENCE / URL — end with **PASS** or **FAIL**.

Full production matrix (many section checks) evolved on **Solidarity Not Charity Can Run**; new games add sections incrementally.