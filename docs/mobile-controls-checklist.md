# Mobile controls checklist

Lessons from production single-file mobile games (including SNC Can Run):

- [ ] **MOBILE setting** — OFF forces desktop/canvas menus on phone; ON/AUTO enables HTML overlay + portrait dashboard (`references/dual-menu-mobile-vs-canvas.md`).
- [ ] **MOVE release** — joystick `pointerup` / `pointercancel` clears `inp` movement flags.
- [ ] **LOOK release** — look pad clears turn state on cancel/up.
- [ ] **Thumb drift** — after release, player must not keep moving.
- [ ] **pointercancel** handled like pointerup on all held buttons.
- [ ] **touch-action: none** on game canvas and control dock.
- [ ] **Options/menu scroll** may use `touch-action: pan-y` on scroll panels only.
- [ ] **resetcontrols** URL or options row restores safe defaults when phone has stale saves.
- [ ] **Layout overlap guard** — minimap vs dock overlap metrics, not only “controls visible.”
- [ ] **Orientation change** — re-run `setMobileMode` + `applyMobileControlSettings` on resize.
- [ ] **Do not** replace per-element handlers with a document-level touch router that `preventDefault`s everything.

Real-phone feel still matters; harness proves structure — distinguish emulation vs device in reports.