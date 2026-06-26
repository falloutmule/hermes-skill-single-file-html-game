# Repo root organization (guard reports + proofs)

Use when organizing a **single-file game repo** **without changing gameplay**: move clutter out of GitHub root, update Markdown links, keep `index.html` and Playwright paths unchanged.

## Two-pass pattern

| Pass | Goal |
|------|------|
| **1 — Docs index** | `README.md`, `PROJECT_STATUS.md`, `CONTRIBUTING.md`, `docs/`, `reports/README.md` |
| **2 — Structural** | `git mv` tracked reports/proofs/backups into `reports/`; rewrite links |

## Target root (tracked)

- `index.html`, `README.md`, `PROJECT_STATUS.md`, `SOURCE_RELEASE_POLICY.md`, `CONTRIBUTING.md`
- Optional visible cleanup report at root (e.g. `REPO_ORGANIZATION_REPORT.md`)
- `docs/`, `reports/`, `tests/`, `src/`, `.gitignore`
- After CI card: `package.json`, `package-lock.json`, `.github/workflows/` (harness-only)

## Folder layout

```
reports/
  README.md          # hub (guard order table)
  guards/            # current guard *_REPORT.md
  reference/         # study-only material
  history/           # superseded notes
  proofs/current/    # archived proof-*.json/png
  backups/           # index.before-*.html
```

## Move rules

1. Inventory with `git ls-files` at repo root first.
2. **Tracked** — `git mv` only.
3. **Untracked** gitignored reports/proofs — shell `mv` for local cleanliness; `git add -f` only when publishing history.
4. **Exclude** root cleanup report from batch `*_REPORT.md` moves if it must stay at root.
5. **Do not** change `index.html` or Playwright paths in a docs-only pass.

## Playwright proof output

- Harness **writes** fresh `proof-*` to **repo root** by design.
- **`.gitignore`** ignores scratch proofs; archive under `reports/proofs/current/` when committing history.

## Verification (docs-only)

- `git diff index.html` and `tests/run_selfcheck_playwright.js` — **empty**
- Grep: no stale links to moved reports
- Harness optional when gameplay/tests untouched — state why in organization report

## Commit message

`Organize <project> repo root structure` (docs-only).