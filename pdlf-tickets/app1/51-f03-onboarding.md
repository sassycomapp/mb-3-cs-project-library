---
title: "51 — F-03 Registration, onboarding and setup"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-86b0678c"
updated: "2026-11-18"
---

## Description

**Feature:** F-03 Registration, onboarding and setup (UF-01, UF-02; architecture reference §5, §12 F-03; `spec-client-activation-runbook`; `spec-onboarding-implementation-plan` — the project's 10-step wizard governs; FR-03; SR-08, SR-08a, SR-08b, SR-09). Built last of the features because its ten steps use each of the parts before it.
**Depends on:** 17 (Vault — the gateway step), 18 (Settings links), 19 (design system), 20 (contact import), 22 (F-16 currency step), 23 (payments step), 29 (email addresses and domain), 33 (public website), 40 (legal acknowledgement), 42 (help language), 43 (the AI helper's website text).

### Forms

- `onboarding.OnboardingForm` — the resumable 10-step wizard with a progress indicator and a short help video on every step; each step saves when completed; bidirectional navigation ("Return to Onboarding" on the Settings form while in progress); resume at the next unfinished step
- Local steps: `onboarding.OnboardingForm.DomainStep` (DNS records and live status), `onboarding.OnboardingForm.ImportStep` (CSV upload and column mapping)

### Server functions

- `server_settings/onboarding_service` — the wizard's step state and completion
- `server_settings/domain_service` — the website and email records' live checks (App 5 writes them in the client's Cloudflare DNS; the wizard checks until every one is live; the email-switch confirmation where the domain already receives email elsewhere)
- The legal acknowledgement recording (SR-09): read the critical excerpts, tick, confirm — recorded once with its timestamp in the instance and in App 5, cannot be reversed (`business_profile.legal_ack_accepted_at`, `data_governance_ack_accepted_at`)
- The contact import step calls `contact_import_export` (ticket 20); the email step creates the Owner's personal address and the default sender (ticket 29's address service); the gateway step writes secret keys into the Vault (ticket 17)

### Tables

- `business_profile` (onboarding fields, legal acknowledgements), `contacts` (import), `email_addresses`, `payment_config`, `vault` (via `vault_service`), `config`

### Acceptance tests

- `BR-01`; `SR-08`, `SR-08a`, `SR-08b`, `SR-09`; UF-02; the onboarding rules (resumability ADRs: `adr-onboarding-resumability`, `adr-onboarding-finality`, `adr-onboarding-vs-settings-boundary`, `adr-onboarding-data-schema-alignment`)
- UF-01's registration and provisioning steps 5–12 are App 5's side (its own run); the instance side begins at the Owner's first sign-in

### Security criteria

- B1 — Owner only (the wizard's scope in the access matrix)
- B10 — the gateway step writes secret keys only into the Vault, masked, never round-tripped (AC-S4)
- AC-S5 pattern — the Owner's FIDO2 second factor is enrolled at first sign-in, before the wizard (ticket 16); onboarding step 1 is Welcome and orientation

## Comments
