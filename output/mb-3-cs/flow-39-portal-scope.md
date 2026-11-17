---
title: "flow-39-portal-scope"
doc-id: "mb-2d981b99"
updated: "2026-11-17"
---
# Scope Card — flow-39-portal

**Intent.** How a customer signs in and reaches every own record: bookings, packages, quotes, invoices, reviews and account. Load-bearing claim: every view derives the customer from the signed-in user — never from a client-supplied id.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Customer sign-in | Customer | password |
| My Dashboard | Customer | upcoming |
| My Bookings | Views | reschedule |
| My Packages | Views | balance |
| My Quotes | Views | accept |
| My Invoices | Views | PDF, pay |
| My Reviews | More views | My Reviews |
| Account | More views | profile, briefing |
| Own records only | Server | derived from the user |
| Pay online | Customer | hosted gateway |

**Main path.** Customer sign-in → My Dashboard → My Bookings → My Quotes → Own records only

## Deferred to cards

- The portal menu's labels
- The morning-briefing switch's detail (ticket 31's drawing)

**Corridor plan.** Customer→server crossings stay at the views lane boundary; the own-records node sits at col 5 in the server lane — crossings cross empty bands at cols 1–2 and 4–5.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
