---
title: "wireframe-alignment-checklist"
doc-id: "mb-64454deb"
updated: "2026-10-01"
---
# Wireframe Alignment Checklist

**Purpose:** Audit every wireframe against all changes made during the UI build preparation session (2026-06-28).
**Source:** All work done today — investigation findings, ADR reversals, document updates, component clarifications.

---

## How to use

For each wireframe, check every item below. Mark PASS or FAIL. If FAIL, note what needs to change.

---

## A. ADR-43 — Role assignment (reversed)

- [ ] Properties table does NOT list `role` as a Designer property
- [ ] Component annotations do not say "set role in Designer" or "role: X" as a Designer instruction
- [ ] If role is mentioned in annotations, it is noted as set in code (not Designer)

## B. ADR-44 — Responsive behaviour (wrap_on)

- [ ] No breakpoint tables or pixel-based responsive rules
- [ ] No references to 998px, 640px, or 1024px breakpoints
- [ ] No references to "mobile-first" with pixel thresholds

## C. ADR-45 — HTMLTemplate banned

- [ ] No `HtmlTemplate` component anywhere in the wireframe
- [ ] No references to `anvil_slot_repeat` property

## D. ADR-37 superseded

- [ ] No references to "custom layout approach"
- [ ] No references to "HtmlTemplate shell"
- [ ] Layout wireframes use `NavigationRailLayout` or `NavigationDrawerLayout` (not custom ColumnPanel shells)

## E. DataRowPanel

- [ ] DataRowPanel uses `drp_` prefix (not `flow_` or unnamed)
- [ ] DataRowPanel is used for DataGrid row templates (not FlowPanel)
- [ ] DataRowPanel is listed in the component annotations

## F. M3 component standard

- [ ] Every component is in the approved M3 list or approved custom list
- [ ] InteractiveCard uses only `elevated`, `filled`, or `outlined` (not `tonal`)
- [ ] No `SidesheetContent` component
- [ ] No `Label` component (use `Text`)
- [ ] Card uses `Card` component (not `ColumnPanel` with role class)
- [ ] No `HtmlPanel` component (use `ColumnPanel`)

## G. Icon component

- [ ] Icon components use `ico_` prefix
- [ ] Icon is understood as `IconButton` used decoratively (no click event)
- [ ] If wireframe shows an icon-only button with click handler, it should be `IconButton` with `btn_` prefix

## H. mi: prefix for icons

- [ ] All icon annotations use `mi:` prefix (e.g., `mi:dashboard`, `mi:edit`, `mi:delete`)
- [ ] No plain icon names without `mi:` prefix (e.g., not just `dashboard`)

## I. Writeback properties

- [ ] Properties table includes writeback (W) for all 8 writeback-capable components:
  - TextBox (`text`)
  - TextArea (`text`)
  - DropdownMenu (`selected_value`)
  - DatePicker (`selected_date`)
  - Checkbox (`checked`)
  - Switch (`selected`)
  - RadioGroupPanel (`selected_value`)
  - Slider (`value`)
- [ ] No writeback listed for non-writeback components (RadioButton, FileLoader, Button, etc.)

## J. Max-width values

- [ ] Max-width annotations match final values:
  - Auth: 400px
  - List: 1200px
  - Dashboard: 1200px
  - Editor/Viewer/Forms: 900px
  - Public: 900px
  - Settings: 1000px
  - Onboarding: 600px
  - Card (grid item): 320px
  - Modal form: 480px
  - Modal detail: 640px

## K. Card radius

- [ ] Card annotations reference 8px radius (not 12px)

## L. Tag/badge composition

- [ ] Tag/badge components use `m3.Button` or `m3.Card` composition (not Label)
- [ ] No `Label` component used for tags or badges

## M. Dark mode

- [ ] No references to dark mode in wireframes (deferred to V2)

## N. Carousel viewport

- [ ] Carousel viewport uses `ColumnPanel` with role-driven CSS (not HtmlTemplate)
- [ ] Role annotation is `carousel-viewport` (sets position: relative, overflow: hidden via CSS)

## O. Section structure (ADR-27)

- [ ] Section 1 — Introduction present
- [ ] Section 2 — Wireframe Diagram present
- [ ] Section 3 — Properties Table present
- [ ] No sample data in wireframe
- [ ] No behaviour text in wireframe diagram
- [ ] Containers drawn as containers with names inset in border

---

*End of Wireframe Alignment Checklist*
