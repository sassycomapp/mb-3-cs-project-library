# Checklist — BookingListForm-RowTemplate

**Wireframe:** `wireframes/bookings/BookingListForm/BookingListForm-RowTemplate/wireframes-bookings-BookingListForm-RowTemplate.html`
**Date:** 2026-06-28
**Auditor:** Build agent

---

## A. ADR-43 — Role assignment (reversed)

- [x] Properties table does NOT list `role` as a Designer property — PASS (line 65: "No Designer-only properties")
- [x] Component annotations do not say "set role in Designer" — PASS

## B. ADR-44 — Responsive behaviour (wrap_on)

- [x] No breakpoint tables or pixel-based responsive rules — PASS
- [x] No references to 998px, 640px, or 1024px breakpoints — PASS

## C. ADR-45 — HTMLTemplate banned

- [x] No `HtmlTemplate` component anywhere in the wireframe — PASS

## D. ADR-37 superseded

- [x] No references to "custom layout approach" — PASS
- [x] No references to "HtmlTemplate shell" — PASS

## E. DataRowPanel

- [x] DataRowPanel uses `drp_` prefix — PASS (drp_booking_row on line 43)
- [x] DataRowPanel is used for DataGrid row templates — PASS
- [x] DataRowPanel is listed in the component annotations — PASS

## F. M3 component standard

- [x] Every component is in the approved M3 list or approved custom list — PASS (DataRowPanel, Text, FlowPanel, IconButton — all approved)
- [x] InteractiveCard uses only `elevated`, `filled`, or `outlined` — N/A
- [x] No `SidesheetContent` component — PASS
- [x] No `Label` component — PASS
- [x] Card uses `Card` component — N/A
- [x] No `HtmlPanel` component — PASS

## G. Icon component

- [x] Icon components use `ico_` prefix — N/A (no Icon components)
- [x] Icon is understood as `IconButton` used decoratively — N/A

## H. mi: prefix for icons

- [x] All icon annotations use `mi:` prefix — PASS (mi:visibility, mi:edit, mi:delete on lines 53-55)
- [x] No plain icon names without `mi:` prefix — PASS

## I. Writeback properties

- [x] Properties table includes writeback (W) for all 8 components — N/A
- [x] No writeback listed for non-writeback components — PASS

## J. Max-width values

- [x] Max-width annotations match final values — N/A

## K. Card radius

- [x] Card annotations reference 8px radius — N/A

## L. Tag/badge composition

- [x] Tag/badge components use `m3.Button` or `m3.Card` composition — N/A
- [x] No `Label` component used for tags or badges — PASS

## M. Dark mode

- [x] No references to dark mode — PASS

## N. Carousel viewport

- [x] Carousel viewport uses ColumnPanel with role-driven CSS — N/A

## O. Section structure (ADR-27)

- [x] Section 1 — Introduction present — PASS (line 29)
- [x] Section 2 — Wireframe Diagram present — PASS (line 38)
- [x] Section 3 — Properties Table present — PASS (line 61)
- [x] No sample data in wireframe — PASS
- [x] No behaviour text in wireframe diagram — PASS
- [x] Containers drawn as containers with names inset in border — PASS

---

## Result

**Status: PASS — no fixes needed**

---

*End of checklist*
