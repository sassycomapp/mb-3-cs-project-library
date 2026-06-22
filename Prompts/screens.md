Model -Copy this to use

blog Line 175


---
/design-html

Before starting, use GBrain with source_id 'project-library' to 
search for: "[PACKAGE]", "[PACKAGE] form", "screen production 
standard", "DESIGN.md", "MUI aesthetic overlay", "empty state", 
"palette tokens". Use all findings as context.

---

MANDATORY PRE-WORK — DO NOT SKIP

Read every item below in full before producing any screen. 
Do not proceed until all are read:

1. /mnt/c/_mb2-cs-app/project-library/docs/screen-production-standard.md
   — The canonical rules for screen HTML production. Follow 
   exactly.
2. /mnt/c/_mb2-cs-app/project-library/docs/DESIGN.md
   — Pay particular attention to: MUI Aesthetic Implementation 
   section (the 9 adopted variables), Design Tokens, Typography, 
   Spacing/Radius/Elevation, AdminLayout, Empty States, and 
   any [PACKAGE]-specific sections.
3. /mnt/c/_mb2-cs-app/project-library/docs/DESIGN_palette_section.md
   — Professional Blue is the default demonstration palette.
4. /mnt/c/_mb2-cs-app/project-library/docs/theme_patched.css
   — The deployment-ready theme with MUI patch. All CSS custom 
   properties in your output MUST derive from this file. Read 
   the full contents.
5. /mnt/c/_mb2-cs-app/project-library/docs/theme_mui_patch.css
   — The MUI patch in isolation. Understand what changed from 
   base M3 and why.
6. /mnt/c/_mb2-cs-app/project-library/docs/m3_component_mapping.md
7. /mnt/c/_mb2-cs-app/project-library/docs/ui-standards.md
8. /mnt/c/_mb2-cs-app/project-library/docs/spec-material-3-theme.md
9. /mnt/c/_mb2-cs-app/project-library/adr/37-ui-customization-approach.md
10. /mnt/c/_mb2-cs-app/project-library/adr/15-material-3-theme-component-scope.md
11. /mnt/c/_mb2-cs-app/project-library/adr/27-design-rules.md
    — Section 1.3 specifically: architecture determines 
    wireframes, wireframes determine screens.
12. Any architecture, ADR, or design documents in 
    project-library/docs/, project-library/adr/, and 
    project-library/implementation/ that bear on [PACKAGE] 
    forms and their content, flows, and states.
13. MATERIAL ICONS — REQUIRED IN ALL SCREENS

All screens that include navigation or icon components MUST 
import the Material Icons font in the HTML head:

<link href="https://fonts.googleapis.com/icon?family=Material+Icons" 
      rel="stylesheet">

Icon spans must use class="material-icons" and contain the 
icon ligature name only. Without this import, icon names 
render as plain text and corrupt the layout.

---

STYLE REFERENCE

/mnt/c/_mb2-cs-app/project-library/gstack-outputs/design-reference-screens.html

This file correctly demonstrates the current design system 
with the MUI aesthetic overlay fully applied. Match its 
visual quality, token usage, component styling, and aesthetic 
character exactly.

---

CRITICAL — WIREFRAMES ARE YOUR STRUCTURAL AUTHORITY

The wireframes for this package are your sole authority for 
component structure. Every component you render in a screen 
must exist in the corresponding wireframe. Do not add, remove, 
or restructure components beyond what the wireframe specifies.

Read each wireframe before producing its screen.

---

CRITICAL — MUI AESTHETIC OVERLAY

MUI is an aesthetic-only CSS overlay. There are NO MUI React 
components, NO sx props, NO MUI imports, NO MUI CDN links — 
ever. The 9 adopted MUI variables must be visibly present in 
every screen:

1. Surface distinction — body uses var(--surface); panels 
   and left nav use var(--surface-container-low)
2. Card shadow — hsla(220,30%,5%,0.07) 0px 4px 16px 0px, 
   hsla(220,25%,10%,0.07) 0px 8px 16px -5px
3. Card border radius — 8px
4. Typography heading weight — 600
5. Dividers — outline-variant at 40% opacity
6. State hover opacity — 0.08
7. State pressed/focused opacity — 0.12
8. App bar — var(--surface) background, 1px hairline 
   bottom border at outline-variant 40% opacity
9. Left nav — var(--surface-container-low) background, 
   1px hairline right border at outline-variant 40% opacity

---

CRITICAL — APP BAR COLOR

The app bar background MUST use var(--surface) — which 
renders as white or near-white in the Professional Blue 
palette. It must NEVER be black, dark grey, or any hardcoded 
dark color. A dark app bar is a hardcoded color violation.

---

CRITICAL — NO HARDCODED COLORS

Every color value in the output MUST resolve through a CSS 
custom property from theme_patched.css. No hardcoded hex 
values, rgb(), rgba() with specific color values, or HSL 
values that are not defined as CSS custom properties. 
The only exception is the MUI card shadow which uses 
hsla() values as defined in DESIGN.md MUI section.

---

CRITICAL — ONE SCREEN AT A TIME. NO BATCHING. NO TEMPLATING.

You must produce each screen individually and completely 
before moving to the next. For each screen:
1. Read the corresponding wireframe in full.
2. Read the relevant specification documents for that 
   specific form.
3. Produce the complete screen HTML.
4. Verify all 9 MUI variables are present.
5. Verify no hardcoded colors exist.
6. Verify the empty state (if applicable) shows all four 
   required components visibly.
7. Save the file.
8. Only then move to the next form.

---

MANDATORY EMPTY STATE RULE

Every screen for a form that displays a list, data grid, or 
data-driven content MUST visibly render the empty state 
below the main screen content. The empty state MUST show 
all four components:

1. Icon — context-appropriate
2. Heading — warm, friendly message
3. Body text — brief explanation
4. Primary action button — clearly rendered as a filled 
   M3 button

An empty state section missing the primary action button 
is non-compliant.

---

SAMPLE DATA

Every screen MUST include realistic sample data appropriate 
to the consulting/services vertical (a salon, spa, therapy 
practice, or consulting business). No placeholder text such 
as "Lorem ipsum". Use plausible South African business data 
with ZAR currency where financial figures appear.

---

TASK

Produce screens for the [PACKAGE] package forms. The forms 
in scope are:

[PACKAGE] forms and screen output paths:
  Example:
  - DashboardAnalyticsForm
    /mnt/c/_mb2-cs-app/project-library/screens/[PACKAGE]/DashboardAnalyticsForm/
  - RevenueReportForm
    /mnt/c/_mb2-cs-app/project-library/screens/[PACKAGE]/RevenueReportForm/

For each form (in order, one at a time):
1. Read the corresponding wireframe from:
   /mnt/c/_mb2-cs-app/project-library/wireframes/[PACKAGE]/[FormName]/
2. Read relevant specification documents.
3. Produce the screen HTML matching the wireframe structure 
   and the design-reference-screens.html aesthetic.
4. Verify all 9 MUI variables, no hardcoded colors, and 
   complete empty state.
5. Save the file.
6. Move to the next form only when the current one is complete.

File naming convention: screen-[package]-[FormName].html
(Check nomenclature.md for the exact naming standard.)

---

/design-html

---

FINAL STEPS — REQUIRED, IN THIS ORDER

1. Save a report to:
   /mnt/c/_mb2-cs-app/project-library/gstack-outputs/
   The report MUST include:
   - Each screen created and its file path
   - Confirmation that all 9 MUI variables are present 
     in each screen
   - Confirmation that no hardcoded colors were used
   - Confirmation that every applicable empty state 
     includes all four required components
   - Any specification gaps encountered

2. Commit and push:
   cd /mnt/c/_mb2-cs-app/project-library
   git add -A
   git commit -m "[PACKAGE] screens produced per design system 2026-06-18"
   git push origin main

3. Sync GBrain:
   gbrain sync --source project-library
   gbrain embed --stale