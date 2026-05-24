# Build Progress — `Habitability-Zone-Calculator`

> **Purpose:** Living status doc. Charter is in `CLAUDE.md` — do NOT modify after build starts. Update this file at the end of each work session.

**Last updated:** 2026-05-24 (v0.2 shipped — Phase 2 Framework readout · UNCERTAIN handling · tidal-locking + stellar-activity flags)

---

## TL;DR — pick up here

**v0.2 is live.** The Habitability Framework panel renders below the computed outputs with all 6 criteria (Liquid water, Atmosphere, Temperature, Magnetic field, Energy source, Stable orbit). Each row shows a MEETS / PARTIAL / FAILS / UNCERTAIN badge and click-expands to reveal the 5-field reasoning (Why it matters / How to detect / Earth's value / Where it fails) — mirroring the Habitability Framework Template students fill in Block 7. UNCERTAIN propagation works via a new "Unknown / not measured" Atmosphere option. Tidal-locking and stellar-activity flags surface in callout notes below the framework and modify the relevant criterion verdicts per §7.6 / §7.7.

**Acceptance tests verified (manual + logic spot-check):**
- Test 1 (Earth): 6/6 MEETS ✓
- Test 2 (Mars): Liquid water FAILS, Atmosphere PARTIAL, Temperature PARTIAL, Magnetic FAILS, Energy MEETS, Orbit MEETS — the ~2/2/2 split charter §10 expects ✓
- Test 3 (Venus — key teaching test): Liquid water FAILS, Atmosphere PARTIAL, Temperature FAILS — 3 habitability-critical criteria all degraded even though planet is near HZ ✓
- Tests 4 + 5 (Earth@Venus-distance, Earth-no-atmosphere): Framework shifts predictably
- New UNCERTAIN test (atmosphere=Unknown): Liquid water + Atmosphere + Temperature all show UNCERTAIN; other 3 criteria still resolve from physics
- New tidal-locking test (M5V + 0.02 AU): callout appears, Temperature steps down
- New stellar-activity test (M5V + low mass forcing Unlikely magnetic): callout appears, Atmosphere steps down

The next session should start **Phase 3 — Exoplanet presets + Compare mode + Export**:
1. Build out the full 12-preset list from CLAUDE.md §10 (8 student-choice candidates + Earth/Mars/Venus + Kepler-22 b worked-example). Replace the 3 baseline buttons with a preset dropdown.
2. Add Compare mode — side-by-side Earth + Mars + chosen exoplanet (or any 3 planets) in parallel columns. Same framework readout for each.
3. Add Export — JSON or printable HTML including all inputs, computed outputs, the "Show assumptions" content, and the new "For your AI Documentation Template" subsection per the audit-phase §4 amendment.
4. Wire URL parameters for teacher-side preloading (`?preset=trappist1e&compare=earth,mars`).
5. Run acceptance tests 6, 7, 8, 9 from §11 — Kepler-22 b UNCERTAIN handling, TRAPPIST-1 e tidal-locking + PARTIAL atmosphere, Compare-mode parallel columns, Export round-trip.

---

## Charter revisions applied (2026-05-24)

Six surgical edits to `CLAUDE.md` after auditing against the as-built Unit 1 (`hs-earth-env-site/src/units/unit-1/`). The charter structure, physics, and §7 pedagogical guardrails are unchanged.

1. **§1.6 added** — new subsection on the Planet Hunter AI partner relationship (different jobs; both feed the AI Documentation Template per the K/U gate).
2. **§3.7 — docx filename corrected** ("Exoplanet Data Cards.docx", no "(8 cards)" suffix) + expanded to list all 8 student-choice candidates and the Kepler-22 b worked-example reference.
3. **§4 Mode 1 + §8 acceptance criteria** — criteria count changed from "6-8" to "5-7" to match the Goldilocks Report rubric ("Define 5–7 conditions").
4. **§4 Mode 3 export** — new requirement: export includes a "For your AI Documentation Template" section with inputs used, assumptions panel content, and a "what the calculator may have got wrong" note.
5. **§5 "Critical UI rules" bullet 1** — hover-to-trace on framework rows now exposes all 5 fields of the Habitability Framework Template (name, why it matters, how to detect, Earth's value, where it can fail). Turns the calculator into a worked example of the Template students fill in Block 7.
6. **§8 + §10 + §11 #6** — added 12-preset host-star parameter table (lifted from the site's Block 4 HZ table + Goldilocks Report Step 1 candidates table); added Kepler-22 b as the 12th preset labeled "worked-example reference"; clarified that presets override generic star-type defaults; reworked acceptance test #6 to use the new preset label.

---

## What's built

- `CLAUDE.md` — durable charter, audited and locked (~16KB after revisions).
- `README.md` — project overview.
- `BUILD_PROGRESS.md` — this file.
- `.gitignore` — `.DS_Store`, `node_modules/`, `*.log` (mirrors Plate Tectonics).
- **`habitability-zone-calculator.html` (v0.2)** — single-file simulator, Phases 1 + 2 complete. Vanilla HTML/CSS/JS. ~1370 lines. Includes:
  - 7 input sliders/dropdowns (mass, radius, albedo, distance, star type, atmosphere, age)
  - **Atmosphere dropdown** includes "Unknown / not measured" option to surface UNCERTAIN (Phase 2)
  - 7 computed outputs, each as a `<details>` row with click-to-expand traces showing the calculation in plain English with current numbers substituted (hover-to-trace per §5)
  - **Habitability framework panel** (Phase 2) — 6 criteria (Liquid water, Atmosphere, Temperature, Magnetic field, Energy source, Stable orbit) each with MEETS/PARTIAL/FAILS/UNCERTAIN badge and 5-field hover-to-trace (Why it matters / How to detect / Earth's value / Where it fails)
  - **Tidal-locking + stellar-activity flags** (Phase 2) — automatic callout notes when M-class host star + close orbit (tidal locking) or M-class + non-Likely magnetic field (stellar activity) is detected; criterion verdicts downgraded per §7.6 / §7.7
  - 3 baseline buttons: Load Earth / Load Mars / Load Venus (NOT the full 12-preset system — Phase 3)
  - "Show assumptions" drawer with all 6 formulas, 4 lookup tables, star defaults, and physical constants
  - "This is a model" banner with AI involvement disclosure
  - Plate Tectonics v1 palette mirrored exactly (--accent #c9542d etc.)
  - No `innerHTML` anywhere — all DOM updates via `textContent` + `createElement` for XSS safety
  - Responsive grid (stacks below 720px)
  - Repo: https://github.com/matthewignash/habitability-zone-calculator

---

## Recommended build sequence

### Phase 1 — Physics core + UI skeleton (no framework readout) ✅ COMPLETE
- ✅ Single-file `habitability-zone-calculator.html`.
- ✅ Sliders/dropdowns for the 7 input parameters (mass, radius, albedo, distance, star type, atmosphere, age).
- ✅ Computed outputs panel: T_eq, T_surface, water phase, atm retention, magnetic field, HZ position.
- ✅ Hover-to-trace on every computed output (per §5 critical UI rule).
- ✅ "Show assumptions" panel (partial — additional models and primary-source citations land by v1).
- ✅ Acceptance tests 1–5 verified (Earth, Mars, Venus baselines + Earth@Venus-distance + Earth-no-atm).

### Phase 2 — Framework readout ✅ COMPLETE
- ✅ 6 framework criteria displayed with 4-state coloring (MEETS / PARTIAL / FAILS / UNCERTAIN).
- ✅ Hover-to-trace on framework rows exposes all 5 Template fields per §5.
- ✅ Wired the framework criteria to the underlying computed outputs.
- ✅ Implemented the UNCERTAIN state for unknown atmospheric composition (per §7.3) via new "Unknown / not measured" Atmosphere dropdown option.
- ✅ Implemented tidal-locking and stellar-activity flags (per §7.6, §7.7) with callout notes that explain the criterion downgrade.

### Phase 3 — Presets + Compare mode + Export
- Load all 12 presets from §10 (8 student-choice exoplanets + Earth/Mars/Venus + Kepler-22 b worked-example).
- Compare mode: three planets side-by-side in parallel columns.
- Export: JSON or printable HTML, including all inputs + computed outputs + assumptions + the "For your AI Documentation Template" subsection per §4.
- URL parameters for teacher-side preloading.

### Phase 4 — Polish + tests + site integration
- Save/load via localStorage.
- Run all 9 acceptance test scenarios from §11. Fix any failures.
- Add npm sync script in `hs-earth-env-site/package.json` (mirror `sync-simulator-v2`).
- Embed test: works as iframe from Eleventy site.
- Build a launcher page at `hs-earth-env-site/src/units/unit-1/habitability-zone-calculator.njk` (or wire into one of Blocks 4 / 6 / 7 / 8).

---

## Resolved design questions

- **Framework criteria count:** **5–7** (matches the Goldilocks Report rubric's "Define 5–7 conditions" requirement). Likely v1 candidates per the layout mockup in §5: Liquid water, Atmosphere, Temperature, Magnetic field, Energy source, Stable orbit — a 6-criterion default set. The 5-field hover-to-trace mirrors the Habitability Framework Template students complete in Block 7.
- **Earth / Mars / Venus as "presets" or always-visible baselines?** Both. They're selectable presets in Explore mode (so students can sanity-check the math), AND they're the default Earth + Mars + chosen-exoplanet trio in Compare mode (matching the Goldilocks Report's three-case structure).
- **Albedo: slider or computed?** Slider, with a "typical for this atmosphere" reference value shown alongside (gives students more parameter space while signaling realistic values).

---

## Future (v2+) — NOT in v1

Per CLAUDE.md §9:
- Animated orbital simulation (full Kepler orbits)
- Real-time data feeds from NASA Exoplanet Archive
- 3D rendering of the planet
- Tidal heating calculation
- Atmospheric escape time integration
- Biosignature gas analysis
- Google Classroom integration

A possible v2 feature worth designing for: **side-by-side overlay of stellar HZ for different star types**, so students can see why M-dwarfs have HZ so close in. Not required for v1.
