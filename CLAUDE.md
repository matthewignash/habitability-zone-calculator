# Habitability Zone Calculator — Project Charter

> **Purpose of this document:** Durable charter for the Habitability Zone Calculator (Unit 1 — Earth and Universe). Hand-off brief for Claude Code. **Do NOT modify this file once built — it is the design contract.** Living build status goes in `BUILD_PROGRESS.md`.

---

## 1. Purpose and curricular placement

**Course:** HS Earth & Environmental Science at AISC Chennai (American International School Chennai). AY 2026-27.

**Unit:** Unit 1 — Earth and Universe (Habitability and Systems). 10 × 80-min blocks.

**Why this simulator exists:** Unit 1's authentic deliverable is the Goldilocks Report — students apply a multi-criteria habitability framework to Earth, Mars, and a chosen exoplanet, and defend their classification before a "NASA Evaluation Committee" Q&A. Currently students apply the framework on paper using static data cards. The calculator lets them explore parameter space directly: what would happen to Earth at 1.2 AU? What if Mars had Earth's mass? Does TRAPPIST-1e's tidal locking matter? They develop intuition for which framework criteria are sensitive to which physical parameters — which is the difference between memorizing the framework and understanding it.

**Pedagogical thesis:** Habitability is multi-criteria. No single number determines it. A planet can be in the habitable zone and uninhabitable (Venus). A planet can be outside the conventional HZ and potentially habitable (subsurface oceans on Europa, Enceladus). Equilibrium temperature is computable; surface conditions are emergent from a set of interacting factors; biology is uncertain. A calculator that produces a binary "habitable / not habitable" verdict would actively miseducate. This calculator must surface the framework, the inputs, the calculations, and the uncertainty — and let the student judge.

**Recommended use in the unit:**
- **Block 4 (Habitable zone, AU, light-year):** Brief intro to the calculator. Students play with Earth defaults, perturb one parameter at a time, observe.
- **Block 5 (Exoplanet detection):** Students load each of the 8 exoplanet presets, see how transit-derived parameters translate into framework criteria.
- **Block 6 (Reading exoplanet data cards / choosing your planet):** Students use the calculator to help choose their exoplanet for the Goldilocks Report.
- **Block 7 (Habitability framework — building the comparison):** Students use the calculator's comparison mode (Earth + Mars + their exoplanet side by side) as the spine of their framework analysis.
- **Block 8 (Goldilocks Report drafting):** Students export their comparison as evidence in the report.

### 1.6 Relationship to the Planet Hunter AI partner

The course also has a Planet Hunter AI partner (a Socratic reasoning tutor for exoplanet habitability, available to students from Block 6 onward at `/ai-partners/planet-hunter/` on the course site). The calculator and the Planet Hunter do different jobs and pair together: **the calculator computes and visualizes** (parameters → outputs → framework verdict per criterion); **the Planet Hunter reasons** (it pushes the student to define what habitability means in their framework, refuses to give a verdict, and stays Socratic). A well-equipped student uses both — calculator to surface what the data says, Planet Hunter to interrogate what to make of it.

This combination has rubric consequences. Per the Goldilocks Report rubric, the K/U strand has a gate: if the student used AI at any stage and the AI Documentation Template is missing, K/U caps at 3–4 Developing. **Using the calculator counts as AI-assisted work** (it was built with AI assistance per §7.8 below, and it embeds simplified models that may be wrong in specific ways). The calculator's Compare-mode export (§4) must therefore include a clearly-labeled "for your AI Documentation Template" section so students who used the calculator can satisfy the K/U gate without doing extra documentation work.

---

## 2. NGSS standards alignment

### Primary standards addressed

| Code | Standard | How the calculator hits it |
|------|----------|----------------------------|
| **HS-ESS1-4** | Use mathematical or computational representations to predict the motion of orbiting objects in the solar system. | The calculator uses computational representations of orbital parameters to derive surface conditions. |
| **HS-ESS1-6** | Apply scientific reasoning and evidence from ancient Earth materials, meteorites, and other planetary surfaces to construct an account of Earth's formation and early history. | Comparing Earth's habitability conditions to Mars and exoplanets is exactly this kind of reasoning. |
| **HS-ESS2-4** | Use a model to describe how variations in the flow of energy into and out of Earth's systems result in changes in climate. | Surface temperature emerges from incoming stellar radiation × albedo × atmospheric greenhouse — directly modeled. |
| **HS-ETS1-4** | Use a computer simulation to model the impact of proposed solutions to a complex real-world problem with numerous criteria and constraints. | The calculator IS the computational tool called for in this standard. |

### Three-Dimensional Learning components

- **Science & Engineering Practices (SEPs):** Developing and Using Models; Using Mathematics and Computational Thinking; Analyzing and Interpreting Data; Constructing Explanations
- **Disciplinary Core Ideas (DCIs):** ESS1.A (The Universe and Its Stars); ESS1.B (Earth and the Solar System); ESS2.A (Earth Materials and Systems)
- **Crosscutting Concepts (CCCs):** Systems and System Models; Energy and Matter; Scale, Proportion, and Quantity; Cause and Effect

---

## 3. Domain knowledge — the physics the calculator must model

> **Use the formulas and constants below as v1 defaults. Where simplification is required, document it in "Show assumptions." Where the science is genuinely uncertain (atmospheric composition of most exoplanets), the UI must say so.**

### 3.1 Equilibrium temperature (no atmosphere)

T_eq = T_star × √(R_star / (2 × d)) × (1 - A)^0.25

Where:
- T_star = effective temperature of the host star (K)
- R_star = radius of the host star (m)
- d = orbital distance of planet from star (m)
- A = bond albedo of the planet (dimensionless, 0-1)

This gives the equilibrium temperature with NO atmosphere. Earth's T_eq ≈ 255 K (-18°C) — well below freezing.

### 3.2 Surface temperature (with greenhouse adjustment)

T_surface ≈ T_eq + ΔT_greenhouse

Where ΔT_greenhouse depends on atmospheric composition. Simplified bands for v1:
- No atmosphere: ΔT = 0
- Thin CO₂ (Mars-like, ~6 mbar): ΔT ≈ +5
- Earth-like atmosphere (~1 bar, N₂/O₂ + trace GHGs + water vapor): ΔT ≈ +33
- Thick CO₂ (Venus-like, ~92 bar): ΔT ≈ +500
- H₂/He thick (gas giant): ΔT ≈ +200 (highly variable)

Earth: T_eq 255 K + 33 ΔT = 288 K (15°C). Correct.

### 3.3 Atmospheric retention (Jeans escape, simplified)

A planet retains a gas if escape velocity is high enough relative to gas molecular speed at exospheric temperature.

For v1, use this lookup heuristic:

| Surface gravity (relative to Earth) | Retains H₂? | Retains H₂O? | Retains N₂/O₂? | Retains CO₂? |
|-------------------------------------|-------------|--------------|----------------|--------------|
| <0.2 (small moon-like) | No | No | No | Marginal |
| 0.2-0.4 (Mars-like) | No | No | Marginal | Yes |
| 0.4-0.7 (Mars/super-Earth boundary) | No | Marginal | Yes | Yes |
| 0.7-1.5 (Earth-like) | Marginal | Yes | Yes | Yes |
| >1.5 (super-Earth, mini-Neptune) | Yes | Yes | Yes | Yes |

Adjust downward by one band if T_surface > 500 K (hot planet loses atmosphere faster).

Surface gravity g_planet = G × M_planet / R_planet²; the ratio to Earth's gravity is (M/M_⊕) / (R/R_⊕)².

### 3.4 Water phase at surface

At Earth atmospheric pressure (~1 bar), water is liquid from 273 K to 373 K. At higher pressure, the liquid range expands; at lower pressure, it contracts. At ~6 mbar (Mars surface), liquid water is not stable — it sublimates directly to vapor.

For v1, use this simplified phase logic:

| T_surface | Pressure | Phase |
|-----------|----------|-------|
| <273 K | any | Frozen at surface (subsurface liquid possible if internal heat) |
| 273-373 K | ≥0.1 bar | Liquid |
| 273-373 K | <0.1 bar | Sublimation (no stable surface liquid) |
| >373 K | <50 bar | Gaseous (water vapor) |
| >373 K | ≥50 bar | Supercritical water (Venus-like) |

### 3.5 Magnetic field plausibility (rough heuristic)

A magnetic field requires a molten, convecting interior. v1 uses this rough estimator:

| Planet mass | System age | Likely magnetic field? |
|-------------|-----------|------------------------|
| <0.3 M_⊕ | any | Unlikely (cools too fast, like Mars) |
| 0.3-0.5 M_⊕ | <2 Gyr | Marginal |
| 0.3-0.5 M_⊕ | >2 Gyr | Unlikely (Mars-like loss) |
| 0.5-2 M_⊕ | <8 Gyr | Likely |
| 0.5-2 M_⊕ | >8 Gyr | Marginal |
| >2 M_⊕ | <10 Gyr | Likely (more internal heat) |

This is a crude proxy. Real magnetic field generation depends on rotation, composition, and interior dynamics that the calculator does not model. Label clearly as "estimated."

### 3.6 Stellar habitable zone

The conventional HZ for a star is bounded by:
- Inner edge: where runaway greenhouse begins (Venus-zone)
- Outer edge: where maximum greenhouse effect cannot maintain liquid water (Mars-zone)

For a sun-like (G2V) star, HZ is approximately 0.95-1.67 AU.

For other star types, scale by stellar luminosity: HZ_inner = 0.95 × √(L_star / L_sun), HZ_outer = 1.67 × √(L_star / L_sun).

M-dwarfs have HZ very close to the star (0.02-0.2 AU for late M-dwarfs). This is why TRAPPIST-1's HZ contains 3+ planets in tiny orbits.

### 3.7 The exoplanet presets (8 candidates + worked-example reference)

Load preset parameters from the U1 Assessment Support exoplanet data cards (`Unit 1_ Earth and Universe/Assessment Support/U1 Material — Exoplanet Data Cards.docx`). The 8 student-choice candidates are: TRAPPIST-1 e, Proxima Centauri b, K2-18 b, Kepler-186 f, Kepler-452 b, LHS 1140 b, TOI 700 d, Gliese 581 d. Each card provides:
- Distance from Earth (ly)
- Planet mass (M_⊕)
- Planet radius (R_⊕)
- Orbital distance (AU)
- Host star type + temperature
- Known/estimated atmosphere (often "unknown")
- Other notes (tidal locking, system age, etc.)

In addition, **Kepler-22 b is included as a 12th preset, labeled "worked-example reference (not a student-choice candidate)"**. Block 8 of Unit 1 quotes the Kepler-22 b worked example verbatim (the "Honest reading" verdict). Having Kepler-22 b loadable lets students reproduce the exemplar's analysis in the calculator and compare it to their own work. The full preset list is therefore 12: Earth, Mars, Venus (sanity-check baselines), 8 student-choice exoplanets, and Kepler-22 b (worked-example reference). See §10 for the full parameter table.

If a parameter is unknown for an actual exoplanet, the calculator must label that field as "estimated" or "unknown" in the UI — not silently substitute a default.

---

## 4. Core simulator mechanic

The student plays with a **single-planet workbench** with three modes:

### Mode 1: Explore (default)
Sliders for: planet mass, planet radius, orbital distance, star type, atmospheric composition, system age. As they adjust, the calculator shows in real time:
- Equilibrium temperature (computed from §3.1)
- Surface temperature (computed from §3.2)
- Atmospheric retention status (computed from §3.3)
- Water phase at surface (computed from §3.4)
- Magnetic field plausibility (computed from §3.5)
- Stellar HZ for this star + whether the planet is in it (computed from §3.6)
- **Framework readout**: each of the 5-7 habitability framework criteria as MEETS / PARTIAL / FAILS / UNCERTAIN with the reasoning visible

### Mode 2: Load preset
Pull any of the 8 exoplanet presets (or Earth / Mars / Venus as additional presets). All sliders snap to that planet's parameters. Student then perturbs from there — "what if Kepler-22b had Mars's atmosphere?"

### Mode 3: Compare
Side-by-side view of Earth + Mars + the chosen exoplanet (or any 3 planets the student picks). Same framework readout for each, in three columns. This is the mode that directly supports the Goldilocks Report.

The student should be able to **export the comparison view** as a structured JSON / printable HTML to paste into the Goldilocks Report as evidence. The export must include the input parameters and the "Show assumptions" content for transparency.

The export **must also include a clearly-labeled "For your AI Documentation Template" section** with three subsections: (a) the input parameters the student chose and which presets they loaded; (b) the full "Show assumptions" content (formulas, lookup tables, constants — so the student can OPVL the model); and (c) a verbatim "what the calculator may have got wrong" note listing the specific simplifications — the simplified greenhouse bands in §3.2, the Jeans-escape lookup heuristic in §3.3 instead of full integration, the magnetic-field rough proxy in §3.5, and any UNCERTAIN entries that the calculator surfaced. Students paste this section into the AI Documentation Template per the K/U gate (see §1.6).

---

## 5. UI / UX requirements

### Layout

```
┌────────────────────────────────────────────────────────────┐
│  Header: title + "this is a model" banner + Mode tabs      │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  LEFT PANEL (inputs)         │  RIGHT PANEL (outputs)      │
│                              │                             │
│  Planet                      │  Equilibrium T:   255 K     │
│  ├ Mass     [====| ]  1.0 M⊕ │  Surface T:       288 K     │
│  ├ Radius   [====| ]  1.0 R⊕ │  Water phase:     Liquid    │
│  ├ Albedo   [==|   ]  0.30   │  Atm retention:   N₂/O₂ ✓   │
│                              │  Magnetic field:  Likely    │
│  Orbit                       │  In HZ:           Yes       │
│  ├ Distance [====| ]  1.0 AU │                             │
│                              │  ─── Framework readout ──   │
│  Star                        │  Liquid water:   MEETS  ✓   │
│  ├ Type     [G2V (Sun)  ▼]   │  Atmosphere:     MEETS  ✓   │
│                              │  Temperature:    MEETS  ✓   │
│  Atmosphere                  │  Magnetic field: MEETS  ✓   │
│  ├ Comp     [Earth-like ▼]   │  Energy source:  MEETS  ✓   │
│                              │  Stable orbit:   MEETS  ✓   │
│  System                      │                             │
│  ├ Age      [====| ]  4.5 Gyr│  Hover any row to see       │
│                              │  the calculation             │
│                              │                             │
│  [Load preset ▼] [Reset]     │  [Compare mode]             │
│  [Show assumptions]          │  [Export]                   │
└────────────────────────────────────────────────────────────┘
```

### Critical UI rules

- **Every output must be traceable.** Hover any computed-output row (T_eq, T_surface, water phase, etc.) to see the calculation in plain English: "T_eq = 5778 K × √(696,000,000 / (2 × 1.50 × 10^11)) × (1 - 0.30)^0.25 ≈ 255 K." Hovering any **framework-readout row** exposes all five fields of the Habitability Framework Template (from the U1 Assessment Support template students fill in Block 7): **(1) name** of the criterion, **(2) why it matters** for life, **(3) how to detect** (which calculator output produced this verdict), **(4) Earth's value** as the baseline reference, and **(5) where this criterion can fail** (the threshold the current planet has crossed, if any). This turns the calculator into a worked example of the Framework Template — students see exactly the structure they're being asked to produce.
- **"Show assumptions" panel** opens a side drawer listing every formula and lookup table the calculator uses, with the exact constants. This is non-negotiable.
- **Framework readout** uses 4-state coloring: MEETS (green) / PARTIAL (yellow) / FAILS (red) / UNCERTAIN (grey, with reason). UNCERTAIN must appear whenever a real exoplanet's atmospheric composition is unknown. Do not silently default to Earth-like.
- **Compare mode** displays three planets in parallel columns with the same framework readout. Differences highlighted (e.g., Earth's "Atmosphere: MEETS" next to Mars's "Atmosphere: FAILS — too thin").
- **"This is a model" banner** is always present. Includes a one-line link to "Show assumptions."

### Visual style

- Match the Tectonic City Builder + Cauvery Simulator palette (shared CSS variables) so the family of simulators feels coherent.
- Use distinct colors for the four framework states.
- Star type selector should show example star alongside its name (G2V → "the Sun"; M5V → "TRAPPIST-1"; K2V → "Tau Ceti"). This grounds abstract terms.

---

## 6. Tech stack

- **Vanilla HTML + CSS + JS.** No framework. No build step.
- **Single file preferred** (`habitability-zone-calculator.html`) for v1. If complexity demands, split: `index.html` + `physics.js` + `presets.js` + `framework.js` + `styles.css`.
- **No external dependencies.** All visuals as inline SVG (consider a small visualizer of the planet in orbit if time allows — but optional for v1).
- **`localStorage`** for save/load.
- **JSON export** of compare-mode results so students can paste into the Goldilocks Report.
- **iframe-friendly.** No fixed dimensions; responsive.

---

## 7. Pedagogical guardrails — what the calculator MUST NOT do

These are non-negotiable. Push back if any future build session tries to relax them.

1. **Never give a single "habitability score."** Habitability is multi-criteria. There is no overall %.
2. **Never display "HABITABLE / NOT HABITABLE" as a binary.** The framework readout has 4 states (MEETS / PARTIAL / FAILS / UNCERTAIN) per criterion, never a planet-level verdict.
3. **Never silently substitute default values for unknown exoplanet data.** If atmospheric composition is unknown for an actual exoplanet, the calculator must show UNCERTAIN and explain why.
4. **Never imply Earth's biology is the only template for life.** The "Atmosphere" criterion should note that anaerobic life exists; the "Energy source" criterion should note that chemosynthesis exists (Earth's ocean-floor vents, hypothetical Europa). The framework readout must distinguish "habitable for complex Earth-like life" from "potentially habitable for any biology."
5. **Never hide the model.** "Show assumptions" must always be one click away. Every output traceable.
6. **Surface tidal locking as a factor.** Many M-dwarf HZ planets are tidally locked (Proxima b, TRAPPIST-1e/f/g). The framework should treat tidal locking as PARTIAL on temperature stability (terminator zone may be habitable) and PARTIAL on long-term climate, NOT as automatic FAIL.
7. **Surface stellar activity as a factor.** M-dwarfs are often flaring stars — high UV/X-ray output can strip atmospheres. If host star is an active M-dwarf and the planet lacks a strong magnetic field, mark the "Atmosphere" criterion as PARTIAL even when retention math says it should hold.
8. **Disclose AI involvement.** Front-page banner: "This is a model, built with AI assistance. Apply OPVL to it as you would to any source. See 'Show assumptions' for the rules."

---

## 8. Acceptance criteria for v1

A student can:
- [ ] Adjust planet mass, radius, orbital distance, albedo, star type, atmospheric composition, and system age via the input panel.
- [ ] See equilibrium temperature, surface temperature, atmospheric retention, water phase, magnetic field plausibility, and HZ position update in real time.
- [ ] See a framework readout with each of the 5-7 criteria marked MEETS / PARTIAL / FAILS / UNCERTAIN.
- [ ] Hover any output to see the calculation that produced it.
- [ ] Hover any **framework-readout row** and see all five Framework Template fields exposed: name, why it matters, how to detect, Earth's value, where this criterion can fail.
- [ ] Open "Show assumptions" and see every formula and lookup table.
- [ ] Load any of the 8 student-choice exoplanet presets + Earth, Mars, Venus + Kepler-22 b (worked-example reference).
- [ ] Switch to Compare mode and view 3 planets in parallel columns.
- [ ] Export the compare-mode results as JSON or printable HTML to paste into the Goldilocks Report.
- [ ] Save and reload a custom planet from localStorage.

A teacher can:
- [ ] Pre-load a class via URL parameter (e.g., `?preset=kepler-22b&compare=earth,mars`).
- [ ] Understand from the UI what each calculation does.
- [ ] Trust that the calculator does not produce a "habitable / not habitable" verdict.

---

## 9. Out of scope for v1

- Animated orbital simulation (a static SVG of the system is fine; full Kepler orbits are v2+).
- Real-time data feeds (all data is in `presets.js`).
- 3D rendering of the planet.
- Tidal heating calculations (mention it in the assumptions panel; don't compute it).
- Atmospheric escape time integration (use the lookup table from §3.3; full Jeans escape integration is v2+).
- Biosignature gas analysis (way beyond v1 scope).
- Integration with Google Classroom.
- Anything requiring authentication or a server.

---

## 10. Data / parameter table

### Physical constants
- G = 6.674 × 10⁻¹¹ m³/(kg·s²)
- M_⊕ = 5.972 × 10²⁴ kg
- R_⊕ = 6.371 × 10⁶ m
- M_sun = 1.989 × 10³⁰ kg
- R_sun = 6.957 × 10⁸ m
- T_sun (effective) = 5778 K
- L_sun = 3.828 × 10²⁶ W
- 1 AU = 1.496 × 10¹¹ m

### Star type defaults (v1 — limit to these 5 types in Explore mode)

| Type | Example | T_eff (K) | R_star (R_sun) | L_star (L_sun) | HZ inner (AU) | HZ outer (AU) | Notes |
|------|---------|-----------|----------------|----------------|---------------|---------------|-------|
| F8V | Tau Ceti analog | 6200 | 1.10 | 1.50 | 1.16 | 2.04 | Brighter than Sun |
| G2V | The Sun | 5778 | 1.00 | 1.00 | 0.95 | 1.67 | Baseline |
| K2V | Epsilon Eridani | 5000 | 0.85 | 0.30 | 0.52 | 0.91 | Cooler, longer-lived |
| M5V | TRAPPIST-1 | 2800 | 0.12 | 0.00056 | 0.022 | 0.040 | Very cool, often flares |
| M3V | Proxima Centauri | 3050 | 0.15 | 0.0017 | 0.039 | 0.069 | Cool, active flaring |

**Presets override star-type defaults.** When a preset is loaded, the host star's specific published parameters (T_eff, R_star, L_star, computed HZ) override the generic star-type defaults above. The generic types are for Explore-mode free-play where the student is constructing a hypothetical planet around a hypothetical star. In Preset and Compare modes the host star is the actual specific star — two M3V planets in the candidate list (Proxima b and K2-18 b) have host stars whose luminosities differ by roughly 15× (0.0015 L☉ vs. 0.0233 L☉), so a generic "M3V default" would mislead. The per-preset values below come from the site's Block 4 HZ table (`src/units/unit-1/block-4.njk`) and the assessment Step 1 candidates table — these are the authoritative published values.

### Preset host-star + planet parameters (12 presets)

| Preset | Host star | T_eff (K) | L_star (L☉) | Inner HZ (AU) | Outer HZ (AU) | Planet orbital distance (AU) | Notes |
|--------|-----------|-----------|-------------|---------------|---------------|------------------------------|-------|
| Earth | Sun (G2V) | 5778 | 1.00 | 0.95 | 1.67 | 1.00 | Sanity-check baseline |
| Mars | Sun (G2V) | 5778 | 1.00 | 0.95 | 1.67 | 1.524 | Sanity-check baseline; just outside conventional HZ |
| Venus | Sun (G2V) | 5778 | 1.00 | 0.95 | 1.67 | 0.723 | Sanity-check baseline; in HZ but uninhabitable |
| TRAPPIST-1 e | TRAPPIST-1 (M8V) | 2566 | 0.000553 | 0.022 | 0.032 | 0.029 | Tidally locked; M-dwarf flaring |
| Proxima Centauri b | Proxima Cen (M5.5V) | 3050 | 0.0015 | 0.037 | 0.053 | 0.0485 | Closest known exoplanet; M-dwarf flaring |
| K2-18 b | K2-18 (M3V) | 3457 | 0.0233 | 0.146 | 0.210 | 0.143 | Sub-Neptune; possible hycean |
| Kepler-186 f | Kepler-186 (M1V) | 3755 | 0.0412 | 0.194 | 0.279 | 0.432 | First Earth-sized in HZ; 580 ly (no atmospheric data) |
| Kepler-452 b | Kepler-452 (G2V) | 5757 | 1.19 | 1.04 | 1.50 | 1.046 | "Earth's older cousin"; ~1.6 R⊕ (composition uncertain) |
| LHS 1140 b | LHS 1140 (M4.5V) | 3216 | 0.00427 | 0.062 | 0.090 | 0.0936 | Rocky, quiet M-dwarf; JWST target |
| TOI 700 d | TOI 700 (M2V) | 3480 | 0.0233 | 0.146 | 0.210 | 0.163 | Earth-sized in HZ; TESS discovery |
| Gliese 581 d | Gliese 581 (M3V) | 3498 | 0.0135 | 0.111 | 0.160 | 0.218 | **Contested existence** — possible stellar-activity artifact |
| Kepler-22 b | Kepler-22 (G5V) | 5518 | 0.79 | 0.85 | 1.22 | 0.85 | **Worked-example reference** (not a student-choice candidate); atmospheric composition unknown |

**Authoritative source:** if any value above diverges from the current published value in the U1 Assessment Support data cards, update this table — the data cards are the source of truth, this table mirrors them.

### Atmospheric composition presets

| Preset | ΔT greenhouse | Pressure (bar) | Notes |
|--------|---------------|----------------|-------|
| None / vacuum | 0 | 0 | Moon-like |
| Thin CO₂ | +5 | 0.006 | Mars-like |
| Earth-like (N₂/O₂ + GHGs) | +33 | 1.0 | Standard |
| Thick CO₂ | +500 | 92 | Venus-like |
| H₂/He thick | +200 | 1000 | Gas giant; not relevant for HZ rocky |
| Unknown | display "UNCERTAIN" in framework | — | For exoplanets where atmosphere is not measured |

### Default Earth parameters (sanity check baseline)
- Mass: 1.0 M_⊕
- Radius: 1.0 R_⊕
- Albedo: 0.30
- Orbital distance: 1.0 AU
- Star: G2V
- Atmosphere: Earth-like
- System age: 4.5 Gyr
- Expected outputs: T_eq ≈ 255 K, T_surface ≈ 288 K (15°C), water Liquid, atmosphere retained, magnetic field Likely, in HZ. Framework: 6/6 MEETS.

### Default Mars parameters (sanity check baseline)
- Mass: 0.107 M_⊕
- Radius: 0.532 R_⊕
- Albedo: 0.25
- Orbital distance: 1.524 AU
- Star: G2V
- Atmosphere: Thin CO₂
- System age: 4.5 Gyr
- Expected outputs: T_eq ≈ 210 K, T_surface ≈ 215 K (-58°C), water Frozen, atmosphere Marginal (CO₂ retained, others lost), magnetic field Unlikely, just outside conventional HZ. Framework: ~2/6 MEETS, ~2 PARTIAL, ~2 FAILS.

### Default Venus parameters (sanity check baseline)
- Mass: 0.815 M_⊕
- Radius: 0.949 R_⊕
- Albedo: 0.75 (high cloud reflectivity)
- Orbital distance: 0.723 AU
- Star: G2V
- Atmosphere: Thick CO₂
- System age: 4.5 Gyr
- Expected outputs: T_eq ≈ 230 K (low because of high albedo), T_surface ≈ 730 K (runaway greenhouse), water Gaseous/Supercritical, atmosphere retained, magnetic field Unlikely (no internal dynamo), in HZ but uninhabitable. Framework: ~1/6 MEETS — useful for teaching that HZ ≠ habitable.

---

## 11. Acceptance test scenarios (run before declaring v1 done)

1. **Earth baseline test.** Defaults → expected outputs above. Framework: 6/6 MEETS.
2. **Mars baseline test.** Defaults → expected outputs above. Framework shows the mix described.
3. **Venus baseline test.** Defaults → high surface temp, water gas/supercritical. Framework shows mostly FAILS even though planet is technically in HZ. **This is the key teaching scenario** — students see that HZ ≠ habitable.
4. **Earth-at-Venus-distance test.** Earth defaults, change orbital distance to 0.72 AU. Surface temp should rise dramatically (runaway greenhouse risk). Framework should shift several criteria from MEETS to FAILS.
5. **Earth-without-atmosphere test.** Earth defaults, change atmosphere to "None / vacuum." Surface temp drops to T_eq (~255 K). Water freezes. Atmospheric retention N/A. Framework changes significantly.
6. **Kepler-22 b preset test (worked-example reference).** Load the Kepler-22 b preset (clearly labeled in the UI as "worked-example reference — not a student-choice candidate"). Atmospheric composition is unknown — framework should show UNCERTAIN for atmosphere-dependent criteria, NOT silently default to Earth-like. The verdict the calculator surfaces should roughly match the Block 8 worked-example "Honest reading" — passes stellar + orbital criteria, fails or UNCERTAIN on composition. If the calculator's verdict diverges, surface that as a teaching opportunity (the worked example is one analysis; the calculator's is another; both rely on the same data).
7. **TRAPPIST-1e preset test.** Load preset. Should show: in HZ of M5V star; surface gravity ~Earth-like (radius 0.92, mass 0.69); tidally locked flag set; framework PARTIAL on temperature stability and atmosphere (M-dwarf flaring noted).
8. **Compare mode test.** Compare Earth + Mars + Kepler-22b. Three parallel columns. Differences visually clear.
9. **Export test.** Export the compare view as JSON. Re-import it (or just view it) — all parameters, calculations, and assumptions present.

If any of these tests gives a binary "habitable / not habitable" verdict, or silently uses Earth-like atmosphere for an unknown exoplanet, or hides a calculation, the build has violated a §7 guardrail. Fix before declaring done.

---

## 12. References

- Kasting, Whitmire, Reynolds (1993), "Habitable Zones around Main Sequence Stars," Icarus 101 (foundational HZ paper)
- Kopparapu et al. (2013), "Habitable Zones around Main-Sequence Stars: New Estimates," ApJ 765 (updated HZ boundaries)
- NASA Exoplanet Archive (https://exoplanetarchive.ipac.caltech.edu/) for verified exoplanet parameters
- Seager (2013), "Exoplanet Habitability," Science 340 (review of the multi-criteria framing)
- Lammer et al. (2009), "What makes a planet habitable?" Astron Astrophys Rev 17 (the multi-factor argument)

Any v1 implementation choice not directly traceable to one of these (or to the U1 Assessment Support data cards) must be labeled `[simplified for v1, not primary-source-traceable]` in the assumptions panel.

---

## 13. Maintenance contract

If a future build session wants to add features:
- Read this file first.
- Check `BUILD_PROGRESS.md` for current state.
- Any new feature must preserve the pedagogical guardrails in §7. If a feature requires breaking one of those rules, escalate to Matthew — do not silently relax.
- The data tables in §10 may be updated as new exoplanet data becomes available, but the change must be documented in a `CHANGELOG.md` with source and rationale.
- The UNCERTAIN state must always be available and used. Removing it to "clean up" the UI is not a permitted optimization.
- The "Show assumptions" panel must always be one click away — do not hide it behind a settings menu.
