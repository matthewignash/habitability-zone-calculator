# Build Progress — `Habitability-Zone-Calculator`

> **Purpose:** Living status doc. Charter is in `CLAUDE.md` — do NOT modify after build starts. Update this file at the end of each work session.

**Last updated:** 2026-05-24 (v0.1 shipped — Phase 1 Physics core + UI skeleton; acceptance tests 1–5 pass)

---

## TL;DR — pick up here

**v0.1 is live in the repo.** Single-file `habitability-zone-calculator.html` with 7 input sliders/dropdowns, 7 computed outputs (all with click-to-expand traces showing the calculation in plain English), Earth/Mars/Venus baseline buttons, and a full "Show assumptions" drawer. "This is a model" banner + AI involvement disclosure present per §7.8.

**Acceptance tests 1–5 verified.** Earth (T_eq=255, T_surf=288, all 6/6 ≈ MEETS), Mars (T_eq=210, T_surf=215, frozen + magnetic Unlikely), Venus (T_eq=232, T_surf=732, supercritical water — the key teaching test), Earth-at-Venus-distance (T_eq rises to 300, T_surf to 333), Earth-no-atmosphere (T_surf collapses to T_eq=255, water freezes).

The next session should start **Phase 2 — Framework readout**:
1. Re-read `CLAUDE.md` §4 Mode 1, §5 (hover-to-trace 5-field mapping), §7 (guardrails 3, 6, 7), and §10 (12-preset table).
2. Add a "Framework readout" panel below the computed outputs. 6 default criteria (Liquid water, Atmosphere, Temperature, Magnetic field, Energy source, Stable orbit) per the §5 mockup.
3. Implement 4-state coloring (MEETS / PARTIAL / FAILS / UNCERTAIN). UNCERTAIN appears whenever atmospheric composition is "unknown" (this hooks into Phase 3 presets).
4. Implement the 5-field hover-to-trace per §5 amendment — each framework row exposes name / why it matters / how to detect / Earth's value / where this criterion can fail.
5. Implement tidal-locking and stellar-activity flags (§7.6, §7.7) — will be hooks for Phase 3 presets.
6. Verify acceptance test #3 (Venus baseline) now shows mostly FAILS even though planet is in/near HZ — the key teaching scenario.

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
- **`habitability-zone-calculator.html` (v0.1)** — single-file simulator, Phase 1 complete. Vanilla HTML/CSS/JS. ~700 lines. Includes:
  - 7 input sliders/dropdowns (mass, radius, albedo, distance, star type, atmosphere, age)
  - 7 computed outputs, each as a `<details>` row that expands to show the calculation in plain English with current numbers substituted (hover-to-trace per §5)
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

### Phase 2 — Framework readout
- 5-7 framework criteria displayed with 4-state coloring (MEETS / PARTIAL / FAILS / UNCERTAIN).
- Hover-to-trace on framework rows exposes all 5 Template fields per §5.
- Wire the framework criteria to the underlying computed outputs.
- Implement the UNCERTAIN state for unknown atmospheric composition (per §7 guardrail #3).
- Implement tidal-locking and stellar-activity flags (per §7 guardrails #6, #7).

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
