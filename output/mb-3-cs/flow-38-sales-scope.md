---
title: "flow-38-sales-scope"
doc-id: "mb-6f16bdfe"
updated: "2026-11-17"
---
# Scope Card — flow-38-sales

**Intent.** How a deal moves through its pipeline and how a quote becomes exactly one invoice on acceptance. Load-bearing claim: acceptance creates one invoice however often it is asked, and Staff see and move only their own deals.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Deals board | Team | stages; Staff own |
| New deal | Team | lead stage |
| Move / won / lost | Team | reason on loss |
| Quote editor | Team | lines |
| Send | Server | PDF + accept link |
| /quote/:token | Customer | accept or decline |
| One invoice | Server | deal → won |
| Declined | Server | deal stays open |
| Expiry | Server | nightly |

**Main path.** Deals board → New deal → Move / won / lost → Send → /quote/:token

## Deferred to cards

- The stage list's badges (DESIGN.md)
- Company grouping's detail (ticket 7's drawing)

**Corridor plan.** Team→server crossings at cols 2–3; server→customer at cols 3–4, through empty bands.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
