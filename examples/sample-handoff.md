# HERMES HANDOFF — Example: add score HUD color

## Goal

Change the minimal game HUD accent from gray to green when score &gt; 0. **Gameplay only** — one file `examples/minimal-single-file-game.html` or project `index.html`.

## Do not

- Do not add external CSS or JS.
- Do not refactor the game loop.
- Do not skip self-check.
- Do not ask Travis to confirm color on phone if harness passes.

## Backup

Copy `index.html` → `index.before-hud-color.html` (or `reports/backups/`).

## Implementation scope

- Bump `BUILD_ID` (e.g. `mini2`).
- In **RENDER** section only: conditional HUD color in canvas or DOM — smallest diff.

## Verification

1. Open over local HTTP or file (canvas games: prefer HTTP for save tests).
2. `CR.runFullSelfCheck()` → `pass: true`.
3. If Playwright exists: `npm run test:selfcheck` exit 0.

## Report

Use `templates/hermes-report-template.md` under `reports/guards/`.

## Final wording

**Success:** Minimal HUD color card passed. Self-check pass. No manual check required. Evidence in `reports/guards/HUD_COLOR_REPORT.md`.

**Failure:** State exact failing check, proof JSON path, and next fix — do not claim partial success.