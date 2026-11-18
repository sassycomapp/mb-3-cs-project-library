---
title: "mb-3-cs-c21-app1-concept-check"
document: Step 21 concept check report — mb-3-cs c21 app1
state: 36 pairs checked; 12 contradictions found, corrected at source with David's decisions; verdict PASS
doc-id: "mb-785f444c"
updated: "2026-11-18"
---

# Step 21 — Concept Check Report (run app1, route Application)

Written at Step 21 from the concept documents of `c21-config.md`, all read in full.
Numbering note (David's request answered): the ticket-22 finding was labelled C13 in
the decision list presented in chat; no C12 was dropped — the list had skipped a
number. This report renumbers it **C12**, so the contradictions run C1–C12 with no
gap. The two observations are O1 and O2; both were also fixed, at David's direction.

## Documents checked

| Doc | File |
|---|---|
| A — Reviewed plan | `docs-local/design/app1/design-doc.md` |
| B — Test plan | `docs-local/design/app1/test-plan.md` |
| C — Access matrix | `docs-local/design/app1/access-matrix.yaml` |
| D — Threat model | `docs-local/design/app1/threat-model.md` |
| E — Glossary | `docs-local/GLOSSARY.md` |
| F — ADRs | `adr-local/` (5 ADRs, each read in full) |
| G — Authoritative schema | `docs-standard-local/authoritative-schema.md` |
| H — Tickets and flows | `pdlf-tickets/app1/` (52 tickets), `pdlf-outputs/c19/` ticket list, 28 flow diagrams |
| I — Design system | `docs-local/DESIGN.md` |

## The 36 pairs

| Pair | What was compared | Result |
|---|---|---|
| A×B | §22 rows 1–18 against test-plan §1–§18; lifecycles §19; plan-billing §20; test IDs | Consistent — §20 (F-37) is an addition, no conflict |
| A×C | §23.2 seams, §24 security decisions, high-value actions and step-up against the matrix's functions, scopes and step-up flags | Consistent; matrix's provisional-functions note is the Step-22 listing (see Access check) |
| A×D | §23.1 transaction list against B9's controls; push path, SAQ A, SES-not-Brevo, secrets, rate limits | Consistent |
| A×E | §12 open questions (settled at Step 16), trade-boundary terms, lifecycles | Consistent |
| A×F | The five local ADRs against the sections that carry them (§14.9, §23, §24, §6, §17) | Consistent |
| A×G | 71 tables in 11 groups; money in cents; lifecycles vs allowed values; vault names; trade links | Consistent (O1 wording fixed) |
| A×H | Feature coverage F-01–F-37; divergence inventory vs ticket assignments; flows cover all 46 UF; endpoint counts | **C2**, **C11** — corrected; otherwise consistent |
| A×I | Booking/invoice lifecycles, palettes, status badges, Settings tab count | **C1** — corrected; otherwise consistent |
| B×C | IN-02 generation; plan-summary Owner-only; push endpoint; portal scopes | Consistent |
| B×D | Every test ID the threat model names against the test plan | **C6, C7** — corrected; otherwise consistent |
| B×E | Mock-up invoice, referral kit, reserve, consent record | Consistent |
| B×F | Contacts/lead ADRs, M3 checks, conformance selection (DC-08) | Consistent |
| B×G | 71 tables; plan_summary display-text; retention rules | Consistent |
| B×H | Every test ID a ticket names against the test plan | Consistent — all found |
| B×I | Lifecycles, plan-billing preview, role enforcement | Consistent |
| C×D | Restricted projections; five roles; 403 pattern; residual token rotation | Consistent |
| C×E | Five roles; masked reads; suppression scopes | Consistent |
| C×F | ADRs against matrix entries | Consistent |
| C×G | writable_fields against schema columns; vault names; consents fields | Consistent |
| C×H | Matrix functions/forms against every ticket's named functions and forms | Consistent; absent items covered by the matrix's provisional note (see Access check) |
| C×I | Settings tabs and their RBAC against DESIGN.md's SettingsForm | Consistent (15 tabs) |
| D×E | Glossary's security-relevant terms against the threat model | Consistent |
| D×F | SES-not-Brevo decision; Sentinel gates; secrets lifecycle | Consistent |
| D×G | No tenant column; 71 tables; retention; ST-06 | Consistent |
| D×H | B1–B12 security criteria against each ticket's security criteria | Consistent |
| D×I | 30-minute idle; consent surface; cookie rules | Consistent |
| E×F | Glossary against the ADRs' terms | Consistent |
| E×G | Every glossary term's schema counterpart | **C4** — corrected; otherwise consistent |
| E×H | Terms in the tickets against the glossary | **C9** (term gaps) — corrected; otherwise consistent |
| E×I | Terms in DESIGN.md against the glossary | **C9** — corrected; otherwise consistent ("Terms & Conditions" is the glossary's term) |
| F×G | Each ADR against the schema it governs (71 tables; tables in/out) | **C10** — corrected; otherwise consistent |
| F×H | ADRs against the tickets' package and naming choices | Consistent after C10 (ticket 20 follows the scaffold's `server_customers/`) |
| F×I | Design ADRs against DESIGN.md's rules | Consistent |
| G×H | Every table every ticket names against the schema; plan_summary; endpoint counts | **C12** — corrected; otherwise consistent |
| G×I | DESIGN.md's design-related schema expectations against the schema | **C3** — corrected; otherwise consistent |
| H×I | Every ticket's forms, panes, badges and steps against DESIGN.md | **C5, C8** — corrected; otherwise consistent |

## The contradictions, their corrections, and the re-check

Each contradiction was shown to David with two statements and a recommended
correction; David approved all corrections as recommended, plus both observations,
on 2026-11-18. Each source document was corrected with his decision and the pair
re-checked.

| # | Pair | The two statements | Correction applied | Re-check |
|---|---|---|---|---|
| C1 | A×I | Design doc F-05: "SettingsForm **(14 tabs**…)"; DESIGN.md tab table, scaffold-spec, PRD FR-05 and ticket 18: **15 tabs** | Design doc F-05 row → "15 tabs" | Pass — 15 everywhere |
| C2 | A×H | Ticket 04: Brevo module "deleted at **ticket 28**"; ticket 29 and the ticket list: "C1 → **29**". Ticket 28 is time tracking | Ticket 04 → "ticket 29" | Pass |
| C3 | G×I | DESIGN.md PaymentGatewayForm named schema field **`paypal_enabled`** and omitted `paypal_client_id`, `paypal_test_mode`; the schema's `payment_config` has none of the former and holds the latter two | DESIGN.md's field list aligned to the schema (9 named fields, all existing) | Pass — every named field exists in `payment_config` |
| C4 | E×G | Glossary Payment option: a service or order offers paying "**or from a customer package**"; the schema's `services.payment_option` and `orders.payment_option` have no `package` value (only `bookings.payment_option` does) | Glossary reworded: services and orders offer full, deposit, pay on the day, invoiced; the package is paid from at booking | Pass |
| C5 | H×I | Ticket 51: "the FIDO2 enrolment **at step 1**"; flow-51's step 1 panel: "Password / FIDO2 enrol"; DESIGN.md step 1 is "**Welcome and orientation**" (set-password and 2FA happen at first sign-in, before the wizard — as flow-map-onboarding-access shows) | Ticket 51's criterion reworded: the Owner's FIDO2 factor is enrolled at first sign-in (ticket 16); onboarding step 1 is Welcome. **Deferred item for Step 22:** the flow-51 card's step-1 label is corrected when the flows are next touched | Pass |
| C6 | B×D | Design doc AC-S15 and threat model B2 name test **`SC-08`**; the test plan never named it (it exists in the catalogue: the three planes are separate) | Test plan §21 "sessions and passwords" row gains SC-08 | Pass |
| C7 | B×D | Threat model B11 names test **`ST-13`** (static analysis and dependencies clean); the test plan never named it | Test plan §21 "testing discipline" row gains ST-13 beside SC-18 | Pass |
| C8 | H×I | Ticket 34's HomepageEditorForm edited "**features (up to 5)**"; DESIGN.md's shared section inventory (8 sections) has no Features element | Ticket 34's editor list aligned to the shared section inventory (hero, intro, services showcase, video, testimonials, trust strip, CTA); no Features element, per David | Pass |
| C9 | E×H, E×I | Terms used in the tickets and DESIGN.md with no glossary entry: **"morning briefing"** (tickets 39/44, DESIGN.md AccountForm, schema `users.preferences.morning_briefing`), **"system currency"** and **"display currency"** (tickets 21/22, DESIGN.md tabs, schema), **"Unassigned"** (ticket 29; the schema's own note for an empty `address_id`) | Four entries added to GLOSSARY.md: System currency, Display currency, Unassigned, Morning briefing | Pass — the Terms check now passes in full |
| C10 | F×G | ADR `crm-package-replaces-marketing`: "The server-side package is `server_marketing/`"; scaffold-spec also defines **`server_customers/`** (contact, company, segment, import services) — and ticket 20 follows the scaffold | One clarifying sentence added to the ADR: the contacts services live in the scaffold's separate `server_customers/` package; `server_marketing/` holds campaign, broadcast, lead-capture and landing-page services. The ADR's naming rule stands unchanged | Pass |
| C11 | A×H | Design doc F-34 row named **six** management endpoints; §23.7, ticket 47 and the access matrix name **seven** (`plan-summary`, added at Step 16) | F-34 row's endpoint list gains `plan-summary` | Pass |
| C12 | G×H | Ticket 22's tables line named "`business_profile` (`system_currency`, **`display currency` fields**)"; the schema's `business_profile` has no display-currency field — display currency is `services.display_currency` only | Ticket 22's line corrected to `business_profile.system_currency`, `services.display_currency`, `config` | Pass |

**Observations (fixed at David's direction, no contradiction):**

| # | What | Fix |
|---|---|---|
| O1 | Design doc §14.9 said the common/trade marking is "derived mechanically … applied to the schema's typed links", yet three trade tables (`availability_rules`, `availability_exceptions`, `booking_metadata_schemas`) hold no link into a trade table and are trade by domain | §14.9 reworded: trade-domain tables are trade by domain; the hard rule (no common table links a trade table) is stated unchanged |
| O2 | The ticket list's divergence line omitted the `blog_service` rename assignment (ticket 36 itself carries it) and held the editing artifact "20's neighbours n/a" | The line now reads "… → 16 (auth_service), 36 (blog_service), 21 (catalogue_service), …"; the artifact is removed |

## The fixed checks

| Check | Result | Evidence |
|---|---|---|
| Tables | PASS | Every table every ticket names exists in `authoritative-schema.md` (verified table by table across all 52 tickets) |
| Access | PASS | Every server function and form a ticket names is in `access-matrix.yaml` or is listed for Step 22 to add by the matrix's own provisional note ("Function names are provisional: each entry is confirmed or renamed when its feature's server code is written, and further functions are added then"). Absent from the matrix and thereby listed for Step 22: the functions `get_layout_context`, `get_public_business_profile`, `save_page_content`, `generate_share_captions`, `create_paypal_order`, `capture_paypal_order`, `end_user_sessions`, the `vault_totp_service` functions, the settings currency functions, `company_service`/`segment_service`, `catalogue_service`/`resource_service`/`group_session_service`, `order_service`, `package_service`/`package_tasks`, `time_entry_service`, `inbox_service`, the landing/lead-capture/automation/blog/review/quote-pdf/quote-tasks/voice/briefing/accounts-report/accounting-export/backup/history/audit-archive functions, `demo_service`, `onboarding_service`, `domain_service`, `help_service`, `usage_service`; and the forms the layouts, the eleven shared components, the Settings local tabs, the local templates, `crm.ContactImportForm`, `inbox.ComposeForm` and the analytics sub-forms — every one of them embedded in, or hosted by, a form the matrix already governs, except the analytics sub-forms, which Step 22 adds |
| Terms | PASS | After C9, every term the tickets and DESIGN.md use is the glossary's term |
| Tests | PASS | Every test ID every ticket names exists in the test plan; after C6 and C7 the threat model's and design doc's named IDs do too |
| Decisions | PASS | All five local ADRs are followed by the documents they govern (C10's clarification added); the design doc's §14.9 carries the judgement-call ADR |
| Receipts | PASS | `ts_report.py {folder} --prefix mb-3-cs-{step}-app1- --check {receipt}` passes for c17, c18, c19 and c20 (c20 not skipped — Step 20 is complete) |

## Verdict

All 36 pairs checked; every contradiction found was corrected at its source with
David's decision and the pair re-checked clean; every fixed check passes. **The
concept has no contradictions. VERDICT: PASS — the concept is ready to lock at the
gate `concept-locked`.**

One deferred item rides forward to Step 22 (recorded in the ledger row's notes):
correct the flow-51 card's step-1 label ("Password / FIDO2 enrol") when the flows
are next touched — onboarding step 1 is Welcome and orientation.

*End of report*
