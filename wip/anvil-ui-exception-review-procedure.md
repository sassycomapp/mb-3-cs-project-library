# Per-UI Exception Review Procedure

What I do with each new wireframe HTML + screenshot pair, before filling in the Template's
`{{EXCEPTION_NOTES}}` placeholder for that specific build. The goal is to add a note only
when something is genuinely missing from the Standard's coverage — not to restate what the
Standard (and therefore the agent) should already know.

---

## Steps

**1. Locate the wireframe's outer container.**
Identify the top-level container (e.g. `col_main`) that bounds everything the wireframe
declares. Everything in the screenshot outside that boundary — sidebar, app bar,
notification/profile icons, footer, sign-out — is page chrome per Standard §9.1. Confirmed
out of scope; never mentioned in the outgoing prompt.

**2. Locate any DataGrid / RepeatingPanel.**
If the wireframe declares one, its populated-looking row content in the screenshot belongs
to a separate row-template Form (Standard §9.2) — out of scope for this build. The only
note this generates is a reminder to set the RepeatingPanel's `template` property, naming
the expected row-template Form if it's determinable from naming convention (typically
`{FormName}RowTemplate`).

**3. Check Buttons/IconButtons/TextBoxes within the remaining in-scope area for undeclared icons.**
Compare each Button/IconButton, and each TextBox, against the screenshot. If it shows an
icon with no `mi:` annotation in the wireframe text:
  - If the icon matches Standard §10's lookup table → no note needed, the agent has this.
  - If it's ambiguous, unusual, or not in the table → add a specific note naming the
    component and the closest `mi:` icon.
  - For Buttons specifically, also note `icon_alignment` ("leading"/"trailing") if it's
    not obvious from the screenshot which side the icon sits on.

**4. Check input-style components for visible appearance mismatches.**
Compare TextBox/TextArea/DropdownMenu/DatePicker against the screenshot's visible styling
(outlined vs. filled, etc.) per Standard §6.2. Note only genuine mismatches against what
the wireframe declares.

**5. Check writeback coverage.**
Cross-reference the wireframe's properties table against visibly-interactive fields in the
screenshot. Flag only if a field clearly accepts user input in the screenshot but has no
writeback declaration in the wireframe.

**6. Compose `{{EXCEPTION_NOTES}}`.**
Keep it to genuine exceptions only. If nothing qualifies after steps 1–5, this section is
empty — that's the expected steady-state once the Standard has absorbed the recurring
patterns, not a sign the review was skipped.

---

*This procedure is itself provisional — if a build surfaces a new recurring pattern,
the fix belongs in the Standard (so all future builds inherit it), not as a permanent
fixture of this per-UI review.*
