# Failure-Mode Catalog — Single-File HTML Games

Structured checklist of how AI-edited **one-file HTML** browser games break, and the prevention pattern for each. Use before, during, and after every serious **guard card** or gameplay change.

---

## 1. Purpose

Single-file games concentrate markup, styles, input, simulation, render, save, and mobile layout in one document. Small edits often break unrelated systems. This catalog lists **known failure modes**, how they **look** in the browser, what usually **causes** them, how to **prevent** them, what **harness proof** to require, explicit **do-not** rules, and **public SNC Can Run** examples where production already proved the guard.

**Deliverable unchanged:** the shipped artifact remains **one `index.html`** (typically repo root for GitHub Pages).

---

## 2. Severity model

| Label | Meaning |
|-------|---------|
| **BLOCKER** | Do not ship; fix before merge or Pages deploy |
| **HIGH** | Fix before starting the next card |
| **MEDIUM** | Acceptable only with documented reason and follow-up |
| **LOW** | Watch / polish; track in backlog |

---

## 3. Catalog entries

Each entry uses the same shape: failure mode → symptoms → cause → prevention → harness → do-not → SNC example (when available).

---

### A. Deliverable drift

- **Failure mode:** Deliverable drift  
- **Symptoms:** User asked for single-file HTML; agent ships React/Vite/multi-file app as the “game.”  
- **Common cause:** Defaulting to framework scaffolds; confusing optional `src/` scaffold with release artifact.  
- **Prevention pattern:** Shipped artifact is **one `index.html`**; `src/` is optional and non-runtime until build parity is proven.  
- **Harness proof:** Release artifact check — HTTP serves root `index.html`; constitution / `proof-release-artifact.json`.  
- **Do-not rule:** Do not change deliverable type unless the user explicitly asks.  
- **SNC example:** `SOURCE_RELEASE_POLICY.md` + Playwright `resolveReleaseArtifactPath()` → root `index.html`.  
- **Severity:** BLOCKER  

---

### B. Runtime external dependency leak

- **Failure mode:** Runtime external dependency leak  
- **Symptoms:** Works on dev machine; GitHub Pages or offline play fails; network tab shows CDN scripts/fonts/assets.  
- **Common cause:** `<script src="https://…">`, external CSS, runtime image URLs.  
- **Prevention pattern:** No runtime external JS/CSS/assets unless explicitly approved; procedural or inlined assets only.  
- **Harness proof:** Playwright network observer — fail on unexpected third-party requests (allow `data:`, `blob:`, local HTTP).  
- **Do-not rule:** Do not add CDN libraries to the shipped HTML casually.  
- **SNC example:** Full selfcheck `noExternalScript` / request guard in CI.  
- **Severity:** BLOCKER  

---

### C. Inline handler / eval creep

- **Failure mode:** Inline handler / eval creep  
- **Symptoms:** `onclick="…"`, `eval(`, dynamic `Function(`, inline handlers in generated menu HTML.  
- **Common cause:** Quick fixes; copy-paste from tutorials.  
- **Prevention pattern:** `addEventListener` + `data-action` dispatchers; constitution bans `eval`.  
- **Harness proof:** Static scan on release artifact (strip constitution comments if needed to avoid false positives on rule text).  
- **Do-not rule:** Do not use `eval` or inline handlers in shipped game code.  
- **SNC example:** Constitution static check in `run_selfcheck_playwright.js`.  
- **Severity:** HIGH  

---

### D. Logic entropy / unrelated breakage

- **Failure mode:** Logic entropy / unrelated breakage  
- **Symptoms:** Visual tweak breaks save/load, input, map gen, or timers.  
- **Common cause:** Unscoped edits; no section boundaries; multi-card drive-by refactors.  
- **Prevention pattern:** Named **SECTION** blocks; **one Kanban card**; smallest diff; backup before edit.  
- **Harness proof:** `CR.runFullSelfCheck()` after every card; diff summary in report.  
- **Do-not rule:** Do not “clean up” unrelated systems while fixing one bug.  
- **SNC example:** Fifteen-rule constitution + one-card handoffs through `controls1`.  
- **Severity:** HIGH  

---

### E. Render mutates gameplay

- **Failure mode:** Render mutates gameplay  
- **Symptoms:** HUD draw, fog pass, or debug overlay changes inventory, position, timer, or triggers save.  
- **Common cause:** Side effects inside `render()` / `drawHUD()` / sprite post-process.  
- **Prevention pattern:** **INPUT → ACTIONS → SIMULATION → RENDER** one-way; render reads state only.  
- **Harness proof:** State snapshot before/after render-only paths; save round-trip unchanged after visual-only card.  
- **Do-not rule:** Do not write gameplay state from draw functions.  
- **SNC example:** Render failure guard + full-run progression unchanged on visual cards.  
- **Severity:** HIGH  

---

### F. Save schema corruption

- **Failure mode:** Save schema corruption  
- **Symptoms:** Old saves throw, load impossible state, or soft-lock after upgrade.  
- **Common cause:** Changed `SAVE.serialize()` without **SAVE_VERSION** bump and migration.  
- **Prevention pattern:** Bump **SAVE_VERSION** and migrate only when **game save** schema changes.  
- **Harness proof:** Save/load round-trip; corrupted JSON recovery; mid-run reload tests.  
- **Do-not rule:** Do not reuse game save keys for unrelated settings without version discipline.  
- **Note:** Separate `localStorage` keys (e.g. `cannedRun.controls.v1`, options) for control overrides **do not** require game **SAVE_VERSION** bump if `cannedRun.save.v1` schema is unchanged.  
- **SNC example:** Declarative controls card — `saveFormatUnchanged: true` in `proof-declarative-controls.json`.  
- **Severity:** BLOCKER (for game save); MEDIUM (for isolated option keys with their own `v`)  

---

### G. Harness pollution

- **Failure mode:** Harness pollution  
- **Symptoms:** Normal play shows benchmark arena, seed 12345, TIME ~999, tiny test map after `?selfcheck=1` or dev run.  
- **Common cause:** Self-check leaves PLAY state; harness writes real `localStorage` save.  
- **Prevention pattern:** `crWithTemporaryState`; block saves when `harnessOnly`; restore on exit.  
- **Harness proof:** `CR.runHarnessIsolationSelfCheck()`; normal URL boot after full selfcheck.  
- **Do-not rule:** Do not call `SAVE.save()` from harness scenes without isolation.  
- **SNC example:** `HARNESS_STATE_ISOLATION_REPORT.md` + `harnessIsolation.pass` in CI summary.  
- **Severity:** BLOCKER  

---

### H. Mobile viewport jump

- **Failure mode:** Mobile viewport jump  
- **Symptoms:** Layout jumps when Chrome URL bar hides/shows; controls jump on rotate.  
- **Common cause:** `100vh` only; ignoring `visualViewport`; resize without re-applying mobile layout.  
- **Prevention pattern:** `100dvh` where shell needs height; `visualViewport` for sizing; debounced resize + `setMobileMode` / `applyMobileControlSettings` on orientation.  
- **Harness proof:** Viewport resize/orientation section; landscape vs portrait URL params.  
- **Do-not rule:** Do not assume `innerWidth` equals visible area on mobile.  
- **SNC example:** `VIEWPORT_SAFEAREA_HARDENING_REPORT.md`, `runViewportSafeAreaSelfCheck`.  
- **Severity:** HIGH  

---

### I. HiDPI blur / stretch

- **Failure mode:** HiDPI blur / stretch  
- **Symptoms:** Blurry canvas; wrong hit boxes; half-screen black on phone.  
- **Common cause:** CSS size ≠ backing store; wrong DPR policy for low-res upscale vs native-resolution games.  
- **Prevention pattern:** One authority for CSS size, backing buffer, DPR, and input scale; low-res buffer games often **skip** DPR multiply (see skill pitfalls).  
- **Harness proof:** Layout proofs at multiple viewports; pixel sharpness spot checks when feasible.  
- **Do-not rule:** Do not multiply backing store by DPR blindly on pixel-upscale raycasters.  
- **SNC example:** Portrait dashboard + `playfieldLayout()` reading `view.width/height`.  
- **Severity:** HIGH  

---

### J. Safe-area clipping

- **Failure mode:** Safe-area clipping  
- **Symptoms:** Controls under notch, gesture bar, or rounded corners; PAUSE/MAP clipped.  
- **Common cause:** Full-bleed rects without `env(safe-area-inset-*)`.  
- **Prevention pattern:** Safe bands on dock/stats; bottom buffer; OPTIONS scroll with `pan-y`.  
- **Harness proof:** Portrait safe-area self-check; control bounds inside safe band.  
- **Do-not rule:** Do not redesign layout wholesale — add padding where measured.  
- **SNC example:** `viewportSafeArea.pass`, `proof-options-back-reachable.png`.  
- **Severity:** HIGH  

---

### K. Browser steals gestures

- **Failure mode:** Browser steals gestures  
- **Symptoms:** Page scrolls/zooms instead of MOVE/LOOK; double-tap zoom on buttons.  
- **Common cause:** Missing `touch-action`; `passive: true` on handlers that need `preventDefault`.  
- **Prevention pattern:** `touch-action: none` on game + controls; `pan-y` only on scrollable menu panels; viewport `user-scalable=no` where appropriate.  
- **Harness proof:** Pointer/touch torture; no unexpected scroll during stick drag.  
- **Do-not rule:** Do not attach global `document` touch routers that `preventDefault` everything (breaks per-element controls).  
- **SNC example:** Mobile control reliability guard; anti-pattern doc on document-level router.  
- **Severity:** HIGH  

---

### L. Stuck input

- **Failure mode:** Stuck input  
- **Symptoms:** MOVE/LOOK/SPRINT/GIVE stays on after thumb up, menu open, or tab blur.  
- **Common cause:** Missing `pointercancel`; no `clearInputState` on pause/menu/edit; GIVE held every frame.  
- **Prevention pattern:** `pointerup` + `pointercancel`; clear on pause/resume/edit exit; edge-trigger discrete actions.  
- **Harness proof:** Synthetic `pointercancel`; release tests in `runMobileControlReliabilitySelfCheck`.  
- **Do-not rule:** Do not leave `inp.give` true without edge detection.  
- **SNC example:** `MOBILE_CONTROL_RELIABILITY_REPORT.md` (`controlrel1`).  
- **Severity:** HIGH  

---

### M. Multi-touch conflict

- **Failure mode:** Multi-touch conflict  
- **Symptoms:** MOVE cancels LOOK; one `changedTouches[0]` steals both; capture on look pad blocks joystick.  
- **Common cause:** Single-touch assumptions; `setPointerCapture` on concurrent pads.  
- **Prevention pattern:** Per-control pointer/touch IDs; document capture exceptions for simultaneous MOVE + LOOK.  
- **Harness proof:** Simultaneous MOVE + LOOK section; multitouch recipes in skill references.  
- **Do-not rule:** Do not clear all look state on any `touchend`.  
- **SNC example:** Concurrent controls reference patterns (public skill docs).  
- **Severity:** HIGH  

---

### N. Control layout drift

- **Failure mode:** Control layout drift  
- **Symptoms:** New UI card moves proven portrait controls, MENU, or minimap without user request.  
- **Common cause:** Ad-hoc `placeRect` changes; dock height applied to fixed bands.  
- **Prevention pattern:** Frozen default layout; harness overlap guards; declarative schema for overrides only.  
- **Harness proof:** Default layout parity; `getLayoutProof` / dock rect stability across dock heights.  
- **Do-not rule:** Do not change mobile layout unless the card explicitly says so.  
- **SNC example:** MENU lock below minimap; `layoutguard2`; declarative `defaultLayoutParity`.  
- **Severity:** HIGH  

---

### O. Custom controls corrupt defaults

- **Failure mode:** Custom controls corrupt defaults  
- **Symptoms:** Edit mode persists offscreen/broken controls; corrupt JSON bricks layout.  
- **Common cause:** No clamp; no reset; no schema version on override blob.  
- **Prevention pattern:** Normalized `x,y,w,h`; key version `v`; clamp to safe viewport; RESET + corrupt recovery.  
- **Harness proof:** Edit → move → reload → reset; `corruptRecovers` in declarative self-check.  
- **Do-not rule:** Do not store overrides in game save without separate key/version.  
- **SNC example:** `DECLARATIVE_CUSTOM_CONTROLS_REPORT.md` (`controls1` / `4e311e7`).  
- **Severity:** HIGH  

---

### P. Menu / input trap

- **Failure mode:** Menu / input trap  
- **Symptoms:** Stuck in OPTIONS/edit/help; gameplay pointers fire under modal; menu `innerHTML` rebuild swallows taps.  
- **Common cause:** No gating on PLAY pointers; per-frame menu rebuild between touchstart/touchend.  
- **Prevention pattern:** Scene/modal ownership; gate gameplay input in edit/options; store touch action on start, dispatch on end.  
- **Harness proof:** Open/close menu, resume, onboarding dismiss; declarative `noStuckAfterEdit`.  
- **Do-not rule:** Do not run PLAY input handlers while edit mode or blocking overlay is open.  
- **SNC example:** Onboarding + declarative edit mode gating (`onboard1`, `controls1`).  
- **Severity:** HIGH  

---

### Q. Proofless success

- **Failure mode:** Proofless success  
- **Symptoms:** Agent says “done” with no browser run, no JSON, no CI.  
- **Common cause:** Narration substituted for tools; skipped Playwright on “docs only” gameplay ships.  
- **Prevention pattern:** Report must list local harness, CI when present, artifact name, URLs; PASS/FAIL explicit.  
- **Harness proof:** `npm run test:selfcheck` exit 0; GitHub Actions artifact upload.  
- **Do-not rule:** Do not ask the user to manually verify what the harness can prove.  
- **SNC example:** GitHub Actions `snc-can-run-proof-artifacts` on every gameplay push.  
- **Severity:** BLOCKER  

---

### R. Repo-root clutter

- **Failure mode:** Repo-root clutter  
- **Symptoms:** `proof-*.json`, backups, and `*_REPORT.md` pile in repo root; GitHub home unreadable.  
- **Common cause:** `.gitignore` + no `reports/` convention.  
- **Prevention pattern:** `reports/guards/`, `reports/proofs/`, `reports/backups/`; index in `reports/README.md`.  
- **Harness proof:** Organization checklist; docs-only cleanup card.  
- **Do-not rule:** Do not leave guard history only on local disk.  
- **SNC example:** `SNC_REPO_ORGANIZATION_REPORT.md` + `reports/guards/*`.  
- **Severity:** MEDIUM  

---

### S. Private data leak

- **Failure mode:** Private data leak  
- **Symptoms:** Local paths, tokens, `.env`, full private repo inventory in public commit.  
- **Common cause:** Copy-paste from Hermes session; handoff dumps.  
- **Prevention pattern:** Secret scan before push; scope-limited docs; generic examples only in skill repo.  
- **Harness proof:** Grep/scan for `TOKEN`, `SECRET`, `API_KEY`, `.env`, user home paths.  
- **Do-not rule:** Do not copy private Hermes skill tree wholesale into public repos.  
- **SNC example:** Skill publish checklist in `references/standalone-hermes-skill-github-publish.md` (Hermes local skill, not in this repo).  
- **Severity:** BLOCKER  

---

### T. AI overreach

- **Failure mode:** AI overreach  
- **Symptoms:** Agent starts architecture rewrite, extra cards, or broad refactors unprompted.  
- **Common cause:** Helpful “improvements”; merging multiple handoffs.  
- **Prevention pattern:** One task/card; report **unchanged by contract** list; smallest change.  
- **Harness proof:** Git diff scope matches card; gameplay baseline commit cited.  
- **Do-not rule:** Do not broaden scope unless the user asks.  
- **SNC example:** Stable baseline `controls1` — docs-only skill work does not touch SNC `index.html`.  
- **Severity:** HIGH  

---

## 4. Required guard report language

Every serious card should end with (or link to) a guard report using:

```text
WHAT WAS DONE
WHAT WAS VERIFIED
WHAT FAILED
CURRENT EXACT STATE
REMAINING BLOCKERS
NEXT ACTIONABLE STEP
EVIDENCE
GITHUB PAGES URL   (when applicable)
```

Template: **`templates/hermes-report-template.md`**. State **PASS** or **FAIL**, not “looks good.”

---

## 5. How to use this catalog

| Phase | Action |
|-------|--------|
| **Before a card** | Pick relevant failure modes (use **`templates/failure-mode-audit-template.md`**). |
| **During a card** | Add or extend harness checks for those modes. |
| **Before completion** | Prove relevant modes did **not** occur (or document MEDIUM exceptions). |
| **After completion** | Record modes guarded in **`reports/guards/<CARD>_REPORT.md`**. |

---

## 6. SNC Can Run examples (public reference only)

Use **[Solidarity Not Charity Can Run](https://github.com/falloutmule/solidarity-not-charity-can-run)** as proof that the patterns work — **do not** copy large `index.html` into the skill repo.

| Topic | SNC guard report (public) |
|-------|---------------------------|
| Onboarding | `reports/guards/ONBOARDING_FIRST_RUN_HELP_REPORT.md` (`onboard1`) |
| Visual readability | `reports/guards/VISUAL_READABILITY_POLISH_REPORT.md` (`visual1`) |
| Rectangle regression | `reports/guards/VISUAL_RECTANGLE_REGRESSION_FIX_REPORT.md` (`visualfix1`) |
| Sound / feedback | `reports/guards/SOUND_FEEDBACK_PASS_REPORT.md` (`sound1`) |
| Declarative controls | `reports/guards/DECLARATIVE_CUSTOM_CONTROLS_REPORT.md` (`controls1`) |
| CI proof artifacts | `reports/guards/GITHUB_ACTIONS_CI_REPORT.md` + artifact `snc-can-run-proof-artifacts` |

Baseline reference only: **BUILD_ID `controls1`**, gameplay commit **`4e311e7`** (as of catalog addition).

---

## Related files in this skill repo

- **Audit checklist:** [../templates/failure-mode-audit-template.md](../templates/failure-mode-audit-template.md)  
- **Hermes skill rules:** [../SKILL.md](../SKILL.md)  
- **SNC lessons summary:** [../references/snc-can-run-lessons.md](../references/snc-can-run-lessons.md)