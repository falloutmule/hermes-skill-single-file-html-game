# SNC Can Run — lessons (public summary)

**Solidarity Not Charity Can Run** validated this Hermes workflow on a real shipped single-file roguelike. This document captures reusable lessons **without** private notes or repo clutter.

## Single-file shipped artifact

- GitHub Pages serves **root `index.html`** only at runtime.
- Optional `src/` scaffold exists for future extraction; harness tests the release file.

## AI-safe constitution + guard cards

- Fifteen-rule constitution at top of script.
- One **Kanban card** per change: backup → smallest diff → harness → report → commit.
- **BUILD_ID** cache-bust on every gameplay ship.

## Mobile controls and layout

- Portrait **stacked dashboard**: FPV → minimap → stats → controls dock.
- Harness gates for dock height, MENU lock, overlap, control reliability.
- **Layout frozen** after harness proof unless user requests change.
- Real-phone confirmation for **feel**; emulation is not “mobile complete.”

## Harness depth (grow over time)

- `window.CR.runFullSelfCheck()` composed from section checks.
- Playwright observer: local HTTP, network guard, proofs, screenshots.
- Specialized guards: Hall E2E, render failure, harness isolation, viewport, procedural seeds, full-run save/load.

## Root organization cleanup

- Docs-only pass: move `*_REPORT.md` and proof archives under **`reports/`**.
- Root README stays short; **`reports/README.md`** indexes guard history.

## GitHub Actions CI

- `npm run test:selfcheck` on Ubuntu with Playwright Chromium.
- Upload proof artifacts on every run.
- CI wiring does not require gameplay changes.

## What to copy for a new game

Start minimal (`examples/minimal-single-file-game.html`), add guards as the game grows — not every prototype needs the full SNC matrix on day one.