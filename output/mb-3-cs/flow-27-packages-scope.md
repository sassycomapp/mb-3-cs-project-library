---
title: "flow-27-packages-scope"
doc-id: "mb-dfa1f407"
updated: "2026-11-17"
---
# Scope Card — flow-27-packages

**Intent.** How a customer buys a bundle of sessions and spends it across bookings. Load-bearing claim: draw-down and return happen inside the same transaction as the booking or cancellation they belong to.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Package editor | Customer | sessions + price |
| Buy | Customer | balance appears |
| Use a session | Customer | at booking |
| Draw-down | Server | same transaction |
| Return | Server | inside the window |
| Expiry | Server | notice, then lapse |

**Main path.** Package editor → Buy → Use a session → Draw-down → Return

## Deferred to cards

- The expiry-notice lead time
- Package display in the portal (ticket 26's drawing)

**Corridor plan.** Customer→server crossings at cols 1–3 through the empty band; the server lane carries the money rules.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
