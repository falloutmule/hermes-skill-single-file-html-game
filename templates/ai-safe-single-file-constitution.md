<!--
  Paste inside <script> at top of single-file game.
  Customize BUILD_ID, SAVE_VERSION, title, and SECTION markers for your project.
-->
/* ==========================================================================
   AI-SAFE SINGLE-FILE CONSTITUTION
   ==========================================================================
   1. Named SECTION edits only — do not rewrite unrelated blocks.
   2. SAVE_VERSION — bump and migrate when save format changes.
   3. INPUT → ACTIONS → SIMULATION → RENDER (one-way).
   4. Render code must not mutate gameplay state.
   5. Scene transitions own setup/teardown.
   6. CONFIG / DEBUG / URL query flags only for toggles.
   7. No runtime external JS, CSS, or asset URLs unless approved.
   8. No eval().
   9. No inline onclick/on* attributes — addEventListener only.
  10. Public harness API via window.CR only.
  11. One Kanban card at a time.
  12. Ship gate: runFullSelfCheck and Playwright when project has harness.
  13. Harness scenes temporary and state-isolated.
  14. Release artifact remains single-file HTML.
  15. Do not ask the user to find bugs the harness can test.
   ========================================================================== */
const BUILD_ID = 'dev1';
const SAVE_VERSION = 1;