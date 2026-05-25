# Build Progress — `Habitability-Zone-Calculator`

> **Purpose:** Living status doc. Charter is in `CLAUDE.md` — do NOT modify after build starts. Update this file at the end of each work session.

**Last updated:** 2026-05-25 (Phase 5 ✅ — calculator wired into hs-earth-env-site)

---

## TL;DR — pick up here

**The calculator is live on the course site.** Phase 5 (site integration) is complete:

- **Sync script** added at `hs-earth-env-site/package.json`: `npm run sync-habitability` copies the calculator HTML into `src/assets/simulators/habitability/index.html` at build time.
- **Launcher page** at `/units/unit-1/habitability-zone-calculator/` iframes the simulator and supports URL params passthrough (e.g. `?preset=kepler22b` propagates into the iframe).
- **Block cross-links** in place: Block 4 (Scale of Cosmos — replaced placeholder Interact card), Block 6 (Choose Your Exoplanet — alongside Planet Hunter in Choice C), Block 7 (Habitability Framework — `?view=compare` deep link in Choice C), Block 8 (Drafting Day — Compare-mode export + `?preset=kepler22b` worked-example link in Choice C).
- **Unit 1 landing** has a Habitability Zone Calculator card in the Resources panel.

The standalone calculator stays at v0.4. Future work that would benefit from a calculator-side commit: v2 features (e.g., radiative-transfer greenhouse, planetary rotation modeling for magnetic field), more presets, additional framework criteria. None are blocking; the calculator is feature-complete for v1 classroom use.

The next session can pivot back to the site (e.g., Unit 2 skeleton) or to another simulator brief (Cauvery / Land Allocation / Energy Mix — all at v0-charter-only state).

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
- **`habitability-zone-calculator.html` (v0.4)** — single-file simulator, Phases 1 + 2 + 3 + 4 (polish) complete. Vanilla HTML/CSS/JS. ~2280 lines. Includes:
  - 7 input sliders/dropdowns (mass, radius, albedo, distance, star type, atmosphere, age)
  - **Atmosphere dropdown** includes "Unknown / not measured" option to surface UNCERTAIN (Phase 2)
  - 7 computed outputs, each as a `<details>` row with click-to-expand traces showing the calculation in plain English with current numbers substituted (hover-to-trace per §5)
  - **Habitability framework panel** (Phase 2) — 6 criteria (Liquid water, Atmosphere, Temperature, Magnetic field, Energy source, Stable orbit) each with MEETS/PARTIAL/FAILS/UNCERTAIN badge and 5-field hover-to-trace (Why it matters / How to detect / Earth's value / Where it fails)
  - **Tidal-locking + stellar-activity flags** (Phase 2) — automatic callout notes when M-class host star + close orbit (tidal locking) or M-class + non-Likely magnetic field (stellar activity) is detected; criterion verdicts downgraded per §7.6 / §7.7
  - **12-preset system** (Phase 3) — preset dropdown with Earth / Mars / Venus + 8 student-choice exoplanets + Kepler-22 b worked-example. Specific host stars (TRAPPIST-1, Proxima Centauri, K2-18, Kepler-186, Kepler-452, LHS 1140, TOI 700, Gliese 581, Kepler-22) added to the STARS table with their published luminosities. Manual input change auto-clears the preset selection.
  - **Compare mode** (Phase 3) — tab toggle between Explore (single-planet workbench) and Compare (3-column side-by-side framework readout). Default loadout: Earth / Mars / TRAPPIST-1 e. Per-column preset dropdown. Diverging rows highlighted.
  - **Export** (Phase 3) — Copy as JSON / Download JSON buttons in Compare view. Output includes all 3 planets' inputs + computed outputs + framework verdicts + flags + assumptions + a dynamically-generated "For your AI Documentation Template" markdown string students paste into their Goldilocks Report.
  - **localStorage save/load** (Phase 4) — inputs + preset + view + Compare slots persist across refreshes. Storage key `hzc.v04.state` with version field for safe upgrades. Clear button in the assumptions drawer.
  - **URL parameters** (Phase 4) — `?preset=<key>`, `?compare=<k1>,<k2>,<k3>`, `?view=<explore|compare>` all supported. URL wins over localStorage. Example teacher URLs documented in the drawer.
  - **Printable HTML view** (Phase 4) — new "📄 Print view" button in Compare actions opens a paper-friendly self-contained doc in a new tab. Browser Print → Save as PDF produces a figure for Goldilocks Report submissions.
  - **Compare-column flag chips** (Phase 4) — tidal-locking + stellar-activity flags surface inside each Compare column with hover-tooltips, parallel to the Explore-mode flag callouts.
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

### Phase 3 — Presets + Compare mode + Export ✅ COMPLETE
- ✅ All 12 presets implemented (8 student-choice exoplanets + Earth/Mars/Venus + Kepler-22 b worked-example).
- ✅ Compare mode: three planets side-by-side in parallel columns with divergence highlighting.
- ✅ Export: JSON via Copy + Download, including all inputs + computed outputs + assumptions + the "For your AI Documentation Template" subsection per §4.

### Phase 4 — Polish ✅ COMPLETE
- ✅ localStorage save/load with versioned key (`hzc.v04.state`).
- ✅ URL parameters for teacher preloading (`?preset=`, `?compare=`, `?view=`).
- ✅ Printable HTML view in Compare mode (Print → Save as PDF supportable).
- ✅ Compare-column flag chips for tidal-locking + stellar-activity, parallel to the Explore-mode callouts.

### Phase 5 — Site integration ✅ COMPLETE
- ✅ `sync-habitability` npm script in `hs-earth-env-site/package.json` copies the calculator HTML into `src/assets/simulators/habitability/index.html`.
- ✅ Unit 1 launcher .njk page at `src/units/unit-1/habitability-zone-calculator.njk` with iframe + URL params passthrough.
- ✅ Links from Unit 1 Blocks 4 (Interact card swap) / 6 (Choice C inline) / 7 (Choice C `?view=compare` deep link) / 8 (Choice C export workflow + `?preset=kepler22b`).
- ✅ Unit 1 landing Resources panel has a Calculator card.
- Live launcher: https://hs-earth-env-site.vercel.app/units/unit-1/habitability-zone-calculator/ (after Vercel auto-redeploy)
- Site repo cross-link: https://github.com/matthewignash/hs-earth-env-site

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
