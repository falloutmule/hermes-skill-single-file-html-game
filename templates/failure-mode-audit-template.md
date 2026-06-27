# Failure-mode audit

Copy this block into a guard report or card notes. Check only modes relevant to the card; list unchecked modes as **N/A** with one-line reason if skipped.

---

**Goal:**  
**Card:**  
**Repo:**  
**Baseline (BUILD_ID / commit):**  
**Files touched:**

---

## Relevant failure modes

- [ ] **A.** Deliverable drift  
- [ ] **B.** External dependency leak  
- [ ] **C.** Inline handler / eval creep  
- [ ] **D.** Logic entropy  
- [ ] **E.** Render mutates gameplay  
- [ ] **F.** Save schema corruption  
- [ ] **G.** Harness pollution  
- [ ] **H.** Mobile viewport jump  
- [ ] **I.** HiDPI blur / stretch  
- [ ] **J.** Safe-area clipping  
- [ ] **K.** Browser steals gestures  
- [ ] **L.** Stuck input  
- [ ] **M.** Multi-touch conflict  
- [ ] **N.** Control layout drift  
- [ ] **O.** Custom controls corrupt defaults  
- [ ] **P.** Menu / input trap  
- [ ] **Q.** Proofless success  
- [ ] **R.** Repo-root clutter  
- [ ] **S.** Private data leak  
- [ ] **T.** AI overreach  

**Modes actively guarded this card (IDs):**  
**Modes explicitly N/A:**

---

## Proof required

| Item | Value |
|------|--------|
| **Commands** | e.g. `npm run test:selfcheck`, `node --check`, local HTTP smoke |
| **Artifacts** | e.g. `proof-*.json`, CI artifact name |
| **Screenshots** | e.g. `proof-control-edit-*.png` (if applicable) |
| **CI** | Run URL + conclusion (if pushed) |

---

## Unchanged by contract

List systems that must not change on this card (layout, SAVE_VERSION, Hall, map gen, etc.):

---

## Result

**PASS** / **FAIL**

**Notes:**