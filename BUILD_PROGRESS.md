# Build Progress — `Habitability-Zone-Calculator`

> **Purpose:** Living status doc. Charter is in `CLAUDE.md` — do NOT modify after build starts. Update this file at the end of each work session.

**Last updated:** 2026-05-24 (charter audited + revised against the as-built Unit 1; build still not started)

---

## TL;DR — pick up here

The charter has been audited against the now-shipped Unit 1 on the course site and revised in 6 places to fix drift. **The next session is unblocked to start Phase 1 — Physics core + UI skeleton.** Charter is locked again per §13.

The next session should:
1. Read `CLAUDE.md` end-to-end (especially §3 Physics, §7 Pedagogical guardrails, §10 Data table + 12-preset table, §11 Acceptance test scenarios).
2. Use the §10 12-preset table directly — every published parameter is in the table; the docx data cards are the source of truth if anything looks off.
3. Build v0.1 — UI skeleton + physics math, NO framework readout yet. Verify the Earth / Mars / Venus baseline tests (§11 #1-3) give the expected temperatures.
4. v0.2 — Add framework readout with 4-state coloring + 5-field hover-to-trace per §5. Verify the Venus test (§11 #3) correctly shows mostly FAILS even though planet is in HZ. This is the key teaching scenario.
5. v0.3 — Add Compare mode + Export (with the new "For your AI Documentation Template" subsection per §4).
6. Run all 9 acceptance test scenarios. Fix any failures.

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

Nothing functional yet. Folder + 3 docs:
- `CLAUDE.md` — durable charter, audited and locked (~16KB after revisions).
- `README.md` — project overview.
- `BUILD_PROGRESS.md` — this file.

---

## Recommended build sequence

### Phase 1 — Physics core + UI skeleton (no framework readout)
- Single-file `habitability-zone-calculator.html`.
- Sliders/dropdowns for the 7 input parameters (mass, radius, albedo, distance, star type, atmosphere, age).
- Computed outputs panel: T_eq, T_surface, water phase, atm retention, magnetic field, HZ position.
- Hover-to-trace on every computed output (per §5 critical UI rule).
- "Show assumptions" panel (can be partial in v0.1, complete by v1).
- Test against Earth / Mars / Venus baselines from §10.

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
