# MASTER WIREFRAME PROMPT TEMPLATE

**How to use this file:** Copy everything below the line into OpenCode chat.
Before pasting, replace the four placeholder blocks marked `[[ REPLACE: ... ]]`
with the correct values for the package you are running. Do not change
anything else in this template.

---

Before starting, use GBrain with source_id 'project-library' to
search for: 

Before starting, use GBrain with source_id 'project-library' to
search for: "[PACKAGE]", "[PACKAGE] form",
"[PACKAGE] architecture", "design rules", "scaffold spec",
"nomenclature", "m3 component mapping", "empty state". Use all
findings as the specification baseline before touching any file.

Use all findings as the specification baseline before touching any file.

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
   project-library/implementation/ that bear on the
   [PACKAGE] package and its
   forms. Use your GBrain findings above as the starting point,
   then read the actual files.
7. Any relevant prior findings in project-library/gstack-outputs/
   concerning [PACKAGE] forms, states, flows,
   or design review findings.

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

Every named form and sub-template provided in the task list
represents UI that must be built. By default, EVERY item in
the list MUST receive its own complete wireframe document.

A wireframe may be skipped ONLY if the architecture has
eliminated the distinct visual/structural requirement entirely
— meaning the item no longer corresponds to any distinct UI
structure that needs to be specified.

A wireframe may NOT be skipped merely because the architecture
states the variant will be implemented as an internal panel
within the parent Anvil Form rather than as a separate Anvil
Form or Component. "Built into the parent form" is a CODE
ORGANIZATION decision, not a UI ELIMINATION decision. If a
layout variant has its own distinct visual structure — different
component arrangement, different content emphasis, different
included/excluded elements — it requires its own complete
wireframe document, regardless of which Anvil class will
ultimately contain it.

If unsure whether a sub-template has been genuinely eliminated
versus merely folded into the parent's code, the default is to
PRODUCE THE WIREFRAME. Skipping is the exception and must be
justified by genuine elimination of the UI structure, not by
implementation convenience.

Calibration examples:
- CORRECT TO SKIP: A RowTemplate is skipped when the parent form
  has switched from a RepeatingPanel (needing a custom row
  template) to a GridPanel (auto-rendering, no custom row
  needed). The row-level UI structure no longer exists at all.
- INCORRECT TO SKIP: A named template variant (e.g. a "classic",
  "two-column", or "full-width" layout) that represents a
  genuinely different page arrangement — different sidebar
  presence, different element ordering, different included
  components — even though all variants will be implemented as
  panels within one Anvil Form, toggled programmatically. Each
  variant requires its own full wireframe document.

---

MANDATORY COVERAGE ACCOUNTING

You are given an explicit, complete list of every wireframe file
path in scope for this package. Before finishing, you MUST
account for EVERY single item in that list — no exceptions, no
silent omissions. For each item, the final report must state
either:

(a) CREATED — with file path and ADR-27 Section 7 compliance result, or
(b) SKIPPED — with the specific architectural reason it was
    genuinely eliminated (per the criteria above)

Before submitting your report, cross-check it line-by-line
against the full input list. If any item from the list does not
appear under either category, that is a process failure — go
back and resolve it before finishing.

---

RESUME BEHAVIOR — SKIP ALREADY-VERIFIED WIREFRAMES

If a wireframe file in the list below already exists, open it
and check it against ADR-27 Section 7's compliance checklist.
If it fully complies, skip it — do not recreate or modify it.
Record it in the final report as "Already complete — skipped,
re-verified compliant."

If it exists but does NOT fully comply, or is empty/placeholder,
treat it as not done and create it properly.

This allows this prompt to be re-run safely on a partially
completed package without redoing verified work.

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

Produce or correct the wireframes for the
[PACKAGE] package. 

The complete list of items in scope is:

[[ REPLACE: full list of every wireframe file path for this
package, one per line, including all forms and all
sub-templates/row templates — e.g.:
/mnt/c/_mb2-cs-app/project-library/wireframes/bookings/AvailabilitySettingsForm/wireframe-bookings-AvailabilitySettingsForm.html
/mnt/c/_mb2-cs-app/project-library/wireframes/bookings/BookingCalendarForm/wireframe-bookings-BookingCalendarForm.html
/mnt/c/_mb2-cs-app/project-library/wireframes/bookings/BookingCalendarForm/tmpl_day_event/wireframe-bookings-tmpl_day_event.html
... etc., list every single file ]]

For each item (in order, one at a time):
1. Check resume behavior — if it already exists and is ADR-27
   compliant, skip and record as already complete.
2. If not done: verify architectural backing per the criteria
   above (default to producing the wireframe).
3. Derive complete component structure, states, and RBAC
   constraints from project-library specifications.
4. Produce the wireframe per ADR-27 three-section structure.
5. Complete the ADR-27 Section 7 compliance checklist.
6. Save the file to its designated path above.
7. Move to the next item only when the current one is fully
   resolved (created, skipped-with-genuine-reason, or
   already-complete).

File naming convention:
wireframe-[PACKAGE]-[FormName].html
(Check nomenclature.md for the exact naming standard.)

---

/design-consultation

---

FINAL STEPS — REQUIRED, IN THIS ORDER

1. Save a report to:
   /mnt/c/_mb2-cs-app/project-library/gstack-outputs/
   The report MUST list every item from the task list above
   individually with its outcome (Created / Already Complete /
   Skipped-with-reason). Cross-check against the full list
   before submitting — no item may be absent from the report.

2. Commit and push:
   cd /mnt/c/_mb2-cs-app/project-library
   git add -A
   git commit -m "[PACKAGE] wireframes completed per ADR-27 [[ add current date ]]"
   git push origin main

3. Sync GBrain:
   gbrain sync --source project-library
   gbrain embed --stale
