---
title: "flow-24-bookings-scope"
doc-id: "mb-de814345"
updated: "2026-11-17"
---
# Scope Card — flow-24-bookings

**Intent.** The product's core: how a customer books through three server calls, how the team runs the calendar, and how contention, holds and late payments are decided. Load-bearing claim: confirm_booking re-checks slot, resource and places inside the same transaction that confirms — and a late payment either reinstates inside that discipline or refunds in full.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Booking flow | Customer | service → provider |
| Slots | Customer | call 2 |
| Meeting + intake | Customer | no server call |
| confirm_booking | Server | one transaction |
| Confirmed | Server | invoice + reminders |
| Payment hold | Server | 15 minutes |
| D5 late webhook | Server | reinstate or refund |
| Calendar | Team | open |
| Complete / no-show | Team | invoice |
| Recurring offer | Customer | weekly |

**Main path.** Booking flow → Slots → Meeting + intake → confirm_booking → Confirmed

## Deferred to cards

- The 3-second performance budget's page detail
- Availability rule maths
- Travel-buffer computation

**Corridor plan.** The customer→server crossings (intake→confirm_booking) cross at the empty band at cols 2–3; the D5 shadow stays in the server lane at cols 4–5.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
