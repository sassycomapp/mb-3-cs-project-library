Before starting, use GBrain with source_id 'project-library' to 
search for: "[PACKAGE]", "[PACKAGE] form", "design rules", 
"scaffold spec", "nomenclature", "m3 component mapping", 
"empty state", "[PACKAGE] architecture". Use all findings as 
the specification baseline before touching any file.

---

MANDATORY PRE-WORK — DO NOT SKIP

Per ADR-27 Section 0, read every item below in full before 
creating any wireframe. Do not proceed until all are read:

1. /mnt/c/_mb2-cs-app/project-library/adr/27-design-rules.md
   — in full. This is the governing document for every 
   wireframe in this project. You will apply the Section 7 
   compliance checklist to each wireframe before it is 
   considered complete.
2. /mnt/c/_mb2-cs-app/project-library/docs/nomenclature.md
3. /mnt/c/_mb2-cs-app/project-library/docs/m3_component_mapping.md
4. /mnt/c/_mb2-cs-app/project-library/docs/scaffold-spec.md
5. /mnt/c/_mb2-cs-app/project-library/docs/DESIGN.md
6. Any architecture, ADR, or design documents in 
   project-library/docs/, project-library/adr/, and 
   project-library/implementation/ that bear on the [PACKAGE] 
   package and its forms. Use your GBrain findings above as 
   the starting point, then read the actual files.
7. Any relevant prior findings in project-library/gstack-outputs/ 
   concerning [PACKAGE] forms, states, flows, or design review 
   findings.

---

CRITICAL — SOURCE OF AUTHORITY

No application code has been written. mb-3-cs/client_code/ 
contains empty scaffolding only and must NOT be consulted or 
relied upon as a source of truth for any architectural or 
design decision.

The ONLY source of authority is project-library — its docs/, 
adr/, and implementation/ folders. Every component name, data 
field, container structure, and design decision must be derived 
exclusively from project-library documents.

If you cannot find a clear specification for any form or 
component in project-library, do NOT invent it. Flag it as a 
specification gap in the final report.

---

CRITICAL — DO NOT TRUST ANY EXISTING WIREFRAME

Every wireframe file in the wireframes/ folder is a placeholder 
only. All existing wireframes are corrupt to some degree. None 
of them — without exception — may be used as a reference for 
format, structure, components, or content.

There is no Obsolete wireframes archive. Do not look for one.

The ONLY authority for wireframe FORMAT is ADR-27 Section 2, 
which defines the exact three-section structure with annotated 
examples. Read it directly. The ONLY authority for wireframe 
CONTENT is project-library's docs/, adr/, and implementation/ 
files.

---

CRITICAL — ARCHITECTURE CHECK BEFORE CREATING ANY WIREFRAME

Before creating a wireframe for any form or sub-template in 
this package, verify from scaffold-spec.md and the relevant 
architecture documents that the form is architecturally 
backed — i.e. that it genuinely requires its own UI.

If a placeholder folder or file exists but the architecture 
specifies that the parent form handles its functionality 
differently (e.g. via a grid panel rather than a repeating 
panel with a row template), do NOT create a wireframe for it. 
Record this finding in the final report with the architectural 
reason.

---

CRITICAL — ONE WIREFRAME AT A TIME. NO BATCHING. NO TEMPLATING.

You must produce each wireframe individually and completely 
before moving to the next. For each wireframe:
1. Read the relevant specification documents for that specific 
   form.
2. Design the complete component structure.
3. Write the full three-section wireframe HTML document per 
   ADR-27 Section 2.
4. Run through the ADR-27 Section 7 compliance checklist 
   item by item and confirm every item passes.
5. Save the file.
6. Only then move to the next form.

You may NOT produce multiple wireframes in a single pass. 
You may NOT reuse structure from one wireframe as a template 
for the next. Every wireframe is an independent document 
derived from its own specification.

---

MANDATORY EMPTY STATE RULE

Every form that displays a list, data grid, or data-driven 
content MUST include an empty state. The empty state is a 
dashed-border container (hidden on load, `visible = False`) 
containing EXACTLY these four components in this order — 
no more, no fewer:

1. ico_empty_state (Icon) — context-appropriate icon
2. hdg_empty_state (Heading) — warm, friendly heading
3. txt_empty_state (Text) — brief explanatory text
4. btn_empty_action (Button) — primary action button

A wireframe with an empty state missing any of these four 
components is non-compliant and must be corrected before 
it is accepted.

For forms that do not display data-driven content (e.g. 
layout shells, configuration forms, single-entity editors), 
assess from the specification whether an empty state is 
appropriate. If not applicable, note this in the report.

---

TASK

Produce or correct the wireframes for the [PACKAGE] package. 
The forms in scope are:

[PACKAGE] forms:
  [LIST ALL FORMS AND SUB-TEMPLATES FOR THIS PACKAGE]
  Example:
  - DashboardAnalyticsForm
    /mnt/c/_mb2-cs-app/project-library/wireframes/[PACKAGE]/DashboardAnalyticsForm/
  - RevenueReportForm
    /mnt/c/_mb2-cs-app/project-library/wireframes/[PACKAGE]/RevenueReportForm/
  - RevenueReportForm-RowTemplate (verify architectural backing 
    before creating)
    /mnt/c/_mb2-cs-app/project-library/wireframes/[PACKAGE]/RevenueReportForm/RevenueReportForm-RowTemplate/

For each form (in order, one at a time):
1. Verify architectural backing from scaffold-spec.md and 
   the relevant architecture documents.
2. Derive the complete component structure, states, and RBAC 
   constraints from project-library specifications.
3. Produce the wireframe per ADR-27 three-section structure.
4. Complete the ADR-27 Section 7 compliance checklist.
5. Save the file to its designated path above.
6. Move to the next form only when the current one is complete.

File naming convention: wireframe-[package]-[FormName].html
(Check nomenclature.md for the exact naming standard.)

---

/design-consultation

---

FINAL STEPS — REQUIRED, IN THIS ORDER

1. Save a report to:
   /mnt/c/_mb2-cs-app/project-library/gstack-outputs/
   The report MUST include:
   - Each wireframe created and its file path
   - ADR-27 Section 7 compliance result for each wireframe
   - Any forms/sub-templates skipped due to no architectural 
     backing, with the architectural reason stated
   - Any specification gaps found where project-library 
     documents were insufficient to determine the form's 
     structure

2. Commit and push:
   cd /mnt/c/_mb2-cs-app/project-library
   git add -A
   git commit -m "[PACKAGE] wireframes rebuilt per ADR-27 2026-06-18"
   git push origin main

3. Sync GBrain:
   gbrain sync --source project-library
   gbrain embed --stale