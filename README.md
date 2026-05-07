# Washington County Transportation Development Review Tool

A single-file web application for Washington County (Oregon) Department of Land Use & Transportation transportation planners. Replaces the legacy VBA/Excel transportation review system with a modern, defensible workflow that produces staff-report findings, conditions of approval, and authority exhibits.

## What it does

For every transportation development review case (subdivisions, partitions, middle-housing, conditional uses, etc.), this tool walks the planner from intake through report generation, producing legally defensible findings under *Nollan*, *Dolan*, *Sheetz*, *Hill*, and the relevant Oregon authorities (CDC Article V, WCCO Chapter 15.08, R&O 86-95, Ord 738, the adopted TSP).

Workflow tabs:

- **Start** — case identification, applicant info, URMD status, ITE LU code (with dropdown of 40+ common codes)
- **Scope** — R&O 86-95 / CDC determination engine
- **Roads** — per-road existing conditions, RDCS designation, TSP Table 3 ultimate-urban dimensions, and overlay handling
- **Impact** — intersection-level analysis (SPIS, MUTCD warrants, turn lanes)
- **Nexus** — five-step constitutional analysis (interest, *Nollan* nexus, *Dolan* proportionality, comparison metrics, aggregate burden)
- **Plan Review / Traffic Review / Lighting** — facility-permit checklists with PDF generation for engineer review
- **Authority** — TSP Table 3 per-road regulatory view (functional class, overlays, cross-section comparison, §501 RDCS items)
- **Report** — generates the full staff report with section-letter renumbering, defensibility alerts, narrative-vs-calc divergence checks, and TDT calculation by app type

## Quick start

### Use locally

Just download `index.html` and open it in any modern browser. The tool is fully self-contained — no install, no build step, no server required. Case data persists in `localStorage` per browser.

### Publish on GitHub Pages

1. Create a new GitHub repo (public or private — Pages works on both with the right plan)
2. Copy this scaffold's contents into the repo:
   ```
   index.html
   README.md
   LICENSE
   .gitignore
   .gitattributes
   docs/
   .github/workflows/validate.yml
   ```
3. Commit and push:
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/YOUR-ORG/YOUR-REPO.git
   git push -u origin main
   ```
4. In the repo settings → Pages → set source to "Deploy from branch" → `main` → `/` (root)
5. Wait ~1 minute, then visit `https://YOUR-ORG.github.io/YOUR-REPO/`

The tool will be live at that URL. Anyone with the link can use it. Each user's data stays in their browser's `localStorage`.

### Iterate

Edit `index.html` directly, commit, push. GitHub Pages redeploys automatically. The included GitHub Action (`.github/workflows/validate.yml`) runs a JavaScript syntax check on every push so broken code doesn't get deployed.

## Architecture

A deliberate single-file design:

- **No build pipeline.** Open the `.html` in a browser; everything works.
- **No backend.** All logic runs in the browser. Case data lives in `localStorage`.
- **Two external CDN deps.** Google Fonts (Source Sans 3) for typography, pdf-lib (cdnjs) for facility-permit PDF generation. Both load over HTTPS and have no telemetry.
- **No analytics, no tracking.** Nothing leaves the browser.

The tool's regulatory data — RDCS cross-sections from Ord 738 Exhibits 1–6, TSP Table 3 design parameters, CDC §501 RDCS triage logic, ITE Land Use codes — is all encoded as JS objects at the top of the script.

## Updating regulatory data

When Washington County adopts amendments to:

- **Ordinance 738** (cross-sections): update the `XSEC` object near the top of the script
- **TSP** (Table 3 design parameters, overlays): update the `TSP_TABLE3` array
- **CDC Article V (§501)**: update `RDCS` and `getDefaults()` logic
- **TDT rates**: planner enters per-case; no code change needed unless the per-trip vs per-unit basis policy changes
- **R&O 86-95 successor**: update the safety-criterion `appliesIf` logic and citation strings

Each data block is commented with its source ordinance and last-verified date.

## Known limitations

- **URMD auto-derive.** Currently a captured field. Auto-deriving from address would require URMD boundary GeoJSON, which isn't yet integrated.
- **No collaboration model.** Each planner's `localStorage` is local. To share a case, use Save/Load JSON (saves a `.json` file the planner can email).
- **PDF library load.** The Plan Review / Traffic Review PDF generator uses `pdf-lib` from cdnjs. If the network is offline, those features won't work, but the rest of the tool still does.

## Files

| File | Purpose |
|---|---|
| `index.html` | The application — open in any browser to use |
| `README.md` | This file |
| `LICENSE` | License terms (MIT by default — adjust for your use) |
| `.gitignore` | Excludes editor/OS junk from version control |
| `.gitattributes` | Normalizes line endings across platforms |
| `docs/CHANGELOG.md` | Recent iteration notes |
| `docs/USAGE.md` | Planner workflow notes |
| `.github/workflows/validate.yml` | CI: runs JavaScript syntax check on every push |

## Authors / acknowledgments

Built collaboratively between Tony Mills (Washington County DLUT, Associate Planner) and Anthropic's Claude. The legal/regulatory framework — application of *Nollan*/*Dolan*/*Sheetz*/*Hill*/*Rosenzweig*, the CDC §501 triage logic, the integration of Ord 738 with the adopted TSP — is Tony's design. The implementation, refactoring, audit fixes, and integration of the TSP Table 3 against the Ord 738 exhibits is the collaboration product.

This tool does not replace professional engineering or legal review. It is a workflow and documentation aid for staff planners. All findings and conditions remain subject to engineering analysis, county counsel review, and public hearing process.

## License

See `LICENSE`. The default MIT license is permissive; consider whether your organization's use case calls for a different license (e.g., a public-domain dedication for government work, or an internal-use-only license).
