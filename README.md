# WaCo TransReview

**Washington County, Oregon — Transportation Development Review Tool**

An offline, single-file web application for transportation planners reviewing land use applications against the Washington County Community Development Code (CDC), Transportation System Plan (TSP), and Road Design and Construction Standards (RDCS / Ord. 738).

## Quick Start

1. Download `index.html`
2. Open in any modern browser (Chrome, Edge, Firefox, Safari)
3. No server, no internet connection, no installation required

## What It Does

The tool walks a transportation planner through the full development review process:

| Tab | Purpose |
|---|---|
| **Start** | Site info, applicant, existing/proposed use, frontage roads |
| **Scope** | Trip generation (auto-derived from ITE 11th Ed.), Article V applicability |
| **Access** | CDC 501-8.5 access management standards, existing/proposed driveways, 440-10 compliance |
| **TSP** | Plan overlays (pedestrian, bicycle, freight, trails, transit), planned projects |
| **Roads** | Existing conditions → Proposed by applicant → Ultimate design (RDCS). Gap analysis auto-computes conditions |
| **Impact** | Intersection analysis, SPIS, signal warrants, turn lanes, ADA ramps |
| **Nexus** | Proportionality analysis (Nollan/Dolan/Koontz), cost estimates |
| **Plan Review** | RDCS checklist + PDF generation for engineering review teams |
| **Traffic Review** | MUTCD/ADA/Safety checklist + PDF generation |
| **Lighting** | Street lighting checklist + PDF generation |
| **Report** | Full report generation (pre-app comments, internal memo, CFCC findings) |

## Design Principles

1. **System Architecture** — Data enters once and flows downstream. Each tab narrows the review. No redundant entry.
2. **Human Behavior** — The UI matches how a planner thinks. No cognitive translation required.
3. **API (Deterministic Logic)** — Every gate is a testable predicate from the CDC. Example: `IF (nonconforming access) AND (trip increase ≥ 25%) THEN (compliance required)` — CDC 440-10.

## Embedded Data

The tool contains the following reference data (no external dependencies):

- **RDCS Cross-Sections** (Ord. 738) — 23 designations (A-1 through CI-6)
- **TSP Table 3** — 32 entries with overlay adjustments (streetscape, bikeway)
- **TDT Project List** — 471 projects
- **CIP FY 2025-30** — 81 capital projects
- **ITE Trip Generation** — 39 use category mappings (11th Edition rates)
- **CDC 501-8.5 Access Spacing** — standards per classification
- **Driveway Standards** — WCRDCS 1010-1090
- **Nexus Case Law** — Nollan, Dolan, Koontz framework

## Key Code References

| Code Section | What It Controls |
|---|---|
| CDC 501-2.1 | Article V applicability — land divisions |
| CDC 501-2.2 | Article V applicability — new structures (2,000 SF / 14 trip thresholds) |
| CDC 501-2.3 | Article V applicability — change in use |
| CDC 501-2.6 | Limited scope — SFR/duplex/middle housing |
| CDC 501-8.1 | Adequate access and critical services |
| CDC 501-8.2 | Frontage improvements (sidewalk, lighting, drainage) |
| CDC 501-8.4 | ROW dedication |
| CDC 501-8.5 | Access management standards |
| CDC 440-10 | Non-conforming access compliance trigger (25% ADT increase) |
| Ord. 738 | Road Design and Construction Standards |
| R&O 86-95 | Traffic impact analysis thresholds |

## File Management

- **Save**: File → Save JSON (downloads a `.json` case file)
- **Load**: File → Load JSON (restores all fields, roads, intersections)
- **Auto-save**: Saves to browser localStorage on every change
- **Print**: Report tab → Print (browser print dialog)

## Browser Support

Tested on Chrome 120+, Edge 120+, Firefox 120+, Safari 17+. Requires JavaScript enabled. Uses pdf-lib (embedded) for PDF generation.

## License

Internal tool — Washington County Department of Land Use & Transportation.

## Contact

Tony Mills, Associate Planner  
Washington County DLUT — Transportation Planning  
tony_mills@washingtoncountyor.gov  
503-846-3519
