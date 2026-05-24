# Habitability Zone Calculator

Browser-based physics calculator for Unit 1 (Earth and Universe) of the HS Earth & Environmental Science course at AISC Chennai. Students adjust planet mass, radius, orbital distance, star type, atmospheric composition, and system age, and see in real time which criteria of the habitability framework are MEETS / PARTIAL / FAILS / UNCERTAIN. Includes 8 exoplanet presets (loaded from the U1 Assessment Support data cards) plus Earth, Mars, and Venus as sanity-check baselines. Direct support for the Goldilocks Report assessment.

**Pedagogical thesis:** Habitability is multi-criteria and partly uncertain. No single number determines it. The calculator surfaces the framework, the inputs, and the calculations — and lets the student judge.

## Status

v0 — charter written and audited against the as-built Unit 1; build not started. Charter is locked. Next session starts Phase 1 (Physics core + UI skeleton). See `BUILD_PROGRESS.md`.

## Design contract

See `CLAUDE.md` for the durable charter (do not modify once built).

## Tech stack

Vanilla HTML + CSS + JS, no build step. Designed to embed as an iframe from the Eleventy course site.

## Deploy

Once v1 is built, deploy to GitHub Pages or Vercel as a static page.

## Attribution

Built with Claude Code assistance, 2026. Course context: HS Earth & Environmental Science, AISC Chennai, AY 2026-27.
