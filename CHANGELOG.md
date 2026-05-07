# Changelog

This log captures substantive changes to the tool. Entries reflect what changed in the application logic and regulatory data, not every keystroke.

## v10 — May 2026

### TSP Table 3 integration

- Encoded the Functional Classification Design Parameters table from the adopted Washington County TSP (effective November 18, 2024). Includes the base entries for each classification plus the overlay variants (Streetscape, Enhanced Major Street Bikeway, both).
- Added `tspClassOf(rcls, desig)` and `tsp3For(cls, lanes, overlays)` helpers.
- ROW finding citation chain expanded to: TSP Strategy 5.1.2 + Table 3 → Ord 738 Exhibit X → CDC 501-8.1.B(9).
- Overlay treatment in standards table now cites the specific TSP map (Pedestrian System Map for Streetscape, Bicycle System Map for Enhanced Bikeway) and shows the explicit overlay adjustment.
- Roads tab augmented with TSP Table 3 ultimate-urban panel alongside Ord 738 cross-section. Green panel when they differ (with guidance to condition to TSP ultimate); blue panel when they match.

### Authority tab redesign

- Replaced legacy 1 KB cross-app matrix with an 8 KB per-road regulatory authority view.
- Each road shows: TSP functional classification, applicable overlays with map citations, cross-section comparison table (TSP / Ord 738 / Existing), and §501 RDCS items broken into Required vs Conditional with full citation chains.

### Defensibility & validation

- New defensibility alert at top of Section I (Conditions of Approval): lists any imposed condition whose nexus on Section F is incomplete, with a per-step breakdown (government interest / essential nexus / LOS impact).
- Narrative-vs-calc divergence flag at top of Section F: parses planner-entered percentages from nexus narratives and compares against intersection share / road traffic share computed from case data. Flags divergences > 2 percentage points.
- Critical-data validation panel at top of report: lists any roads missing posted speed, side of road, ROW, frontage, or measurement source attribution. Plus URMD status if unset.

### Section letter renumbering

- Sections now letter sequentially based on which sections actually emit. Previously PLA reports produced A, B, D, F, G, H (gaps where Applicable Standards, Traffic Safety, and Nexus were intentionally skipped). Now PLA produces A, B, C, D, E, F.
- Refactored to compute all section letters once at the top of `generateReport()` based on `_hasStandards`, `_hasTrafficSec`, `_hasNexus` flags.

### URMD annexation field

- Added Site tab field with four states: In URMD / Not in URMD — annexation required / Not in URMD — outside service area / Unknown — verify with County. Plus optional notes field.
- The maintenance provisions section now uses the captured value instead of a `[VERIFY: is/must be annexed]` placeholder. If unset, falls back to a missing-data warning.

### ITE Land Use Code

- Replaced free-text input with HTML `<datalist>` providing 40+ common ITE codes (Residential, Commercial, Industrial categories). Free entry still allowed.

### TDT calculation

- Now computes per-unit for residential land divisions (SUB, PAR, MHLD, MHD, MOD, PLA), per-trip for commercial and other applications (CUP, DR, etc.). Previously always per-trip, producing implausible figures for residential cases (e.g., $510K for a 6-lot subdivision instead of $51K).
- New `tdtBasisFor(app)` and `tdtComputed(app, rate, trips, units)` helpers; report now shows the basis label (e.g., "$8500/unit × 6 units") so the calculation is transparent.

### RDCS auto-populate

- Turn Lane / Signal Warrant (RDCS item #11) auto-populates "Applicable" only when actual MUTCD warrants are documented on the Impact tab (`hasTurnLaneWarrant() || hasSignalWarrants()`). Previously fired on bare trip count > 40.
- Safety Improvements criterion (`safetyImp.appliesIf`) similarly tightened: requires actual SPIS hazard, signal warrant, turn-lane warrant, or off-site improvement. R&O 86-95 ties safety analysis to documented hazards/warrants, not bare trip counts.

### Conditioning logic

- Half-street improvement condition gated on frontage status not equal to "Meets Design".
- Street lighting condition gated on lighting status not equal to "Yes" (or frontage being unimproved).
- Section E Minimum Safety Improvements list rebuilt to enumerate only applicable items (SPIS mitigation if hazards present, signal install if warrants met, etc.) instead of generic four-item boilerplate.

### Nexus inclusion

- Section F (Nexus and Proportionality Analysis) now appears for 9 of 12 application types (was 5). Added: MHD, MOD, TF, NCRD. Excluded by design: PLA, PLAN, ZC.

### Citation corrections

- Ordinance 738 Exhibit citations corrected via `exhibitFor(desig)` helper:
  - Arterials (A-*) → Exhibit 1
  - Collectors (C-*) → Exhibit 2 (was incorrectly 3)
  - Neighborhood Routes (NR-*) → Exhibit 3 (was incorrectly 4)
  - Local Roads (L-*) → Exhibit 4 (was incorrectly 5)
  - Commercial/Industrial (CI-*) → Exhibit 5 (newly handled)
  - Special Area (SA*) → Exhibit 6 (newly handled)

### Minor fixes

- `runDefensibilityCheck` no longer throws ReferenceError when run before any road has triggered conditions (icon constants hoisted to function scope).
- Proposal text capitalization preserved in report (was being lowercased).
- Phantom TIA fields removed from `CASE_FIELDS` (16 deprecated SPIS/Offsite/TurnLane/Warrants/W1-W8 fields, now driven by `S.intersections`).

## Earlier iterations

This file was a continuous iteration from the original VBA/Excel form (`WaCo_TransReview_App.xlsm`). The migration introduced the constitutional analysis framework (Nollan/Dolan/Sheetz/Koontz/Hill), per-road engineering integration with PDF round-trip for facility permits, the impact-area intersection grid (replacing flat SPIS/Offsite/TurnLane/Warrants fields), the Authority Map exhibit generator, and the seven-scenario walkthrough harness used for regression testing.
