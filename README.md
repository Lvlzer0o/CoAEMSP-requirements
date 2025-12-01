# CoAEMSP Student Minimum Competency Tracker

This repository documents a repeatable Notion setup for tracking CoAEMSP Student Minimum Competency (SMC) progress. It is designed for a single student (example: Trenton Cadena, capstone target March 31, 2026) but can be duplicated for cohorts.

## What’s here
- **README.md**: Implementation guide for the Notion dashboard.
- **LICENSE**: MIT license for this repository.
- **.gitignore**: Basic ignores for local/editor artifacts.

## Notion database schema
Create a single database called **Competency Tracker** with these properties:
- `Competency Name` (Title)
- `Table` (Select: Age Groups, Conditions, Skills, Field Experience, EMT Skills)
- `Category` (Select: Pediatric, Adult, Geriatric, Trauma, Cardiac, Respiratory, etc.)
- `Minimum Required` (Number)
- `Actual Completed` (Number)
- `Status` (Formula)
- `Progress Bar` (Formula)
- `Deficit` (Formula)
- `Priority` (Select: Critical, High, Medium, Complete)
- `Last Updated` (Date)
- `Notes` (Text)

### Formulas
- `Status`  
  ```
  if(
    prop("Actual Completed") >= prop("Minimum Required"),
    "✅ Met",
    if(
      prop("Actual Completed") == 0,
      "🔴 No Progress",
      if(
        prop("Actual Completed") / prop("Minimum Required") < 0.5,
        "🟡 Partial",
        "🟢 Nearly There"
      )
    )
  )
  ```
- `Progress Bar`  
  ```
  slice("▮▮▮▮▮▮▮▮▮▮", 0, floor(prop("Actual Completed") / prop("Minimum Required") * 10)) +
  slice("▯▯▯▯▯▯▯▯▯▯", 0, 10 - floor(prop("Actual Completed") / prop("Minimum Required") * 10))
  ```
- `Deficit`  
  ```
  if(prop("Minimum Required") - prop("Actual Completed") > 0, prop("Minimum Required") - prop("Actual Completed"), 0)
  ```

## Populate the data
Enter one row per requirement from the CoAEMSP spreadsheet, using the total minimum and total actual values for each item. Suggested rows include:
- **Table 1 (Age Groups):** Pediatric total, each pediatric age subgroup, Adult, Geriatric.
- **Table 2 (Conditions):** Trauma, Psychiatric/Behavioral, Obstetric delivery, Complicated obstetric delivery, Distressed neonate, Cardiac complaints, Cardiac arrest, Dysrhythmias, Medical neurologic, Respiratory, Other medical.
- **Table 3 (Skills):** IV access, IV/IO meds, airway skills, cardiology skills, etc. Use the total required/actual for each skill (lab + patient).
- **Table 4 (Field/Capstone):** Field Experience, Capstone Field Internship.
- **Table 5 (EMT Skills):** One row per EMT skill with program minimum and actual.

## Recommended views in Notion
- **Kanban by Status:** Columns for 🔴 No Progress → 🟡 Partial → 🟢 Nearly There → ✅ Met.
- **Critical Items table:** Filter Priority = Critical; sort by `Deficit` descending.
- **Table drill-downs:** One view per `Table` value (Age Groups, Conditions, Skills, Field Experience, EMT Skills).
- **Gallery or list:** Show `Progress Bar`, `Deficit`, and `Priority` for quick scanning.

## Weekly workflow
1) After each shift/rotation, update `Actual Completed` and `Last Updated`.  
2) Set `Priority` to **Critical** for zero-progress items with looming deadlines (e.g., capstone).  
3) Review the Critical Items view and schedule remediation.  
4) Drag items across the Kanban as progress is made.

## Notes on totals vs. breakdowns
- The national minimums often exceed the sum of subgroup minimums (e.g., pediatric total vs. age subgroup minimums). Track the total row and the subgroup rows separately so both standards stay visible.
- Skills that allow simulation are still tracked as total required/actual; use `Notes` to log whether attempts were sim or live.

## Extending the template
- Add relations/rollups for cohorts if you track multiple students.  
- Add a “Date Completed” to build a timeline view.  
- Export to PDF weekly for instructor review.

## License
MIT — see `LICENSE`.
