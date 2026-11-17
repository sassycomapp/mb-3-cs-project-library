---
title: "flow-21-services-scope"
doc-id: "mb-ad92b38c"
updated: "2026-11-17"
---
# Scope Card — flow-21-services

**Intent.** How a client's team publishes what it sells: bookable time or delivered work, with providers, resources, group sessions and intake forms. Load-bearing claim: the pricing model decides the whole booking shape — duration takes slots, unit takes orders.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Services list | Owner / Manager | Services list |
| Service editor | Owner / Manager | pricing model first |
| Duration | Server | slots + providers |
| Unit | Server | ordered work |
| Resources | Owner / Manager | rooms + equipment |
| Group sessions | Owner / Manager | places + limit |
| Intake schema | Owner / Manager | client-defined questions |
| Images + video | Owner / Manager | public presentation |
| is_active | Data | only active are public |
| Categories | Data | filing |

**Main path.** Services list → Service editor → Duration → Intake schema → is_active

## Deferred to cards

- The eight palettes' fit to services imagery
- Deposit and payment-terms defaults

**Corridor plan.** Team→server crossings at cols 1–2; server→data crossings at cols 4–5, both through empty bands.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
