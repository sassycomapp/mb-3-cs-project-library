---
title: "flow-18-settings-scope"
doc-id: "mb-bfaca89d"
updated: "2026-11-17"
---
# Scope Card — flow-18-settings

**Intent.** How the Owner and Manager configure the business through one tabbed hub where every difference between clients is a row, never code. Load-bearing claim: tab visibility is the interface; the server functions enforce.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Settings | Owner / Manager | Owner, Manager |
| 15 tabs | Hub | role visibility |
| Business profile | Owner tabs | language, tax |
| Users | Owner tabs | invite, roles |
| Data | Owner tabs | history, export |
| Plan | Owner tabs | Owner only |
| Palette | Manager tabs | live preview |
| Invoicing | Manager tabs | terms, tax |
| Switches | Manager tabs | marketing, blog |
| Rows | Settings as data | never code |

**Main path.** Settings → 15 tabs → Business profile → Invoicing → Switches → Rows

## Deferred to cards

- The full 15-tab list (DESIGN.md)
- The four tabs built with their features (tickets 23, 29, 33, 40)

**Corridor plan.** User→hub crossings stay in lanes; hub→data crossings cross at the empty band at col 5.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
