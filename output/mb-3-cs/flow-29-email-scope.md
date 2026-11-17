---
title: "flow-29-email-scope"
doc-id: "mb-7de238f0"
updated: "2026-11-17"
---
# Scope Card — flow-29-email

**Intent.** How the team reads, replies and organises email on its own domain, and how SES carries every message in and out. Load-bearing claim: the trust boundary is Amazon SES with SNS-signed events — never Brevo — and each member sees only their own address and desks.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Team Inbox | Team | address+folder |
| Conversation | Team | contact linked |
| Reply | Team | AI draft |
| Addresses | Team | desks, signatures |
| Organise | Team | Trash 30 d |
| Unassigned | Team | Owner moves |
| Send | Outbound (SES) | client domain |
| Receive | Inbound (SES) | SNS signature |
| 90-day archive | Data | bodies to R2 |
| Suppress | Data | bounce/opt-out |

**Main path.** Team Inbox → Conversation → Reply → Send → Organise

## Deferred to cards

- The domain records' detail
- The unsubscribe endpoint's link rules

**Corridor plan.** Team↔SES crossings (compose→send, receive→inbox) cross at the empty bands at cols 2–3.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
