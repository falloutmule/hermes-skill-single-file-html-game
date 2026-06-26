# Repo organization

Keep the **GitHub root** readable:

**Good root:** `index.html`, `README.md`, `PROJECT_STATUS.md`, `SOURCE_RELEASE_POLICY.md`, `CONTRIBUTING.md`, `docs/`, `reports/`, `tests/`, optional `src/`, `.gitignore`, CI files.

**Move out of root:**

- Guard reports → `reports/guards/`
- History / tuning → `reports/history/`
- Reference studies → `reports/reference/`
- Archived proofs → `reports/proofs/current/`
- Backups → `reports/backups/` (`index.before-*.html`)

**README** is the front door — links to `reports/README.md`, not a dump of every `*_REPORT.md`.

**PROJECT_STATUS.md** tracks current BUILD_ID, URLs, completed cards, frozen rules.

Harness may still **write new** `proof-*` to repo root; `.gitignore` them locally; archive committed proofs under `reports/proofs/`.

See `references/repo-root-organization-guard-reports.md`.