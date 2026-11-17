---
title: "flow-map-booking-trade-scope"
doc-id: "mb-68db03b3"
updated: "2026-11-17"
---
# Scope Card — flow-map-booking-trade

**Intent.** How a customer books (all four meeting types), orders delivered work, and buys and uses packages; how the team runs the day. Load-bearing claim: the server confirms every booking inside one transaction — exactly one customer wins a contested slot.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| /services | Customer | categories |
| Service detail | Customer | provider + slots |
| Booking flow | Customer | 7 steps, one submit |
| Payment options | Customer | full |
| Confirmed | Server | invoice |
| Group session | Customer | places left |
| Order flow | Customer | brief + files |
| Manage order | Team | in progress → delivered |
| Calendar | Team | month / week / day |
| confirm_booking | Server | one transaction |

**Main path.** /services → Service detail → Booking flow → Payment options → confirm_booking → Confirmed

## Deferred to cards

- Availability maths (the pure module)
- The D5 late-payment shadow (ticket 13's drawing)
- Travel-time detail for at-my-location

**Corridor plan.** The customer→server crossings (payopts→confirm_booking, confirm_booking→confirmed) cross at the empty band at cols 3–4.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
