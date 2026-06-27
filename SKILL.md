---
name: single-file-html-game
description: "Hermes skill for planning, building, testing, verifying, and shipping AI-safe single-file HTML games."
version: 1.0.0
platforms: [linux, macos, windows]
---

# Single-file HTML game (Hermes skill)

## 1. Purpose

Hermes skill for **planning, building, testing, verifying, and shipping** single-file HTML games. The release artifact is **one HTML file** (typically `index.html` at repo root for GitHub Pages). Use in-browser self-check (`window.CR`) and optional Playwright + GitHub Actions for objective proof before claiming success.

## 2. Core rules

- **Preserve requested deliverable type** — if the user asked for one HTML file, ship one HTML file.
- **Final release artifact is one HTML file** unless they explicitly approve another format.
- **No runtime external JS/CSS/assets** unless explicitly approved.
- **No `eval`.**
- **No inline event handlers** (`onclick=` on elements) — use `addEventListener`.
- **No hidden globals** — expose debug/harness API via **`window.CR`** only.
- **No secrets** in HTML, reports, or commits.
- **One task / Kanban card at a time** — no drive-by refactors.
- **Backup before game-code edits** — `index.before-<topic>.html`.
- **Verify before claiming success** — browser or harness proof, not narration.
- **Do not substitute plans for execution** — run tools, produce artifacts.
- **Do not ask the user for manual confirmation** when harness proof can verify layout, boot, self-check, or E2E.

## 3. AI-safe single-file constitution

Paste at top of `<script>` (see `templates/ai-safe-single-file-constitution.md`):

- **Named section edits** — change only labeled SECTION blocks.
- **SAVE_VERSION** bump and migration when save schema changes.
- **INPUT → ACTIONS → SIMULATION → RENDER** — one-way data flow.
- **Render must not mutate gameplay state.**
- **Scene lifecycle ownership** — setup/teardown on state transitions.
- **CONFIG / DEBUG / URL flags** — centralize toggles; no magic scattered flags.
- **No runtime external dependencies.**
- **Smoke tests required** — at minimum `CR.runFullSelfCheck()`.
- **Harness state isolation** — `crWithTemporaryState` pattern; no benchmark maps in real saves.
- **Release artifact proof** — Playwright tests **root `index.html`** (or `CR_RELEASE_ARTIFACT=dist` when build parity exists).

## 4. Recommended project layout

```
index.html              # shipped artifact (GitHub Pages)
SOURCE_RELEASE_POLICY.md
PROJECT_STATUS.md
CONTRIBUTING.md
src/                    # optional scaffold (not runtime until build proven)
tests/
  run_selfcheck_playwright.js
docs/
reports/
  README.md
  guards/               # guard card reports
  proofs/               # archived proof-*.json/png
  backups/              # index.before-*.html
package.json            # optional, for Playwright CI only (not game runtime)
.github/workflows/      # optional selfcheck.yml
```

## 5. Development workflow

1. **Inspect** current `BUILD_ID`, `PROJECT_STATUS.md`, last guard report.
2. **Confirm scope** — one card; layout frozen if harness says so.
3. **Create backup** — copy `index.html` → `reports/backups/` or `index.before-*.html`.
4. **Smallest change** that satisfies the card.
5. **Static checks** — constitution phrases, no eval, title, policy files.
6. **Browser / `?selfcheck=1`** — `CR.runFullSelfCheck()` returns `pass: true`.
7. **Playwright** when present — `npm run test:selfcheck` exit 0.
8. **Verify release artifact** — same file Pages serves.
9. **Write report** under `reports/guards/` using standard sections.
10. **Commit** with clear scope; bump `BUILD_ID` for cache bust.
11. **GitHub Pages URL** with `?v=<short-commit>`.

## 6. Mobile requirements

- **`touch-action`** — `none` on game surface; `pan-y` only where menus scroll.
- **Pointer capture** on drag controls where appropriate; **exception** for concurrent MOVE + LOOK (see references).
- **`pointercancel`** — clear input same as `pointerup`.
- **Joystick release** clears movement (`clearMoveInput` / zero analog axes).
- **Viewport / safe-area** — `visualViewport`, `100dvh`, safe bands on phones.
- **Layout usability** — measure **overlap** (minimap vs controls), not only visibility.
- **Controls/layout frozen** once harness-proven unless user requests change.

## 7. Harness requirements

| Check | Notes |
|-------|--------|
| Boot | Game reaches expected state without throw |
| Console / page errors | Zero uncaught errors in test run |
| External requests | Fail if fetch to third-party URLs (allow data/blob) |
| Input | Synthetic pointer/touch on controls |
| Resize / orientation | Layout stable across viewports |
| Save/load | Round-trip `localStorage` key if game saves |
| State isolation | After self-check, normal URL is not benchmark arena |
| Release artifact | HTTP server serves same path as Pages |
| Proof artifacts | `proof-*.json`, screenshots when useful |
| **GitHub Actions CI** | Recommended — see `docs/github-actions-ci.md` |

Not every toy needs the full matrix; serious ships should grow toward SNC-level guards incrementally.

## 8. Failure-mode catalog

- **[docs/failure-mode-catalog.md](docs/failure-mode-catalog.md)** — common breakages (deliverable drift, harness pollution, stuck input, save corruption, layout drift, proofless success, etc.) with severity, prevention, and harness proof.
- Before every **serious card**, identify which catalog modes (A–T) apply; use **[templates/failure-mode-audit-template.md](templates/failure-mode-audit-template.md)**.
- Guard reports should **explicitly state** which failure modes were guarded (and which were N/A).

## 9. Report format

```
WHAT WAS DONE
WHAT WAS VERIFIED
WHAT FAILED
CURRENT EXACT STATE
REMAINING BLOCKERS
NEXT ACTIONABLE STEP
EVIDENCE
GITHUB PAGES URL
```

Use **`templates/hermes-report-template.md`**. End with explicit **PASS** or **FAIL** — not “looks good.”

## 10. Do-not list

- No broad rewrites without request  
- No changing deliverable type (HTML → React, etc.)  
- No moving goalposts mid-card  
- No fake success without harness/browser evidence  
- No manual user check as substitute for automated proof when harness exists  
- No publishing secrets, tokens, or `.env`  
- No adding npm/runtime dependencies to the **shipped** HTML casually  
- No repo-wide cleanup unless requested  
- No private repo inventory or unrelated GitHub archaeology  

## Examples and references in this repo

- **examples/minimal-single-file-game.html** — tiny demo with `CR.runFullSelfCheck()`  
- **examples/sample-handoff.md** — Hermes handoff pattern  
- **references/snc-can-run-lessons.md** — what production proved  
- **references/playwright-cr-full-selfcheck-harness.md** — full external harness  
- **references/repo-root-organization-guard-reports.md** — clean GitHub root  
- **docs/failure-mode-catalog.md** — failure modes A–T + SNC public examples  

## When to load

User asks for browser game, HTML5 game, single-file game, GitHub Pages game, mobile canvas game, or AI-safe game workflow with self-check / Playwright.