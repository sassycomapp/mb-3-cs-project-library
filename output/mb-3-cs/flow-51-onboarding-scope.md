---
title: "flow-51-onboarding-scope"
doc-id: "mb-1a56ff10"
updated: "2026-11-17"
---
# Scope Card — flow-51-onboarding

**Intent.** The Owner's resumable ten-step wizard from first sign-in to the dashboard. Load-bearing claim: each step saves when completed, the wizard resumes at the next unfinished step, and the legal acknowledgement is recorded once and cannot be reversed.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| 1 Password | Owner — setup | FIDO2 enrol |
| 3 Profile | Owner — setup | + AI first text |
| 5 Currency | Owner — setup | locked later |
| 7 Gateway | Owner — setup | keys to Vault |
| 9 Import | Owner — setup | CSV, skippable |
| 2 Language | Owner — choices | five languages |
| 4 Palette | Owner — choices | live preview |
| 6 Legal ack | Owner — choices | recorded once |
| 8 Domain | Owner — choices | records live |
| 10 Complete | Owner — choices | dashboard |
| Resume | Server | next unfinished |
| Return link | Settings | while in progress |

**Main path.** 1 Password → 2 Language → 3 Profile → 4 Palette → 5 Currency → 6 Legal ack → 7 Gateway → 8 Domain → 9 Import → 10 Complete

## Deferred to cards

- App 5's provisioning steps (UF-01's system side)
- Each step's help video
- The assisted-setup request

**Corridor plan.** The owner→server crossings (gateway→domain) cross at the empty band at col 4; the links lane carries the Settings handover.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
