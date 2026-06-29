# Checklist — LoginForm

**Wireframe:** `wireframes/auth/LoginForm/wireframe-auth-LoginForm.html`
**Date:** 2026-06-28
**Auditor:** Build agent

---

## A. ADR-43 — Role assignment (reversed)

- [x] Properties table does NOT list `role` as a Designer property — PASS (lines 269-277: only appearance, writeback, hide_text listed)
- [x] Component annotations do not say "set role in Designer" — PASS
- [x] If role is mentioned in annotations, it is noted as set in code — PASS (no role annotations found)

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
- [x] Layout wireframes use `NavigationRailLayout` or `NavigationDrawerLayout` — N/A (not a layout wireframe)

## E. DataRowPanel

- [x] DataRowPanel uses `drp_` prefix — N/A (no DataRowPanel in this wireframe)
- [x] DataRowPanel is used for DataGrid row templates — N/A
- [x] DataRowPanel is listed in the component annotations — N/A

## F. M3 component standard

- [x] Every component is in the approved M3 list or approved custom list — PASS (TextBox, CheckBox, Button, Link — all approved)
- [x] InteractiveCard uses only `elevated`, `filled`, or `outlined` — N/A (no InteractiveCard)
- [x] No `SidesheetContent` component — PASS
- [x] No `Label` component — PASS
- [x] Card uses `Card` component (not `ColumnPanel` with role class) — N/A (no Card in this wireframe)
- [x] No `HtmlPanel` component — PASS

## G. Icon component

- [x] Icon components use `ico_` prefix — N/A (no Icon components)
- [x] Icon is understood as `IconButton` used decoratively — N/A
- [x] If wireframe shows an icon-only button with click handler, it should be `IconButton` with `btn_` prefix — N/A

## H. mi: prefix for icons

- [x] All icon annotations use `mi:` prefix — N/A (no icon annotations)
- [x] No plain icon names without `mi:` prefix — PASS

## I. Writeback properties

- [x] Properties table includes writeback (W) for all 8 components — PASS (txt_email and txt_password both have writeback = W)
- [x] No writeback listed for non-writeback components — PASS

## J. Max-width values

- [x] Max-width annotations match final values — N/A (form type is Auth, max-width 400px per Standard)

## K. Card radius

- [x] Card annotations reference 8px radius — N/A (no Card in this wireframe)

## L. Tag/badge composition

- [x] Tag/badge components use `m3.Button` or `m3.Card` composition — N/A (no tags/badges)
- [x] No `Label` component used for tags or badges — PASS

## M. Dark mode

- [x] No references to dark mode — PASS

## N. Carousel viewport

- [x] Carousel viewport uses ColumnPanel with role-driven CSS — N/A (no carousel)

## O. Section structure (ADR-27)

- [x] Section 1 — Introduction present — PASS (line 17)
- [x] Section 2 — Wireframe Diagram present — PASS (line 42)
- [x] Section 3 — Properties Table present — PASS (line 251)
- [x] No sample data in wireframe — PASS
- [x] No behaviour text in wireframe diagram — PASS
- [x] Containers drawn as containers with names inset in border — PASS

---

## Result

**Status: PASS — no fixes needed**

All 15 checklist sections pass. This wireframe is compliant with all design decisions and ADRs.

---

*End of checklist*
