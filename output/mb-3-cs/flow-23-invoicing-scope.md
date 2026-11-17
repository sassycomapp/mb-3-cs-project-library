---
title: "flow-23-invoicing-scope"
doc-id: "mb-f6e0ab6c"
updated: "2026-11-17"
---
# Scope Card — flow-23-invoicing

**Intent.** How an invoice is created, issued, paid and refunded, and how the gateways' word reaches the books. Load-bearing claim: every amount is the server's; a webhook is verified, idempotent and applied once; card data never touches the instance.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| New invoice | Owner / Manager | lines: manual |
| Tax at issue | Server | rate + label fixed |
| Send | Owner / Manager | PDF + payment link |
| Payment gateways | Owner / Manager | keys to the Vault |
| Hosted payment | Money | card data never here |
| Webhook | Money | verified once |
| Applied once | Server | booking or invoice paid |
| Record payment | Owner / Manager | cash / transfer |
| Refund | Owner / Manager | step-up |
| Overdue | Server | nightly + reminder |

**Main path.** New invoice → Tax at issue → Send → Hosted payment → Webhook → Applied once

## Deferred to cards

- The invoice status lifecycle's full table
- Sequence-key transaction detail

**Corridor plan.** finance→money crossings (send→linkpay, linkpay→webhook) cross at the empty bands at cols 2–4.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
