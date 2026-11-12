---
title: "adr-plan-summary-display-text"
status: Accepted
date: 2026-11-12
doc-id: "mb-f22fd6c4"
updated: "2026-11-12"
---
# The plan summary's amounts are display text

Every amount in a client instance is stored as whole cents of the system currency, because Anvil's number column is floating-point and decimal amounts would round wrongly. The plan summary is the one exception: its amounts are display text in the subscription's currency, exactly as Paddle states them, written only by App 5 through the plan-summary management endpoint. We decided this so the Owner sees precisely what Paddle will charge — the mock-up invoice must match Paddle's next-transaction preview — and because the plan summary is not a money record of the client's business: the money rules (whole cents, the system currency) govern the trade tables, not this display. Reversing it would mean a schema change and a writer change in App 5 and every client instance.

Marked **common**.

## Considered options

- **Store Paddle's display text verbatim** (chosen): the preview the Owner sees is byte-for-byte Paddle's own.
- **Store whole cents and re-render** — rejected: the instance would restate Paddle's figures and could drift from them, and the plan summary is not the client's money record.

## Consequences

- The money rules of the schema do not apply to the plan summary's `lines` and `total`; every other amount in the instance stays in whole cents.
- The Plan and billing page renders the stored text; it never reformats it.

*End of `adr-plan-summary-display-text` ADR*
