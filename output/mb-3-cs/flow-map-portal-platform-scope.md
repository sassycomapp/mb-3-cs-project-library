---
title: "flow-map-portal-platform-scope"
doc-id: "mb-353fac1a"
updated: "2026-11-17"
---
# Scope Card — flow-map-portal-platform

**Intent.** How customers use their portal, how teams use AI, accounts, data and help, and how Mybizz operates the platform: alerts, releases, demos and plan billing. Load-bearing claim: a customer sees only their own records, and Mybizz reaches an instance only through its Bearer-token endpoints.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Customer sign-in | Customer | 2nd factor |
| Portal | Customer | 7 views |
| Quote accept | Customer | one invoice |
| AI + WhatsUp | Team | through App 5 |
| Expenses + accounts | Team | Wave/Xero/QuickBooks |
| R2 data | Client storage | history+restore |
| Help | Team | to App 5 |
| At-risk alerts | Mybizz (App 5) | usage + inactivity |
| Release | Mybizz (App 5) | reaches every client |
| Demo apps | System | nightly reset |
| Plan + billing | Customer | mock-up invoice |
| Referral kit | Customer | share + credits |

**Main path.** Customer sign-in → Portal → Quote accept → Help → At-risk alerts

## Deferred to cards

- Each platform flow's detail (their feature tickets' drawings)
- The exit handover's steps
- UF-37's schema-migration discipline

**Corridor plan.** Customer→team and team→Mybizz crossings cross at the empty bands at cols 1–2 and 3–4.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
