---
title: "flow-37-reviews-scope"
doc-id: "mb-74883a99"
updated: "2026-11-17"
---
# Scope Card — flow-37-reviews

**Intent.** How a completed appointment asks for a review, how the customer gives it, and how the Owner approves it public. Load-bearing claim: only an approved review can ever show publicly.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Completed | Server | request sent + task |
| Request email | Customer | token link |
| /review/:token | Customer | stars + comment |
| Awaiting | Server | not public |
| Approve / decline | Owner | Approve / decline |
| Public | Server | approved only |

**Main path.** Completed → Request email → /review/:token → Awaiting → Approve / decline

## Deferred to cards

- Testimonials typed in by the business
- The portal's My Reviews view (ticket 26's drawing)

**Corridor plan.** Server→customer and customer→server crossings at cols 0–2; the owner lane's decision at col 4 — all through empty bands.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
