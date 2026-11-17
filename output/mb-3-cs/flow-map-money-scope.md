---
title: "flow-map-money-scope"
doc-id: "mb-10b66e19"
updated: "2026-11-17"
---
# Scope Card — flow-map-money

**Intent.** How recorded hours become invoice lines, how invoices are issued and paid, and how webhooks carry the gateway's word. Load-bearing claim: every amount is the server's; a webhook is verified, idempotent, and applied exactly once.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Time entries | Staff | timer or hand |
| Billable | Staff | uninvoiced only |
| Create invoice | Finance | from entries |
| New invoice | Finance | manual lines |
| Send | Finance | PDF + link |
| Webhook | Money path | verified once |
| Applied once | Server | background |
| Record pay | Finance | cash/transfer |
| Refund | Finance | step-up |
| Overdue | Server | nightly |

**Main path.** Time entries → Billable → Create invoice → Send → Webhook → Applied once

## Deferred to cards

- Gateway API detail
- R2 key layout
- The D5 reinstatement (ticket 13's drawing)

**Corridor plan.** The staff→money crossing (send→webhook) crosses at the empty band at cols 3–4; webhook→applied continues forward.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
