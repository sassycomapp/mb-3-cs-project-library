---
title: "flow-map-onboarding-access-scope"
doc-id: "mb-a15a8cdf"
updated: "2026-11-17"
---
# Scope Card — flow-map-onboarding-access

**Intent.** How a new client registers, is provisioned by App 5, and reaches the Owner's working instance; how team members and the Vault are reached. Load-bearing claim: every account reaches the instance through sign-in with a second factor, and no password is ever emailed.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Register | New client | business |
| Confirm email | New client | Confirm email |
| Cloudflare setup | New client | R2 |
| Plan + Paddle | New client | first payment |
| Provision | Mybizz (App 5) | instance |
| Welcome email | Mybizz (App 5) | no password sent |
| Set password + 2FA | Owner | FIDO2 for Owner |
| Onboarding wizard | Owner | 10 steps, resumable |
| Staff invite | Team | role + addresses |
| Sign-in + 2FA | Team | every role |
| Vault open | Vault (Owner) | TOTP step-up |
| Settings handover | Settings (Owner) | ongoing config |

**Main path.** Register → Confirm email → Cloudflare setup → Plan + Paddle → Provision → Welcome email → Set password + 2FA → Onboarding wizard

## Deferred to cards

- App 5's provisioning internals (the runbook's steps 5–11)
- The ten wizard steps' detail (ticket 28's own drawing)
- Suppression and domain records

**Corridor plan.** App 5 crossings (paddle→provision, provision→welcome) cross at the empty band between the visitor and Mybizz lanes at cols 3–4.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
