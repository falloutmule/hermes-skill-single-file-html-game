# Harness patterns

## `window.CR` namespace

Export harness and debug APIs on **`globalThis.CR = window.CR = { ... }`**. Avoid scattering test hooks on `window` ad hoc.

## Self-check structure

- **`runFullSelfCheck()`** returns `{ pass, checks, errors, buildId }`.
- Compose smaller checks: layout, input, levels, render, save/load.
- Use **`crWithTemporaryState`** so benchmark maps and harness saves do not leak.

## Playwright wrapper

- Serve repo root over **`http://127.0.0.1:<port>`**.
- Load `index.html?mobile=on` (and project-specific flags).
- **`page.evaluate(() => CR.runFullSelfCheck())`**.
- Network guard: fail on external URLs.
- Write **`proof-playwright-summary.json`** with aggregate `pass`.

## Proof JSON and screenshots

- One JSON per section; summary aggregates gates.
- PNGs for layout regressions when cards require them.

## State isolation

After `?selfcheck=1`, user URL must show **title/menu**, not benchmark TIME ~999 arena.

## Final artifact testing

**`resolveReleaseArtifactPath()`** — same file GitHub Pages serves.

## No manual substitute

When Playwright passes, do **not** ask the user “does it work?” for items the harness already proved. Optional real-phone confirmation is for **feel** and cache-bust verification only.