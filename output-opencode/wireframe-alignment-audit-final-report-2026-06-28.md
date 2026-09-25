---
title: "wireframe-alignment-audit-final-report-2026-06-28"
doc-id: "wireframe-alignment-audit-final-report-2026-06-28"
---
# Wireframe Alignment Audit — Final Report

**Date:** 2026-06-28
**Scope:** All 82 wireframes in `wireframes/`
**Method:** Individual checklist per wireframe against 15-section alignment checklist

---

## Summary

| Category | Count |
|---|---|
| Wireframes audited | 82 |
| PASS (no fixes needed) | 75 |
| PASS (fixes applied) | 7 |
| FAIL | 0 |

---

## Fixes Applied During Audit

| Wireframe | Fix |
|---|---|
| `blog/CommentModerationForm/CommentModerationForm-RowTemplate` | Removed reference to deleted "Programmatic behaviour" section |
| `invoices/InvoiceEditorForm` | Removed behaviour note after Properties Table |
| `invoices/InvoiceViewerForm` | Removed status table and behaviour note after Properties Table |
| `components/ClauseBuilder` | Removed behaviour note after Properties Table |
| `dashboard/DashboardForm` | Removed behaviour note after Properties Table |
| `crm/ContactViewerForm` | Removed behaviour note after Properties Table |
| `services/ServiceViewerForm` | Removed behaviour note after Properties Table |

---

## Checklist Sections Verified

| Section | What was checked | Issues found |
|---|---|---|
| A. ADR-43 (role assignment) | Properties table does not list `role` as Designer property | 0 |
| B. ADR-44 (wrap_on) | No breakpoint tables or pixel-based responsive rules | 0 |
| C. ADR-45 (HTMLTemplate banned) | No HtmlTemplate component | 0 |
| D. ADR-37 superseded | No references to custom layout approach | 0 |
| E. DataRowPanel | `drp_` prefix used correctly | 0 |
| F. M3 component standard | All components approved | 0 |
| G. Icon component | `ico_` prefix used correctly | 0 |
| H. mi: prefix | All icon annotations use `mi:` prefix | 0 |
| I. Writeback properties | All 8 components listed where applicable | 0 |
| J. Max-width values | Match final values | 0 |
| K. Card radius | 8px (MUI overlay) | 0 |
| L. Tag/badge composition | No Label used for tags | 0 |
| M. Dark mode | No dark mode references | 0 |
| N. Carousel viewport | ColumnPanel with role-driven CSS | 0 |
| O. Section structure (ADR-27) | 3-section structure, no sample data, no behaviour text | 7 (all fixed) |

---

## Wireframes by Folder

| Folder | Wireframes | Status |
|---|---|---|
| analytics/ | 3 | All PASS |
| auth/ | 4 | All PASS |
| bookings/ | 11 | All PASS |
| blog/ | 14 | All PASS (1 fix) |
| components/ | 5 | All PASS (1 fix) |
| crm/ | 9 | All PASS (1 fix) |
| dashboard/ | 2 | All PASS (1 fix) |
| invoices/ | 3 | All PASS (2 fixes) |
| landing_pages/ | 6 | All PASS |
| layouts/ | 4 | All PASS |
| onboarding/ | 1 | All PASS |
| public_pages/ | 9 | All PASS |
| services/ | 4 | All PASS (1 fix) |
| settings/ | 7 | All PASS |
| **Total** | **82** | **75 PASS, 7 fixed** |

---

## Phase 1-3 Decisions Reflected

All wireframes now reflect:
- ADR-43: Role set in code (reversed from Designer)
- ADR-44: `wrap_on` for responsive behaviour (no breakpoints)
- ADR-45: HTMLTemplate banned (NavigationRailLayout/NavigationDrawerLayout for layouts)
- ADR-37: Superseded by ADR-45
- DataRowPanel with `drp_` prefix
- Icon = IconButton shorthand with `ico_` prefix
- `mi:` prefix for all icon annotations
- Writeback for all 8 components where applicable
- Max-width values finalized
- Card radius = 8px (MUI overlay)
- Dark mode deferred to V2
- No sample data in wireframes
- No behaviour text in wireframes

---

*End of Wireframe Alignment Audit — Final Report*
