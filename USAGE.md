# Usage notes

Practical guidance for using the tool day-to-day.

## Workflow (typical case)

1. **Start tab** — enter casefile, application type, address, taxlot, acreage, applicant, owner. Set URMD annexation status. Indicate whether the case is internal (county jurisdiction) or external (city jurisdiction with county-frontage component).

2. **Scope tab** — confirm what's in scope per R&O 86-95 / CDC determination. Net new trips drive the threshold logic. Sets up which roads need analysis.

3. **Roads tab** — for each road on the site frontage:
   - Set name, direction (which side of site), classification, RDCS designation
   - Existing conditions: ROW from CL, paved width, frontage status, sidewalk/curb/lighting/landscape strip, posted speed, measurement source
   - TSP overlays if applicable (Streetscape, Bikeway, Pedestrian Parkway)
   - Existing driveways: location, permit status, design compliance, spacing
   - The **TSP Table 3 panel** appears below the cross-section dropdown — it tells you whether the Ord 738 designation matches the TSP ultimate-urban standard, and if not, it directs you to condition to the TSP standard for development requiring half-street improvements.

4. **Impact tab** — add intersections in the impact area. Enter existing ADT, site contribution ADT, SPIS score, MUTCD warrants met, turn-lane warrants, off-site improvements. The site % is auto-computed; ≥ 10% triggers R&O 86-95 §D.1.1.

5. **Nexus tab** — for each criterion the case triggers, work through the five-step constitutional analysis:
   - Step 1: which government interests does this exaction serve?
   - Step 2: essential nexus narrative (per *Nollan*)
   - Step 3: rough proportionality (per *Dolan*) — for safety conditions, this is satisfied by *Hill*'s sub-impact framework; for non-safety conditions, document the LOS impact
   - Step 4: review the auto-computed comparison metrics (cost per ADT, cost per unit, traffic share, frontage share)
   - Step 5: aggregate cumulative-burden narrative

6. **Plan Review / Traffic Review / Lighting tabs** — generate facility-permit PDFs for each engineering team. Each PDF is a fillable form with the case context pre-populated. Email the PDF to the engineer; when they return it, import the completed PDF to bring their decisions back into the tool.

7. **Authority tab** — review the per-road regulatory view. Use it as a quick reference and as the basis for the printable Authority Map exhibit (button at bottom).

8. **Report tab** — generate the staff report. Read it before finalizing. The report leads with three diagnostic panels:
   - **Missing data** (orange) — fields not yet entered that will appear as `[VERIFY]` in the report
   - **Defensibility alert** (red) at top of Section I — conditions imposed without complete nexus on Section F
   - **Divergence alert** (yellow) at top of Section F — planner narrative percentages that don't match auto-computed values

   Resolve all three before sending the report out.

## Reading the TSP Table 3 panel

The Roads tab shows two cross-section reference standards for each road:

- **Ord 738 cross-section** (the designation you selected): the dimensions for that specific designation per the adopted Road Design and Construction Standards. Many designations are interim sections with gravel shoulders and ditches.
- **TSP Table 3 ultimate urban** (computed from classification + lanes + overlays): the policy-level maximum dimensions per the adopted TSP, including overlay adjustments.

When they match, the panel is blue with a confirmation note. When they differ, the panel is green with a warning: typically the Ord 738 designation is an interim or narrower variant, and for development requiring half-street improvements you should condition to the TSP ultimate.

Example: C-2 (Minor Collector, 2-lane). Ord 738 says 74 ft ROW / 36 ft paved. TSP Table 3 says 74 / 50 — the same ROW, but 50 ft paved is the ultimate urban width. The 36 ft is the rural/interim section. For an urban C-2 development, condition the half-street to provide the 50 ft paved standard.

## Handling the defensibility alert

If Section I shows a defensibility alert, do not submit the report. Each row in the alert means a condition is being imposed but the corresponding constitutional analysis on the Nexus tab is missing or incomplete.

The fix is procedural: go to the Nexus tab, fill in the missing steps for each flagged criterion, regenerate the report, confirm the alert is gone.

## Handling the divergence alert

If Section F shows a divergence alert, your nexus narrative claims a percentage (e.g., "the development contributes 14% at the SW Hall/Murray intersection") that doesn't match the auto-computed value from the case data (e.g., 1.52% based on the 380 ADT site share against 25,000 existing ADT).

This is a high-LUBA-risk situation. Either correct the narrative to use the actual computed percentage, or update the case data (intersection ADTs, site contribution) to match what the narrative is asserting. Both numbers will appear in the staff report and applicants' counsel will compare them.

## Importing engineer-completed PDFs

After emailing a Plan Review or Traffic Review PDF to an engineer, when they reply with the filled form, use the import field on the Plan Review / Traffic Review tabs. The tool extracts:

- RDCS item confirmations (checkboxes)
- Comments per RDCS item (text)
- Overall assessment narrative
- Design exceptions (count + descriptions)
- Administration deposit amount

Imported data goes into the corresponding road's `engr` and `rdcs` fields. The report then incorporates the engineering review.

If the engineer replies with an email instead of returning the PDF, paste their text into the "Import Reply" textarea — the tool tries to parse common formats ("1. ROW Dedication: Applicable — comment...").

## Save / Load case

- **Save case** — saves all current data as a JSON file. Default filename is `casefile_TransReview_YYYY-MM-DD.json`.
- **Load case** — restores from a saved JSON file. Replaces current data.

Use Save before risky operations (clearing the form, switching app types) so you have a recovery point.

## What this tool does not do

- It does not perform engineering analysis. RDCS designations and cross-section standards come from your selection; the tool doesn't validate engineering feasibility.
- It does not perform legal review. The constitutional analysis framework is provided, but you (and county counsel) are the lawyers.
- It does not replace public hearing or notice processes.
- It does not file documents with the County or with LUBA.
- It does not bind the County. Conditions in the generated report are draft until adopted by the decision-making authority.
