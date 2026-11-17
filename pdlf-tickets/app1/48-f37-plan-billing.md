---
title: "48 — F-37 Plan, billing and referrals"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-308b26aa"
updated: "2026-11-17"
---

## Description

**Feature:** F-37 Plan, billing and referrals (UF-45, UF-46; ADR `tiers-model`; `spec-client-billing` §4.1; `spec-api-specification` `/management/plan-summary`; SR-65, SR-66; FR-40). Common feature (Mybizz's own billing of the client).
**Depends on:** 18 (the PlanBillingTab host), 47 (the plan-summary management endpoint writes the row).

### Forms

- None new. `settings.SettingsForm`'s `PlanBillingTab` (built as a placeholder at ticket 18) is completed here: the plan and next renewal, the mock-up invoice, the referral kit.

### Server functions

- `server_settings/plan_service` — `get_plan_summary`, `update_referral_kit` (Owner only); the page renders the stored text and never reformats it (ADR `plan-summary-display-text`); the referral kit's switches write `business_profile.referral_banner_in_emails`, `referral_badge_on`, `referral_popup_on`

### Tables

- `plan_summary`, `business_profile` (`referral_*`)

### Acceptance tests

- Test-plan §20 (the eight steps, Owner-only refusals, the data checks); `IN-02` (Owner only), `ST-02`; `SR-65`, `SR-66`; UF-45, UF-46; `RL-23` at App 5 and ledger level
- `plan_summary` matches Paddle's next-transaction preview; its amounts are display text in the subscription's currency — not money records of the client's business; no whole-cents rule applies to them

### Security criteria

- B5 — the row is written only through the Bearer-token endpoint (ticket 47); a forged push is refused by the same 403
- B1 — the tab and both functions are Owner only; a refused call writes nothing

## Comments
