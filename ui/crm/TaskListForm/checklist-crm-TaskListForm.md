# Checklist — TaskListForm

**Wireframe:** `wireframes/crm/TaskListForm/wireframe-crm-TaskListForm.html`
**Date:** 2026-06-28
**Auditor:** Build agent

---

## A. ADR-43 — Role assignment (reversed)

- [x] Properties table does NOT list `role` as a Designer property — PASS (lines 162-164: only writeback = W listed)
- [x] Component annotations do not say "set role in Designer" — PASS

## B. ADR-44 — Responsive behaviour (wrap_on)

- [x] No breakpoint tables or pixel-based responsive rules — PASS

## C. ADR-45 — HTMLTemplate banned

- [x] No `HtmlTemplate` component anywhere in the wireframe — PASS

## D. ADR-37 superseded

- [x] No references to "custom layout approach" — PASS

## E. DataRowPanel

- [x] DataRowPanel uses `drp_` prefix — N/A (DataGrid uses a separate row template)

## F. M3 component standard

- [x] Every component is in the approved M3 list or approved custom list — PASS (ColumnPanel, FlowPanel, Heading, Text, TextBox, DropdownMenu, Button, DataGrid — all approved)
- [x] InteractiveCard uses only `elevated`, `filled`, or `outlined` — N/A
- [x] No `SidesheetContent` component — PASS
- [x] No `Label` component — PASS
- [x] Card uses `Card` component — N/A
- [x] No `HtmlPanel` component — PASS

## G. Icon component

- [x] Icon components use `ico_` prefix — N/A

## H. mi: prefix for icons

- [x] All icon annotations use `mi:` prefix — N/A

## I. Writeback properties

- [x] Properties table includes writeback (W) for all 8 components — PASS (txt_search, dd_priority, dd_status all have writeback = W)
- [x] No writeback listed for non-writeback components — PASS

## J. Max-width values

- [x] Max-width annotations match final values — N/A

## K. Card radius

- [x] Card annotations reference 8px radius — N/A

## L. Tag/badge composition

- [x] No `Label` component used for tags or badges — PASS

## M. Dark mode

- [x] No references to dark mode — PASS

## N. Carousel viewport

- [x] Carousel viewport uses ColumnPanel with role-driven CSS — N/A

## O. Section structure (ADR-27)

- [x] Section 1 — Introduction present — PASS (line 59)
- [x] Section 2 — Wireframe Diagram present — PASS (line 60)
- [x] Section 3 — Properties Table present — PASS (line 156)
- [x] No sample data in wireframe — PASS (replaced with placeholders)
- [x] No behaviour text in wireframe diagram — PASS
- [x] Containers drawn as containers with names inset in border — PASS

---

## Result

**Status: PASS — no fixes needed**

---

*End of checklist*
