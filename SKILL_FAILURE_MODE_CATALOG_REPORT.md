# Skill failure-mode catalog — report

**Task:** Add reusable failure-mode catalog to Hermes single-file HTML game skill repo.  
**SNC reference baseline (unchanged):** `controls1` / `4e311e7` — reference only.

## WHAT WAS DONE

- Added **`docs/failure-mode-catalog.md`** — purpose, severity model, catalog entries **A–T** (symptoms, cause, prevention, harness, do-not, SNC public examples), guard report language, how to use, SNC example table.
- Added **`templates/failure-mode-audit-template.md`** — per-card checklist for modes A–T + proof / unchanged / PASS-FAIL.
- Added **`docs/README.md`** — index linking catalog and other docs.
- Updated **`SKILL.md`** — new §8 Failure-mode catalog; §9 Report format; §10 Do-not; link in examples list.
- Updated **`README.md`** — repo structure, quick-start pointer, links to catalog + audit template.
- Updated **`references/snc-can-run-lessons.md`** — pointer to catalog (no SNC code copied).

## WHAT WAS VERIFIED

- **Required files exist:** catalog, audit template, `docs/README.md`.
- **Markdown links:** relative links from `README.md`, `SKILL.md`, `docs/failure-mode-catalog.md`, `docs/README.md` → catalog + template (repo-local).
- **Deliverable rule preserved:** `SKILL.md` still states final release artifact is **one HTML file** / root `index.html`.
- **No runtime dependencies added:** docs/templates only; `examples/minimal-single-file-game.html` untouched.
- **SNC gameplay:** `solidarity-not-charity-can-run` — no changes to `index.html` or gameplay commits this task (`HEAD` remains docs closure `386f3f0`; no new SNC commit from this handoff).
- **No wholesale private skill copy:** catalog written for public skill repo; distilled patterns + public SNC report links only.
- **Secret scan (this commit’s new/edited paths):** no API keys, `ghp_`, or `.env` files. New docs mention `TOKEN`/`SECRET` only as scan guidance. Pre-existing **`REPO_CREATION_REPORT.md`** still contains historical local paths from initial repo creation — not expanded by this card.

## WHAT FAILED

- Nothing blocking ship of skill docs.

## CURRENT EXACT STATE

| Item | Value |
|------|--------|
| **Skill repo** | `hermes-skill-single-file-html-game` |
| **Local path** | `C:\Users\fallo\Documents\HermesProjects\hermes-skill-single-file-html-game` |
| **New catalog** | `docs/failure-mode-catalog.md` |
| **New template** | `templates/failure-mode-audit-template.md` |
| **SNC touched** | No |

## REMAINING BLOCKERS

- None.

## NEXT ACTIONABLE STEP

- Optional: install or symlink this repo as a Hermes skill if desired (not verified in this task).

## EVIDENCE

| Evidence | Path / value |
|----------|----------------|
| Catalog | `docs/failure-mode-catalog.md` |
| Audit template | `templates/failure-mode-audit-template.md` |
| Docs index | `docs/README.md` |
| SKILL.md §8 link | `docs/failure-mode-catalog.md` |
| README.md links | catalog + `templates/failure-mode-audit-template.md` |
| Secret scan | `rg` on repo — no live secrets in new files; policy mentions only |
| Commit | *(filled after `git commit`)* |
| GitHub repo | https://github.com/falloutmule/hermes-skill-single-file-html-game |

## RESULT

**PASS** (pending push confirmation in commit hash below).