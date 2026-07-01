# Declarative custom mobile controls

**Goal:** Let players adjust MOVE / GIVE / LOOK / SPRINT positions on portrait mobile without breaking FPV / minimap / fixed MENU band unless they explicitly drag in edit mode.

## Architecture

1. **`INPUT_CONFIG`** — named controls; fixed chrome (e.g. MENU) **`editable: false`** in edit mode.
2. **Legacy layout first** — keep existing `portraitLayout()` (or equivalent); apply user overrides **after** clamps.
3. **Normalized rects** — `x,y,w,h` in 0–1 viewport space; sanitize min sizes; respect safe-area insets.
4. **Persistence** — separate storage key from gameplay `SAVE_VERSION` options blob.
5. **Edit mode** — pause gameplay input; show edit bar (DONE / CANCEL / RESET); optional SIZE − / + on bar for phone-friendly resize.
6. **OPTIONS** — rows for **EDIT CONTROLS** and **RESET CONTROLS**; extend **`?resetcontrols=1`** to clear custom layout overrides.

## Pitfall: OPTIONS panel re-opens every frame

If `drawMobileMenu()` calls `rmenuShow()` each frame while `state === OPTIONS`, a semi-transparent options panel can cover the control dock during edit. **Fix:** early-return in `drawMobileMenu()` when `_controlEditActive`; hide options chrome on enter edit.

## Self-check

- **`runDeclarativeControlsSelfCheck()`** (name as appropriate) inside full self-check when the feature ships.
- Checks: default layout parity (empty overrides), hit test, edit persist/reload, reset, corrupt JSON recovery, **`finally` clears overrides** so harness does not pollute later tests.
- **Pointer capture:** do not call `setPointerCapture` without a real `pointerdown` on that element in headless/Playwright.

## Playwright

- Section: default screenshot → self-check → persist override → moved/reset proofs → `proof-declarative-controls.json`.
- Include declarative pass in aggregate `corePass`.

## Handoff

- Bump **`BUILD_ID`** per card for cache bust (`?v=<BUILD_ID>`).
- Phone URL with `?mobile=on&v=...`; compact bullets for Telegram.

## Related

- `docs/mobile-controls-checklist.md`
- `references/dual-menu-mobile-vs-canvas.md` — feature only applies when `mobileMode` is on