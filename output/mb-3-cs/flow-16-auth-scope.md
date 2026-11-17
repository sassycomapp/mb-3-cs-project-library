---
title: "flow-16-auth-scope"
doc-id: "mb-6661c6fd"
updated: "2026-11-17"
---
# Scope Card — flow-16-auth

**Intent.** How every account holder signs in with a second factor, how team members are invited and set passwords, and how sessions and resets behave. Load-bearing claim: the Owner's factor is FIDO2 only, and high-value actions need a factor given in the last five minutes.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Sign-in | People | email + password |
| Second factor | Auth server | app or security key |
| Route by role | Auth server | Route by role |
| Invite | People | Owner or Manager |
| Set password | People | emailed link |
| Password reset | People | 10-minute link |
| 30-minute idle | Guards | re-sign-in |
| Owner ends sessions | Guards | any user |
| Step-up | Guards | factor < 5 min old |

**Main path.** Sign-in → Second factor → Route by role

## Deferred to cards

- Users service settings (Anvil editor)
- The role table's scopes (the access matrix)

**Corridor plan.** The people→auth crossing (signin→twofa) crosses at the empty band at col 0–1; guard crossings at cols 4–5.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
