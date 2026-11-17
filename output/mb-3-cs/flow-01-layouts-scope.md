---
title: "flow-01-layouts-scope"
doc-id: "mb-190d140e"
updated: "2026-11-17"
---
# Scope Card — flow-01-layouts

**Intent.** How any user lands in the application and reaches every page through its layout: role routing, the menus, and the three always-available controls. Load-bearing claim: the interface hides what a role cannot use and is never the enforcement.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Sign-in | Any user | password+2FA |
| Route by role | Routing | before init |
| AdminLayout | Team shell | rail+menu |
| Always there | Team shell | 3 controls |
| Sample banner | Team shell | until cleared |
| CustomerLayout | Customer shell | drawer |
| PublicLayout | Public shells | header |
| BlankLayout | Public shells | no chrome |
| ErrorLayout | Public shells | 404/403/500 |
| Page form | Pages | on its layout |

**Main path.** Sign-in → Route by role → AdminLayout → Always there → Page form

## Deferred to cards

- Menu item list (architecture reference §9)
- Palette application detail

**Corridor plan.** Route→menu crossings stay within the shell lane; menu→page crosses at the empty band at col 4.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
