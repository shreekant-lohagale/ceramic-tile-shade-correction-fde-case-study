@"
# Ceramic Tile Shade Correction — FDE Case Study

Forward Deployed Engineer case study for a ceramic tile manufacturing shade-correction problem.

## Deliverables

- `Kestrel_Task3_Case_Study.md` — full case study
- `Kestrel_Task3_Procurement_Note.md` — one-page client/procurement note
- `Kestrel_Task3_Submission_Form_Answers.md` — submission responses

## Proposed Approach

The proposed solution is a batch-start correction recommender that uses fired-tile measurements and historical correction outcomes to recommend bounded printer-input adjustments.

The pilot keeps QC approval in the loop and does not directly control the printer.

## Key Goals

- Reduce the current multi-hour shade-correction cycle
- Reduce repeated trial-and-error variants
- Produce recommendations in minutes
- Measure success using the client's existing QC process
- Keep human approval before production changes

## Confidentiality

Raw client discovery-call notes and confidential source material are intentionally excluded from this repository. This repository contains only the proposed solution, assumptions, evaluation plan, and submission artifacts.
"@ | Set-Content README.md