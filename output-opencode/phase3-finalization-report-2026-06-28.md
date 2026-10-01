---
title: "phase3-finalization-report-2026-06-28"
doc-id: "mb-3fdcc20c"
updated: "2026-10-01"
---
# Phase 3 Finalization Report

**Date:** 2026-06-28
**Scope:** Document finalization based on audit decisions and research findings

---

## Documents Updated

| # | File | Changes Made |
|---|---|---|
| 1 | `ui-build-prep/anvil-ui-build-standard.md` | ADR references updated (ADR-43/44/45, no more adr-register.md The use of registers has been discontinued. Please review). §6.0 reversed to code-based role assignment. CSS gap section updated. Dark mode deferred to V2. `drp_` note removed. Icon clarified as IconButton shorthand. Remaining Open Items updated. |
| 2 | `ui-build-prep/anvil-ui-build-prompt-template.md` | ADR-1/adr-register.md The use of registers has been discontinued. Please review reference updated to ADR-43. |
| 3 | `adr/37-ui-customization-approach.md` | Marked **Superseded by ADR-45**. NavigationRailLayout/NavigationDrawerLayout now used per ADR-45. |
| 4 | `adr/43-role-property-assignment-mechanism.md` | **Reversed.** Original decision (Designer) superseded by project rule: if property can be set programmatically, it must be. Role now set in code. |
| 5 | `docs/spec-material-3-theme.md` | ADR-43 reference updated to show code-based role assignment. Code examples now correct per reversal. |
| 6 | `docs/spec-component-properties.md` | Added writeback-capable components summary (8 components). Removed HTMLTemplate from Designer-only list. |
| 7 | `docs/design-direction.md` | Card radius updated from 12px to 8px (MUI overlay value). |
| 8 | `docs/nomenclature.md` | Added Icon prefix `ico_` with note that it maps to IconButton used decoratively. |

---

## Key Decisions Applied

| Decision | Impact |
|---|---|
| ADR-37 superseded by ADR-45 | NavigationRailLayout/NavigationDrawerLayout are now the standard for layout shells |
| ADR-43 reversed | Role assignment now in code, not Designer Properties Panel |
| Dark mode deferred to V2 | Not included in V1 build scope |
| Icon = IconButton shorthand | Wireframes use `Icon`, implementation maps to `IconButton` with no click event |
| Writeback list complete | 8 components confirmed: TextBox, TextArea, DropdownMenu, DatePicker, Checkbox, Switch, RadioGroupPanel, Slider |
| Card radius = 8px | MUI overlay value, not base M3 12px |
| M3 dependency is the standard | All component lists must match Anvil M3 theme dependency |

---

## Remaining Steps

| Step | Status | Next Action |
|---|---|---|
| Step 4 — Refresh wireframes | Ready | Use `/design-consultation` |
| Step 5 — Refresh screens | Ready | Use `/design-html` |
| Step 6 — Deploy Standard to Anvil | Ready | Manual file replacement |
| Step 7 — Prepare first prompt | Ready | After Steps 4-6 complete |

---

*End of Phase 3 Finalization Report*
