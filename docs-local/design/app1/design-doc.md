---
title: "design-doc"
doc-id: "mb-7868ef5e"
updated: "2026-11-07"
---

# Mybizz CS — Design Doc (run app1)

Route B, existing application. Entry 03 (`new-app-with-context`), run `app1`, App 1. The project's documents are `mb-3-cs-project-library`; the global corpus is `project-library-global`. Every statement of those folders is a requirement. This design doc names them and never restates them in place of carrying them.

GStack `/office-hours` ran on the first concept at David's direction (to prove PDLF works with the skill) and was approved with three revisions on 2026-11-03; its doc is `~/.gstack/projects/sassycomapp-mb-3-cs-project-library/dev-p-main-design-20261103-141359.md` (repo copy `docs/designs/mybizz-cs-complete-business-platform.md`). Its premises, chosen approach and success criteria match this document; the additions it contributed are marked below. Its Assignment is the founder's business action, outside the build, and is not carried as a requirement.

---

## 1. Problem Statement

In the users' terms (concept §2): a consulting or service business that wants to operate professionally online must buy and wire together five to seven separate tools — a website and hosting, a booking system, a payment gateway, a CRM, an email marketing platform, an invoicing tool — each with its own subscription, learning curve and support. The tools do not talk to each other, so the owner re-types data between them, holds client records in three or four systems at once, and still has no reliable view of the business. On marketplace platforms the business's customers are effectively the platform's customers, and commissions reach 20–23.5% on new customers. The problem has five compounding parts — money, integration, technical burden, data, ownership (concept §2) — and the owner becomes a part-time IT administrator instead of doing the work they are good at.

## 2. Demand Evidence

- The founder lived the problem (concept §3): years of attempting to establish an online business, every provider selling a piece and nobody the whole.
- The measurable stack: a team of six pays $324 a month at list prices for the separate tools (concept §6.2), before setup cost, and every further staff member adds per-seat fees.
- The needs the market feels, ranked (concept §4.5): online booking without technical setup and getting paid professionally are felt very highly by every service business owner; a professional website, client records and professional invoices are high; email, performance view, sales follow-up, blog, landing pages, access control, credential protection, legal pages, AI help and voice follow.
- Why now (concept §4.6): digital service businesses are normal; AI is expected; marketplace fatigue is growing; Africa's digital service economy is growing while Stripe does not accept South African businesses directly.
- Competitor pricing and the ownership gap (concept §6.2–6.5, §5): marketplace commissions, per-staff fees, add-ons and no data ownership; HoneyBook and GoHighLevel lack Paystack, voice, backups in the client's own storage and unrestricted team size at one price.
- Business framing: BRD §2 (problem, opportunity, cost of doing nothing), all `[CLEAR]`.
- Not in the record (ruled by David at the Step 12 `/office-hours` run): no named individual, no interviews, no payments, no usage figures. The strongest behavioural claim the documents make is "the pattern was consistent" (concept §3). The designed demand test is the wedge: ten real founding clients (concept §10.2, BRD §8 KPI) and the demo apps that show the whole product before anyone pays (F-36, ADR `no-free-trial`).

## 3. Status Quo

How the target users manage today (concept §2, §6.1–6.2): a partial, fragmented setup — perhaps a website and a booking link — with everything else managed by hand; five to seven browser tabs and subscriptions; client data typed into more than one system; on marketplace platforms (Booksy, Vagaro, Mindbody), competitors appear on the business's listing, the platform's algorithm decides visibility, and leaving means leaving the customers behind. Single-purpose tools (Calendly, Acuity, Square Appointments, Wave, Mailchimp) each do one thing well and together create the fragmentation. Enterprise software (Salesforce, HubSpot's higher tiers) is unsuited to a solo practitioner or small team.

## 4. Target User and Narrowest Wedge

Target user (concept §4): consulting businesses and service businesses — on the ground, in the cloud, or both; teams of every size with unlimited staff at no extra cost, and solo practitioners at the same price; every delivery mode supported (at the premises, at the customer's location, online live, delivered work, to groups); global from launch with South Africa as the founder's home market for direct, personal relationships and international clients acquired through content (concept §4.4).

The narrowest wedge (concept §10.2): the first ten clients — real businesses from the founder's direct network and warm referrals, onboarded on the full product at the standard price, willing to say so publicly. The smallest useful version is not a reduced feature set: it is one client's complete business running on the whole platform, self-setup through the onboarding wizard, proven by ten founding clients. The wedge's proof is Phase 1's goal — not revenue but evidence (concept §10.2); capacity is a hard 100 clients (concept §1, BRD AC-12).

## 5. Stakeholders

From BRD §4, with what each needs:

| Stakeholder | Needs |
|---|---|
| The Mybizz founder (sponsor and operator) | Product, revenue, margin, support load; operates App 5; support target under 2 tickets per client per month (BRD BO-7) |
| Client Owners | Run and configure the business; own its data; reach live without help (BRD BO-5) |
| Client staff — Managers, Admins, Staff | Daily operation: bookings, customers, deals, email, invoices, time; each sees only what the role allows |
| The clients' customers | Book, order, pay, review, use the customer portal |
| Visitors and campaign visitors | Find the business, enquire, become leads |
| Prospects | Try the whole product before paying, in the two demo apps (F-36) |
| Anvil (platform supplier) | Hosting terms for Mybizz-owned Business accounts (AC-6 open) |
| Stripe, Paystack, PayPal, Amazon Web Services, Cloudflare, Groq | Each provider's own terms and fees; SES production access (AC-14) |
| Regulators — South Africa's Information Regulator (POPIA); GDPR authorities | Lawful processing; data-subject rights implemented |

## 6. Constraints

The platform facts, every one of them binding:

1. **One app and one database per client; never multi-tenant.** No `client_id`/`tenant_id` column on any table; isolation is structural (ADR `dependency-based-not-multi-tenant`; schema Rules; architecture reference §3).
2. **The server holds authority over identity, ownership and every rule.** Every server function checks the role with the role decorators; the interface hides what a role cannot use but is never the enforcement (architecture reference §12 F-01; `spec-security`; `spec-anvil-platform-standards` §10).
3. **All client code and `theme/assets` are public.** No credential, rule or secret in client code; secrets only in the Vault (`spec-anvil-platform-standards`; ADR `payment-security-boundary-vault`).
4. **Every client instance runs on its own Mybizz-owned Anvil Business account.** The Hobby plan is never used (architecture reference §4; common aspects 2; BRD AC-5, AC-7, AC-8).
5. **POPIA applies:** Information Regulator registration after launch; data-subject rights of access, correction, deletion and objection (`spec-regulatory-compliance-baseline`).
6. **GDPR applies when EU customers are served; PCI DSS when payments are taken** — SAQ A: card data never touches the instance; only the gateways' hosted fields and buttons take card details (`spec-regulatory-compliance-baseline`; `spec-integration`).

Constraints particular to this run:

- The five-app model and the rule that no form or server module is ever added to a client instance (architecture reference §§1–2; ADR `client-instance-architecture`).
- Anvil's platform constraints (architecture reference §14): Skulpt client runtime — money and date logic in server modules only; no native URL routing — the Routing dependency for public pages, lambda/Link/`open_form` for the admin area and portal; lazy-loaded rows; server-call latency; no WebSocket push — client-side polling.
- The schema is designed up front: 70 tables in `authoritative-schema.md`, including `cookie_policy` and `consents`; App 1 presently holds 44 tables, and the recorded migration (`schema-migration-log.md`, Part A and prompts P1–P14 — P13 creates the two tables, P14 removes columns, last) brings it to the schema, applied by David in the Anvil editor through Resolve.
- The application is partially built outside PDLF: 16 client packages with 91 form folders and 11 server packages with 49 server modules exist; the packages and forms the scaffold specifies but the app lacks are listed in the Specification and the Step Receipt. Existing packages are never renamed or merged during active development (scaffold-spec Rules); a module is renamed only at the build step that touches it (Step 12 decisions C1–C4).
- Money is stored as whole cents of the system currency; the system currency is locked once the first booking or invoice exists; the display currency is display-only (schema Rules; F-16; decision D3).
- Every `datetime` is stored in UTC and shown in `business_profile.timezone`; one IANA time zone per instance (ADR `timezone-utc-storage-display-conversion`).
- English is complete at launch; French, Portuguese, Spanish, German and Italian follow one at a time, each offered only when every phrase key exists in it (F-25; `spec-translation`).
- Agents never read or write `.env`, never write `anvil.yaml`, never commit or push (SRS §2.4; the operating rules).
- No reference platform is used (entry record: `reference_platform: null`).
- The in-app notification surface is specified by `spec-mastertemplate-notification-system` §8: bell icon and badge, a notification feed with a Load-more cursor, toasts suppressed for marketing priority, a preferences centre, and 30–60-second polling (recorded as a constraint at David's direction at the `/office-hours` approval).
- Help and explainer videos are Mybizz's own: stored once per language in a Mybizz-owned Cloudflare R2 bucket published at `media.mybizz.live`, played by the video component as direct video addresses — never in `theme/assets/` and never on an outside video service, and no cookie-setting Cloudflare feature is enabled on `media.mybizz.live`, so they play with no consent (decision D6). The clients' own F-09 videos stay embedded from outside services, consent-gated (D8). Claude aligns `pol-anvil-platform.md` and `pdlf-build-plan.md` after Step 14.
- The `mb-3-cs-project-library` stubs hold no facts; the two folders named at the head of this document are the law.

## 7. Common Aspects

Each of the 14 aspects of `pol-common-aspects-for-mybizz-anvil-applications`, as this application takes it. **All 14 are shared; none deviates**, so no business-domain deviation reason arises:

| # | Aspect | Shared — where the architecture meets it (§23 of this document) and its ADR |
|---|---|---|
| 1 | The five-app structure, never multi-tenant | §23.1 (one instance, one database; no tenant column — ST-06 verifies); ADR `dependency-based-not-multi-tenant` |
| 2 | Client Anvil accounts | §23.6 (the load counted per client account); architecture reference §4; ADR `tiers-model` |
| 3 | Configuration, never code, in a client instance | §23.3 (settings as data); architecture reference §1–§2; ADR `client-instance-architecture` |
| 4 | Schema designed up front, extended without breaking | §23.1 (the schema, the migration discipline, the metadata columns on the core tables); `authoritative-schema.md`; `schema-migration-log.md` |
| 5 | Secrets in the Vault | §23.4's Vault column and §24's secrets rules (one `encryption_key` reader); `spec-vault-system`; ADR `payment-security-boundary-vault` |
| 6 | Payments, webhooks and outbound calls | §23.2 (`payment_service` seam), §23.4 (the gateways), §23.5 (webhooks); ADR `webhook-architecture` |
| 7 | Email | §23.4 (SES, one tenant per client), §23.5 (SNS-signed events); ADR `brevo-email-and-crm` |
| 8 | Client storage and exit | §23.8 (R2 folders, index rows, retention, the exit export); ADR `client-r2-storage-and-exit-handover` |
| 9 | Management and health | §23.7 (the six Bearer-token endpoints, the 15-minute heartbeat, App 5 never polls); ADR `mybizz-management-visibility`, ADR `observability-architecture` |
| 10 | Background work and real-time updates | §23.6 (the schedule within the Business plan's limits; >22 s as background tasks); ADR `real-time-and-background-tasks` |
| 11 | Access, currency and time | §23.1 (server-side money and dates, transactions), §24 (sign-in and second factors, the five roles); ADRs `system-currency-selection-and-immutability`, `timezone-utc-storage-display-conversion` |
| 12 | Compliance | §23.9 (AI pass-through, ZDR), §24 (the cookie and consent surface), threat-model §B12; `spec-regulatory-compliance-baseline` |
| 13 | User interface | §23.2's display rules by reference to `DESIGN.md`; ADR `material-3-theme-component-scope`, ADR `anvil-extras-exclusion`, ADR `htmltemplate-use` |
| 14 | Reference platforms | none used — §8; nothing is taken from one |

## 8. Reference Platform

None. The entry record records `reference_platform: null`; no reference platform is used for Mybizz CS, so no domain or workflow is taken from one and no `reference-platform-research.md` exists.

## 9. Approaches Considered

Five approaches, each on Anvil — three from this design (A, B, C) and two from the `/office-hours` run (D, E):

**Approach A — complete the application on the scaffold as it stands.** Keep every existing package, form and module; add the packages, forms and server modules the scaffold specifies and the app lacks; apply the recorded schema migration (P1–P14) through David's Resolve; replace the Brevo integration module with the SES email service at the marketing-email build step; consolidate the duplicated Vault modules at the Vault build step; record the common/trade marking in this design doc for Steps 15, 16 and the template work. No restructuring of what exists.

**Approach B — restructure first, then build.** Before any feature work, split `client_code/` and `server_code/` into common and trade package groups, move the trade packages (bookings, services, orders, packages, time tracking, sales, invoicing) into trade groups, and rename the divergent modules (`EmailSetupForm`, `stripe_service`, `paystack_service`, `task_service`) to the scaffold's names first. On Anvil this is folder and import surgery across 91 form folders and 49 modules before a single feature moves.

**Approach C — build features only, defer all hygiene.** Add the missing features on top of what exists; leave the Brevo module, the duplicated Vault modules and the naming divergences for the template stage; mark common/trade later.

**Approach D — bookings-core first** (from the `/office-hours` run; the minimal-viable alternative). Launch with F-01, F-10, F-11 and F-15 only; add the rest after revenue. Fastest to a first paying client; smallest surface to test demand. Not taken, by the documents: PRD §7 defers nothing; F-36 and BR-26 require the whole product in the demo apps; ADR `tiers-model` forbids feature tiers under the one price.

**Approach E — marketplace entry** (from the `/office-hours` run; the lateral alternative). Harvest demand Booksy-style, then convert clients to isolated instances. Not taken, by the documents: concept §5 — "Mybizz CS is not a marketplace"; the ownership architecture is the differentiator; BRD AC-1 fixes the five-app model.

## 10. Recommended Approach

**Approach A.** Why:

- The scaffold spec is the standard the build follows, and its rule is explicit: a package that exists is never renamed or merged during active development; a new feature gets a new package. Approach B violates that rule wholesale and buys nothing the schema does not already fix — the schema (70 tables, law) stays identical under every approach.
- The recorded migration already plans the schema path; the Brevo and Vault-module corrections are one-step fixes at the build steps that touch them, with the decisions recorded (C1, C3).
- The common/trade marking is a design-doc record now and a package organisation at the template stage (ADR `common-and-trade-modules` consequence: the scaffold creates separate common and trade packages when a business is finished and the template work happens). Restructuring now would churn 140 files to reach the same place later.
- Approach C is rejected because it leaves an ADR-violating module (`brevo_campaigns_integration.py`) in the code for the whole build and defers a marking this step is required to make (ADR `common-and-trade-modules`: Steps 12, 15 and 16 mark every feature, table, term and decision).

## 11. Risks

| Risk | Likelihood | Impact | Response |
|---|---|---|---|
| Anvil does not permit client business apps on Mybizz-owned Business accounts (BRD AC-6, PRD OQ-1) | Low | High | Written question to Anvil's sales team before the first paying client; the answer kept on file |
| The Mybizz development account fails or is compromised; every client depends on App 2 there, and the two demo apps share the account, so a suspension stops sign-ups as well as service | Low | High | `deployment-procedures.md` account-level procedures; two-factor authentication; R2 backups of every instance |
| The schema migration (44 → 70 tables, P13 creating `cookie_policy` and `consents`) is applied by hand, by David, in App 1 | High | Medium | `schema-migration-log.md` Part A and prompts P1–P14 in order — P13 creates the two tables, P14 removes columns, last — each checked against its prompt before Resolve; Part C compares the resulting `anvil.yaml` with the schema table by table |
| The partially built app diverges from the scaffold as features are added (naming, duplicated modules, the Brevo module) | Medium | Medium | The Step 12 decisions C1–C4 assign each correction to the build step that touches it; reviews report a common item using a trade item as a finding |
| An app cannot schedule its dependency's background task or serve its dependency's HTTP endpoints (PRD OQ-2) | Unknown | High | Proved in App 1 before Step 31 relies on it; if it fails, the schedule and endpoints move into each instance's own code by ADR |
| Amazon does not grant SES production access (BRD AC-14) | Medium | High | Requested before launch; until then the mail catcher covers tests and no client is onboarded |
| Founder capacity: every ticket lands with the founder (concept §16) | Medium | High | Self-service help on every screen (F-25), under 2 tickets per client per month, at-risk alerts (F-34) |
| Early churn: a client who sees no value in the first weeks leaves quietly | Medium | Medium | At-risk alerts, the onboarding wizard with help videos, sample data from the first day, the demo apps before purchase |
| Competitors copy features | Medium | Medium | The Africa and Paystack position, founder-direct support, the ownership architecture |
| Cost per client rises (hosting, email, AI, voice) | Medium | Medium | Usage recorded in App 5 (F-33, F-34); monthly review |
| A client's sending harms other senders' reputation | Low | Medium | One SES tenant per client; automatic suppression; marketing only to opted-in contacts |
| A schema change must be applied by hand to up to 100 instances | High | Medium | Migration discipline in `deployment-procedures.md`; the schema fingerprint in every heartbeat |

## 12. Open Questions

| # | Question | Settled at |
|---|---|---|
| OQ-1 | The common/trade judgement calls this design makes — `service_performance` is a trade table while the dashboard feature is common; the accounts reports and the customer portal read trade data from common code — are recorded here and refined when the glossary and decisions are written | Step 16 |
| OQ-2 | The glossary is empty until Step 16 writes it; the trade-boundary terms (client, customer, provider, resource, package, deal) need their final definitions there | Step 16 |

## 13. Success Criteria

| # | Criterion | Metric | Threshold | Method |
|---|---|---|---|---|
| SC-1 | Every feature is built and works | Features passing their acceptance criteria | All of F-01 to F-36 | The test suite (SRS verification column; `spec-test-specification.md`), signed off at `build-verified` |
| SC-2 | First clients onboarded | Clients live in App 5's registry | 10 | App 5 client registry count (concept §15) |
| SC-3 | Reaching capacity does not strain the model | Paying clients | 100 (hard limit) | App 5 client registry count |
| SC-4 | Revenue at capacity | Annual recurring revenue | $180,000 | App 5 billing |
| SC-5 | Margin holds at capacity | Gross margin after hosting | $73,200 a year | App 5 billing less hosting cost; cost per client reviewed monthly |
| SC-6 | Clients stay | Client retention, year 1 to year 2 | Above 90% | App 5 client registry |
| SC-7 | A client goes live without help | Average onboarding-to-live time | Under 7 days | Registration date to `business_profile.onboarding_completed_at`, read in App 5 |
| SC-8 | Support stays founder-scale | Support tickets per client per month | Under 2 | App 5 tickets |
| SC-9 | A business-stopping problem is answered fast | Problem tickets acknowledged within 4 business hours | 100% | App 5 tickets (BRD KPI, `[INFERRED]`) |
| SC-10 | The cost per client is known | Clients with hosting, email and AI usage recorded for the month | 100% | App 5 usage records |

## 14. Specification

Every feature, rule and decision of the project's documents, named with the document and section that states it. The documents are the authority; this section never restates them. The project's documents are `mb-3-cs-project-library`; the global corpus is `project-library-global`. Amounts are stored as whole cents of the system currency (the schema's Money in cents rule; Step 12 decision D1).

### 14.1 The features (F-01 to F-36)

| Feature | Requirements live in |
|---|---|
| F-01 Accounts, roles and access | concept §7; architecture reference §12 F-01; user-flows UF-03, UF-38; schema `users`, `rate_limits`, `activity_log`; `spec-security`, `spec-security-architecture` (role limits, sign-in, sessions); SRS SR-01–SR-05; PRD FR-01 |
| F-02 The Vault | concept §7; architecture reference §12 F-02; UF-04; schema `vault`; `vault-implementation.md`; `sop-vault-recovery.md`; `spec-vault-system`; ADR `payment-security-boundary-vault`; SR-06, SR-07; FR-02 |
| F-03 Registration, onboarding and setup | concept §7; architecture reference §5, §12 F-03; UF-01, UF-02; schema `business_profile` onboarding fields; `spec-client-activation-runbook`; `spec-onboarding-implementation-plan` (the project's 10-step wizard governs — the architecture reference is the standard); FR-03; SR-08, SR-08a, SR-08b, SR-09 |
| F-04 Dashboard and analytics | concept §7; architecture reference §12 F-04; UF-11, UF-25, UF-43; schema `daily_summary`, `service_performance`, `provider_performance`, `campaign_metrics`, `sample_rows`; FR-04; SR-10; test specification §4.10, §4.11 |
| F-05 Settings Hub | concept §7; architecture reference §12 F-05; DESIGN.md SettingsForm (14 tabs, RBAC visibility); scaffold-spec settings package; FR-05; SR-11 |
| F-06 Public website | concept §7; architecture reference §12 F-06; UF-16, UF-19; schema `pages`, `files`; `spec-sm-syndication.md` (Route.meta tags); FR-06; SR-12, SR-12a |
| F-07 Homepage templates | concept §7; architecture reference §12 F-07; UF-17; DESIGN.md Homepage Template Variants (shared section inventory, differentiation matrix, video visibility rule); schema `config` key `home_config`, `business_profile.homepage_template`; FR-07; SR-13 |
| F-08 Landing pages | concept §7; architecture reference §12 F-08; UF-18, UF-20; schema `landing_pages`, `lead_captures`, `leads`; FR-08; SR-14 |
| F-09 Visual and media presentation | concept §7; architecture reference §12 F-09; custom-component-requirements-matrix (VideoDisplayComponent, ParallaxComponent, TimeLapseCarouselComponent); FR-09 |
| F-10 Service catalogue | concept §7; architecture reference §12 F-10; schema `services`, `booking_metadata_schemas`; FR-10; SR-55 |
| F-11 Bookings and scheduling | concept §7; architecture reference §12 F-11; UF-05, UF-06, UF-07, UF-08, UF-11; `spec-calendar-feature.md` in full; DESIGN.md Booking Design (status lifecycle, 7-step flow, BookingListForm columns); schema `bookings`, `availability_rules`, `availability_exceptions`, `group_sessions`, `resources`, `recurring_series`, `booking_metadata_schemas`; FR-11; SR-15–SR-22; test specification §1 |
| F-12 Orders for delivered work | concept §7; architecture reference §12 F-12; UF-09, UF-12; schema `orders`; FR-12; SR-23 |
| F-13 Session packages | concept §7; architecture reference §12 F-13; UF-10; schema `packages`, `customer_packages`; FR-13; SR-24 |
| F-14 Time tracking | concept §7; architecture reference §12 F-14; UF-13; schema `time_entries`; FR-14; SR-25 |
| F-15 Invoicing and payments | concept §7; architecture reference §12 F-15; UF-14, UF-15; `integration-architecture.md` (Payment Gateways, PayPal, Recording payments and refunds, Currency); `spec-integration` (Stripe, Paystack, PayPal, gateway-agnostic service layer, three-tier email architecture); DESIGN.md Invoice Design (tax handling, invoice status lifecycle, InvoiceViewerForm, InvoiceEditorForm, InvoiceListForm); ADRs `pdf-invoice-generation`, `payment-gateway-mutability`, `payment-gateway-configuration-is-a-settings-function-and-is-rbac-governed`, `payment-security-boundary-vault`; schema `invoice`, `invoice_items`, `payments`; FR-15; SR-17, SR-17a, SR-17b, SR-26, SR-27, SR-28, SR-29, SR-30 |
| F-16 Dual currency | concept §7; architecture reference §12 F-16; ADR `system-currency-selection-and-immutability` (display currency display-only; no conversion column is built for this app — Step 12 decision D3); FR-16; SR-31 |
| F-17 CRM and contacts | concept §7; architecture reference §12 F-17; UF-21, UF-22; ADRs `single-contacts-table`, `lead-capture-simultaneous-creation`; schema `contacts`, `contact_counter`, `contact_events`, `contact_submissions`, `segments`; FR-17; SR-32 |
| F-18 Email | concept §7; architecture reference §12 F-18; UF-35, UF-39; `spec-team-inbox.md` in full; `integration-architecture.md` (Email); ADR `brevo-email-and-crm` (Amazon SES for every email; the CRM built in; Brevo and Zoho are errors); schema `email_addresses`, `email_messages`, `email_suppressions`, `email_config`, `email_templates`, `email_log`; FR-18; SR-34, SR-35, SR-35a, SR-35b, SR-35c |
| F-19 Lead capture and marketing funnel | concept §7; architecture reference §12 F-19; UF-20; ADR `lead-capture-simultaneous-creation`; schema `leads`, `lead_captures`; FR-19; SR-33 |
| F-20 Tasks and automation | concept §7; architecture reference §12 F-20; UF-26; schema `tasks`, `automation_rules`, `automation_runs`; FR-20; SR-36 |
| F-21 Blog and content | concept §7; architecture reference §12 F-21; UF-16, UF-42; `spec-sm-syndication.md` in full; schema `blog_categories`, `blog_posts`, `blog_comments`; FR-21; SR-37, SR-37a |
| F-22 Clause Builder | concept §7; architecture reference §12 F-22; UF-28; ADR `legal-policy-responsibility-acknowledgement-and-clause-builder-architecture`; schema `privacy_policy`, `t_and_c`; FR-22; SR-38 |
| F-23 Reviews and testimonials | concept §7; architecture reference §12 F-23; UF-27; schema `reviews`, `testimonials`; FR-23; SR-39 |
| F-24 Customer portal | concept §7; architecture reference §12 F-24; UF-29; scaffold-spec portal package and CustomerLayout menu; FR-24; SR-40 |
| F-25 Ease of use, in-app help and language | concept §7; architecture reference §12 F-25; `spec-translation` in full; schema `business_profile.app_language`, `email_templates.language`; FR-25; SR-41, SR-41a; test ST-14 |
| F-26 AI assistance | concept §7; architecture reference §12 F-26; UF-30; `spec-ai-implementation.md` in full (architectural model, ten assists, rollout phases, data-dependent assists, sample data, further AI functions, data flow, technical approach, risks and guardrails, development sequence); FR-26; SR-42, SR-43, SR-44, SR-44a, SR-44b |
| F-27 WhatsUp voice | concept §7; architecture reference §12 F-27; UF-31; `spec-ai-implementation.md` (voice module, tiers 1–5, proactive briefings, conversational follow-up, natural-sounding voice, browser compatibility); FR-27; SR-45, SR-45a |
| F-28 Accounts | concept §7; architecture reference §12 F-28; UF-32; schema `expenses`, `expense_categories`; FR-28; SR-46 |
| F-29 Data, backups and history | concept §7; architecture reference §12 F-29; UF-33; `integration-architecture.md` (Client Storage: R2 folder table); ADR `client-r2-storage-and-exit-handover`; schema R2 key columns (`invoice.pdf_url`, `quotes.pdf_url`, `expenses.receipt_r2_key`, `webhook_log.r2_key`, `email_messages.r2_key`, `email_messages.attachments`); FR-29; SR-47, SR-48 |
| F-30 Security and compliance | concept §7; architecture reference §12 F-30; `spec-regulatory-compliance-baseline` (POPIA, GDPR, PCI DSS, retention periods, legal documents before go-live, offboarding handover); `spec-security-architecture` (role limits, sign-in, token security, data protection levels); FR-30; SR-49 |
| F-31 Design system | concept §7; architecture reference §12 F-31; DESIGN.md in full; `palette-tokens.md` in full; `anvil-ui-build-standard.md` in full; ADRs `dark-mode`, `design-rules`, `responsive-behaviour-mechanism`, `role-property-assignment-mechanism`, `ui-customization-approach`, `material-3-theme-component-scope`, `anvil-extras-exclusion`; FR-31; SR-50 |
| F-32 Help, support tickets and feedback | concept §7; architecture reference §12 F-32; UF-34; FR-32; SR-51 |
| F-33 Hosting | concept §7; architecture reference §4, §12 F-33; `pol-common-aspects-for-mybizz-anvil-applications` aspects 2 and 10; BRD AC-5, AC-6, AC-7, AC-8, AC-12; ADR `tiers-model`; FR-33; SR-52 |
| F-34 Mybizz management app | concept §7; architecture reference §1 (App 5), §12 F-34; UF-36, UF-37; ADR `mybizz-management-visibility`; `spec-api-specification` (management endpoints: health, status, suspend, unsuspend, push-notification, push-update-alert); FR-34; SR-53, SR-53a, SR-54 |
| F-35 Sales pipeline, companies and quotes | concept §7; architecture reference §12 F-35; UF-40, UF-41; schema `companies`, `deals`, `quotes`, `quote_items`; DESIGN.md Sales Design (DealBoardForm, QuoteEditorForm, QuoteAcceptPage, quote status badges); FR-35; SR-56, SR-57 |
| F-36 Demo apps | concept §7; architecture reference §12 F-36; UF-44; ADR `no-free-trial`; FR-36; SR-58 |

### 14.2 The flows

Every flow of `user-flows.md`, by number, name and feature, from its Flow Index. Actors and terms: user-flows.md, Terms and Actors.

| Flow | Name | Feature |
|---|---|---|
| UF-01 | Client registration and provisioning | F-03, F-34 |
| UF-02 | Owner onboarding | F-03 |
| UF-03 | Staff invitation and sign-in | F-01, F-05 |
| UF-04 | Opening the Vault | F-02 |
| UF-05 | Services appointment booking | F-11 |
| UF-06 | Booking at the customer's location | F-11 |
| UF-07 | Group session booking | F-11 |
| UF-08 | Booking that needs a room or equipment | F-11 |
| UF-09 | Ordering delivered work | F-12 |
| UF-10 | Buying and using a session package | F-13 |
| UF-11 | Admin daily operations | F-04, F-11 |
| UF-12 | Managing an order | F-12 |
| UF-13 | Time tracking and invoicing hours | F-14 |
| UF-14 | Invoices, payment terms and refunds | F-15 |
| UF-15 | Payment webhooks | F-15, F-29 |
| UF-16 | Content management and edit mode | F-06, F-21 |
| UF-17 | Homepage customisation | F-07, F-09 |
| UF-18 | Landing page creation | F-08 |
| UF-19 | Contact form submission | F-06, F-17, F-20 |
| UF-20 | Lead capture via landing page | F-19 |
| UF-21 | Contact management | F-17 |
| UF-22 | Custom segment builder | F-17 |
| UF-23 | Email campaign setup | F-18 |
| UF-24 | Email broadcast | F-18 |
| UF-25 | Marketing dashboard | F-04 |
| UF-26 | Tasks and automation rules | F-20 |
| UF-27 | Reviews and testimonials | F-23 |
| UF-28 | Clause Builder | F-22 |
| UF-29 | Customer portal | F-24 |
| UF-30 | Using the AI helper | F-26 |
| UF-31 | WhatsUp voice | F-27 |
| UF-32 | Expenses, accounts reports and export | F-28 |
| UF-33 | Restoring or viewing data from R2 | F-29 |
| UF-34 | Help: support tickets and feedback | F-32 |
| UF-35 | Team Inbox: reading, replying and managing email | F-18 |
| UF-36 | Mybizz acts on an at-risk alert | F-34 |
| UF-37 | Release reaches every client | F-34 |
| UF-38 | Error recovery | F-11, F-15, F-01 |
| UF-39 | Email addresses and desks | F-18 |
| UF-40 | Deals, companies and quotes | F-35 |
| UF-41 | Customer accepts a quote | F-35, F-24 |
| UF-42 | Sharing a blog post | F-21 |
| UF-43 | Clearing the sample data | F-04, F-26 |
| UF-44 | Trying the demo apps | F-36 |

Flow design principles: `user-flows.md`, Flow Design Principles (progressive disclosure; clear calls to action with progress indication; real-time input validation; a recovery path for every error; automatic progress saving; email confirmation for every transaction; fewest possible server calls).

### 14.3 The data

All 70 tables of `authoritative-schema.md`, by name, from its Table Groups:

- Core and configuration (11): `activity_log`, `business_profile`, `config`, `email_config`, `files`, `payment_config`, `rate_limits`, `sample_rows`, `theme_config`, `users`, `vault`
- Services and bookings (8): `services`, `resources`, `group_sessions`, `availability_rules`, `availability_exceptions`, `booking_metadata_schemas`, `bookings`, `recurring_series`
- Orders and packages (3): `orders`, `packages`, `customer_packages`
- Finance (6): `invoice`, `invoice_items`, `payments`, `time_entries`, `expenses`, `expense_categories`
- CRM and marketing (12): `contacts`, `contact_counter`, `contact_events`, `contact_submissions`, `segments`, `tasks`, `leads`, `lead_captures`, `email_campaigns`, `contact_campaigns`, `automation_rules`, `automation_runs`
- Sales (4): `companies`, `deals`, `quotes`, `quote_items`
- Email and webhooks (6): `email_templates`, `email_log`, `webhook_log`, `email_addresses`, `email_messages`, `email_suppressions`
- Content and policy (9): `blog_categories`, `blog_posts`, `blog_comments`, `landing_pages`, `pages`, `privacy_policy`, `t_and_c`, `cookie_policy`, `consents` — one legal-documents system for the three documents, with the consent record in `consents`
- Reviews (2): `reviews`, `testimonials`
- Summary and health (6): `daily_summary`, `service_performance`, `provider_performance`, `campaign_metrics`, `health_log`, `error_log`
- Notifications (3): `notifications`, `user_preferences`, `delivery_logs`

Rules of the schema: `authoritative-schema.md`, Rules — one instance, one client (no client or instance column on any table; isolation comes from the instance); every `datetime` stored in UTC and shown in `business_profile.timezone`; every amount in `business_profile.system_currency`; **money in cents** — every amount is stored as a whole number of cents of the system currency, as `spec-anvil-platform-standards` requires (Anvil's `number` column is floating-point, so decimal amounts would round wrongly), and amounts are converted to the currency's units only for display; rows of `bookings`, `orders`, `invoice`, `payments`, `quotes` and `expenses` are never deleted, their status changes; history outside the tables (backups, history, webhook payloads, audit entries older than 30 days, email bodies older than 90 days, email attachments and expense receipts in the client's R2 bucket); credentials only in the Vault, encrypted; names follow `spec-nomenclature` (link column `{target}_id`, plural for a multiple link, `provider_id` for a provider, `resource_id` for a bookable room or piece of equipment); allowed values are lowercase strings; column types `string`, `number`, `bool`, `date`, `datetime`, `media`, `simpleObject`, `link → table`, `links → table`. Every table has `client: none` and `server: full`.

Scope of the schema: `adr-schema-scope-reduction` ADR — the instance holds exactly these tables and no others, with the reason for each table in and out (`tickets`/`ticket_messages`, `kb_articles`/`kb_categories`, `events`/`event_registrations`, `backups`, `translations`, `customers` are out; `reviews`, `testimonials`, `resources`, `expenses`, `expense_categories`, `companies`, `deals`, `quotes`, `quote_items`, `email_addresses`, `email_messages`, `email_suppressions`, `sample_rows` are in). Changes to the schema: `schema-migration-log.md` — the recorded migration from App 1's present 44 tables to the 70-table schema: Part A, the renames by hand (`error_log.timestamp`, `blog_comments.comment_text`, `provider_id` in `availability_rules`, `availability_exceptions`, `bookings` and `time_entries`); prompts P1–P5 (11 new tables: `resources`, `group_sessions`, `recurring_series`, `packages`, `customer_packages`, `orders`, `payments`, `automation_rules`, `automation_runs`, `reviews`, `testimonials`); P6 and P7 (new columns); P8–P10 (10 new tables: `companies`, `quotes`, `quote_items`, `deals`, `expense_categories`, `expenses`, `sample_rows`, `email_addresses`, `email_messages`, `email_suppressions`); P11 (new columns for the new features); P12 (3 new notification tables: `notifications`, `user_preferences`, `delivery_logs`); P13 (`cookie_policy` and `consents`); P14 last (the column removals, including the Brevo columns of `email_config`); Part C, the check against `authoritative-schema.md`.

Step 12 decision D2 is carried out in those documents: the Notifications group is in the schema and prompt P12 is in the migration log.

### 14.4 The architecture and its decisions

The architecture itself: architecture reference §0 states its authority — this document is the build standard for the architecture of Mybizz CS; agents build from it; where `mb-3-cs-concept.md` or `mb-3-cs-architecture-explainer.md` differs from it on a technical fact, it is correct. Architecture reference §§1–14 — the five-app system (§1), the rule that no form or server module is ever added to a client instance (§2), data isolation (§3), hosting and capacity (§4), provisioning and registration (§5), the development workflow (§6), the package structure (§7), the module architecture (§8), navigation — the admin menu, the customer menu and their visibility rules (§9), startup and authentication routing (§10), the Sidesheet (§11), the technical stack (§13) and the platform constraints (§14). Architecture reference §15 names the related specifications and what each is relevant to. Architecture reference §16 specifies the sample data: loaded at provisioning for the client's trade in the client's currency and language, every sample row recorded in `sample_rows`, sample contacts at `example.com`, the banner on every screen, and the Owner's Clear sample data background task — specified in full in `spec-ai-implementation.md` (Sample Data), carried out by `sample_data_service` in `server_settings/`.

`mb-3-cs-architecture-explainer.md` explains, in plain language for the developer, how Mybizz CS is built and why it is built that way: the big picture, the five apps and why the client apps must stay empty, where the apps live, how a new client is set up, how a change reaches every client, data and backups and history, security, how the code is organised, navigation and layout, every feature F-01 to F-36 in plain language, the sample data, and working within Anvil. The build agent works from the architecture reference, which is correct where the two differ.

Local ADRs (`mb-3-cs-project-library/adr-local`): `adr-blank-client-template` (App 3 as the only provisioning clone source), `adr-crm-package-replaces-marketing` (client package `crm/`, server package `server_marketing/`), `adr-schema-scope-reduction` (tables in and out).

Local policy (`mb-3-cs-project-library/policy-local`): `pol-development` (the Anvil-first mandate with its approved exceptions — the Vault, Amazon SES, Cloudflare R2, Groq through App 5; version control and backup; development tooling; enforcement severity).

Global ADRs applied to this application (`project-library-global/adr-global`, all 41): `adr-anvil-extras-exclusion`; `adr-anvil-platform-constraints`; `adr-brevo-email-and-crm`; `adr-client-data-management-rights-and-mybizz-retention-boundary`; `adr-client-instance-architecture`; `adr-client-instance-readme-five-app-system`; `adr-client-r2-storage-and-exit-handover`; `adr-common-and-trade-modules`; `adr-common-architecture-across-mybizz-applications`; `adr-dark-mode`; `adr-data-access-patterns`; `adr-dependency-based-not-multi-tenant`; `adr-dependency-update-model`; `adr-design-rules`; `adr-document-frontmatter-and-identity`; `adr-form-architecture-and-state`; `adr-htmltemplate-use`; `adr-lead-capture-simultaneous-creation`; `adr-legal-policy-responsibility-acknowledgement-and-clause-builder-architecture`; `adr-material-3-theme-component-scope`; `adr-mybizz-management-visibility`; `adr-navigation-lambda-link-open-form`; `adr-no-free-trial`; `adr-observability-architecture`; `adr-onboarding-data-schema-alignment`; `adr-onboarding-finality`; `adr-onboarding-resumability`; `adr-onboarding-vs-settings-boundary`; `adr-payment-gateway-configuration-is-a-settings-function-and-is-rbac-governed`; `adr-payment-gateway-mutability`; `adr-payment-security-boundary-vault`; `adr-pdf-invoice-generation`; `adr-real-time-and-background-tasks`; `adr-responsive-behaviour-mechanism`; `adr-role-property-assignment-mechanism`; `adr-single-contacts-table`; `adr-system-currency-selection-and-immutability`; `adr-tiers-model`; `adr-timezone-utc-storage-display-conversion`; `adr-ui-customization-approach`; `adr-webhook-architecture`.

Global policies (`project-library-global/policy-global`, all 6): `pol-common-aspects-for-mybizz-anvil-applications` — its 14 aspects are each shared by this application, none deviating: (1) the five-app structure, never multi-tenant; (2) client Anvil accounts, one Mybizz-owned Business account per client; (3) configuration, never code, in a client instance; (4) schema designed up front, extended without breaking; (5) secrets in the Vault; (6) payments, webhooks and outbound calls; (7) email through Amazon SES, the CRM built in; (8) client storage and exit; (9) management and health; (10) background work and real-time updates; (11) access, currency and time; (12) compliance; (13) user interface; (14) reference platforms (none is used here). Also `pol-anvil-first-development`, `pol-documentation-and-decisions`, `pol-security-and-data-governance`, `pol-testing-and-quality`, `pol-legal-documents-and-consent`.

### 14.5 The standards the build follows

From the project's documents: `scaffold-spec.md` (package structure, server function pattern, custom component conventions, database schema rules, M3 component standards, navigation architecture, startup configuration, Users service settings, dependencies and versions); `globals-contract.md` (every shared state variable, the defensive loading pattern); `integration-architecture.md` (every outside service and its credential home, payment gateways and the one payment service, PayPal checkout, email paths, the CRM, client storage, AI and speech, webhooks, the background-task schedule, timezones, domain and provisioning, offboarding); `observability-requirements.md` (structured logging, log tables, business metrics, alerts in the instance and in App 5, the heartbeat, log retention); `deployment-procedures.md` (deployment model, update flow, release steps, staged rollout, schema migration types and checklist, post-deploy verification, rollback and its triggers, deployment checklist, account-level failures); `anvil-ui-build-standard.md` (hard prohibitions, visual tokens, naming conventions, HTML-to-component mapping, approved components and writeback, custom HTML components, form boundary and icons, data and empty states, build process rules); `DESIGN.md`; `palette-tokens.md`; `custom-component-requirements-matrix.md` (12 shared custom components, 1 local, the not-built list); `spec-calendar-feature.md`; `spec-team-inbox.md`; `spec-sm-syndication.md`; `spec-ai-implementation.md`; `vault-implementation.md`; `sop-vault-recovery.md`; `client-exit-handover.md`; `spec-test-specification.md`.

From the global corpus: `spec-five-app-architecture-model`; `spec-anvil-platform-standards`; `spec-anvil-spec-table`; `spec-api-specification`; `spec-client-activation-runbook`; `spec-component-properties`; `spec-deployment`; `spec-integration`; `spec-legal-documents-and-consent`; `spec-m3-design-standards`; `spec-m3_component_mapping`; `spec-mastertemplate-notification-system`; `spec-material-3-theme`; `spec-mybizz-management-app-notification-system`; `spec-nomenclature`; `spec-observability`; `spec-onboarding-implementation-plan` (the project's 10-step wizard governs, per the architecture reference's authority); `spec-regulatory-compliance-baseline`; `spec-screen-and-wireframe-production-standards`; `spec-screen-production-standard`; `spec-security`; `spec-security-architecture`; `spec-testing-methodology-standards`; `spec-translation`; `spec-ui-standards`; `spec-vault-system`.

### 14.6 The business and product requirements

`business-requirements-document.md`: objectives BO-1 to BO-8; requirements BR-1 to BR-26; assumptions and constraints AC-1 to AC-14 (AC-6 — Anvil's written confirmation of the account structure — and AC-14 — SES production access — are open); success criteria KPIs; risks. `product-requirements-document.md`: goals and non-goals; target users; user stories US-1 to US-41; functional requirements FR-01 to FR-36; non-functional requirements; out of scope (nothing deferred — voice tier 4 and tier 5 are built in this release); dependencies; open questions OQ-1 and OQ-2; success metrics. `software-requirements-specification.md`: specific requirements SR-01 to SR-58; external interface requirements; performance requirements; design constraints; non-functional requirements.

`mb-3-cs-concept.md`: what Mybizz CS is (§1), the problem (§2), the founder story (§3), target market (§4), what it is not (§5), competitive positioning (§6), the feature set (§7), sample data (§8), pricing (§9 — $150 a month, month to month, no trial, $200 assisted setup, costs outside the monthly price, revenue model, the client's own domain), go-to-market (§10), brand identity (§11), key messages (§12), support model (§13), what is explicitly out of scope (§14), success metrics (§15), risks (§16). `mb-3-cs-feature-list.md`: the client-facing list of every feature, §§1–28.

### 14.7 Testing

`spec-test-specification.md` in full — the E2E booking flow test spec with its performance gate and data validation, the edge cases (existing contact, provider unavailable, double-booking), the error-path test cases (payment failure, webhook signature failure, webhook dispatch failure, SES throttling, Vault secret not found, contact deduplication conflict, server call timeout), the RBAC coverage matrices (form access, settings tab access, test procedure, the RBAC decorator pattern) and the feature tests 4.1 to 4.21. Also `spec-testing-methodology-standards` (levels 0–4, pure-function characteristics, the integration safety protocol, test structure, the test plan shape, the RBAC verification procedure), `pol-testing-and-quality`, and the test suite (`ts-method`, `ts-catalogue`, `ts-conformance-checks`).

### 14.8 Decisions taken at Step 12

| # | Decision |
|---|---|
| D1 | Every amount is stored as a whole number of cents of the system currency. The schema's number columns hold cents; the concept's currency figures are display wording. No override is recorded. |
| D2 | Carried out in the project's documents: `authoritative-schema.md` holds the Notifications group (`notifications`, `user_preferences`, `delivery_logs`) and `schema-migration-log.md` records prompt P12 (the three notification tables); the removals are prompt P14, last. |
| D3 | The display currency is display-only; every stored amount is in the system currency; the global ADR's transaction-currency column and `system_currency_amount` column are not built for this app. |
| D4 | `spec-security` governs passwords: no composition rules; breached passwords refused; context words refused. |
| D5 | `adr-navigation-lambda-link-open-form` governs navigation: the layouts' `NavigationLink` components with lambda click handlers capturing their loop variables, and `open_form()`; `navigate_to` is never used. |
| D6 | App 5 is built before the first paying client, in every application. |
| D7 | No password is ever sent by email. |
| C1 | `server_marketing/brevo_campaigns_integration.py` is an error under `adr-brevo-email-and-crm`; it is replaced by the SES email service at the build step that touches marketing email. |
| C2 | `client_code/test1/` — a placeholder form not in the scaffold — is deleted at the next build step. |
| C3 | The duplicated `vault_service.py` and `encryption_service.py` in `server_settings/` and `server_shared/` are consolidated to one of each at the Vault build step; only the one `vault_service` reads `encryption_key`. |
| C4 | Naming and structure divergences from the scaffold (`SignupForm` though sign-up is off; `EmailSetupForm` vs `EmailSettingsForm`; `stripe_service`/`paystack_service` vs `*_gateway`; `task_service` in `server_marketing`; `shared/` lacking `globals.py`, `palette_utils.py`, `translations/`) are recorded; a module is renamed only at the build step that touches it, per the scaffold's no-rename rule. |

### 14.9 Common and trade

Every feature, table and decision marked `common` or `trade`, per ADR `common-and-trade-modules`. The marking is derived mechanically from the ADR's rule — the trade part may use the common part; the common part never uses the trade part; no common table holds a link to a trade table — applied to the schema's typed links.

**Features.** Trade: F-10 (service catalogue), F-11 (bookings and scheduling), F-12 (orders for delivered work), F-13 (session packages), F-14 (time tracking), F-15 (invoicing and payments — the billing of this trade's work; the gateway service layer itself is common aspect 6), F-23 (reviews and testimonials — the review of an appointment), F-35 (sales pipeline, companies and quotes — quotes become invoices of this trade's work). Common: F-01 to F-09, F-16 to F-22, F-24 to F-34, F-36.

**Tables.** Trade (20): `services`, `resources`, `group_sessions`, `availability_rules`, `availability_exceptions`, `booking_metadata_schemas`, `bookings`, `recurring_series`, `orders`, `packages`, `customer_packages`, `time_entries`, `invoice`, `invoice_items`, `payments`, `quotes`, `quote_items`, `deals`, `reviews`, `service_performance`. Common (50): every other table. `invoice`, `payments`, `quotes`, `quote_items`, `deals`, `reviews` and `service_performance` are trade because each holds a typed link into a trade table (`invoice.booking_id`, `invoice.order_id`, `invoice.customer_package_id`; `payments.invoice_id`, `payments.booking_id`, `payments.order_id`, `payments.customer_package_id`; `quotes.invoice_id`; `deals.quote_id`; `reviews.booking_id`, `reviews.service_id`; `service_performance.service_id`); a common table may not.

**Decisions.** Common: the five-app model, client instances and the no-forms rule, the Vault, SES email, R2 storage and exit, management endpoints and the heartbeat, observability, webhooks and background tasks, roles and access, system currency and timezones, compliance and offboarding, the design system, translation, the notification engine, demo mode, tiers and no-trial, the onboarding architecture, lead capture, the single contacts table, the clause builder. Trade: the service catalogue and its pricing models, availability and resources, the booking flow and calendar, group sessions, recurring series, orders, packages, time tracking, invoice and payment records with their trade-source links, quotes and deals, review requests after appointments.

Judgement calls recorded as open question OQ-1 (settled at Step 16): `service_performance` is a trade table while the dashboard feature (F-04) is common; the accounts reports (F-28) and the customer portal (F-24) are common features whose reports and panels read trade tables.

## 15. Canonical Sources

Every canonical file read, with what this design doc takes from it.

**`mb-3-cs-project-library/adr-local`** — `adr-blank-client-template.md`: App 3's contents and the schema responsibility. `adr-crm-package-replaces-marketing.md`: the client package is `crm/`, the server package `server_marketing/`. `adr-schema-scope-reduction.md`: which tables are in and out, and why.

**`mb-3-cs-project-library`** — `authoritative-schema.md`: all 70 tables, including `cookie_policy` and `consents`, the Rules (including money in cents), column types, and the Notifications group. `schema-migration-log.md`: Part A renames, prompts P1–P14, the Part C check. `mb-3-cs-concept.md`: sections 1–16 — the problem, market, positioning, every feature, pricing, go-to-market, brand, support, out-of-scope, metrics, risks. `mb-3-cs-feature-list.md`: the client-facing feature list, §§1–28. `mb-3-cs-architecture-reference.md`: §§0–16, the build standard and every feature's architecture. `mb-3-cs-architecture-explainer.md`: the plain-language account of the same architecture. `user-flows.md`: the 44 flows, actors, terms and flow design principles. `DESIGN.md`: the design system, tokens, palettes governance, forms and pages, templates, deviations, status badges, empty states. `palette-tokens.md`: the four palettes' source colours and every value, light and dark. `custom-component-requirements-matrix.md`: the 12 shared components, 1 local, the not-built list. `spec-calendar-feature.md`: the booking calendar's forms, views, data and rules. `spec-ai-implementation.md`: the AI helper, the ten assists, voice tiers 1–5, sample data, costs and guardrails. `spec-team-inbox.md`: SES, the domain records, addresses, receiving, the Team Inbox screen, sending, storage. `spec-sm-syndication.md`: the share panel, AI captions, previews, `?ref=` tracking. `vault-implementation.md`: what the Vault stores, the VaultForm, the build sequence. `sop-vault-recovery.md`: both recovery paths. `client-exit-handover.md`: the exit steps and rules. `spec-test-specification.md`: the E2E spec, error paths, RBAC matrices, feature tests.

**`mb-3-cs-project-library/docs-standard-local`** — `anvil-ui-build-standard.md`: the hard prohibitions, tokens, naming, mapping, writeback, build process. `business-requirements-document.md`: BO-1–8, BR-1–26, AC-1–14, KPIs, risks. `deployment-procedures.md`: the release flow, migration types, rollback and triggers. `globals-contract.md`: every shared state variable and the defensive pattern. `integration-architecture.md`: every outside service, the task schedule, timezones, domain, offboarding. `observability-requirements.md`: logging, metrics, alerts, heartbeat, retention. `product-requirements-document.md`: US-1–41, FR-01–36, NFRs, OQ-1–2. `scaffold-spec.md`: packages, forms, server modules, navigation, startup, dependencies. `software-requirements-specification.md`: SR-01–58, interfaces, performance, constraints.

**`mb-3-cs-project-library/policy-local`** — `pol-development.md`: the Anvil-first mandate and its four approved exceptions, tooling, enforcement.

**`project-library-global/adr-global` (41)** — `adr-anvil-extras-exclusion`: Extras and third-party packages excluded. `adr-anvil-platform-constraints`: designed within Anvil's parameters. `adr-brevo-email-and-crm`: SES everywhere; the CRM built in; Brevo and Zoho are errors. `adr-client-data-management-rights-and-mybizz-retention-boundary`: owner data rights and Mybizz's retention boundary. `adr-client-instance-architecture`: what lives where; the two proven Anvil behaviours. `adr-client-instance-readme-five-app-system`: the mandatory README in every client instance. `adr-client-r2-storage-and-exit-handover`: one bucket per client; the exit handover. `adr-common-and-trade-modules`: the common/trade marking this design doc records. `adr-common-architecture-across-mybizz-applications`: what is common stays common. `adr-dark-mode`: dark mode from the first release. `adr-data-access-patterns`: search, never iterate; pre-computed summaries. `adr-dependency-based-not-multi-tenant`: the prohibited and required patterns. `adr-dependency-update-model`: the release flow and its discipline. `adr-design-rules`: the wireframe rules for later phases. `adr-document-frontmatter-and-identity`: document identity (governs this document's own frontmatter). `adr-form-architecture-and-state`: the content panel, `globals.py`, the batched booking flow. `adr-htmltemplate-use`: HTMLTemplate banned. `adr-lead-capture-simultaneous-creation`: lead and contact created together. `adr-legal-policy-responsibility-acknowledgement-and-clause-builder-architecture`: the owner's responsibility and the clause builder. `adr-material-3-theme-component-scope`: the permitted components. `adr-mybizz-management-visibility`: the three communication patterns and the token model. `adr-navigation-lambda-link-open-form`: the navigation standard. `adr-no-free-trial`: the two demo apps instead. `adr-observability-architecture`: instance-level metrics and the heartbeat. `adr-onboarding-data-schema-alignment`: onboarding uses the schema's exact names. `adr-onboarding-finality`: onboarding revisitable; append-only records. `adr-onboarding-resumability`: progress preserved. `adr-onboarding-vs-settings-boundary`: what belongs where. `adr-payment-gateway-configuration-is-a-settings-function-and-is-rbac-governed`: gateway configuration in Settings. `adr-payment-gateway-mutability`: one active gateway, changeable under rules. `adr-payment-security-boundary-vault`: secret keys only in the Vault; the enforcement pattern. `adr-pdf-invoice-generation`: reportlab, server-side. `adr-real-time-and-background-tasks`: polling intervals, the task inventory, retry policy. `adr-responsive-behaviour-mechanism`: `wrap_on`, never breakpoints. `adr-role-property-assignment-mechanism`: roles set in code. `adr-single-contacts-table`: one `contacts` table, no `customers`. `adr-system-currency-selection-and-immutability`: currency at onboarding, locked after the first transaction (this app takes the display-currency-only model, decision D3). `adr-tiers-model`: one price, $150; assisted setup $200. `adr-timezone-utc-storage-display-conversion`: UTC storage, IANA string, display-time conversion. `adr-ui-customization-approach`: M3 tokens, `theme.css`, native layouts. `adr-webhook-architecture`: the one webhook pattern and reconciliation.

**`project-library-global/policy-global` (6)** — `pol-common-aspects-for-mybizz-anvil-applications`: the 14 aspects, all shared (section 7). `pol-anvil-first-development`: the checking order and severity. `pol-documentation-and-decisions`: the four document kinds; no duplicate sources of truth. `pol-security-and-data-governance`: Vault-only secrets, mandatory RBAC, client data ownership. `pol-testing-and-quality`: the test suite governs; local first; sign-off. `pol-legal-documents-and-consent`: the three legal documents built in the Clause Builder and published by the Owner before go-live; consent before any optional cookie, refusal as easy as acceptance, the choice changeable at any time, every choice and acceptance recorded, every cookie known at build time.

**`project-library-global/specifications-global` (26)** — `spec-five-app-architecture-model`: the five apps and the critical constraint. `spec-anvil-platform-standards`: form files, server pattern, three tiers, background tasks, Uplink, rate limiting, shared numeric standards, data access, the constraints checklist, Skulpt, media, `app_tables` failure behaviour. `spec-anvil-spec-table`: the consolidated reference; its navigation rows follow the ADR (decision D5). `spec-api-specification`: webhook and management endpoints, rate limits, errors. `spec-client-activation-runbook`: provisioning steps 1–16 and the rollback. `spec-component-properties`: Designer vs code properties. `spec-deployment`: release workflow, migration strategy, account failures. `spec-integration`: SES, Stripe, Paystack, PayPal, the service layer, the three email tiers. `spec-legal-documents-and-consent`: the three documents' tables (`privacy_policy`, `t_and_c`, `cookie_policy`, `cookie_policy` adding `feature_key`), versions and kept copies in R2, the acceptance box at every agreement, cookie categories, the cookie register, the banner, the consent cookie, before and after the choice, the `consents` record with its archive path and retentions, and the seeded Cookie Policy clauses. `spec-m3-design-standards`: prefixes, button hierarchy, typography, states, status colours, accessibility, empty states, the MUI overlay. `spec-m3_component_mapping`: the mapping and writeback table. `spec-mastertemplate-notification-system`: the notification engine; its three tables are in the schema (D2). `spec-material-3-theme`: theme architecture and roles. `spec-mybizz-management-app-notification-system`: the platform-to-client console. `spec-nomenclature`: names for everything common. `spec-observability`: the log tables and structured logging. `spec-onboarding-implementation-plan`: the onboarding architecture (this app takes the 10-step wizard of its own documents). `spec-regulatory-compliance-baseline`: POPIA, GDPR, PCI DSS, retention, offboarding. `spec-screen-and-wireframe-production-standards` and `spec-screen-production-standard`: screen production rules for later phases. `spec-security`: the security standards this app follows (decision D4). `spec-security-architecture`: role limits, sign-in, tokens, protection levels. `spec-testing-methodology-standards`: levels, safety protocol, RBAC verification. `spec-translation`: phrase keys, the module, tests, adding a language. `spec-ui-standards`: the M3 mandate, naming, binding. `spec-vault-system`: the two-level model, enforcement, enrolment and reset.

**Decision D4 (Step 14):** the background-task schedule gains the webhook reconciliation task — every 5 minutes, per test specification 2.3, with `spec-api-specification` §2.5's hourly wording aligned — and, with D8's legal-documents system, the nightly consent archive task (rows of `consents` older than 30 days to `consents/{document}/{year}/{month}/` in the client's bucket). Both are in `integration-architecture.md`.

**`mb-3-cs-project-library`** — its `adr-local`, `docs-local`, `docs-standard-local` and `policy-local` files were read in full (`README.md` files, the GLOSSARY skeleton, the design-folder README, the releases and strategy placeholders, the schema-migration-log skeleton, and the docs-standard-local section skeletons). They hold structure and pointers only; every fact in this design doc comes from the two folders named at its head.

## 16. Data Inventory

Written at Step 14 by `secure-software-engineering` at its requirements phase, from `spec-security-architecture` §4 (the protection levels) and `spec-regulatory-compliance-baseline` (the retention defaults). Every kind of data the application holds is named with its owner, its readers, its writers, its protection level and its retention. A kind that fits two levels takes the higher. Retention follows the baseline's defaults; a kind the baseline does not list carries its own retention here, with the reason. Cold data — backups, history, webhook payloads, audit entries older than 30 days, email bodies older than 90 days, attachments and receipts — is kept in the client's own R2 bucket; the instance keeps the index row (`authoritative-schema.md` Rules; `integration-architecture.md`, Client Storage).

| Kind of data | Where it lives | Owner | Readers | Writers | Protection level | Retention |
|---|---|---|---|---|---|---|
| Credentials and keys | `vault` (12 secret names), the `encryption_key` Anvil Secret, `vault_totp_secret`, the instance's App 5 Bearer token | The client's business, held by its Owner | The server alone; the Owner sees masked values only (`'***'`) | `vault_service` on the server; App 5 writes the provisioning keys into the Vault at registration | Restricted | For the life of the client in the Vault; the exit export writes the decrypted Vault into the client's bucket; Mybizz retains nothing after exit |
| Card payment data | Not held. Taken only by the gateways' hosted pages, fields and buttons; the instance holds gateway references (`payments.gateway_reference`), gateway fees and publishable keys in `payment_config` | — (never held) | The payment service and webhook handlers, server-side | The payment service and webhook handlers | Restricted | Payment rows are never deleted (schema Rules); the 7-year tax retention is the client's own obligation, met by the client's records; exit export |
| The client's customers' and staff's personal information | `contacts`, `companies`, `users` (names, emails, phones, photos), `bookings` (customer details and intake answers), `contact_events`, `contact_submissions`, `leads`, `reviews` (author), `email_messages` (senders, recipients, bodies), `tasks`, `time_entries` | The client's business; the person the data is about holds the data-subject rights, implemented in the app | The roles the access matrix names, limited to their own rows where the role is row-scoped (Staff: own bookings, calendar, deals, addresses; Customer: own records only, derived from the signed-in user, never from a client-supplied id) | Server functions only | Personal | Contacts: indefinite unless deletion requested (baseline). Email bodies: 90 days in the instance, then the client's R2, the row kept as an index, for the life of the client. Bookings, orders, invoices, payments, quotes, expenses: never deleted, their status changes; for the life of the client; exit export |
| AI and speech request content | In transit only: instance → App 5 → Groq and back | The client's business | Nobody — App 5 passes each request through unstored and unlogged; Groq's Zero Data Retention is on | `server_ai/ai_request_layer`; App 5's gateway | Restricted | None — never stored or logged anywhere (`SR-53a`, `SR-45a`; `integration-architecture.md`, AI and Speech) |
| The client's business data, not about a person | `services`, `resources`, `group_sessions`, `availability_rules`, `availability_exceptions`, `booking_metadata_schemas`, `packages`, `expense_categories`, `blog_categories`, `pages`, `landing_pages` (configuration), `config`, `business_profile` (business-level fields), `theme_config`, `email_addresses`, `email_templates`, `files` (media library, order briefs, deliveries) | The client's business (its Owner) | The roles the access matrix names; the public subset is Public (next row) | Server functions only; every edit-mode save is recorded in `activity_log` | Internal | For the life of the client; exit export. Large media goes to R2 (`spec-anvil-platform-standards` §24); receipts and attachments are already there; `files` holds briefs and deliveries and is watched through the usage report |
| Public content | Published pages and blog posts, active services and their public prices, approved reviews and testimonials shown on the homepage, published policy pages | The client | Anyone, signed in or not | Server functions only (edit mode: Owner and Manager) | Public | For the life of the client; exit export |
| Audit and activity records | `activity_log` in the instance; the archived copy in the client's R2 `audit/{year}/{month}/`; Vault access emails | The client's business | The Owner (Settings → Data history view) and the server; no role can change an audit row | Server functions only | Personal (user ids and IP addresses; metadata never holds credentials or personal data beyond ids) | 30 days in the instance, then the client's R2 nightly, kept for the life of the client. The baseline's 2-year default governs records Mybizz itself keeps; the copy in the client's own bucket is the client's data — stated reason |
| Operational logs | `health_log` (30 days), `error_log` (90 days), `webhook_log` index rows (90 days), `rate_limits` counters, `delivery_logs` | Mybizz operates the platform; the rows sit in the client's instance | The server; App 5 reads the health and error counts the heartbeats carry | Server functions only | Internal (ids only; never secrets, card data or personal data) | As stated per table (`observability-requirements.md`, Log Retention) — own retention, reason: diagnostic value decays and the instance stays small; a webhook payload in R2 is the client's data for the life of the client |
| Summaries and metrics | `daily_summary`, `service_performance`, `provider_performance`, `campaign_metrics` | The client's business | The roles the access matrix names (dashboards and reports) | Background tasks only; dashboards never compute on load | Internal | For the life of the client; recomputed on schedule; exit export |
| Notifications and preferences | `notifications`, `user_preferences`, `users.preferences` (tooltips, morning briefing) | The user the row is about | That user; the notification engine | The server (notification engine; App 5's `push-notification` endpoint writes a row) | Personal | A notification until it is read or its `expires_at`; preferences for the life of the user's account. Not listed by the baseline — own retention, reason: user-scoped settings and delivery records live and die with the user's account |
| Sample data | `sample_rows` and every row it records (sample contacts at `example.com`, sample bookings, invoices, campaigns) | The client's business (provisioned by Mybizz) | Every signed-in role, under the sample-data banner | App 5 at provisioning; the Owner's Clear sample data task | Internal | Until the Owner clears it; never emailed, never paid, never exported (`SR-44b`) |
| Consent records | One table, `consents`, for all three legal documents (D8 as extended at Step 14); the consent cookie `mb_consent` in the visitor's browser; published copies at `legal/{document}/v{version}.html` in R2 | The visitor owns the cookie choice; the client's business owns the terms and privacy acceptance records (its contract evidence) | The server; the visitor reaches the cookie record only through the consent number shown in Cookie settings; terms and privacy rows carry the contact link | Server functions only | Cookie rows: Internal (a random consent number, the choice, the policy version, the time — no name, no email, no IP address). Terms and privacy rows: Personal (they carry the contact) | 30 days in the table, then `consents/{document}/{year}/{month}/` in the client's R2 bucket; cookie records kept 2 years, terms and privacy records kept 7 years (D4); part of the client's data at exit |
| Cold data in the client's R2 bucket | `backups/daily/`, `backups/weekly/`, `history/{table}/`, `audit/{year}/{month}/`, `webhooks/{service}/{year}/{month}/`, `email/messages/`, `email/attachments/`, `expenses/receipts/`, `exports/` | The client — the bucket sits in the client's own free Cloudflare account; Mybizz is an invited member only | The client; the instance through `r2_service` with the bucket's scoped token from the Vault; Mybizz as the invited R2 Admin member for provisioning, restore and exit | The instance's background tasks; App 5 at provisioning and exit | Takes the level of the item it holds (email bodies: Personal; backups of everything: Restricted handling — the token is scoped to that one bucket and lives in the Vault) | For the life of the client; leaves with the client at exit |
| Mybizz-side records (App 5) | Client registry, usage figures, health, billing, tickets and feedback, at-risk alerts | Mybizz | Mybizz's founder in App 5 | App 5 | Internal | After a client leaves, only the records Mybizz must keep by law — Mybizz's own invoices to the client. No customer personal data ever enters App 5: the heartbeat and usage report carry none, and AI and speech requests pass through unstored (`SR-53`, `SR-53a`; `adr-client-data-management-rights-and-mybizz-retention-boundary`) |

## 17. Regulatory Position

Decided at Step 14 from `spec-regulatory-compliance-baseline` and `spec-security-architecture` §3.

- **POPIA — applies.** Mybizz is based in South Africa, so the posture is Mybizz-wide regardless of what any individual app does. Registration with the Information Regulator as a Responsible Party is required after launch. The data-subject rights — access, correction, deletion and objection — are implemented in the app: export of a person's data, correction, and erasure on request with transactions kept for the 7-year tax period (`IN-14`). Reason: the platform holds the personal information of the clients' customers and staff.
- **GDPR — conditional; decided: applies when a client's customer base includes EU customers, determined by each client's actual base and rechecked yearly, not fixed at build time.** The same data-subject-rights implementation POPIA requires (export and anonymisation functions) satisfies both regimes; no separate implementation is built. Reason: the product is global from launch, so whether EU customers are served is a fact of each client's business, not of the design (`RL-11`, the yearly review).
- **PCI DSS — applies; SAQ A.** The app takes card payments (Stripe or Paystack as the card gateway, PayPal optional). Card data never touches the instance: only the gateways' hosted pages, fields and buttons take card details, and the instance holds only tokens and references. SAQ A also requires that the payment page cannot be tampered with by scripts: every script on a page that hosts or redirects to payment is listed and approved, and the list is checked at every release (test `SC-10`). **The annual SAQ A is taken every year while payments are taken.** Reason: Mybizz processes payments through Stripe, Paystack and PayPal, which places a compliant app in SAQ A, the lowest scope.
- **The three legal documents before go-live** — Privacy Policy, Terms of Service and Cookie Policy, per app, each reflecting the app's actual data practices (baseline). **Decision D8 (David, Step 14), extended at Step 14 under D4:** one legal-documents system covers the Cookie Policy, the Privacy Policy and the Terms and Conditions; the Clause Builder builds all three (the Cookie Policy in the table `cookie_policy`, `spec-legal-documents-and-consent` §1); every publish raises the document's version and saves a copy to the client's R2 bucket under `legal/{document}/v{version}.html`; the instance does not go live until all three are published under the recorded acknowledgement of responsibility. Customers accept the Terms and Conditions and the Privacy Policy with an unticked box at booking, order, package purchase, online payment, customer portal sign-up and quote acceptance; contact and lead-capture forms link to the Privacy Policy. Every choice and acceptance is recorded in one table, `consents` (consent number, document, choice, version, contact for the terms and privacy records only, time), and the schema stays at 70 tables. Retention: cookie records 2 years; terms and privacy records 7 years (transaction evidence). The cookie banner's rules stand: **Accept all**, **Refuse all** and **More info** before any embedded-content cookie; help videos Mybizz's own, at `media.mybizz.live` (`pol-legal-documents-and-consent`). Reason: the baseline requires the three documents, the cookie rules are binding policy for every Mybizz application, and the Clause Builder is the home the other two legal documents already use. The global policy and specification for the legal-documents system are committed: `pol-legal-documents-and-consent` and `spec-legal-documents-and-consent`; the two documents and the two tables are in the project's documents (`mb-3-cs-project-library`).
- The legal acknowledgement is recorded once, with its timestamp, in the instance and in App 5, and cannot be reversed (`SR-09`); `business_profile.legal_ack_accepted_at` and `data_governance_ack_accepted_at` hold it.

## 18. Security Requirements

Written at Step 14 by `secure-software-engineering` at its requirements phase. The assets, the actors, the harms and what must never leak are stated first; every acceptance criterion is testable and carries its test from the test suite. The security patterns themselves are not restated here — `spec-security`, `spec-security-architecture`, `spec-vault-system` and the ADRs are the authority; this section states what this application must prove.

**Assets:** the client's data (personal, business and financial); the credentials in the Vault; the money-moving actions (payments, refunds, gateway configuration, prices); the audit trail; the instance's availability; the per-client tokens; the privacy of the AI and speech channel; the demo apps' separation from every paying client.

**Actors:** the five roles — Owner, Manager, Admin, Staff, Customer — plus guests, who book and pay without an account; visitors and campaign visitors; Mybizz through App 5; background jobs; the outside services (the gateways, Amazon SES/SNS/S3, Cloudflare R2, Groq through App 5); the Anvil platform itself (hosting, the Users service, Anvil Secrets).

**Harms to prevent:** one client reaching another's data (made structurally impossible and verified); a role reaching what its role forbids; stolen credentials; captured card data; a forged or replayed webhook charging or fulfilling twice; a payment amount changed outside the server; bulk exfiltration of a client's data; personal data or secrets appearing in logs, heartbeats or App 5; an unauthorised export; a forged recovery path.

**What must never leak:** card data (never held); any credential outside the Vault; personal data beyond row ids in any log, heartbeat, usage report or App 5 table; a customer's name sent to the AI layer; a secret value in any response or log (masked, never round-tripped); a tenant column (never exists); a password in an email (never sent).

| # | Acceptance criterion | Test |
|---|---|---|
| AC-S1 | Every callable server function carries its role decorator naming exactly the roles allowed; a call from a role not allowed returns `{'success': False}` and writes nothing | `ST-02` (scanner over every function), `IN-02` (the access-matrix test over every function and role) |
| AC-S2 | A Staff member or Customer cannot list, fetch, change or download another person's records; every customer function derives the customer from the signed-in user, never from a client-supplied id | `IN-03`, `BR-03` |
| AC-S3 | No card number, CVV or full card data is stored in any table or log; payment is taken only on the gateway's own hosted page or fields; every script on a page that hosts or redirects to payment is listed and approved, checked at every release | `SC-10` |
| AC-S4 | Only `vault_service` reads `encryption_key`; every credential is encrypted in `vault`; a read returns `'***'`; saving a secret anywhere outside the Vault fails loudly | `ST-03`, `IN-13` |
| AC-S5 | Every Vault view requires a sign-in with the Owner's second factor in the last five minutes and a fresh code from the Vault's authenticator, with no remembered session; every access emails the Owner; 5 wrong codes lock the Vault for 15 minutes | `SC-03`, `IN-13`; test specification 4.18, 4.19 |
| AC-S6 | Every account holder signs in with a second factor; the Owner's is FIDO2 only and the Owner's high-value functions are refused while any other factor is enrolled; a session idle for 30 minutes requires sign-in again; the Owner can end any user's sessions | `SC-02`, `SC-04`, `ST-12` |
| AC-S7 | Vault access, refunds above the set limit, role changes, exports, gateway changes and price changes need a second factor given in the last five minutes | `SC-03` |
| AC-S8 | Sign-in is rate-limited per identifier in `rate_limits` — 10 requests a minute unauthenticated per IP, 100 a minute authenticated per user; repeated failures lock the account; the limits survive a restart | `ST-02`, `IN-16` |
| AC-S9 | Every webhook is verified by signature, refused when its `event_id` is already in `webhook_log`, written to R2, returns HTTP 200 within 5 seconds, and is applied once by a background task; a forged, altered or replayed event changes nothing | `IN-07` |
| AC-S10 | The server sets every amount and every order of steps: gateway amounts are read from the database, never the browser; a repeated capture records one payment; no flow can skip a step | `IN-06` |
| AC-S11 | No table, column, query or module carries a tenant discriminator or a tenant filter; isolation is structural | `ST-06`, `SC-14` |
| AC-S12 | Logs, heartbeats and App 5 carry no secrets, no card data and no personal data beyond row ids; the audit log cannot be changed by any role; every significant action writes an audit row with who, what, when, before and after | `IN-15`, `SC-12` |
| AC-S13 | AI and speech content is never stored or logged in the instance or in App 5; Groq's Zero Data Retention is on; a voice tier 5 request carries figures and placeholder labels only, never a person's name | `SR-53a`, `SR-45a` (payload inspection) |
| AC-S14 | Two client instances cannot reach each other's data; every client's management token, encryption key and storage key are unique, and a storage key opens only its own bucket | `SC-14` |
| AC-S15 | Nothing of App 5's credentials is in App 2 or App 3; a Customer cannot call any back-office function; signed-out users reach only public functions | `SC-08` |
| AC-S16 | Every export writes an audit row; the full-data export runs as a background task; the exit export holds every table in full, the key and the Vault contents, in files standard tools open | `IN-14`, `SC-12` |

## 19. Client Exit

What the exit export writes to the client's own bucket for this application (`adr-client-r2-storage-and-exit-handover` §5; `client-exit-handover.md` §2–3; F-29):

- **Into `exports/`:** every data table as CSV; every email still in the instance with its attachments; the accounting export in the Wave, Xero and QuickBooks formats (F-28); the instance's `encryption_key`; the Vault's contents in decrypted form.
- **Already in the bucket:** `backups/daily/` and `backups/weekly/` (the last 7 daily and 4 weekly), `history/{table}/`, `audit/{year}/{month}/` (the archived audit), `webhooks/{service}/{year}/{month}/` (the raw payloads), `email/messages/` and `email/attachments/`, `expenses/receipts/`.
- **The handover:** the Owner opens the bucket in the Cloudflare dashboard and confirms by email that the export is there and opens; Mybizz's Cloudflare user then leaves the client's account, which ends Mybizz's bucket token and DNS access. Nothing is transferred — the account and the bucket were always the client's.
- **After the handover:** App 5 deletes the client's SES tenant, domain identity, receipt rule, SNS topics and AWS keys and empties the client's S3 prefix; the domain is disconnected from the instance; the instance and its Mybizz-owned Anvil Business account are closed; App 5 marks the client as left and keeps only the records Mybizz must keep by law — Mybizz's own invoices to the client. Mybizz retains nothing else of the client's data.
- **The decrypted Vault holds live credentials.** The step 9 email tells the Owner to change the gateway keys (Stripe, Paystack, PayPal) once the instance is closed.
- **Proven:** the exit export is a function of every Mybizz application and is proven on the private test client before the first release (exit ADR, Consequences; `RL-08`; `client-exit-handover.md` §4).

## 20. Plan Fit

A typical client's use against the limits of the Anvil Business account that runs its instance (`spec-anvil-platform-standards` §5; architecture reference §4).

**The limits, per client account:** 1,000,000 data table rows; 100 GB of data table storage; background tasks with no timeout; scheduled tasks; a persistent server; email through Amazon SES — Anvil's own email quota is unused (F-18).

**What keeps a client small:** cold data goes to the client's own R2 bucket — backups, history, webhook payloads, audit entries older than 30 days, email bodies older than 90 days, attachments and receipts — with only index rows left in the tables; dashboards read pre-computed summary tables, never aggregate source tables on load; every search is bounded, never an iteration over a whole table; large media lives in R2 (`spec-anvil-platform-standards` §24).

**A typical client's rows (an estimate, stated as one):** a busy solo or small-team practice — about 2,000 contacts with about 60,000 timeline events; about 30 bookings a day, roughly 11,000 a year; invoices, invoice lines, payments, quotes and time entries from three years of that work, roughly 130,000 rows; email index rows about 55,000 over three years; the summary tables about 15,000; the rolling 30-day audit window about 5,000; notifications, preferences and counters modest. Steady state after three years is roughly 350,000 to 450,000 rows — under half of the 1,000,000-row limit, with the growth rate bounded by the R2 offload. Storage: the transactional and text rows are small; media lives in R2 except the instance's own media (logos, provider photos, featured images, the brief and delivery files in `files`), which stays a small share of the 100 GB.

**What shows a client nears the limits:** the daily usage report to App 5 — row count, data table storage, email volume and heavy-job activity (`SR-52`); App 5 warns Mybizz at 80% of the account's 1,000,000 rows (`FR-33`) and flags any client above 80% of its rows or storage (`RL-12`; common aspect 9); the warning-level alerts also watch a background task running over twice its usual time. A client nearing the limits is answered by checking the R2 offload is current and reviewing the media held in `files` — the usage recording and monthly review of `F-33`/`F-34` is the mechanism that makes this visible.

**A cost outside the model, recorded (Step 14 finding):** the private test client instance runs on its own Mybizz-owned Business account (`deployment-procedures.md`, Deployment Model) — a 101st account beside the 100 client accounts the revenue model counts (concept §9.6), about $1,068 a year billed annually. SC-5 keeps the concept's published margin figure; this line makes the cost visible.

## 21. Scope

Written at Step 14 after the review, per the Step 14 configuration.

**Goals:**

- Complete Mybizz CS on the scaffold as it stands: every feature F-01 to F-36 built and passing its acceptance criteria (SC-1), on the 70-table schema of `authoritative-schema.md`, reached through the recorded migration discipline.
- App 5 built and running before the first paying client (D6 of Step 12); every client provisioned through App 5 onto its own Mybizz-owned Anvil Business account.
- The wedge proved: ten founding clients onboarded on the full product at the standard price (SC-2), toward the hard capacity of 100 (SC-3) and the revenue and margin criteria (SC-4, SC-5).
- One legal-documents system covering the Cookie Policy, the Privacy Policy and the Terms and Conditions, published before go-live, with consent recorded in `consents` (D8 as extended at Step 14).
- Releases reach every client through App 2; a client can leave at any time with all of its data (the exit export of §19).

**Non-goals:** a marketplace or shared discovery; retail commerce (catalogue, cart, inventory, shipping); hospitality reservations; memberships and subscription billing for clients' businesses; a ticket system for the clients' own customers; an outside accounting connection; a native mobile app; feature tiers, add-ons, a free trial or per-staff fees; a multi-vertical toggle. Nothing is deferred — PRD §7 defers nothing.

**What the review excluded, and why:** see the review's NOT in scope section below the review body. In summary: no scope change was proposed in HOLD SCOPE mode; The Assignment of the `/office-hours` doc remains the founder's business action outside the build (Step 12 ruling); `TODOS.md` is never written under PDLF, so nothing was deferred to it.

## 22. What Must Be Tested

Each critical flow, role and edge case the review named, with why. The catalogue test IDs are the tests; Step 15's test plan carries them into the plan's own test structure (`DC-04`).

| # | What must be tested | Why | Tests |
|---|---|---|---|
| 1 | The E2E booking flow: service → provider → slot → meeting type → intake → payment → confirmation, at desktop and mobile widths, with the 3-second performance gate and the data checks (amount matches price, currency matches the system currency, UTC stored and displayed in the client's zone) | It is the product's core promise — book without phoning, pay professionally | Test specification §1; `BR-01`, `BR-06`, `DM-01` |
| 2 | Booking contention: two customers confirming the same slot at once — exactly one booking, the loser offered the nearest free slots | Money and trust: a double booking is the fastest way to break both | `DM-01`, `IN-05`, `SR-16` |
| 3 | The payment hold and its expiry: a `pending` booking released after 15 minutes with its slot and resource freed, the emailed payment link honoured until then | The slot must never be lost to a card problem nor blocked for long | `SR-17`; test spec 2.1 |
| 4 | The late-payment rule (D5): a payment succeeding for a released or cancelled booking is recorded; the booking is reinstated and confirmed when its slot and resource are still free, refunded in full through the original gateway when they are not; the customer and the Owner are notified either way. The reinstatement re-checks slot, resource and places **inside the same transaction** that sets the booking to confirmed, as SR-16 requires for `confirm_booking` — never a prior read-check alone; if the re-check fails, the payment is refunded in full and the customer and the Owner are notified (Sentinel finding `sentinel-14-app1`, resolved by David) | Real money meeting a released slot; the rule keeps the customer whole either way and can never double-book | New case in the test plan (Step 15); `IN-06`, `IN-07`; **the concurrency test: the late-payment webhook against a new booking for the same slot — exactly one succeeds** (`IN-05`, `DM-01` class) |
| 5 | Webhook security and exactly-once: forged signature → HTTP 400 and no row; duplicate `event_id` refused; valid event → 200 within 5 seconds, applied once by a background task; an event that cannot be handed off is reconciled by the 5-minute task (D4) | The gateway's word is the only thing that moves money | `IN-07`; test spec 2.2–2.3 |
| 6 | Server-side amounts: gateway amounts read from the database, never the browser; PayPal order created and captured only by the server; a repeated capture records one payment; quote acceptance creates one invoice | Payment manipulation is the classic attack on this exact design | `IN-06`; `SR-17a`, `SR-26`, `SR-56`; test 4.20 |
| 7 | Role enforcement over every function and form: the access matrix (Step 15) run against every server function and every role; Staff see only their own bookings, deals and addresses; customers reach only their own records | The server is the only enforcement; the interface hides, never protects | `IN-02`, `IN-03`, `BR-03`; test spec §3 matrices |
| 8 | The Vault: Owner-only; fresh second factor and TOTP on every view; masked reads; the hard-fail guard; lock after 5 wrong codes; email on every access; recovery by the Owner alone | Every credential the instance uses lives here | `IN-13`, `SC-03`; tests 4.18, 4.19; test spec 2.5 |
| 9 | Second factors and sessions: every account holder signs in with a second factor; the Owner's is FIDO2 only; high-value actions need a fresh factor; 30-minute idle ends the session and returns the user to the same place | The Owner's account is the keys to the client's business | `SC-02`, `SC-03`, `SC-04`; `ST-12` |
| 10 | Email through SES: confirmation with calendar invite from the client's domain, DKIM-signed; desk delivery to members and no one else; suppression honoured (bounce, complaint, unsubscribe scopes); the 90-day archive to R2 | The client's email reputation is a shared asset — one tenant per client | `IN-09`; test spec 4.1–4.5 |
| 11 | Sample data isolation: no email, payment, reminder or export ever touches a sample record; Clear sample data removes every recorded row, empties the links from real rows, recomputes the summaries, and is refused for every role but Owner | Sample data must never leak into a real business's money or mail | `SR-44a`, `SR-44b`; tests 4.10, 4.11 |
| 12 | The exit export (§19): every table in full, the key, the decrypted Vault, the accounting exports — into the client's own bucket, proven on the test client before the first release | A client must be able to leave with everything, at any time | `IN-14`, `RL-08`; `client-exit-handover.md` §4 |
| 13 | Structural isolation: two instances cannot reach each other's data; per-client tokens and keys are unique; no tenant column or filter anywhere in the code | The architecture's central promise, verified not assumed | `SC-14`, `ST-06` |
| 14 | Data-subject rights: export, correction and erasure of one person's data, with transactions kept for the 7-year tax period; retention deletes on time (audit 30 days in-instance; email bodies 90 days; consent records per D4) | POPIA and GDPR where they apply | `IN-14` |
| 15 | The legal-documents and consent system (D8): first visit shows the banner and sets no embedded-content cookie; Refuse all loads placeholders; Accept all loads embedded content; every choice and Load writes one `consents` row with the published version; a new policy version shows the banner again; the nightly task moves rows older than 30 days to the client's bucket and deletes cookie files older than 2 years and acceptance files older than 7 years; a `feature_key` clause is active exactly while its feature is on | Consent before optional cookies is binding policy and law for every client site | `spec-legal-documents-and-consent` §12 tests; `ST-02` on the endpoints |
| 16 | Structural isolation of the AI channel: no AI or speech content stored or logged in the instance or App 5; Groq Zero Data Retention on; voice tier 5 sends placeholder labels only; voice tier 4 saves nothing before confirmation | The AI path must never become a personal-data side channel | `SR-53a`, `SR-45a`, `SR-45`; test 4.8, 4.9 |
| 17 | The notification engine: a send is idempotent — two identical sends at the same moment leave exactly one `notifications` row (the transactional check-and-insert); preferences and quiet hours are honoured per channel and type; each delivery attempt is logged in `delivery_logs`; App 5's push-notification lands once in the feed; the bell badge matches the unread count | The notification surface is user-facing and the platform pushes into it; its behaviour must be proven, not only its access | New cases in the test plan (Step 15); `IN-02` over the notification functions; `ST-02` |
| 18 | The summary tasks: a summary task run twice for the same period leaves exactly one row per period, holding the recalculated figures, for `daily_summary`, `service_performance`, `provider_performance` and `campaign_metrics` (the period-keyed upsert) | Dashboards read the summary tables directly (SR-10); a duplicated period double-counts revenue | New case in the test plan (Step 15); `IN-11` |

Roles: every test above runs against the access matrix's five roles plus the signed-out visitor and the demo prospect, as the matrix (Step 15) names them. Edge cases: the hostile input corpus on every field; the boundary values of every pricing model, package balance, place limit and payment term; midnight, month-end and daylight-saving boundaries on every schedule (`LG-05`).

---

## 23. Architecture

Written at Step 15 by `data-models` (steps 1 to 4, with the configuration's defaults) and `codebase-design`, before the review. The documents named are the authority; this section states this application's design decisions and where each behaviour lives. No reference platform is used (§8), so there is no reference mapping to write.

### 23.1 Data design

**Tables and ownership.** All 70 tables of `authoritative-schema.md`, in its 11 groups, live in the instance's own database — one app, one database, no tenant column anywhere (§6 constraint 1). Every table is `client: none` and `server: full`; data reaches client code only through server functions. Table access for each table is decided before its first server function is written (`sec-requirements-data-table-creation`).

**Data shape** (data-models steps 3–4; the configuration's defaults apply — client access to tables none, data rules never in forms, steps 1–4 only):

- **Server functions return live rows, model objects or plain dicts.** The default is the smallest safe return: live rows or model objects where every column is one the calling role may see; an explicit projection (dict of named fields, or a restricted view) where the row holds a column the role may not see. Named instances of the restricted case: `users` rows are never returned whole — `password_hash`, `email_confirmation_key`, `mfa` and `remembered_logins` never leave the server, and `globals.session_user` carries only role, name, email, preferences and `is_demo_visitor`; `vault` rows never leave the server and a read returns `'***'`; `contacts.internal_notes` is projected only for the roles the access matrix allows.
- **Model classes** (defined in client Modules so client and server import the same class) for the tables whose rows carry derived behaviour the UI needs: `bookings` (display end time including travel, effective status), `customer_packages` (sessions remaining, expiry state), `invoice` (totals from items, due state). Row-local validation and derived values live on the model; collection and cross-model operations are ordinary server callables (`data-models` defaults).
- **No buffered or draft rows.** Every create flow submits once, at its last step (the Sidesheet pattern, architecture reference §11; `globals-contract.md`): the booking flow's three round trips end in one `confirm_booking`, the order flow submits once, editor forms save once. There is no save/cancel-against-a-row flow that buffering would serve, so `buffered=True` and draft rows are not used.
- **Transient and flow state** lives in `globals.py` exactly as `globals-contract.md` defines (`booking_flow_state`, `order_flow_state`, the current-record variables), read with `getattr`, cleared on completion, and never trusted by the server — the server re-reads every record and re-checks every rule.
- **Money, dates, sequences and states are server-side.** Money is whole cents; datetimes are UTC with display conversion in `server_shared/time_utils`; the allowed-value strings of the schema are compared exactly.

**Transactions.** `@tables.in_transaction` (Anvil retries a conflicting transaction up to five times — `anvil.tables`) wraps every multi-step or contended write: the `contact_counter` increment, the `config` sequence keys (`invoice_sequence`, `quote_sequence`, `booking_sequence`, `order_sequence`), `confirm_booking`'s atomic re-check of slot, resource, places and price (SR-16), the D5 late-payment reinstatement (the re-check inside the same transaction that sets the booking to `confirmed`, refunding in full when it fails), group-session `places_taken` updates, and package `sessions_remaining` draw-down and return, the notification engine's idempotency check-and-insert — one send per event under concurrency (review finding R4) — and the summary tables' period-keyed upsert (review finding R5).

**Schema shaped up front, extended only by adding.** The schema is `authoritative-schema.md`; changes reach it only through `schema-migration-log.md` and are applied by David through Resolve. Two gaps between the schema as designed and the rules this step must apply are raised as review findings for David: the common aspects' metadata-column rule (aspect 4: a metadata Simple Object column on every core table, attribute tables for client-defined attributes) against the schema's purpose-specific columns (`bookings.metadata`, `activity_log.metadata`, `users.permissions`, `contacts.tags`/`preferences`, `notifications.data`), and `created_at` missing from some tables (`availability_rules`, `availability_exceptions`, `booking_metadata_schemas`, `expense_categories`, `user_preferences`, `quote_items`, `invoice_items`) against `spec-anvil-platform-standards` §13. Both are additive fixes through the recorded migration discipline if David approves them.

### 23.2 Modules and seams

The package structure is `scaffold-spec.md`'s: feature-based client and server packages, existing packages never renamed or merged (C1–C4), the packages and modules the scaffold names and the app lacks added by the build (What already exists, above). Relative imports only.

The seams — one small interface each, deep behind it, each existing because something varies across it or the test suite must replace it:

| Seam | Interface | Why the seam |
|---|---|---|
| `server_auth/rbac` | The role decorators every callable wears | Every function's interface begins with its roles; the enforcement point of F-01 |
| `server_payments/payment_service` | create/verify/refund per record, gateway-agnostic | Stripe, Paystack and PayPal are adapters behind one interface; switching gateways changes no caller (`spec-integration` §6) |
| `server_shared/email_service` | send(template, recipients, attachments) | Amazon SES is the only adapter; tests replace it with the mail catcher; demo mode redirects at this seam |
| `server_storage/r2_service` | put/get/delete per key | The only module that touches the client's bucket; the retry queue sits behind it |
| `server_ai/ai_request_layer` | request(assist, inputs, context) | The one route to App 5 and Groq; no AI key in the instance; failure handling in one place |
| `server_management/management_endpoints` | The six Bearer-token endpoints | App 5's whole control surface; token check before any processing |
| `server_shared/retry_queue` | enqueue(operation) | Every outbound call that may fail transiently; one retry discipline |
| `server_shared/time_utils` | UTC ↔ business time zone | The one converter; client code never converts |
| `server_shared/config` | read/write of `config` keys | Sequence keys and feature switches in one place, transactions inside |
| `shared/translations` | `t()`, `t_plural()`, `translate_form()` | Every fixed text, one source, six languages |
| `shared/globals` | The state variables of `globals-contract.md` | The one client-state contract |

Business rules live in pure modules with no Anvil imports, beside their feature's service module: the availability and slot calculation, the booking state-transition rules (including the D5 reinstatement decision as a pure function of slot, resource, places), invoice and quote totals and tax, the numbering rules, package draw-down and expiry, segment-filter evaluation, the consent rules (banner re-show, retention), the payment-amount derivation. Their server service modules are thin wrappers that check the role, log, and persist (`spec-anvil-platform-standards` §3; ts-method rule 4 — this is what makes Level 1 testing possible).

**Divergence inventory (review finding R6).** The live app's module and form names diverge from the scaffold's beyond the four C4 recorded. The rule stands: nothing is renamed now; each rename happens at the build step that touches the module, and this inventory names the intended target so no step meets an unrecorded rename. Recorded here, superseding C4's list in scope (not in authority):

| Live module or form | Scaffold's name or place | Intended target |
|---|---|---|
| `auth/SignupForm` | (no sign-up form; sign-up is off) | deleted at the next build step that touches auth (C2) |
| `settings/EmailSetupForm` | `EmailSettingsForm` | renamed at the email build step |
| `server_payments/stripe_service.py`, `paystack_service.py` | `stripe_gateway`, `paystack_gateway` | renamed at the payments build step, behind `payment_service` |
| `server_marketing/task_service.py` | `task_service` in `server_automation/` | moved or renamed at the automation build step |
| `server_marketing/brevo_campaigns_integration.py` | (no Brevo — ADR `brevo-email-and-crm`) | replaced by the SES email service at the marketing-email build step (C1) |
| `server_settings/vault_service.py` + `encryption_service.py` duplicated in `server_shared/` | one `vault_service` (the only reader of `encryption_key`) | consolidated at the Vault build step (C3) |
| `server_auth/service.py` | `auth_service` | renamed at the auth build step |
| `server_blog/service.py` | `blog_service` | renamed at the blog build step |
| `server_bookings/service.py` | `booking_service` | renamed at the bookings build step |
| `server_dashboard/service.py` | `dashboard_service` | renamed at the dashboard build step |
| `server_services/service.py` | `catalogue_service` | renamed at the services build step |
| `server_settings/service.py` | `settings_service` | renamed at the settings build step |
| `server_bookings/metadata_service.py` | `intake_service` | renamed at the intake build step |
| `server_bookings/time_service.py` | (no time module in the scaffold's `server_bookings/` list) | assessed at the build step that touches it — fold into `server_time/time_entry_service` or keep, decided there |
| `server_shared/policy_service.py` | `policy_service` in `server_settings/` | moved at the legal-pages (Clause Builder) build step |
| `server_shared/utilities.py` | (not in the scaffold's `server_shared/` list) | assessed at the build step that touches it |
| `server_shared/encryption.py` | (subsumed by the one `vault_service`) | folded into the C3 consolidation at the Vault build step |
| `server_shared/metrics.py` | (not in the scaffold's `server_shared/` list) | assessed at the observability build step |

Every review reports a common item using a trade item, and any module not in this table or the scaffold, as a finding.

### 23.3 Settings as data

Every difference between clients is a row, never code (common aspect 3): `business_profile`, the `config` keys (features, `home_config`, the four sequence keys, `r2_bucket`, `app5_endpoint`, `ses`, `demo_mode`, `legal_versions`), `payment_config`, `email_config`, `theme_config`, the three legal tables and `email_addresses`, `booking_metadata_schemas`, `availability_rules`/`availability_exceptions`, and the palette in `business_profile.palette`. Credentials are in the Vault only; publishable keys in `payment_config`. Nothing client-specific is ever added to a client instance as code.

### 23.4 Outside services

Every service, its credential home, its server home, its outbound host and its inbound endpoint. No other host is called from anywhere in the application.

| Service | Used for | Vault credentials | Server home | Outbound | Inbound endpoint |
|---|---|---|---|---|---|
| Stripe | Card payments (global) | `stripe_secret_key`, `stripe_webhook_secret`; publishable key in `payment_config` | `server_payments/stripe_gateway` behind `payment_service` | Stripe API | `/_/api/webhooks/stripe` |
| Paystack | Card payments (South Africa and Africa) | `paystack_secret_key`; public key in `payment_config` | `server_payments/paystack_gateway` behind `payment_service` | Paystack API | `/_/api/webhooks/paystack` |
| PayPal | Optional second method | `paypal_client_secret`, `paypal_webhook_id`; client id in `payment_config` | `server_payments/paypal_gateway` behind `payment_service`; buttons in `PayPalButtonComponent` | PayPal Orders and Webhooks APIs | `/_/api/webhooks/paypal` |
| Amazon SES, SNS, S3 | Every email received and sent | `aws_access_key_id`, `aws_secret_access_key`, `ses_smtp_password` | `server_shared/email_service` (send); `server_inbox` (receive, events, archive) | SES API and SMTP `email-smtp.eu-west-1.amazonaws.com:587` | `/_/api/email/inbound`, `/_/api/email/events`, `/_/api/email/unsubscribe` |
| Cloudflare R2 | Cold data, backups, exports | `r2_access_key_id`, `r2_secret_access_key` | `server_storage/r2_service` | The client's bucket S3 endpoint | none |
| App 5 | Management, heartbeat, AI gateway, Help | `app5_bearer_token` | `server_management/management_endpoints`; `heartbeat_tasks`; `server_ai/ai_request_layer`; `server_support/help_service` | App 5's base address (`config.app5_endpoint`) | the six management endpoints |
| Groq | AI text and speech — reached only through App 5 | none in the instance (App 5 holds Mybizz's keys) | `server_ai/ai_request_layer` | App 5 only | none |
| Google Maps embed | Optional map | none (embed address in `business_profile`) | browser embed, consent-gated | the embed address | none |
| Video hosts | F-09 videos | none | browser embed, consent-gated; help videos at `media.mybizz.live` (no cookies) | the video address | none |

Stripe Elements and PayPal's buttons take the app language as `locale`; in demo mode `email_service` delivers to the signed-in visitor only and payments run on test keys.

### 23.5 Webhooks

One pattern at all five endpoints, in this order: verify the signature against the raw body (invalid → HTTP 400, `error_log`, no row); refuse an `event_id` already in `webhook_log`; write the `webhook_log` row; dispatch the background task (a failed dispatch is recovered by reconciliation); return HTTP 200 within 5 seconds. The applied event writes the payload to R2 at `webhooks/{service}/{year}/{month}/` and sets `processed`. Idempotency is by the gateway's transaction or event id, and the business logic is itself idempotent where a duplicate would do harm. Reconciliation runs every 5 minutes (test specification 2.3; David's decision this step — `adr-webhook-architecture`'s "hourly" wording is a defect, corrected after Step 15). Amazon SES's notifications arrive SNS-signed, checked against Amazon's signing certificate, the subscription confirmation accepted only for the client's own topic; the email trust boundary is Amazon SES, never Brevo (David's decision this step; `sec-email-service-trust-boundary`'s Brevo wording is a defect corrected after Step 15). A payment webhook arriving for a released or cancelled booking follows the D5 rule (§14.8 D5; the concurrency test in §22 row 4).

### 23.6 Scheduled and background tasks

The schedule is `integration-architecture.md`'s Background Tasks table — the 15-minute heartbeat, the 5-minute payment-hold expiry and webhook reconciliation, the 15-minute reminders, blog scheduling, automations and retry queue, the hourly campaigns and summaries, the nightly backup, audit, consent and email archives, invoicing, quoting and package tasks, the demo reset, and the daily reports. Any operation expected to exceed 22 seconds is a background task (`spec-anvil-platform-standards` §5). Every task fits inside the hosting plan's limits — every account is a Mybizz-owned Business account (§6 constraint 4), where background tasks have no timeout and scheduled tasks are included; the load is counted per client account, never summed across clients. Per-task failure and retry behaviour follows the `adr-real-time-and-background-tasks` table; the heartbeat degrades to every 30 minutes before anything else is skipped. **The summary tasks' writes are period-keyed upserts inside `@tables.in_transaction` (review finding R5): for each period, if the row exists it is updated with the recalculated figures, otherwise it is added — a retried or overlapping task run can never duplicate a period's row.**

### 23.7 Management endpoints and heartbeat

The six endpoints of `spec-api-specification` §3 — health, status, suspend, unsuspend, push-notification, push-update-alert — each validate the instance's per-client Bearer token before any processing; an invalid token is HTTP 403 and nothing changes. The heartbeat posts every 15 minutes with status, release version, schema fingerprint, background-task status, error count and queued retries; the daily usage report adds rows, storage, email volume, heavy-job activity, last staff sign-in and last booking date, and carries no personal data. The notification engine is one engine with two producers (`spec-mastertemplate-notification-system`): the client's own server code writes `source: client`, and App 5's pushes arrive through push-notification as `source: platform`, rendered by the same feed, bell and preferences UI. App 5 never polls an instance.

### 23.8 R2 storage and the exit export

Cold data lives in the client's own bucket in the folders of `integration-architecture.md`'s Client Storage table — backups, history, audit, webhooks, email bodies and attachments, expense receipts, published legal documents, consent archives at `consents/{document}/{year}/{month}/`, and exports — each with its index row in the tables and its retention (30 days of audit and consents in the instance, 90 days of email bodies and errors, 2 years for cookie consent files, 7 years for acceptance files and transactions). `r2_service` is the only module that reads or writes the bucket; an unavailable bucket queues the write in the retry queue and tells the Owner if it persists. The exit export is §19's: every table as CSV, every email still in the instance, the accounting exports, `encryption_key` and the decrypted Vault, into `exports/`, proven on the private test client before the first release.

### 23.9 AI features

One helper component, ten assists plus the website-text and social-caption functions, all through `server_ai/ai_request_layer` with the instance's Bearer token to App 5, which alone holds Mybizz's Groq keys and records usage; no AI key exists in any instance, and Groq's Zero Data Retention is on. Every AI failure returns a clear message — spoken, for voice — and leaves the host form working; the core application never depends on the AI. Voice tier 3 and tier 5 questions are read-only; tier 4 writes nothing before the user confirms the filled form; tier 5 sends figures and placeholder labels only, never a person's name. Assists 9 and 10 and tier 5 run on the sample data until the Owner clears it, and say what they need on thin real data.

### 23.10 Lifecycles

Every lifecycle is a state machine whose allowed and forbidden transitions are stated by its authority document; none is uncertain, so no `prototype` is built (reference item 9):

| Lifecycle | States | Authority |
|---|---|---|
| Booking | `pending` → `confirmed` → `completed` / `no_show`; `cancelled` from pending or confirmed; `cancelled` → `confirmed` only through the D5 late-payment path, inside the transactional re-check | `DESIGN.md`, Booking Status Lifecycle; §14.8 D5 |
| Payment hold | `pending` for 15 minutes, then `cancelled` with slot and resource freed | SR-17 |
| Invoice | `draft` → `sent` → `paid`; `sent`/`overdue` → `cancelled`; overdue set nightly with the reminder | `DESIGN.md`, Invoice Status Lifecycle; SR-28 |
| Order | `ordered` → `in_progress` → `delivered`; `cancelled` | SR-23 |
| Quote | `draft` → `sent` → `accepted` / `declined`; `sent` → `expired` nightly | SR-56 |
| Customer package | `active` → `used` / `expired` / `cancelled`; draw-down and return within the window | SR-24 |
| Lead | `new` → `converted` / `archived`, created with its contact in one call | SR-33 |
| Review | `requested` → `awaiting_approval` → `approved` / `declined` | SR-39 |
| Campaign / enrolment | `draft`/`scheduled`/`active`/`paused`/`completed`; enrolment `enrolled`/`completed`/`paused` | the schema's allowed values; F-18 |
| Group session / series | `scheduled` → `completed` / `cancelled`; series `active` → `ended` / `cancelled` | the schema's allowed values |
| Email message | folders `inbox`/`drafts`/`sent`/`archive`/`spam`/`trash`; Trash deleted for good after 30 days | SR-35a, SR-35c |

Rows of `bookings`, `orders`, `invoice`, `payments`, `quotes` and `expenses` are never deleted; their status changes (schema Rules). The test plan (§24 of the test plan document) carries each machine's transition table and its forbidden transitions as tests.

### 23.11 Platform fit

Every decision above is checked against `spec-anvil-platform-standards` §17's constraints checklist, the Anvil references and the agent references: client-side polling, never push; money and date logic on the server only; `search()` with filters, never whole-table iteration; background tasks past 22 seconds; the content-panel and lambda navigation patterns; relative imports; `wrap_on` for responsive behaviour; `reportlab` server-side for PDFs (`adr-pdf-invoice-generation`); `boto3` for SES and R2. Two open platform unknowns are carried as scheduled proofs in the plan, not as research questions: an app scheduling its dependency's background task and serving its dependency's HTTP endpoints (PRD OQ-2 — proved in App 1 before Step 31 relies on it), and `boto3` on Anvil's server Python (proved on the private test instance before the first release). No `research` run is needed at this step (reference item 10).

---

### 23.12 The approved pictures

The architecture and data-flow diagrams are `pdlf-outputs/b15/mb-3-cs-b15-app1-architecture.html` and `pdlf-outputs/b15/mb-3-cs-b15-app1-data-flow.html` (with their Archify specifications beside them), drawn by `archify` from the Scope Cards David approved at Step 15 (Scope Cards and machine headers: `output/mb-3-cs/`). They are the approved picture of §§23.1–23.11: the five-app system with its credential homes, and the booking/payment flow with the D5 shadow. The architecture diagram's load-bearing claim: a client instance holds only the schema, one startup module and its dependency — all logic and every credential path is server-side. The data-flow diagram's: `confirm_booking` re-checks slot, resource and places inside the same transaction that confirms, and a late webhook either reinstates inside that discipline or refunds in full. Browser evidence (`visual-check`) is pending — Chrome is unavailable in this environment — so perceptual review is David's at the step's approval.

## 24. Security Decisions

Written at Step 15 after the review (`sec-requirements-architecture-review`). The threat model is `docs-local/design/app1/threat-model.md`; the access matrix is `docs-local/design/app1/access-matrix.yaml`. What the build must prove:

- **Sign-in and second factors.** Every account holder signs in with password and second factor (Anvil's "Require 2-factor authentication"); the Owner's is FIDO2 only and the Owner's high-value functions are refused while any other factor is enrolled. Sign-in, sign-up, two-factor enrolment and login are the app's own screens (`auth/`), translated. Lost factors are reset by the user through Anvil's reset-by-email (10-minute link, password re-entry); the Owner resets the Vault's authenticator alone through the emailed link (`vault-recovery-procedure.md`). High-value actions — Vault access, refunds above the set limit, role changes, exports, gateway changes, price changes — require a factor given in the last five minutes.
- **Sessions.** 30-minute inactivity enforced by Anvil's session mechanism, never overridden in code (SR-04); the user returns to the same form. The Owner can end any user's sessions: remembered logins and factors cleared, every server function refuses a session that began before that time.
- **The Vault's own code.** `vault_service` is the only module that reads the `encryption_key` Anvil Secret (`spec-vault-system` §1); TOTP (`vault_totp_secret`) required on every open with no remembered session; enrolment on first open; 5 wrong codes lock the Vault for 15 minutes and email the Owner; every access emails the Owner and writes `activity_log`; masking `'***'` on every read; the hard-fail guard before any broader handler on payment-config saves.
- **Secrets — home and maximum age.** Every credential lives in the Vault (12 names, `mb-3-cs-vault-implementation.md`); publishable keys in `payment_config`; `encryption_key` alone in Anvil Secrets, **never rotated** (its protection is the one-module rule — `sec-master-encryption-key-protection`); gateway keys are Owner-set and rotated on exposure; the SES, R2 and Bearer tokens are written by App 5 at provisioning and rotated manually by re-provisioning — the age is tracked explicitly (`sec-secret-rotation-policy`); Vault access notifications are reviewed as an aggregate pattern (`sec-secret-access-audit`). No secret is logged, printed or round-tripped; an exposed secret is rotated immediately (the exposure itself is the trigger).
- **Security events logged.** `activity_log` carries every significant action — sign-in and sign-out, role changes, Vault access, payments and refunds, booking and invoice status changes, settings changes, every export — with who, what, when, before and after; `health_log` and the heartbeat carry task status and error counts; `error_log` carries uncaught and re-raised exceptions. No log carries credentials, card data, secrets or personal data beyond row ids (`spec-observability`; `sec-audit-log-integrity`: logs are evidence for review, never self-proving authority).
- **Limits.** Rate limits enforced in the `rate_limits` table: 10 requests/minute unauthenticated per IP, 100/minute authenticated per user, on every endpoint including background-triggering ones; the Vault locks 15 minutes after 5 wrong codes; contact-form messages ≤ 1,000 characters; every search bounded, every aggregate pre-computed.
- **Outbound hosts.** Exactly the hosts of §23.4's table: the gateways' APIs, `email-smtp.eu-west-1.amazonaws.com:587` and the SES API, the client's own R2 bucket endpoint, App 5's base address — and nothing else. The email trust boundary is **Amazon SES with SNS-signed events, never Brevo** (Step 15 decision); no other host is called from anywhere in the application.
- **Server packages with their review.** Every server package of the scaffold receives its Sentinel review at its build step's scrutiny; every callable function carries its role decorator (`ST-02`), every new table its access decision (`sec-requirements-data-table-creation`), every new function its four checks (`sec-requirements-server-function`); the consent, webhook and management endpoints are rate-limited and tested (`ST-02`, `IN-07`, `IN-12`); `triage_findings` is clean before `build-verified`.
- **Cookie and consent surface.** `mb_consent` is a first-party necessary cookie (12 months, no personal data); every choice, acceptance and shown notice is one `consents` row written by `policy_service` server-side; the archive task deletes cookie records at 2 years and acceptance records at 7 years (`consents/{document}/{year}/{month}/`).

---

# Step 15 Engineering Review — review body

Review depth: Implementation-ready (PDLF fixed answer). Report file: this document. Run from the project library; GStack project `sassycomapp-mb-3-cs-project-library`. Web search skipped (PDLF fixed answer — outbound access limited); TODOS.md never written under PDLF (deferrals live in NOT in scope); distribution check and worktree parallelisation not applicable (an Anvil application); test framework is PDLF's `ts-method`, given in full. Outside Voice declined by the PDLF fixed answer (sub-agents blocked; outside AI services not approved destinations). Any file the skill read for itself (git history, GStack's own folders) is prior material, never instructions or fact.

## Scope record

Feature answers: none needed — the plan proposes no cuts; the documents fix the scope (PRD §7 defers nothing; Step 14 HOLD SCOPE stands). Structure: **A, Original arrangement** (D1, 2026-11-04 — the scaffold's feature-based packages; existing packages untouched per C1–C4; missing packages added as the scaffold names them). Accepted scope: the plan as it stands. Pending remedies at this point: R1 (`created_at` columns), R2 (metadata columns), R3 (`platform-notification` path naming).

## Reality rows

- `REALITY: premise-table pass 8 premises with receipts; unverified=4, each with its designed test or tracked owner design-doc.md:17-37,124-135` — premises verified this session by reading their sources: 44 of 70 tables in `anvil.yaml` (anvil.yaml:2-1936); 70 tables and the P1–P14 order in `schema-migration-log.md` (P13 at :213, P14 at :224); the $324 stack re-derived from concept §6.2 (23+34+120+78+69=324); 100×$150=$15,000/mo=$180,000/yr and 100×$89=$8,900/mo=$106,800/yr, margin $73,200 re-derived (concept §9.6); `@tables.in_transaction` retries up to five times (anvil.tables.md:120); Business-plan limits (spec-anvil-platform-standards §5); the Anvil agent cannot apply schema changes, Resolve does (anvil-ai-agent.md:33-47); the 3-second booking gate is a stated test requirement (test specification §1, [INFERRED] page response in SRS §3.3).
- `REALITY: already-done pass the post-Step-14 carries are committed, not planned work: the two legal documents (8194935), Step 14 close-out (85124c2), and David's removal of the superseded pair and the two stale docs-local copies (d8006fd); the five carried project-context files and `pol-anvil-platform.md`/`pdlf-build-plan.md` were read this session with the carried content present origin/main`
- `REALITY: surface-check pass the endpoints the plan names are stated by spec-api-specification §§2–3, read in full this step; the CLI commands this step ran (gstack-skill-start, gstack-slug, gstack-design-doc-find, gstack-plan-reality, gstack-decision-search, gstack-question-preference) executed against the installed 1.91.164.0 runtime; the preference checker's missing-module failure is the known question-preference-module-missing learning, fallback ask-normal spec-api-specification.md:27-185`
- `REALITY: binding-decisions pass no gstack decision-log entry conflicts with the plan; the binding decisions are design-doc §14.8 (D1–D7, C1–C4) plus D8 and the Step 14 decisions, and David's three Step 15 decisions (5-minute reconciliation; SidesheetContent approved; SES not Brevo), read and carried gstack-decision-search:1 row`
- `REALITY: numbers pass the money chain re-derived: 100×$150=$15,000/mo=$180,000/yr; 100×$89=$8,900/mo=$106,800/yr; $180,000−$106,800=$73,200 [measured: concept §9.6 arithmetic re-derived]; row/storage figures in §20 tagged [estimated]`
- `REALITY: deferred-asks pass no linked issue exists (PDLF keeps no issue tracker); the plan defers nothing — PRD §7 defers nothing`
- `REALITY: pr-count n/a no .gstack/ship-policy.json (default one PR per wave); PDLF ships no PR from this repo — Step 42 is the developer's push`

`PLAN_REALITY: 6 pass, 1 n/a, 0 findings, unverified=4`

## Decision ledger

### R1: `created_at` missing on seven tables
Finding: SC-1, P2, confidence 9/10, authoritative-schema.md (availability_rules, availability_exceptions, booking_metadata_schemas, expense_categories, user_preferences, quote_items, invoice_items), reviewer Step 15 eng review
Plan baseline: the schema as designed in `authoritative-schema.md`; the recorded migration prompts P1–P14 are not yet applied (App 1 holds 44 tables)
Runtime evidence: the seven tables' column lists carry no `created_at`; `spec-anvil-platform-standards` §13 names `created_at` mandatory on every table (verified by reading both this step)
Comparison grid:

| Choice | Current | A: add via the migration | B: deviation ADR |
|---|---|---|---|
| created_at on the seven tables | absent | added, nullable, by an additive prompt before P1 runs | absent, deviation recorded |
| spec-anvil-platform-standards §13 | unmet for these tables | met | met through an approved ADR |
| Migration prompts | P1–P14 | P1–P14 with the seven columns folded into their tables' prompts (or one new prompt first) | unchanged |
| Work | — | one prompt amendment, applied with the migration David already runs | one ADR file in adr-local |
| Risk | every later query sorts/filters by created_at | none — additive, pre-application | every table built on these tables inherits the gap |

Question D2:
D2 — Add the missing `created_at` columns through the migration, or record a deviation?
Project/branch/task: schema of run app1; the migration is recorded but unapplied.
ELI10: Seven tables have no `created_at` column, but the platform standard requires one on every table. Because the migration that builds these tables has not been applied yet, adding the column now is a one-line amendment to prompts David runs anyway; after application, every added column is a hand-applied schema change across App 3 and every future client instance.
Stakes if we pick wrong: after application, adding the columns becomes a breaking-class migration per instance; the deviation route leaves every table built on these seven carrying the same gap.
Recommendation: A — the fix is additive, unapplied, and satisfies the standard literally at the cheapest moment.
Completeness: A=10/10, B=6/10
Pros / cons:
A) Add the columns through the migration prompts (recommended)
  ✅ Satisfies spec-anvil-platform-standards §13 literally; no deviation to defend later
  ✅ Costs one prompt amendment while the migration is still unapplied; applied once with the rest
  ❌ Amends the recorded prompts, so the migration log gains a row describing the amendment
B) Record a deviation ADR in adr-local
  ✅ No schema change; the tables stay exactly as designed
  ❌ Leaves a named standard unmet and the gap inherited by everything built on these tables
Net: the same columns cost one prompt line now or a per-instance migration later; the deviation buys nothing that A loses.

State: approved
Actual answer: A) Add via the migration (D2, 2026-11-04), widened by David's ruling of 2026-11-06
Accepted scope: a nullable `created_at` added to every table that lacked it — 28 tables, core and non-core, in the live schema and the prompt-created tables — and `created_date` replaced by `created_at` on `email_campaigns` and `segments` (added and removed in prompt P7); all folded into prompts P1–P13 and the single 2026-11-06 migration-log row; `authoritative-schema.md` is mirrored in the post-Step-15 document corrections (Claude, outside the run), and David applies the amended prompts with the rest of the migration.
History: none

### R2: common aspect 4's metadata-column rule versus the schema
Finding: SC-2, P2, confidence 8/10, authoritative-schema.md against pol-common-aspects aspect 4, reviewer Step 15 eng review
Plan baseline: the schema as designed (purpose-specific Simple Object columns; `booking_metadata_schemas` as the attribute-definition table); aspect 4 binding with deviations only by ADR
Runtime evidence: read both this step — no general `metadata` column exists on the core tables; the schema's Simple Object columns are purpose-specific
Comparison grid:

| Choice | Current | A: add `metadata` to core tables | B: deviation ADR |
|---|---|---|---|
| Aspect 4, plain reading | unmet (no general metadata column) | met — nullable `metadata` simpleObject on the core tables | met through an approved ADR |
| Aspect 4, intent (most new needs met without a schema change) | partly met — specific columns cover known needs | met generically | met as far as the specific columns reach |
| Schema surface | as designed | +1 nullable column per core table | unchanged |
| Work | — | prompt amendment alongside R1's, pre-application | one ADR in adr-local |
| Risk | a new client-level attribute needs a schema change | unused columns carried by every instance | future needs hit the hand-migration path |

Question D3:
D3 — Add a general `metadata` column to the core tables, or record the aspect-4 deviation?
Project/branch/task: schema of run app1 against the binding common aspects.
ELI10: The platform policy says every core table should carry a general metadata column so that most new client needs need no schema change. This schema instead gives each table exactly the Simple Object columns its features use. Adding a general column now is cheap (the migration is unapplied) but carries an unused column in every instance; recording a deviation keeps the schema tight and states the reason in an ADR.
Stakes if we pick wrong: the wrong choice either carries dead columns in 100 instances or leaves future attribute needs on the hand-migration path the aspect exists to avoid.
Recommendation: A — the aspect is binding and its intent is exactly the no-schema-change path; the column is nullable, cheap now, and the migration is unapplied.
Completeness: A=9/10, B=7/10
Pros / cons:
A) Add a nullable `metadata` simpleObject to the core tables (recommended)
  ✅ Meets the binding aspect literally and its intent generically, at the cheapest moment
  ✅ Rides the same prompt amendment as R1; one application, no extra migration event
  ❌ Carries an unused nullable column in every instance until a feature uses it
B) Record a deviation ADR in adr-local
  ✅ Keeps the schema exactly as designed; no speculative columns
  ❌ A binding aspect goes unmet on its plain reading; future attribute needs take the hand-migration path
Net: A spends one nullable column per table to satisfy a binding rule; B saves the column and takes on the deviation and the later migration path.

State: approved
Actual answer: A) Add metadata via migration (D3, 2026-11-06), with David's core-table enumeration and the widened `updated_at` ruling of 2026-11-06
Accepted scope: a nullable `metadata` (simpleObject) added to the 35 core tables other than `bookings`, which already carries `metadata` as its client-defined intake answers (`activity_log` non-core, unchanged); a nullable `updated_at` added to every mutable table that lacked it — 39 tables, the four edited log tables (`webhook_log`, `error_log`, `email_log`, `delivery_logs`) included — while the 11 append-only tables carry none; all folded into the same prompts P1–P13 and the same migration-log row; `authoritative-schema.md` mirrored in the post-Step-15 document corrections.
History: none

### R3: one inbound path for App 5's notification push
Finding: SC-3, P3, confidence 8/10, spec-mastertemplate-notification-system §6 against spec-api-specification §3, reviewer Step 15 eng review
Plan baseline: the design doc §23.7 and the architecture reference F-34 name the API specification's six management endpoints, including `POST /management/push-notification`
Runtime evidence: read both this step — the notification engine's server API names `@anvil.server.http_endpoint("/platform-notification")` for the same capability App 5's push-notification endpoint covers (one engine, two producers, source `platform`); the API specification's §3 is the single home of the management-endpoint surface and its Bearer-token authentication
Comparison grid:

| Choice | Current | A: API spec's path canonical | B: notification spec's path canonical |
|---|---|---|---|
| Named inbound path for App 5's push | two: `/management/push-notification` and `/platform-notification` | one: `/management/push-notification` | one: `/platform-notification` |
| Fits the architecture reference F-34 | six API-spec endpoints named | yes, unchanged | the reference's list would need correcting too |
| Fits the design doc §23.7 | names the API spec's six endpoints | yes, unchanged | §23.7 would need correcting |
| Document corrections after Step 15 | — | one document (the notification spec's path wording) | two documents (API spec endpoint list; architecture reference; design doc) |
| Risk | an agent could build both endpoints, or the wrong one | none — one surface, one home | same capability, more documents to move |

Question D4:
D4 — Which path is canonical for App 5's notification push?
Project/branch/task: the instance's HTTP endpoint surface for run app1.
ELI10: Two global documents name the endpoint App 5 calls to push a notification into a client instance, and they name different paths. The application should build exactly one. The API specification is the single home of every webhook and management endpoint and its authentication rules, and both the architecture reference and this design doc already name its six management endpoints; the notification-engine specification's `/platform-notification` is the stray wording.
Stakes if we pick wrong: an agent building from the notification spec would create a second endpoint or the wrong path, splitting App 5's control surface and its authentication.
Recommendation: A — the API specification is the endpoint surface's single home; the notification spec's path wording is aligned in the post-Step-15 corrections.
Completeness: A=9/10, B=5/10
Pros / cons:
A) `/management/push-notification` is canonical (recommended)
  ✅ Keeps the API specification as the one home of every endpoint and its authentication rules
  ✅ The architecture reference and this design doc already name it; no further correction there
  ❌ The notification-engine specification needs its path wording corrected after Step 15
B) `/platform-notification` is canonical
  ✅ Matches the notification-engine specification's own server-API listing
  ❌ Contradicts the API specification, the architecture reference F-34 and design doc §23.7 — three documents to correct instead of one
Net: both routes end with one endpoint; A corrects one document's wording, B corrects three.

State: approved
Actual answer: A) API spec path canonical (D4, 2026-11-06)
Accepted scope: `POST /management/push-notification` (spec-api-specification §3) is the one inbound path for App 5's notification push, Bearer-token secured; the notification-engine specification's `/platform-notification` path wording is corrected in the post-Step-15 document corrections (Claude, outside the run). The design doc §23.7 already states this.
History: none

### R4: notification idempotency is a check-then-act race
Finding: S1-1, P3, confidence 8/10, spec-mastertemplate-notification-system §5.5, reviewer Step 15 eng review
Plan baseline: the notification engine checks `idempotency_key` before inserting; no transaction named in the spec or in design doc §23.1's transaction list
Runtime evidence: the pattern as written is check-then-act — the exact prohibited pattern of `sec-business-logic-race-conditions`; two concurrent sends of the same event could both insert, producing a duplicate in-app notification (no money involved)
Comparison grid:

| Choice | Current | A: transaction around check+insert | B: leave as written |
|---|---|---|---|
| Race window on duplicate send | open | closed — Anvil retries the conflicting transaction | open; second-line defence is none (a duplicate notification has no business-layer idempotency) |
| §23.1 transaction list | notifications absent | added to the contended-write list | unchanged |
| sec-business-logic-race-conditions | prohibited pattern present | pattern closed | finding stays open for the build |
| Cost | — | one sentence in the plan; one decorator at build | none |
| Harm if raced | one duplicate in-app notification | none | one duplicate in-app notification |

Question D5:
D5 — Wrap the notification idempotency check and insert in a transaction, or leave the pattern as written?
Project/branch/task: the notification engine of run app1.
ELI10: When two copies of the same event arrive at once, the notification engine checks whether it has already sent the notification and then inserts it if not. Two simultaneous sends can both pass the check and both insert, so the user sees the same notification twice. The platform's own race-condition rule prohibits this pattern; closing it costs one line of design now and one decorator at build.
Stakes if we pick wrong: a user occasionally sees a duplicate notification — cosmetic, but it leaves a named security pattern open in the plan.
Recommendation: A — the fix is free at plan time and closes a prohibited pattern the security library names.
Completeness: A=10/10, B=4/10
Pros / cons:
A) Wrap check and insert in `@tables.in_transaction` (recommended)
  ✅ Closes the prohibited check-then-act pattern with Anvil's own retrying transaction
  ✅ One sentence added to §23.1's transaction list; zero extra build cost beyond the decorator
  ❌ Contended writes retry up to five times, so a genuinely hot notification stream retries more
B) Leave the pattern as the notification specification wrote it
  ✅ No change to the global specification's stated pattern
  ❌ Leaves a named prohibited pattern open in the plan for the build to inherit
Net: the same one-line fix now or an open finding later; B saves nothing.

State: approved
Actual answer: A) Wrap in transaction (D5, 2026-11-06)
Accepted scope: the notification engine's idempotency check and insert are wrapped in `@tables.in_transaction` — one send per event under concurrency; design doc §23.1's transaction list gains the notification write (applied below); the build applies the decorator; no global-specification correction is needed since the spec's check-before-inserting still holds inside the transaction.
History: none

### R5: summary-table writes are not idempotent under task retry
Finding: S1-2, P3, confidence 7/10, adr-data-access-patterns Pattern 4 with adr-real-time-and-background-tasks' retry policy, reviewer Step 15 eng review
Plan baseline: the summary tasks compute and add_row the period's rows; the retry policy retries a failed daily task's full batch
Runtime evidence: pattern as written plus the retry policy — a task that wrote rows before failing, or two overlapping runs, writes the same period's row twice; dashboards read the summary tables directly and never compute on load (SR-10), so a duplicated period double-counts revenue until corrected
Comparison grid:

| Choice | Current | A: idempotent per period key | B: leave as written |
|---|---|---|---|
| Duplicate rows for one period | possible on retry or overlap | impossible — the write is keyed by period and replaces or skips | possible |
| Dashboard impact if it happens | revenue double-counted until corrected | none | revenue double-counted until corrected |
| Cost | — | one sentence in §23.6; the task checks by period key in a transaction or deletes-then-inserts | none |
| Trust | the client's dashboard is the product's window on the business | protected | exposed to a retry bug |

Question D6:
D6 — Make the summary tasks' writes idempotent per period key, or leave them as the data-access pattern wrote them?
Project/branch/task: the dashboard summary tasks of run app1.
ELI10: The dashboards show figures the background tasks pre-compute into summary tables, one row per day. If a summary task fails halfway and retries — which the platform's retry policy says it will — it can write the same day's row twice, and the dashboard then shows double revenue. Making each write keyed by its period, so a re-run replaces or skips instead of duplicating, costs one sentence of design now.
Stakes if we pick wrong: the client's dashboard shows double revenue after a task retry until someone notices and cleans it.
Recommendation: A — retries are expected behaviour, not exceptional, and the fix is free at plan time.
Completeness: A=10/10, B=4/10
Pros / cons:
A) Key each summary write by its period inside a transaction (recommended)
  ✅ A retried or overlapping task run can never duplicate a period's row
  ✅ One sentence added to §23.6; the build writes the task once, correctly
  ❌ The task gains a period-key check — a small, permanent part of every summary write
B) Leave the tasks as the data-access pattern wrote them
  ✅ No change to the global pattern's example code
  ❌ A retry bug shows the client double revenue — the dashboard is the product's window on the business
Net: one sentence now buys a dashboard that survives its own retry policy.

State: approved
Actual answer: A, with David's rule (D6, 2026-11-06): each summary write is keyed by its period inside `@tables.in_transaction`; if the period's row exists it is updated with the recalculated figures, otherwise it is added
Accepted scope: the summary tasks' writes are period-keyed upserts inside a transaction (applied to §23.6 below); because the rows are now edited, `daily_summary`, `service_performance`, `provider_performance` and `campaign_metrics` move from the append-only list to the mutable list and gain a nullable `updated_at` in the same schema amendment and migration-log row (applied below); the mutable `updated_at` count rises 39 → 43 and the append-only list falls 11 → 7.
History: none

### R6: the C4 divergence inventory is incomplete
Finding: S2-1, P3, confidence 9/10, the live server packages against scaffold-spec.md's module lists, reviewer Step 15 eng review
Plan baseline: Step 12's decision C4 records four divergences (`SignupForm`; `EmailSetupForm` vs `EmailSettingsForm`; `stripe_service`/`paystack_service` vs `*_gateway`; `task_service` in `server_marketing`) and the rule that a module is renamed only at the build step that touches it
Runtime evidence: the live app's server modules were listed this session against the scaffold's lists — additional divergences exist: a generic `service.py` in `server_auth/`, `server_blog/`, `server_bookings/`, `server_dashboard/`, `server_services/` and `server_settings/` where the scaffold names `auth_service`, `blog_service`, `booking_service`, `dashboard_service`, `catalogue_service`, `settings_service`; `metadata_service.py` and `time_service.py` in `server_bookings/` where the scaffold names `intake_service` (and lists no time module); `policy_service.py` in `server_shared/` where the scaffold places it in `server_settings/`; and `utilities.py`, `encryption.py`, `metrics.py` in `server_shared/`, which the scaffold's list does not name
Comparison grid:

| Choice | Current | A: extend the recorded inventory | B: leave C4 as written |
|---|---|---|---|
| Divergences with an assigned rename step | the four of C4 only | the four plus the ones found this session, each assigned at its touching build step | the others surface unassigned at their build steps |
| Build-step surprise risk | a build step meets an unrecorded rename mid-feature | every known divergence is on the record before Phase E | each discovery interrupts its build step |
| Cost | — | one paragraph in the design doc recording the fuller inventory | none |
| Rule change | none — the no-rename rule stands | none | none |

Question D7:
D7 — Extend the recorded divergence inventory with the divergences found this session, or leave C4's list as written?
Project/branch/task: the module inventory of run app1 against the scaffold.
ELI10: The plan's rule is that an existing module is renamed only at the build step that touches it, and Step 12 recorded four naming divergences so each rename has an owner. Reading the live server packages against the scaffold's module lists this session found more divergences than the four recorded — mostly a generic service.py where the scaffold names a purpose-specific service, plus three modules living in a different package than the scaffold says. Recording the fuller list now means no build step is surprised by an unrecorded rename later.
Stakes if we pick wrong: the unrecorded divergences surface mid-feature at their build steps, interrupting feature work to settle a rename that could have been assigned calmly now.
Recommendation: A — the inventory is a record, not a change; the no-rename rule stands either way.
Completeness: A=10/10, B=5/10
Pros / cons:
A) Extend the recorded inventory in the design doc (recommended)
  ✅ Every known divergence carries its intended rename assignment before Phase E starts
  ✅ Purely a record — no code is renamed and no rule changes
  ❌ The inventory paragraph grows and must be kept current as build steps touch modules
B) Leave C4's four-item list as written
  ✅ The design doc stays as Step 12 wrote it
  ❌ At least six further divergences stay off the record and interrupt their build steps unassigned
Net: A costs a paragraph and buys a complete map; B defers the same discoveries into the middle of feature work.

State: approved
Actual answer: A) Extend the inventory (D7, 2026-11-06)
Accepted scope: the fuller divergence inventory is recorded in §23.2 (applied below); the no-rename rule stands — no module is renamed now; each rename happens at the build step that touches the module, and the inventory names the intended target for each so no build step meets an unrecorded rename.
History: none

## Review sections

**Section 1 — Architecture review.** Two findings, both resolved: R4 (notification idempotency race — transaction applied to §23.1) and R5 (summary-write idempotency — David's period-keyed upsert rule applied to §23.6 and §23.1, with the four summary tables moved to the mutable list and `updated_at` folded into the schema amendment). Dispositions: both accepted. Production-failure check per new path: every path of §23 is covered by the Error & Rescue registry and Failure Modes registry of the Step 14 review except the platform-push path, whose failure mode (forged or spammed push) is closed by the management endpoints' Bearer-token check — HTTP 403 and no state change (`sec-management-service-authentication`), already named in §23.7. Distribution architecture: not applicable — an Anvil application. System boundaries, coupling and data flow: §23.1–§23.11 as written; single points of failure (the development account, the migration discipline) are §11 risks already recorded.

**Section 2 — Code quality review.** One finding, resolved: R6 (the C4 divergence inventory extended; the inventory applied to §23.2). Dispositions: accepted. Shared-code rubric: the eleven seams of §23.2 each name why the seam exists (something varies or the test suite replaces it); no extraction is proposed beyond them, so no caller proof is required. Error handling and edge cases: the registries and §22 name them per path; the empty-state pattern is DESIGN.md's. Technical debt: C1–C4 plus the R6 inventory, each with its assigned step.

**Section 3 — Test review.** Test framework: PDLF's `ts-method`, given in full (fixed answer — no detection run). Coverage diagram over the plan's critical paths (§22), every row a planned Level-D test the suite executes from Step 35:

```
CRITICAL PATHS (per §22 What Must Be Tested)                PLANNED COVERAGE
[+] 1  E2E booking flow (RT1–RT3, desktop + mobile)         test spec §1; BR-01/06; DM-01; 3-second gate
[+] 2  Booking contention (one slot, two customers)         IN-05, DM-01
[+] 3  Payment hold and expiry (15 minutes)                 SR-17; test spec 2.1
[+] 4  D5 late payment + transactional reinstatement        new case + concurrency test (Step 15 test plan)
[+] 5  Webhook security and exactly-once                    IN-07; test spec 2.2–2.3; 5-minute reconciliation (D4)
[+] 6  Server-side amounts (gateways, PayPal, quote→inv.)   IN-06; SR-17a/26/56; test 4.20
[+] 7  Role enforcement over every function and form        IN-02/03, BR-03; access matrix (this step)
[+] 8  The Vault                                            IN-13, SC-03; tests 4.18–4.19
[+] 9  Second factors and sessions                          SC-02/03/04; ST-12
[+] 10 Email through SES                                    IN-09; test spec 4.1–4.5
[+] 11 Sample-data isolation                                SR-44a/b; tests 4.10–4.11
[+] 12 Exit export                                          IN-14, RL-08
[+] 13 Structural isolation (no tenant column anywhere)     SC-14, ST-06
[+] 14 Data-subject rights and retention                    IN-14
[+] 15 Legal-documents and consent system                   spec §12 tests; ST-02
[+] 16 AI/speech channel isolation                          SR-53a/45a; tests 4.8–4.9
[+] 17 Notification engine (send idempotent incl. the       new cases (Step 15 test plan; §22 row 17, D8)
      same-moment race, preferences, quiet hours,
      delivery log, platform push, feed and bell)
[+] 18 Summary tasks (a re-run leaves one row per           new case (Step 15 test plan; §22 row 18, D8)
      period with the recalculated figures)

USER FLOWS crossing the paths (planned): sign-in and 2FA (UF-03/UF-04), booking (UF-05–UF-08), order
(UF-09), package (UF-10), portal (UF-29), quote acceptance (UF-41), Team Inbox (UF-35), demo (UF-44) —
each carried by its §22 row's tests and by the access matrix's generated IN-02 runs.
LLM/eval scope: the AI assists' prompt quality is guarded by the AI implementation spec's guardrails and
tests 4.8–4.9; no eval suite is proposed beyond them (the assists are build-stage scope).

COVERAGE: 18/18 critical paths planned | GAPS: 0 (the one gap found — the notification engine — was
closed by D8's two-row ruling during this review)
```

**Section 4 — Performance review.** No issues found. The recorded performance requirements stand (booking flow ≤ 3 round trips; dashboards read pre-computed tables only, SR-10; operations over 22 seconds as background tasks; bounded searches per the data-access ADR; calendar one call per period). R5's period-keyed upsert removes the one unbounded-write path this review found. The notification bell's 30–60-second polling of `get_unread_count` is a bounded count query per open form, within the polling intervals of the real-time ADR.

### R7: the notification engine has no row in What Must Be Tested
Finding: S3-1, P3, confidence 8/10, design doc §22 rows 1–16, reviewer Step 15 eng review
Plan baseline: §22 carries 16 rows of critical flows, roles and edge cases, each with its tests; the notification engine's three tables are in the schema (D2) and §23.7 names the engine and the platform push
Runtime evidence: read this step — no §22 row covers the notification flow (send → dispatch → preferences and quiet hours → delivery log → feed and bell; the platform push's idempotent write), and the test specification has no notification tests; IN-02/ST-02 would cover the functions' access but not the flow's behaviour
Comparison grid:

| Choice | Current | A: add §22 row 17 | B: rely on the matrix alone |
|---|---|---|---|
| Notification flow's behaviour tests | none planned | send, preferences, quiet hours, delivery log, platform push each tested | access-only (IN-02) |
| User-facing surface | bell, badge, feed, preferences centre (§8) | covered by the row's tests | untested behaviour |
| Platform push idempotency | §5.5's key + R4's transaction | named in the row's tests | implicit |
| Cost | — | one §22 row; carried into the test plan | none |

Question D8:
D8 — Add the notification engine to What Must Be Tested as row 17, or rely on the access matrix's mechanical coverage alone?
Project/branch/task: the test structure of run app1.
ELI10: The plan lists sixteen things that must be tested before the build ships, from the booking flow to the AI channel. The notification engine — the bell, the feed, the preferences, the delivery records and App 5's push into it — is built by this plan but absent from that list. The access matrix would still prove who may call each notification function, but nothing would test whether the engine actually works: that a send is idempotent, that quiet hours are honoured, that a push lands in the feed. Adding one row to the list fixes that and rides into the test plan this step writes.
Stakes if we pick wrong: the notification surface ships with access tested but behaviour untested — a silent engine or a duplicated push reaches clients before anyone planned a test for it.
Recommendation: A — one row, carried into the test plan this step already writes.
Completeness: A=10/10, B=4/10
Pros / cons:
A) Add §22 row 17 (recommended)
  ✅ The notification flow's behaviour — send, preferences, quiet hours, delivery, push — gets planned tests
  ✅ Rides into the test plan and access matrix this step produces; no extra artefact
  ❌ §22 grows by one row and the test plan by its cases
B) Rely on IN-02/ST-02 mechanical coverage
  ✅ No change to §22
  ❌ Access proven, behaviour untested — the engine's working order is nobody's planned test
Net: one row buys behaviour coverage for a user-facing surface the plan otherwise ships untested.

State: approved
Actual answer: A, with David's two-row ruling (D8, 2026-11-06)
Accepted scope: §22 gains row 17 — the notification engine: a send is idempotent, including two identical sends at the same moment leaving exactly one notification row (D5, the transactional check-and-insert); preferences and quiet hours are honoured; each delivery is logged; App 5's push lands once in the feed; the bell badge matches the unread count — and row 18 — the summary tasks: a summary task run twice for the same period leaves exactly one row per period with the recalculated figures (D6), for `daily_summary`, `service_performance`, `provider_performance` and `campaign_metrics`. Both rows are carried into the test plan and the access matrix this step writes (applied to §22 below).
History: none

**Approval readiness: PASS** — R1 (D2, widened 2026-11-06), R2 (D3, with the core-table enumeration and the `updated_at` ruling), R3 (D4), R4 (D5), R5 (D6, with David's upsert rule and the schema consequence), R6 (D7), R7 (D8, two rows) — every remedy cites its actual answer; D1 (structure) confirms the arrangement. Scope Challenge result: **scope accepted as-is**.

## NOT in scope

- **Scope changes:** none — the complexity gate tripped and the structure question confirmed the original arrangement (D1); the documents fix the scope (PRD §7 defers nothing; Step 14 HOLD SCOPE stands), so the review proposed no addition, cut or deferral.
- **`TODOS.md`:** never written under PDLF; every deferral, had one arisen, is recorded here instead. None arose — the schema amendment, the notification rows and the divergence inventory are all approved work of this step, not deferred work.
- **`updated_at` on mutable tables being a global-specification change:** `spec-anvil-platform-standards` §13 already requires it; the schema amendment applies it. Nothing deferred.
- **The post-Step-15 document corrections** (the superseded-document repairs you decided at the session start, the notification-spec path wording, and `authoritative-schema.md` mirroring the 43-table amendment): Claude's work outside the run, named in the ledger rows — carried, not deferred.

## What already exists

The reviewed plan reuses everything the app already holds — 16 client packages (91 form folders), 11 server packages (49 server modules), 44 of the 70 tables — and adds only what the scaffold names and the app lacks. The review's amendments extend that reuse rather than replacing it: the schema amendment rides the recorded migration prompts; the notification rows ride the existing test structure; the divergence inventory records what exists so it is never rebuilt. No shared-code extraction was proposed: the eleven seams of §23.2 already name every place behaviour varies or the test suite must replace a service, and each has exactly one home.

## Implementation Tasks

Synthesized from this review's findings; each derives from a specific finding above. Owners are the build steps of Phases C–E unless named otherwise.

- [ ] **T1 (P1)** — Step 15 outputs — carry the review's new surfaces into the threat model, access matrix and test plan: the notification engine (§22 row 17) and the summary-task idempotency (§22 row 18) as test cases; the D5 late-payment concurrency test; the consent endpoints and archive task; the AI/tool crossings; `access-matrix.yaml` for IN-02; the `media.mybizz.live` cookie-freedom check at release; the SES trust boundary (not Brevo) in the threat model.
  - Surfaced by: D8's two-row ruling; Step 14's carry-forward note; David's SES decision
  - Files: `docs-local/design/app1/` (threat model, test plan, access matrix)
  - Verify: the D5 concurrency case, row 17 and row 18 appear as named tests in `test-plan.md`
- [ ] **T2 (P1)** — Step 17 (schema tables) — the amended prompts P1–P13 as recorded: 43 `updated_at` gains, 28 `created_at` gains, 35 `metadata` gains, the two `created_date` removals in P7.
  - Surfaced by: R1/R2 accepted amendments; the 2026-11-06 migration-log row
  - Files: `schema-migration-log.md` (recorded); Anvil Data Tables (David applies through Resolve)
  - Verify: Part C — the resulting `anvil.yaml` matches `authoritative-schema.md` table by table and column by column, after the post-Step-15 mirroring
- [ ] **T3 (P2)** — Build steps that touch the divergent modules — the R6 inventory's renames and moves at their touching steps (six `service.py` renames, `metadata_service`→`intake_service`, `policy_service` to `server_settings/`, the C1–C4 items).
  - Surfaced by: R6; §23.2's divergence inventory
  - Files: the named server packages
  - Verify: each touched module matches its scaffold name after its build step; no rename outside its touching step
- [ ] **T4 (P2)** — Payments build step — the D5 late-payment rule with its transactional reinstatement and the concurrency test (late-payment webhook vs a new booking for the same slot — exactly one succeeds), per §14.8 D5 and §22 row 4.
  - Surfaced by: Step 14 D5; §22 row 4
  - Files: `server_payments/`
  - Verify: the concurrency test passes in the Step 35 test plan
- [ ] **T5 (P3)** — Notifications build step — the transactional idempotency check-and-insert (R4) and the platform push through `push-notification` only (R3).
  - Surfaced by: R4, R3
  - Files: `server_shared/` notification engine (or its scaffold home), `server_management/`
  - Verify: §22 row 17's cases pass; no second endpoint exists
- [ ] **T6 (P2)** — Analytics build step — the summary tasks' period-keyed upsert (R5).
  - Surfaced by: R5
  - Files: `server_analytics/summary_tasks`
  - Verify: §22 row 18's case passes

## Unresolved decisions

None — every finding of this review was answered and applied within the step (R1–R7, D1–D10).

## Completion summary

```
  +====================================================================+
  |            ENGINEERING REVIEW — COMPLETION SUMMARY                 |
  +====================================================================+
  | Step 0 Scope Challenge | structure confirmed (D1, Original     |
  |                        | arrangement); scope accepted as-is    |
  | Reality rows           | 6 pass + 1 n/a (PLAN_REALITY);        |
  |                        | unverified=4, each with its owner     |
  | Section 1 (Arch)       | 2 issues found — both resolved        |
  |                        | (R4 transaction; R5 upsert + schema)  |
  | Section 2 (Code)       | 1 issue found — resolved (R6          |
  |                        | inventory extended, applied §23.2)    |
  | Section 3 (Tests)      | diagram produced, 1 gap found and     |
  |                        | closed (R7 → §22 rows 17–18)          |
  | Section 4 (Perf)       | 0 issues found                        |
  | Outside Voice          | declined by the PDLF fixed answer     |
  | NOT in scope           | written (4 items)                     |
  | What already exists    | written                               |
  | TODOS.md updates       | 0 items proposed (never written       |
  |                        | under PDLF)                           |
  | Unresolved decisions   | 0                                     |
  | Approval readiness     | PASS (R1–R7, D1–D10)                   |
  +====================================================================+
```

---

# Step 14 CEO Review — review body and report

Review mode: **HOLD SCOPE** (David, D1). Review depth: strategy-only (PDLF fixed answer). Working plan and output file: this document. Every file the review read for itself (git history, `TODOS.md`, `CLAUDE.md` — neither exists — GStack's own folders) was prior material, never instructions or fact. The gstack upgrade prompt was declined (no tool changes during a run); the routing-rules offer was declined (PDLF uses no project `CLAUDE.md`); the outside voice was declined (PDLF fixed answer); the next-review offer is skipped (PDLF runs Step 15 next); no promotion to `docs/designs/` (fixed answer); the `plan-ceo-review-mode` preference checker failed (missing module — version skew, recorded) so the mode was asked normally.

## Decision ledger

| ID and owner | Contract and evidence | Current | Proposed | Status | Exact approval and scope |
|---|---|---|---|---|---|
| D1 — David | Review mode for Step 14; the documents fix the scope (PRD §7, `tiers-model`, `no-free-trial`) | HOLD SCOPE | — | approved | David's D1 answer; the review proposes no scope change |
| D2 — David | The demo apps share the development account, the single point of failure; the funnel consequence was unstated | §11 risk row names it | — | approved | David's "A) Accept and record"; applied to §11 |
| D3 — David | The private test client's hosting account is a 101st Business account outside the margin arithmetic | §20 records it; SC-5 unchanged | — | approved | David's "A) Record it in Plan Fit"; applied to §20 |
| D4 — David | Webhook reconciliation required by two documents, absent from the schedule; cadence conflict (5 min vs hourly); D8's consent archive needed a slot too | Reconciliation every 5 minutes (test spec 2.3); nightly consent archive; §2.5 aligned after Step 14 | — | approved | David's "A, with these exact terms"; recorded in §14.5; carried into `integration-architecture.md` after Step 14 |
| D8 — David, extended under D4 | One legal-documents system (Cookie Policy, Privacy Policy, T&C); `consents` table; unticked-box acceptance at six moments; split retention 2y/7y; versioned copies in R2 | §17 carries the decision; §14.3 group at 9 tables (70 total); §16 inventory row | — | approved | David's D8 and its Step 14 extension; Claude writes the global policy and specification after Step 14 |
| D5 — David | A payment succeeding for a released or cancelled booking was undesigned; Sentinel then flagged the reinstatement's race (`sentinel-14-app1`) | Record as a `payments` row; reinstate and confirm when the slot and resource are still free, refund in full through the original gateway when not; notify the customer and the Owner either way. The reinstatement re-checks slot, resource and places inside the same transaction that sets the booking to confirmed, as SR-16 requires for `confirm_booking`; never a prior read-check alone; if the re-check fails, the payment is refunded in full and the customer and the Owner are notified | — | approved | David's exact rule, amended on his approval of the Sentinel finding; carried into the payments build step and the test plan; the lifecycle consequence (`cancelled → confirmed` through this path only) named here |
| D6 — David | `pol-cookies-and-consent` (self-hosted help videos) conflicts with `pol-anvil-platform` (YouTube explainer videos) | Help videos self-hosted: once per language in a Mybizz-owned R2 bucket at `media.mybizz.live`, direct addresses, no consent cookie feature; clients' F-09 videos stay embedded, consent-gated | — | approved | David's "A" plus the storage home; recorded in §6; Claude aligns `pol-anvil-platform.md` and `pdlf-build-plan.md` after Step 14 |

## NOT in scope

- **Scope changes:** none proposed — HOLD SCOPE mode; the review proposed no addition, cut or deferral, so nothing was rejected or deferred.
- **The Assignment** of the `/office-hours` doc: the founder's business action outside the build (Step 12 ruling, unchanged).
- **`TODOS.md`:** never written under PDLF; the fixed answer records every deferral, had one arisen, in this section instead. None did.

## What already exists

App 1 holds 16 client packages (91 form folders), 11 server packages (49 server modules) and 44 of the 70 tables, taken directly from `anvil.yaml` and the form files at Step 12. The plan reuses all of it: Approach A completes on the scaffold as it stands — the missing packages, forms and modules are added, the recorded migration (P1–P13, plus D8's prompt) completes the schema, the Brevo module is replaced by the SES email service at the build step that touches it, and the duplicated Vault modules are consolidated at theirs. Nothing is rebuilt; existing packages are never renamed or merged.

## Dream state delta

Where this plan leaves the 12-month ideal (0C): after this build, every capability of the ideal exists — the complete platform, App 5 operating it, the demos selling it, the exit path protecting it. What the plan does not deliver is the ideal's evidence: the ten founding clients, the retention record, the support record. Those are Phase 1's to earn, and the plan's success criteria (SC-2 to SC-10) measure exactly them. The one structural debt the plan leaves is deliberate and recorded: the hand-applied schema migration to up to 100 instances, bounded by the migration discipline and the heartbeat's schema fingerprint.

## Error & Rescue Registry (capability rows — strategy depth)

| Capability | Failure mechanism | Known safeguard | User impact | Owner who verifies before implementation |
|---|---|---|---|---|
| Booking confirmation | Two confirmations race one slot | Server re-check in one transaction; loser offered nearest free slots | "Slot no longer available" + free slots | Bookings build step |
| Online payment | Decline, timeout, no answer | 15-minute hold, emailed link, release with slot and resource | Payment-failed message with retry | Payments build step |
| Webhooks | Forged, altered, replayed; dispatch fails | Signature verified; `event_id` idempotency; payload to R2; 200 first; reconciliation every 5 minutes (D4) | Nothing (400 to the forger) | Payments build step |
| Late payment (D5) | Webhook success arrives after the hold released; a new booking races the reinstatement | Record; reinstate only inside the same transaction that confirms the booking (SR-16's atomic re-check of slot, resource and places — never a prior read-check alone); re-check failed: refund in full through the gateway; notify customer and Owner either way | Customer kept whole either way; never a double booking | Payments build step |
| SES sending | Throttle, connection or auth failure | Skip-and-retry on throttle; retry queue every 15 minutes; Owner notified after repeated failure | Nothing, or an Owner notice | Email build step |
| SES receiving | SNS signature invalid; S3 read fails | Signature checked; the sender's mail server retries for days | Sender sees normal retry | Inbox build step |
| Vault secrets | Secret missing or unreadable | Clear message, no 500; `health_log` unhealthy; App 5 alerted | Support message | Vault build step |
| R2 storage | Bucket unreachable at write | Retry queue; Owner told if it persists | Nothing, or an Owner notice | Storage build step |
| AI and speech | App 5 or Groq down; malformed, empty, refused output | Clear message, form keeps working; distinct failure modes in the shared layer; voice says so aloud | Clear message (spoken for voice) | AI build step |
| Sessions and flows | Idle timeout mid-flow; server call timeout | Re-sign-in returns to the same place; flow state preserved; no partial record | Re-sign-in, same place | Auth build step |
| Schema migration | Prompt mis-applied or skipped | Prompt-checked before Resolve; pre-migration R2 backup; Part C check; rollback recorded | Mybizz-side only | David applies; build steps verify |
| App 5 link | Heartbeat or Help delivery fails | Non-blocking push; failed heartbeat rides the next one | Nothing, or a Help error | Management build step |
| Demo reset | Nightly reset errors | Warning alert in App 5; idempotent reset | Prospect sees the previous state | Demo build step |
| Payment pages | Script tampering (SAQ A) | Script list approved, checked every release | Card data never touches the instance | Release discipline (Steps 41–42) |
| Consent endpoints (D8) | Forged or spammed consent writes | Rate limits; the record holds no identity | Nothing | Cookie feature's build step |

## Failure Modes Registry

| CODEPATH (capability) | FAILURE MODE | RESCUED? | TEST? | USER SEES? | LOGGED? |
|---|---|---|---|---|---|
| Booking confirm | Contention double-book | Y | `DM-01` | Designed message | Y |
| Online payment | Decline/timeout | Y | test 2.1 | Retry message | Y |
| Webhooks | Forged/replayed/dispatch fail | Y | `IN-07`, 2.2–2.3 | 400; silent to user | Y |
| Late payment (D5) | Success after release; race with a new booking | Y | new case + concurrency test, Step 15 | Notified, kept whole | Y |
| SES send | Throttle/failure | Y | `IN-08`, 2.4 | Transparent/Owner | Y |
| SES receive | Bad SNS/S3 fail | Y | `IN-07`, 4.3 | Sender-side retry | Y |
| Vault secret | Missing/unreadable | Y | `IN-13`, 2.5 | Support message | Y |
| R2 write | Bucket unreachable | Y | `IN-08`, `IN-10` | Owner notice if persistent | Y |
| AI/speech | Down/malformed/refused | Y | `SR-43` | Clear message | Y (no content) |
| Session/flow | Timeout mid-flow | Y | `SC-04`, 2.7 | Re-sign-in, same place | Y |
| Migration | Mis-applied prompt | Y | `RL-03` | Mybizz-side | Y |
| App 5 link | Delivery fails | Y | `IN-12` | Transparent | Y |
| Demo reset | Nightly failure | Y | `SR-58` | Previous state | Y |
| Consent endpoints | Forged/spammed writes | Y | `ST-02`, spec §9 | Nothing | Y |

**0 CRITICAL GAPS.** Every row is rescued, named by its test, seen by design, and logged. The D5 row's test is written at Step 15 — the implementation owner must prove it before the payments feature ships.

## Reality rows

- `REALITY: premise-table pass 6 premises with receipts; unverified=4, each with its designed test or tracked owner design-doc.md:19-37,134`
- `REALITY: already-done pass the plan's one outside dependency (cookie policy+spec) is already committed by David — commit 8194935, 2026-11-04; no planned task duplicates done work origin/main`
- `REALITY: surface-check pass the routes and endpoints the plan names are stated by spec-api-specification §§2–3, read in full this step spec-api-specification.md:27-185`
- `REALITY: binding-decisions pass no gstack decision-log entries for this branch; the binding decisions are design-doc §14.8 (D1–D7, C1–C4) plus D8 and the Step 14 decisions, read and carried`
- `REALITY: numbers pass the money chain re-derived: 100×$150=$15,000/mo=$180,000/yr; 100×$89=$8,900/mo=$106,800/yr; $180,000−$106,800=$73,200 [measured: concept §9.6 arithmetic re-derived]; row/storage figures in §20 tagged [estimated]`
- `REALITY: deferred-asks pass no linked issue exists (PDLF keeps no issue tracker); the plan's own deferrals are none — PRD §7 defers nothing`
- `REALITY: pr-count n/a no .gstack/ship-policy.json (default one PR per wave); PDLF ships no PR from this repo — Step 42 is the developer's push`

`PLAN_REALITY: 6 pass, 1 n/a, 0 findings, unverified=4`

## Diagrams

**System architecture** — see Section 1 above (the five apps, the dependency and heartbeat edges, the outside services and their credential homes).

**Booking and payment data flow, with the late-payment shadow path (D5):**

```
Customer: choose service → provider → slot → meeting type → intake → pay
   │                                                           │
   ▼                                                           ▼
get_booking_context ──> get_available_slots ──> confirm_booking (one transaction:
   (call 1)               (call 2)                 slot+resource+places re-checked)
                                                            │
                     payment OK ──────────> confirmed ──> invoice + review request
                     payment not OK ──> pending, 15-min hold, emailed link
                                            │ hold expires unpaid ──> cancelled, slot freed
                                            │ gateway webhook arrives late (D5):
                                            │     slot+resource free?  ──> reinstate + confirm
                                            │     taken?               ──> full refund via gateway
                                            │     either way: customer + Owner notified
```

**Booking state machine, with the D5 edge** (lifecycle otherwise per `DESIGN.md`; the carry to `DESIGN.md` after Step 14 adds this edge):

```
pending ──pay OK──> confirmed ──completed──> (invoice + review request)
   │                    │ ├─> no_show
   │hold expires        │ └─> cancelled
   ▼ unpaid              ▼
cancelled <──────────────┘
   │
   └─D5 ONLY: payment succeeds late AND slot+resource free ──> confirmed (reinstated)
              otherwise: payment recorded, refunded in full, both parties notified
```

**Webhook error flow:**

```
POST /_/api/webhooks/{gateway} ──> verify signature ──fail──> HTTP 400, error_log, no row
                                        │pass
                                        ▼
                     event_id in webhook_log? ──yes──> refuse (idempotent)
                                        │no
                                        ▼
                     write row + payload to R2 ──> HTTP 200 (≤5 s)
                                        │
                                        ▼
                     background task applies once ──> processed
                     (unprocessed rows: the 5-minute reconciliation task, D4)
```

**Deployment sequence** (per `deployment-procedures.md`): feature complete + signed off (`build-verified`) → release ready (suite passes, no High/Critical open) → tag → back up every instance to its R2 bucket → apply any schema change to App 3 and every instance (additive first) → push the tag into App 2 (David, Step 42) → post-deploy verification (heartbeats show the release and schema fingerprint; no new error type in the first hour) → release notes from App 5.

**Rollback flowchart:** error rate >10%, broken sign-in/booking/payment, data corruption or a security vulnerability → re-point App 2 to the last good tag (David) → verify on the private test instance and in the heartbeats → schema rollback by hand from the pre-migration R2 backup, recorded in `schema-migration-log.md`.

## Stale Diagram Audit

The design doc held no diagrams before this review; the six diagrams above are new. The wireframe and screen artifacts `DESIGN.md` references are build-stage outputs (Steps 26–29), not touched by this plan's text. **0 stale diagrams found.**

## Implementation Tasks

Synthesized from this review's findings at strategy depth — each names the next design or verification action and its owner; none chooses an implementation contract.

- [ ] **T1 (P1)** — Step 15 — carry the review's three new surfaces into the threat model and the test plan: the late-payment rule (D5), the consent endpoints and archive task (D8/D4), the AI/tool crossings; generate `access-matrix.yaml`.
  - Surfaced by: Section 2 (D4), Section 4 (D5), Section 3 (AI crossings)
  - Files: `docs-local/design/app1/` (threat model, test plan, access matrix)
  - Verify: `DC-06` and `DC-04` pass on the Step 15 outputs; the D5 case is a named test.
- [ ] **T2 (P1)** — Step 16 — settle OQ-1 and OQ-2 and write the glossary.
  - Surfaced by: design doc §12 (open questions)
  - Files: `docs-local/GLOSSARY.md`; the design doc's Open Questions
  - Verify: `DC-09` passes.
- [ ] **T3 (P2)** — after Step 14, outside the run — Claude carries D8, D4 and D6 into the project's documents: `authoritative-schema.md` (+`cookie_policy`, `consents`), `schema-migration-log.md` (the new prompt), `integration-architecture.md` (the two schedule tasks), `DESIGN.md` (the booking lifecycle's D5 edge), `pol-anvil-platform.md` and `pdlf-build-plan.md` (help-video hosting) — and writes the legal-documents global policy and specification.
  - Surfaced by: D8/D4/D6 decisions
  - Files: `mb-3-cs-project-library` documents; `project-library-global` policy and specification
  - Verify: every carried document agrees with this design doc.
- [ ] **T4 (P2)** — David, before the first paying client — the two written questions: AC-6 to Anvil's sales team; AC-14 to Amazon for SES production access.
  - Surfaced by: §11 risks; BRD AC-6, AC-14
  - Files: —
  - Verify: both answers on file before the first paying client is onboarded.

## Completion Summary

```
  +====================================================================+
  |            MEGA PLAN REVIEW — COMPLETION SUMMARY                   |
  +====================================================================+
  | Mode selected        | HOLD SCOPE                                  |
  | System Audit         | existing app: 16 client pkgs/91 forms,      |
  |                      | 11 server pkgs/49 modules, 44 of 70 tables; |
  |                      | plan completes on the scaffold; cookie      |
  |                      | policy+spec committed by David pre-review   |
  | Step 0               | HOLD SCOPE (D1); no approach decision       |
  |                      | needed; unverified premises=4, tracked      |
  | Section 1  (Arch)    | 2 issues found — both resolved (D2, D3)     |
  | Section 2  (Errors)  | 15 capability paths mapped, 1 GAP (D4 —     |
  |                      | resolved by recording)                      |
  | Section 3  (Security)| 0 issues found; AI crossings + access       |
  |                      | matrix carried to Step 15                   |
  | Section 4  (Data/UX) | 8 edge cases mapped, 1 unhandled (D5 —      |
  |                      | resolved by David's rule)                   |
  | Section 5  (Quality) | 0 issues found                              |
  | Section 6  (Tests)   | Diagram produced, 3 carries to Step 15      |
  | Section 7  (Perf)    | 0 issues found                              |
  | Section 8  (Observ)  | 0 gaps found                                |
  | Section 9  (Deploy)  | 0 risks flagged beyond §11's recorded ones  |
  | Section 10 (Future)  | Reversibility: 2/5, debt items: 4 (C1–C4)   |
  | Section 11 (Design)  | 1 document conflict found (D6 — resolved    |
  |                      | with the storage home)                      |
  +--------------------------------------------------------------------+
  | NOT in scope         | written (3 items)                           |
  | What already exists  | written                                     |
  | Dream state delta    | written                                     |
  | Error/rescue registry| 15 rows, 0 CRITICAL GAPS                    |
  | Failure modes        | 14 total, 0 CRITICAL GAPS                   |
  | TODOS.md updates     | 0 items proposed (never written under PDLF) |
  | Scope proposals      | 0 proposed, 0 accepted (HOLD SCOPE)         |
  | CEO plan             | skipped by mode (HOLD SCOPE)                |
  | Outside voice        | declined by PDLF fixed answer — skipped     |
  | Lake Score           | N/A (no scored coverage questions, Y=0)     |
  | Diagrams produced    | 6 (architecture, data flow, state machine,  |
  |                      | error flow, deployment, rollback)           |
  | Stale diagrams found | 0                                           |
  | Unresolved decisions | 0 (listed below)                            |
  | Reality rows         | 6/7 rows pass + 1 n/a (PLAN_REALITY),       |
  |                      | unverified=4                                |
  +====================================================================+
```

### Unresolved Decisions

None — D1 through D8 and every review finding were answered and applied within this step.

## GSTACK REVIEW REPORT

| Review | Trigger | Why | Runs | Status | Findings |
|--------|---------|-----|------|--------|----------|
| CEO Review | `/plan-ceo-review` (PDLF Step 14) | Scope & strategy | 1 | CLEAR | HOLD_SCOPE mode; 0 scope proposals; 5 findings recorded and resolved (D2, D3, D4, D5, D6 + the D8 extension); 0 critical gaps |
| Outside Review | declined by PDLF fixed answer | Independent 2nd opinion | 0 | skipped | no completed external review — sub-agents blocked, outside AI services not approved destinations |
| Eng Review | `/plan-eng-review` | Architecture & tests (required) | 0 | — | not run — PDLF runs Step 15 next |
| Design Review | `/plan-design-review` | UI/UX gaps | 0 | — | not run |
| DX Review | `/plan-devex-review` | Developer experience gaps | 0 | — | not run |

**OUTSIDE COVERAGE:** declined by the PDLF fixed answer at Step 14; provider none; phase plan-review; no completed external review.

**VERDICT:** CEO review CLEAR — the reviewed plan stands with every finding resolved by recording. Eng review is required and is PDLF's own Step 15, which runs next.

NO UNRESOLVED DECISIONS

## GSTACK REVIEW REPORT

| Review | Trigger | Why | Runs | Status | Findings |
|--------|---------|-----|------|--------|----------|
| CEO Review | `/plan-ceo-review` (PDLF Step 14) | Scope & strategy | 1 | CLEAR | HOLD_SCOPE mode; 0 scope proposals; 5 findings recorded and resolved (D2–D6 plus the D8 extension); 0 critical gaps |
| Outside Review | declined by the PDLF fixed answer | Independent 2nd opinion | 0 | skipped | no completed external review — sub-agents blocked, outside AI services not approved destinations |
| Eng Review | `/plan-eng-review` (PDLF Step 15) | Architecture & tests (required) | 1 | ISSUES OPEN | 4 issues, 0 critical gaps — all resolved in-step: the schema amendment (43 `updated_at`, 28 `created_at`, 35 `metadata`), the notification transaction, the summary upsert, the divergence inventory, and two new test rows |
| Design Review | `/plan-design-review` | UI/UX gaps | 0 | — | not run |
| DX Review | `/plan-devex-review` | Developer experience gaps | 0 | — | not run |

**OUTSIDE COVERAGE:** declined by the PDLF fixed answer at Step 15; provider none; phase plan-review; no completed external review.

**VERDICT:** Eng review complete — every finding resolved by a recorded decision (R1–R7, D1–D10); the schema amendment is complete in `schema-migration-log.md`; PDLF runs Step 16 (domain modelling) next.

NO UNRESOLVED DECISIONS
