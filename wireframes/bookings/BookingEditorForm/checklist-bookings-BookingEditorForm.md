# Checklist — BookingEditorForm

**Wireframe:** `wireframes/bookings/BookingEditorForm/wireframe-bookings-BookingEditorForm.html`
**Date:** 2026-06-28
**Auditor:** Build agent

---

## A. ADR-43 — Role assignment (reversed)

- [x] Properties table does NOT list `role` as a Designer property — PASS (lines 186-190: only writeback = W listed)
- [x] Component annotations do not say "set role in Designer" — PASS
- [x] If role is mentioned in annotations, it is noted as set in code — PASS

## B. ADR-44 — Responsive behaviour (wrap_on)

- [x] No breakpoint tables or pixel-based responsive rules — PASS
- [x] No references to 998px, 640px, or 1024px breakpoints — PASS
- [x] No references to "mobile-first" with pixel thresholds — PASS

## C. ADR-45 — HTMLTemplate banned

- [x] No `HtmlTemplate` component anywhere in the wireframe — PASS
- [x] No references to `anvil_slot_repeat` property — PASS

## D. ADR-37 superseded

- [x] No references to "custom layout approach" — PASS
- [x] No references to "HtmlTemplate shell" — PASS

## E. DataRowPanel

- [x] DataRowPanel uses `drp_` prefix — N/A
- [x] DataRowPanel is used for DataGrid row templates — N/A
- [x] DataRowPanel is listed in the component annotations — N/A

## F. M3 component standard

- [x] Every component is in the approved M3 list or approved custom list — PASS (ColumnPanel, FlowPanel, Card, Heading, Text, TextBox, DatePicker, RadioButton, RadioGroupPanel, Button, IconButton, RepeatingPanel, InteractiveCard, LinearProgressIndicator — all approved)
- [x] InteractiveCard uses only `elevated`, `filled`, or `outlined` — PASS (uses `elevated` on lines 59, 74)
- [x] No `SidesheetContent` component — PASS
- [x] No `Label` component — PASS
- [x] Card uses `Card` component — PASS
- [x] No `HtmlPanel` component — PASS

## G. Icon component

- [x] Icon components use `ico_` prefix — PASS (ico_success on line 169)
- [x] Icon is understood as `IconButton` used decoratively — PASS (type is `IconButton` on line 169)

## H. mi: prefix for icons

- [x] All icon annotations use `mi:` prefix — PASS (mi:check_circle on line 169)
- [x] No plain icon names without `mi:` prefix — PASS

## I. Writeback properties

- [x] Properties table includes writeback (W) for all 8 components — PASS (txt_search_service, dp_booking_date, rbg_meeting_type all have writeback = W)
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

- [x] Section 1 — Introduction present — PASS (line 31)
- [x] Section 2 — Wireframe Diagram present — PASS (line 41)
- [x] Section 3 — Properties Table present — PASS (line 183)
- [x] No sample data in wireframe — PASS
- [x] No behaviour text in wireframe diagram — PASS
- [x] Containers drawn as containers with names inset in border — PASS

---

## Result

**Status: PASS — no fixes needed**

---

*End of checklist*
