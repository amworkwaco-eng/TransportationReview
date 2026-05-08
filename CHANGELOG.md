# Changelog

## v10 — 2026-05-08

### Architecture
- Restructured tab flow: Start → Scope → Access → TSP → Roads → Impact → Nexus → Plan Review → Traffic Review → Lighting → Report
- Data enters once on Start tab and flows downstream — no redundant entry
- Frontage roads (name + TSP classification) captured on Start tab, shared by all downstream tabs

### Start Tab
- Use category dropdown with 39 ITE-mapped sub-categories
- Conditional fields: units, building SF, rooms based on use category
- Number of frontage roads with inline name + classification per road
- Existing structure size visible when structure is retained

### Scope Tab
- Trip generation auto-derived from use category × quantity (ITE 11th Ed.)
- ITE code auto-populated from use category mapping
- Existing ADT auto-derived from existing use type
- Article V applicability evaluated from nature of action (new construction / change in use / land division), not application type
- CDC 501-2.2/2.3 threshold evaluation with actual site data

### Access Tab (rebuilt)
- Step 1: Access Management Standards — auto-derived per-road table from CDC 501-8.5.B(1)-(4)
- Step 2: Existing Driveways — simplified with progressive disclosure (conforming/nonconforming checkboxes)
- Step 3: Proposed Driveways — action + location + design standard (1010-1090)
- Step 4: Legally Existing Non-Conforming Accesses — CDC 440-10 ratio + required documents checklist

### TSP Tab (new)
- Step 1: Functional Classification & Design Standards (auto table)
- Step 2: Plan Overlays per road (pedestrian, bicycle, freight, special designation)
- Step 3: Trails (corridor, name, notes)
- Step 4: Transit (stop, distance, routes, frequency, TOD)
- Step 5: Planned Projects (auto-matched from TDT 471 records + CIP 81 records)

### Roads Tab (rebuilt)
- Road Header: identity + context fields (jurisdiction, cross-streets, speed, PCI, URMD, rail)
- Section 1 — Existing Conditions: ROW, paved width, lanes, frontage, curb, sidewalk, bike, lighting, drainage, source
- Section 2 — Proposed by Applicant: same fields, from submitted plans
- Section 3 — Ultimate Design: TSP classification + RDCS designation with code hierarchy
- Gap Analysis: auto-computed table (Existing vs Proposed vs Required) with status per component
- TSP overlay adjustments shown with per-side (half-width) values per Bicycle Facility Design Toolkit
- RDCS labels simplified: designation + lane count (no Major/Minor/Principal)

### Impact Tab
- Intersection distance dropdown with "At site (abuts frontage)" option

### Report
- Steps for Approval includes access requirements (AMP, design exception, access restriction)
- Facility Permit step lists specific improvements identified per road
- Pre-app report: new "Facility Permit Requirements" section listing specific improvements

### Bug Fixes
- Conditional fields (bldgSF, rooms) now visible for commercial/industrial uses
- Nav button highlighting matches current tab order
- Number inputs: scroll wheel no longer changes values (onwheel blur)
- Plan Review PDF button enables immediately after entering contact info
- Road count reduction properly removes empty roads, confirms before removing roads with data
- Duplicate defCheck ID removed
- Dead CASE_FIELDS entries cleaned up (artVTrigger, artVBasis, trImpactCount)
