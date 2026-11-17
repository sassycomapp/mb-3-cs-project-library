---
title: "flow-26-orders-scope"
doc-id: "mb-1bdba196"
updated: "2026-11-17"
---
# Scope Card — flow-26-orders

**Intent.** How a customer orders delivered work with a brief, and how the team takes the order to delivered. Load-bearing claim: the order submits once and is never deleted — it moves through its status.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Order flow | Customer | brief + attachments |
| Payment | Customer | full |
| Ordered | Server | contact matched |
| In progress | Team | customer emailed |
| Delivered | Team | files + message |
| Balance invoiced | Server | where owed |

**Main path.** Order flow → Payment → Ordered → In progress → Delivered

## Deferred to cards

- Brief attachment size rules
- The delivery email's template

**Corridor plan.** Customer→server crossings at cols 1–2; server↔team at cols 2–4, through empty bands.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
