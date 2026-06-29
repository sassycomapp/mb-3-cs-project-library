# Checklist — EmailSetupForm

**Wireframe:** `wireframes/settings/EmailSetupForm/wireframe-settings-EmailSetupForm.html`
**Date:** 2026-06-28
**Auditor:** Build agent

---

## A. ADR-43 — Role assignment (reversed)

- [x] Properties table does NOT list `role` as a Designer property — PASS (lines 121-122: only writeback and placeholder listed)
- [x] Component annotations do not say "set role in Designer" — PASS

## B. ADR-44 — Responsive behaviour (wrap_on)

- [x] No breakpoint tables or pixel-based responsive rules — PASS

## C. ADR-45 — HTMLTemplate banned

- [x] No `HtmlTemplate` component anywhere in the wireframe — PASS

## D. ADR-37 superseded

- [x] No references to "custom layout approach" — PASS

## E. DataRowPanel

- [x] DataRowPanel uses `drp_` prefix — N/A

## F. M3 component standard

- [x] Every component is in the approved M3 list or approved custom list — PASS (ColumnPanel, FlowPanel, Card, Heading, Text, TextBox, Button — all approved)
- [x] InteractiveCard uses only `elevated`, `filled`, or `outlined` — N/A
- [x] No `SidesheetContent` component — PASS
- [x] No `Label` component — PASS (replaced with Text in earlier fix)
- [x] Card uses `Card` component — PASS
- [x] No `HtmlPanel` component — PASS

## G. Icon component

- [x] Icon components use `ico_` prefix — N/A

## H. mi: prefix for icons

- [x] All icon annotations use `mi:` prefix — N/A

## I. Writeback properties

- [x] Properties table includes writeback (W) for all 8 components — PASS (txt_from_email and txt_from_name both have writeback = W)
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

- [x] Section 1 — Introduction present — PASS (line 49)
- [x] Section 2 — Wireframe Diagram present — PASS (line 51)
- [x] Section 3 — Properties Table present — PASS (line 116)
- [x] No sample data in wireframe — PASS
- [x] No behaviour text in wireframe diagram — PASS
- [x] Containers drawn as containers with names inset in border — PASS

---

## Result

**Status: PASS — no fixes needed**

---

*End of checklist*
