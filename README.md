# Hermes skill — single-file HTML games

Reusable workflow for **AI-safe** browser games shipped as **one `index.html` file**: constitution, mobile controls, in-browser self-check, optional Playwright harness, GitHub Pages, proof artifacts, and guard reports.

**Solidarity Not Charity Can Run** proved this pipeline in production; this repo generalizes it for any serious single-file HTML game — not only SNC.

## What is a single-file HTML game?

One HTML document contains markup, styles, and game logic. Players open it from GitHub Pages (or save locally). **No npm at runtime**, no external JS/CSS URLs, no build step required for the shipped artifact (optional `src/` scaffold is fine if release stays one file).

## How Hermes should use this skill

1. Load **`SKILL.md`** (or install this folder as a Hermes skill).
2. Follow **one card / one task** at a time.
3. Backup before gameplay edits (`index.before-<topic>.html`).
4. Run self-check and Playwright when available — **do not ask the user to manually verify** what the harness can prove.
5. Write a report using **`templates/hermes-report-template.md`**.

**Failure modes:** Before a card, read **[docs/failure-mode-catalog.md](docs/failure-mode-catalog.md)** and copy **[templates/failure-mode-audit-template.md](templates/failure-mode-audit-template.md)** into your workflow.

Small throwaway prototypes can skip the full harness; **shipped** games should use boot checks, `window.CR.runFullSelfCheck()`, and CI when the project invests in quality.

## What this workflow prevents

- Delivering a plan instead of a playable file  
- Broken mobile controls after “desktop works”  
- Fake success without browser proof  
- Harness benchmark scenes leaking into real saves  
- Cluttered repo roots and lost guard history  
- Shipping with `eval`, inline handlers, or secret tokens in HTML  

## Quick start

```text
1. Copy templates/ai-safe-single-file-constitution.md into your game's <script> header.
2. Start from examples/minimal-single-file-game.html for structure.
3. Add window.CR.runFullSelfCheck() and expand checks as the game grows.
4. Add tests/run_selfcheck_playwright.js when ready (see references/).
5. Deploy root index.html to GitHub Pages with ?v=<commit> cache bust.
```

## Repo structure

| Path | Purpose |
|------|---------|
| **SKILL.md** | Full Hermes skill (rules, workflow, harness) |
| **examples/** | Minimal playable demo + sample handoff |
| **templates/** | Constitution, reports, checklists, **failure-mode audit** |
| **docs/** | Policy, mobile, harness, Pages, CI, organization, **failure-mode catalog** |
| **references/** | SNC lessons + Playwright/repo patterns |

## Links

- **Skill:** [SKILL.md](SKILL.md)  
- **Failure-mode catalog:** [docs/failure-mode-catalog.md](docs/failure-mode-catalog.md)  
- **Failure-mode audit template:** [templates/failure-mode-audit-template.md](templates/failure-mode-audit-template.md)  
- **Minimal example:** [examples/minimal-single-file-game.html](examples/minimal-single-file-game.html)  
- **Sample handoff:** [examples/sample-handoff.md](examples/sample-handoff.md)  
- **Dual menu (mobile vs canvas):** [references/dual-menu-mobile-vs-canvas.md](references/dual-menu-mobile-vs-canvas.md)  
- **Declarative custom controls:** [references/declarative-custom-mobile-controls.md](references/declarative-custom-mobile-controls.md)  

Synced from Hermes local skill **`self-contained-html-games`** (2026-07).

Inspired and validated by **Solidarity Not Charity Can Run** (single-file roguelike, Playwright guard suite, GitHub Actions CI). This skill is **reusable** for other titles.

## License

MIT — see [LICENSE](LICENSE).