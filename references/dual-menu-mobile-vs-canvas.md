# Dual menu paths: mobile HTML overlay vs canvas menus

When the user says **graphics came back**, **layout changed**, or **no fullscreen on title**, they often switched render paths — not necessarily a deploy regression.

## Two parallel title UIs

Games that support desktop canvas menus and mobile HTML overlays often draw **both** every frame:

| Layer | When visible | Typical labels |
|-------|----------------|------------------|
| **Canvas** | Title/meta states on canvas path | e.g. **START NEW RUN**, **CONTINUE SAVED RUN** |
| **HTML overlay** | Only when `mobileMode === true` | e.g. **NEW RUN**, **FULLSCREEN**, `build ${BUILD_ID}` footer |

If `mobileMode === false`, HTML menu code usually early-returns; canvas title art and canvas menu list stay visible. HTML never covers pixel art.

## Symptom → diagnosis

| User report | Likely cause |
|-------------|----------------|
| Canvas title art visible; menu says **START NEW RUN** | **`mobileMode` off** — canvas path |
| **No FULLSCREEN** on title | Title fullscreen often on **HTML** row only |
| **No** build id on title footer | Build footer often **HTML** menu only |
| Desktop HUD (**WASD** footer) on phone | **`mobileMode` false** — desktop layout on device |

Investigate **saved MOBILE setting** and URL overrides (`?mobile=on`) before blaming recent commits.

## What flips `mobileMode`

Typical pattern:

```javascript
mobileOverride = options.mobileControls === 'auto' ? null : options.mobileControls;
setMobileMode(isMobile());

function isMobile() {
  if (mobileOverride === 'on') return true;
  if (mobileOverride === 'off') return false;
  return matchMedia('(pointer: coarse)').matches && innerWidth < 1100;
}
```

- **MOBILE: OFF** → desktop layout even on Android.
- **MOBILE: ON** or **AUTO** (coarse + narrow) → HTML menus, portrait dashboard when portrait.
- Portrait mobile intentionally uses a **smaller** top playfield band — not a broken deploy.

## Harness / reports

- Log `mobileMode`, `mobileOverride`, `isMobilePortrait()` in `CR.getDebugState()`.
- Distinguish **emulation PASS** vs **real-phone pending** in handbacks.

## Related

- `docs/mobile-controls-checklist.md`
- `references/declarative-custom-mobile-controls.md` (user-editable controls when mobile on)