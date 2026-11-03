---
title: "design-doc"
doc-id: "mb-7868ef5e"
updated: "2026-11-03"
---

# Mybizz CS — Design Doc (run app1)

Route B, existing application. Entry 03 (`new-app-with-context`), run `app1`, App 1. The project's documents are `mb-3-cs-project-context`; the global corpus is `project-library-global`. Every statement of those folders is a requirement. This design doc names them and never restates them in place of carrying them.

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
- The schema is designed up front: 68 tables in `authoritative-schema.md`; App 1 presently holds 44 tables, and the recorded migration (`schema-migration-log.md`, Part A and prompts P1–P13) brings it to the schema, applied by David in the Anvil editor through Resolve.
- The application is partially built outside PDLF: 16 client packages with 91 form folders and 11 server packages with 49 server modules exist; the packages and forms the scaffold specifies but the app lacks are listed in the Specification and the Step Receipt. Existing packages are never renamed or merged during active development (scaffold-spec Rules); a module is renamed only at the build step that touches it (Step 12 decisions C1–C4).
- Money is stored as whole cents of the system currency; the system currency is locked once the first booking or invoice exists; the display currency is display-only (schema Rules; F-16; decision D3).
- Every `datetime` is stored in UTC and shown in `business_profile.timezone`; one IANA time zone per instance (ADR `timezone-utc-storage-display-conversion`).
- English is complete at launch; French, Portuguese, Spanish, German and Italian follow one at a time, each offered only when every phrase key exists in it (F-25; `spec-translation`).
- Agents never read or write `.env`, never write `anvil.yaml`, never commit or push (SRS §2.4; the operating rules).
- No reference platform is used (entry record: `reference_platform: null`).
- The in-app notification surface is specified by `spec-mastertemplate-notification-system` §8: bell icon and badge, a notification feed with a Load-more cursor, toasts suppressed for marketing priority, a preferences centre, and 30–60-second polling (recorded as a constraint at David's direction at the `/office-hours` approval).
- The `mb-3-cs-project-library` stubs hold no facts; the two folders named at the head of this document are the law.

## 7. Common Aspects

Each of the 14 aspects of `pol-common-aspects-for-mybizz-anvil-applications`, as this application takes it. **All 14 are shared; none deviates**, so no business-domain deviation reason arises:

| # | Aspect | Shared — how this application takes it |
|---|---|---|
| 1 | The five-app structure, never multi-tenant | Shared: architecture reference §1; one instance and database per client |
| 2 | Client Anvil accounts | Shared: one Mybizz-owned Business account per client, named after the client's business; $150 a month |
| 3 | Configuration, never code, in a client instance | Shared: App 4 holds the schema, the startup module and the dependency; every client difference is data in the client's tables |
| 4 | Schema designed up front, extended without breaking | Shared: 68 tables designed in `authoritative-schema.md`; later changes additive, applied by hand from a migration plan |
| 5 | Secrets in the Vault | Shared: every credential in the Vault; the one Anvil Secret is `encryption_key` |
| 6 | Payments, webhooks and outbound calls | Shared: one active card gateway (Stripe or Paystack) plus optional PayPal behind one payment service; webhooks return 200 at once, process in background, idempotent by event id; outbound calls retry with backoff |
| 7 | Email | Shared: Amazon SES for every email received and sent, one tenant per client; the CRM built in |
| 8 | Client storage and exit | Shared: one R2 bucket per client in the client's own Cloudflare account; cold data in the bucket; the exit handover of `offboarding-procedure.md` |
| 9 | Management and health | Shared: management endpoints from the first release, Bearer-token secured; the heartbeat to App 5 every 15 minutes; App 5 never polls |
| 10 | Background work and real-time updates | Shared: background tasks within the Business plan; near-real-time by client-side polling |
| 11 | Access, currency and time | Shared: the five roles; system currency set at onboarding and fixed after the first transaction; UTC storage, display-time conversion |
| 12 | Compliance | Shared: POPIA everywhere; GDPR and PCI DSS where they apply; legal documents before go-live; the standard offboarding |
| 13 | User interface | Shared: Anvil's Material 3 theme only; no Anvil Extras, no `HTMLTemplate`; dark mode from the first release |
| 14 | Reference platforms | Shared in the negative: no reference platform is used, so nothing is taken from one |

## 8. Reference Platform

None. The entry record records `reference_platform: null`; no reference platform is used for Mybizz CS, so no domain or workflow is taken from one and no `reference-platform-research.md` exists.

## 9. Approaches Considered

Five approaches, each on Anvil — three from this design (A, B, C) and two from the `/office-hours` run (D, E):

**Approach A — complete the application on the scaffold as it stands.** Keep every existing package, form and module; add the packages, forms and server modules the scaffold specifies and the app lacks; apply the recorded schema migration (P1–P13) through David's Resolve; replace the Brevo integration module with the SES email service at the marketing-email build step; consolidate the duplicated Vault modules at the Vault build step; record the common/trade marking in this design doc for Steps 15, 16 and the template work. No restructuring of what exists.

**Approach B — restructure first, then build.** Before any feature work, split `client_code/` and `server_code/` into common and trade package groups, move the trade packages (bookings, services, orders, packages, time tracking, sales, invoicing) into trade groups, and rename the divergent modules (`EmailSetupForm`, `stripe_service`, `paystack_service`, `task_service`) to the scaffold's names first. On Anvil this is folder and import surgery across 91 form folders and 49 modules before a single feature moves.

**Approach C — build features only, defer all hygiene.** Add the missing features on top of what exists; leave the Brevo module, the duplicated Vault modules and the naming divergences for the template stage; mark common/trade later.

**Approach D — bookings-core first** (from the `/office-hours` run; the minimal-viable alternative). Launch with F-01, F-10, F-11 and F-15 only; add the rest after revenue. Fastest to a first paying client; smallest surface to test demand. Not taken, by the documents: PRD §7 defers nothing; F-36 and BR-26 require the whole product in the demo apps; ADR `tiers-model` forbids feature tiers under the one price.

**Approach E — marketplace entry** (from the `/office-hours` run; the lateral alternative). Harvest demand Booksy-style, then convert clients to isolated instances. Not taken, by the documents: concept §5 — "Mybizz CS is not a marketplace"; the ownership architecture is the differentiator; BRD AC-1 fixes the five-app model.

## 10. Recommended Approach

**Approach A.** Why:

- The scaffold spec is the standard the build follows, and its rule is explicit: a package that exists is never renamed or merged during active development; a new feature gets a new package. Approach B violates that rule wholesale and buys nothing the schema does not already fix — the schema (68 tables, law) stays identical under every approach.
- The recorded migration already plans the schema path; the Brevo and Vault-module corrections are one-step fixes at the build steps that touch them, with the decisions recorded (C1, C3).
- The common/trade marking is a design-doc record now and a package organisation at the template stage (ADR `common-and-trade-modules` consequence: the scaffold creates separate common and trade packages when a business is finished and the template work happens). Restructuring now would churn 140 files to reach the same place later.
- Approach C is rejected because it leaves an ADR-violating module (`brevo_campaigns_integration.py`) in the code for the whole build and defers a marking this step is required to make (ADR `common-and-trade-modules`: Steps 12, 15 and 16 mark every feature, table, term and decision).

## 11. Risks

| Risk | Likelihood | Impact | Response |
|---|---|---|---|
| Anvil does not permit client business apps on Mybizz-owned Business accounts (BRD AC-6, PRD OQ-1) | Low | High | Written question to Anvil's sales team before the first paying client; the answer kept on file |
| The Mybizz development account fails or is compromised; every client depends on App 2 there | Low | High | `deployment-procedures.md` account-level procedures; two-factor authentication; R2 backups of every instance |
| The schema migration (44 → 68 tables) is applied by hand, by David, in App 1 | High | Medium | `schema-migration-log.md` Part A and prompts P1–P13 in order, each checked against its prompt before Resolve; Part C compares the resulting `anvil.yaml` with the schema table by table |
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
| SC-1 | Every feature is built and works | Features passing their acceptance criteria | All of F-01 to F-36 | The test suite (SRS verification column; `mb-3-cs-test-specification.md`), signed off at `build-verified` |
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

Every feature, rule and decision of the project's documents, named with the document and section that states it. The documents are the authority; this section never restates them. The project's documents are `mb-3-cs-project-context`; the global corpus is `project-library-global`. Amounts are stored as whole cents of the system currency (the schema's Money in cents rule; Step 12 decision D1).

### 14.1 The features (F-01 to F-36)

| Feature | Requirements live in |
|---|---|
| F-01 Accounts, roles and access | concept §7; architecture reference §12 F-01; user-flows UF-03, UF-38; schema `users`, `rate_limits`, `activity_log`; `spec-security`, `spec-security-architecture` (role limits, sign-in, sessions); SRS SR-01–SR-05; PRD FR-01 |
| F-02 The Vault | concept §7; architecture reference §12 F-02; UF-04; schema `vault`; `mb-3-cs-vault-implementation.md`; `vault-recovery-procedure.md`; `spec-vault-system`; ADR `payment-security-boundary-vault`; SR-06, SR-07; FR-02 |
| F-03 Registration, onboarding and setup | concept §7; architecture reference §5, §12 F-03; UF-01, UF-02; schema `business_profile` onboarding fields; `spec-client-activation-runbook`; `spec-onboarding-implementation-plan` (the project's 10-step wizard governs — the architecture reference is the standard); FR-03; SR-08, SR-08a, SR-08b, SR-09 |
| F-04 Dashboard and analytics | concept §7; architecture reference §12 F-04; UF-11, UF-25, UF-43; schema `daily_summary`, `service_performance`, `provider_performance`, `campaign_metrics`, `sample_rows`; FR-04; SR-10; test specification §4.10, §4.11 |
| F-05 Settings Hub | concept §7; architecture reference §12 F-05; DESIGN.md SettingsForm (14 tabs, RBAC visibility); scaffold-spec settings package; FR-05; SR-11 |
| F-06 Public website | concept §7; architecture reference §12 F-06; UF-16, UF-19; schema `pages`, `files`; `sm-syndication-spec.md` (Route.meta tags); FR-06; SR-12, SR-12a |
| F-07 Homepage templates | concept §7; architecture reference §12 F-07; UF-17; DESIGN.md Homepage Template Variants (shared section inventory, differentiation matrix, video visibility rule); schema `config` key `home_config`, `business_profile.homepage_template`; FR-07; SR-13 |
| F-08 Landing pages | concept §7; architecture reference §12 F-08; UF-18, UF-20; schema `landing_pages`, `lead_captures`, `leads`; FR-08; SR-14 |
| F-09 Visual and media presentation | concept §7; architecture reference §12 F-09; custom-component-requirements-matrix (VideoDisplayComponent, ParallaxComponent, TimeLapseCarouselComponent); FR-09 |
| F-10 Service catalogue | concept §7; architecture reference §12 F-10; schema `services`, `booking_metadata_schemas`; FR-10; SR-55 |
| F-11 Bookings and scheduling | concept §7; architecture reference §12 F-11; UF-05, UF-06, UF-07, UF-08, UF-11; `calendar-feature-spec.md` in full; DESIGN.md Booking Design (status lifecycle, 7-step flow, BookingListForm columns); schema `bookings`, `availability_rules`, `availability_exceptions`, `group_sessions`, `resources`, `recurring_series`, `booking_metadata_schemas`; FR-11; SR-15–SR-22; test specification §1 |
| F-12 Orders for delivered work | concept §7; architecture reference §12 F-12; UF-09, UF-12; schema `orders`; FR-12; SR-23 |
| F-13 Session packages | concept §7; architecture reference §12 F-13; UF-10; schema `packages`, `customer_packages`; FR-13; SR-24 |
| F-14 Time tracking | concept §7; architecture reference §12 F-14; UF-13; schema `time_entries`; FR-14; SR-25 |
| F-15 Invoicing and payments | concept §7; architecture reference §12 F-15; UF-14, UF-15; `integration-architecture.md` (Payment Gateways, PayPal, Recording payments and refunds, Currency); `spec-integration` (Stripe, Paystack, PayPal, gateway-agnostic service layer, three-tier email architecture); DESIGN.md Invoice Design (tax handling, invoice status lifecycle, InvoiceViewerForm, InvoiceEditorForm, InvoiceListForm); ADRs `pdf-invoice-generation`, `payment-gateway-mutability`, `payment-gateway-configuration-is-a-settings-function-and-is-rbac-governed`, `payment-security-boundary-vault`; schema `invoice`, `invoice_items`, `payments`; FR-15; SR-17, SR-17a, SR-17b, SR-26, SR-27, SR-28, SR-29, SR-30 |
| F-16 Dual currency | concept §7; architecture reference §12 F-16; ADR `system-currency-selection-and-immutability` (display currency display-only; no conversion column is built for this app — Step 12 decision D3); FR-16; SR-31 |
| F-17 CRM and contacts | concept §7; architecture reference §12 F-17; UF-21, UF-22; ADRs `single-contacts-table`, `lead-capture-simultaneous-creation`; schema `contacts`, `contact_counter`, `contact_events`, `contact_submissions`, `segments`; FR-17; SR-32 |
| F-18 Email | concept §7; architecture reference §12 F-18; UF-35, UF-39; `team-inbox-spec.md` in full; `integration-architecture.md` (Email); ADR `brevo-email-and-crm` (Amazon SES for every email; the CRM built in; Brevo and Zoho are errors); schema `email_addresses`, `email_messages`, `email_suppressions`, `email_config`, `email_templates`, `email_log`; FR-18; SR-34, SR-35, SR-35a, SR-35b, SR-35c |
| F-19 Lead capture and marketing funnel | concept §7; architecture reference §12 F-19; UF-20; ADR `lead-capture-simultaneous-creation`; schema `leads`, `lead_captures`; FR-19; SR-33 |
| F-20 Tasks and automation | concept §7; architecture reference §12 F-20; UF-26; schema `tasks`, `automation_rules`, `automation_runs`; FR-20; SR-36 |
| F-21 Blog and content | concept §7; architecture reference §12 F-21; UF-16, UF-42; `sm-syndication-spec.md` in full; schema `blog_categories`, `blog_posts`, `blog_comments`; FR-21; SR-37, SR-37a |
| F-22 Clause Builder | concept §7; architecture reference §12 F-22; UF-28; ADR `legal-policy-responsibility-acknowledgement-and-clause-builder-architecture`; schema `privacy_policy`, `t_and_c`; FR-22; SR-38 |
| F-23 Reviews and testimonials | concept §7; architecture reference §12 F-23; UF-27; schema `reviews`, `testimonials`; FR-23; SR-39 |
| F-24 Customer portal | concept §7; architecture reference §12 F-24; UF-29; scaffold-spec portal package and CustomerLayout menu; FR-24; SR-40 |
| F-25 Ease of use, in-app help and language | concept §7; architecture reference §12 F-25; `spec-translation` in full; schema `business_profile.app_language`, `email_templates.language`; FR-25; SR-41, SR-41a; test ST-14 |
| F-26 AI assistance | concept §7; architecture reference §12 F-26; UF-30; `mybizz-ai-implementation-spec.md` in full (architectural model, ten assists, rollout phases, data-dependent assists, sample data, further AI functions, data flow, technical approach, risks and guardrails, development sequence); FR-26; SR-42, SR-43, SR-44, SR-44a, SR-44b |
| F-27 WhatsUp voice | concept §7; architecture reference §12 F-27; UF-31; `mybizz-ai-implementation-spec.md` (voice module, tiers 1–5, proactive briefings, conversational follow-up, natural-sounding voice, browser compatibility); FR-27; SR-45, SR-45a |
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

All 68 tables of `authoritative-schema.md`, by name, from its Table Groups:

- Core and configuration (11): `activity_log`, `business_profile`, `config`, `email_config`, `files`, `payment_config`, `rate_limits`, `sample_rows`, `theme_config`, `users`, `vault`
- Services and bookings (8): `services`, `resources`, `group_sessions`, `availability_rules`, `availability_exceptions`, `booking_metadata_schemas`, `bookings`, `recurring_series`
- Orders and packages (3): `orders`, `packages`, `customer_packages`
- Finance (6): `invoice`, `invoice_items`, `payments`, `time_entries`, `expenses`, `expense_categories`
- CRM and marketing (12): `contacts`, `contact_counter`, `contact_events`, `contact_submissions`, `segments`, `tasks`, `leads`, `lead_captures`, `email_campaigns`, `contact_campaigns`, `automation_rules`, `automation_runs`
- Sales (4): `companies`, `deals`, `quotes`, `quote_items`
- Email and webhooks (6): `email_templates`, `email_log`, `webhook_log`, `email_addresses`, `email_messages`, `email_suppressions`
- Content and policy (7): `blog_categories`, `blog_posts`, `blog_comments`, `landing_pages`, `pages`, `privacy_policy`, `t_and_c`
- Reviews (2): `reviews`, `testimonials`
- Summary and health (6): `daily_summary`, `service_performance`, `provider_performance`, `campaign_metrics`, `health_log`, `error_log`
- Notifications (3): `notifications`, `user_preferences`, `delivery_logs`

Rules of the schema: `authoritative-schema.md`, Rules — one instance, one client (no client or instance column on any table; isolation comes from the instance); every `datetime` stored in UTC and shown in `business_profile.timezone`; every amount in `business_profile.system_currency`; **money in cents** — every amount is stored as a whole number of cents of the system currency, as `spec-anvil-platform-standards` requires (Anvil's `number` column is floating-point, so decimal amounts would round wrongly), and amounts are converted to the currency's units only for display; rows of `bookings`, `orders`, `invoice`, `payments`, `quotes` and `expenses` are never deleted, their status changes; history outside the tables (backups, history, webhook payloads, audit entries older than 30 days, email bodies older than 90 days, email attachments and expense receipts in the client's R2 bucket); credentials only in the Vault, encrypted; names follow `spec-nomenclature` (link column `{target}_id`, plural for a multiple link, `provider_id` for a provider, `resource_id` for a bookable room or piece of equipment); allowed values are lowercase strings; column types `string`, `number`, `bool`, `date`, `datetime`, `media`, `simpleObject`, `link → table`, `links → table`. Every table has `client: none` and `server: full`.

Scope of the schema: `schema-scope-reduction` ADR — the instance holds exactly these tables and no others, with the reason for each table in and out (`tickets`/`ticket_messages`, `kb_articles`/`kb_categories`, `events`/`event_registrations`, `backups`, `translations`, `customers` are out; `reviews`, `testimonials`, `resources`, `expenses`, `expense_categories`, `companies`, `deals`, `quotes`, `quote_items`, `email_addresses`, `email_messages`, `email_suppressions`, `sample_rows` are in). Changes to the schema: `schema-migration-log.md` — the recorded migration from App 1's present 44 tables to the 68-table schema: Part A, the renames by hand (`error_log.timestamp`, `blog_comments.comment_text`, `provider_id` in `availability_rules`, `availability_exceptions`, `bookings` and `time_entries`); prompts P1–P5 (11 new tables: `resources`, `group_sessions`, `recurring_series`, `packages`, `customer_packages`, `orders`, `payments`, `automation_rules`, `automation_runs`, `reviews`, `testimonials`); P6 and P7 (new columns); P8–P10 (10 new tables: `companies`, `quotes`, `quote_items`, `deals`, `expense_categories`, `expenses`, `sample_rows`, `email_addresses`, `email_messages`, `email_suppressions`); P11 (new columns for the new features); P12 (3 new notification tables: `notifications`, `user_preferences`, `delivery_logs`); P13 last (the column removals, including the Brevo columns of `email_config`); Part C, the check against `authoritative-schema.md`.

Step 12 decision D2 is carried out in those documents: the Notifications group is in the schema and prompt P12 is in the migration log.

### 14.4 The architecture and its decisions

The architecture itself: architecture reference §0 states its authority — this document is the build standard for the architecture of Mybizz CS; agents build from it; where `mb-3-cs-concept.md` or `mb-3-cs-architecture-explainer.md` differs from it on a technical fact, it is correct. Architecture reference §§1–14 — the five-app system (§1), the rule that no form or server module is ever added to a client instance (§2), data isolation (§3), hosting and capacity (§4), provisioning and registration (§5), the development workflow (§6), the package structure (§7), the module architecture (§8), navigation — the admin menu, the customer menu and their visibility rules (§9), startup and authentication routing (§10), the Sidesheet (§11), the technical stack (§13) and the platform constraints (§14). Architecture reference §15 names the related specifications and what each is relevant to. Architecture reference §16 specifies the sample data: loaded at provisioning for the client's trade in the client's currency and language, every sample row recorded in `sample_rows`, sample contacts at `example.com`, the banner on every screen, and the Owner's Clear sample data background task — specified in full in `mybizz-ai-implementation-spec.md` (Sample Data), carried out by `sample_data_service` in `server_settings/`.

`mb-3-cs-architecture-explainer.md` explains, in plain language for the developer, how Mybizz CS is built and why it is built that way: the big picture, the five apps and why the client apps must stay empty, where the apps live, how a new client is set up, how a change reaches every client, data and backups and history, security, how the code is organised, navigation and layout, every feature F-01 to F-36 in plain language, the sample data, and working within Anvil. The build agent works from the architecture reference, which is correct where the two differ.

Local ADRs (`mb-3-cs-project-context/adr-local`): `blank-client-template` (App 3 as the only provisioning clone source), `crm-package-replaces-marketing` (client package `crm/`, server package `server_marketing/`), `schema-scope-reduction` (tables in and out).

Local policy (`mb-3-cs-project-context/policy-local`): `development-policy` (the Anvil-first mandate with its approved exceptions — the Vault, Amazon SES, Cloudflare R2, Groq through App 5; version control and backup; development tooling; enforcement severity).

Global ADRs applied to this application (`project-library-global/adr-global`, all 41): `adr-anvil-extras-exclusion`; `adr-anvil-platform-constraints`; `adr-brevo-email-and-crm`; `adr-client-data-management-rights-and-mybizz-retention-boundary`; `adr-client-instance-architecture`; `adr-client-instance-readme-five-app-system`; `adr-client-r2-storage-and-exit-handover`; `adr-common-and-trade-modules`; `adr-common-architecture-across-mybizz-applications`; `adr-dark-mode`; `adr-data-access-patterns`; `adr-dependency-based-not-multi-tenant`; `adr-dependency-update-model`; `adr-design-rules`; `adr-document-frontmatter-and-identity`; `adr-form-architecture-and-state`; `adr-htmltemplate-use`; `adr-lead-capture-simultaneous-creation`; `adr-legal-policy-responsibility-acknowledgement-and-clause-builder-architecture`; `adr-material-3-theme-component-scope`; `adr-mybizz-management-visibility`; `adr-navigation-lambda-link-open-form`; `adr-no-free-trial`; `adr-observability-architecture`; `adr-onboarding-data-schema-alignment`; `adr-onboarding-finality`; `adr-onboarding-resumability`; `adr-onboarding-vs-settings-boundary`; `adr-payment-gateway-configuration-is-a-settings-function-and-is-rbac-governed`; `adr-payment-gateway-mutability`; `adr-payment-security-boundary-vault`; `adr-pdf-invoice-generation`; `adr-real-time-and-background-tasks`; `adr-responsive-behaviour-mechanism`; `adr-role-property-assignment-mechanism`; `adr-single-contacts-table`; `adr-system-currency-selection-and-immutability`; `adr-tiers-model`; `adr-timezone-utc-storage-display-conversion`; `adr-ui-customization-approach`; `adr-webhook-architecture`.

Global policies (`project-library-global/policy-global`, all 5): `pol-common-aspects-for-mybizz-anvil-applications` — its 14 aspects are each shared by this application, none deviating: (1) the five-app structure, never multi-tenant; (2) client Anvil accounts, one Mybizz-owned Business account per client; (3) configuration, never code, in a client instance; (4) schema designed up front, extended without breaking; (5) secrets in the Vault; (6) payments, webhooks and outbound calls; (7) email through Amazon SES, the CRM built in; (8) client storage and exit; (9) management and health; (10) background work and real-time updates; (11) access, currency and time; (12) compliance; (13) user interface; (14) reference platforms (none is used here). Also `pol-anvil-first-development`, `pol-documentation-and-decisions`, `pol-security-and-data-governance`, `pol-testing-and-quality`.

### 14.5 The standards the build follows

From the project's documents: `scaffold-spec.md` (package structure, server function pattern, custom component conventions, database schema rules, M3 component standards, navigation architecture, startup configuration, Users service settings, dependencies and versions); `globals-contract.md` (every shared state variable, the defensive loading pattern); `integration-architecture.md` (every outside service and its credential home, payment gateways and the one payment service, PayPal checkout, email paths, the CRM, client storage, AI and speech, webhooks, the background-task schedule, timezones, domain and provisioning, offboarding); `observability-requirements.md` (structured logging, log tables, business metrics, alerts in the instance and in App 5, the heartbeat, log retention); `deployment-procedures.md` (deployment model, update flow, release steps, staged rollout, schema migration types and checklist, post-deploy verification, rollback and its triggers, deployment checklist, account-level failures); `anvil-ui-build-standard.md` (hard prohibitions, visual tokens, naming conventions, HTML-to-component mapping, approved components and writeback, custom HTML components, form boundary and icons, data and empty states, build process rules); `DESIGN.md`; `palette-tokens.md`; `custom-component-requirements-matrix.md` (12 shared custom components, 1 local, the not-built list); `calendar-feature-spec.md`; `team-inbox-spec.md`; `sm-syndication-spec.md`; `mybizz-ai-implementation-spec.md`; `mb-3-cs-vault-implementation.md`; `vault-recovery-procedure.md`; `offboarding-procedure.md`; `mb-3-cs-test-specification.md`.

From the global corpus: `spec-five-app-architecture-model`; `spec-anvil-platform-standards`; `spec-anvil-spec-table`; `spec-api-specification`; `spec-client-activation-runbook`; `spec-component-properties`; `spec-deployment`; `spec-integration`; `spec-m3-design-standards`; `spec-m3_component_mapping`; `spec-mastertemplate-notification-system`; `spec-material-3-theme`; `spec-mybizz-management-app-notification-system`; `spec-nomenclature`; `spec-observability`; `spec-onboarding-implementation-plan` (the project's 10-step wizard governs, per the architecture reference's authority); `spec-regulatory-compliance-baseline`; `spec-screen-and-wireframe-production-standards`; `spec-screen-production-standard`; `spec-security`; `spec-security-architecture`; `spec-testing-methodology-standards`; `spec-translation`; `spec-ui-standards`; `spec-vault-system`.

### 14.6 The business and product requirements

`business-requirements-document.md`: objectives BO-1 to BO-8; requirements BR-1 to BR-26; assumptions and constraints AC-1 to AC-14 (AC-6 — Anvil's written confirmation of the account structure — and AC-14 — SES production access — are open); success criteria KPIs; risks. `product-requirements-document.md`: goals and non-goals; target users; user stories US-1 to US-41; functional requirements FR-01 to FR-36; non-functional requirements; out of scope (nothing deferred — voice tier 4 and tier 5 are built in this release); dependencies; open questions OQ-1 and OQ-2; success metrics. `software-requirements-specification.md`: specific requirements SR-01 to SR-58; external interface requirements; performance requirements; design constraints; non-functional requirements.

`mb-3-cs-concept.md`: what Mybizz CS is (§1), the problem (§2), the founder story (§3), target market (§4), what it is not (§5), competitive positioning (§6), the feature set (§7), sample data (§8), pricing (§9 — $150 a month, month to month, no trial, $200 assisted setup, costs outside the monthly price, revenue model, the client's own domain), go-to-market (§10), brand identity (§11), key messages (§12), support model (§13), what is explicitly out of scope (§14), success metrics (§15), risks (§16). `mb-3-cs-feature-list.md`: the client-facing list of every feature, §§1–28.

### 14.7 Testing

`mb-3-cs-test-specification.md` in full — the E2E booking flow test spec with its performance gate and data validation, the edge cases (existing contact, provider unavailable, double-booking), the error-path test cases (payment failure, webhook signature failure, webhook dispatch failure, SES throttling, Vault secret not found, contact deduplication conflict, server call timeout), the RBAC coverage matrices (form access, settings tab access, test procedure, the RBAC decorator pattern) and the feature tests 4.1 to 4.21. Also `spec-testing-methodology-standards` (levels 0–4, pure-function characteristics, the integration safety protocol, test structure, the test plan shape, the RBAC verification procedure), `pol-testing-and-quality`, and the test suite (`ts-method`, `ts-catalogue`, `ts-conformance-checks`).

### 14.8 Decisions taken at Step 12

| # | Decision |
|---|---|
| D1 | Every amount is stored as a whole number of cents of the system currency. The schema's number columns hold cents; the concept's currency figures are display wording. No override is recorded. |
| D2 | Carried out in the project's documents: `authoritative-schema.md` holds the Notifications group (`notifications`, `user_preferences`, `delivery_logs`; 68 tables in total) and `schema-migration-log.md` records prompt P12 (the three notification tables) and P13 (the removals, last). |
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

**Tables.** Trade (20): `services`, `resources`, `group_sessions`, `availability_rules`, `availability_exceptions`, `booking_metadata_schemas`, `bookings`, `recurring_series`, `orders`, `packages`, `customer_packages`, `time_entries`, `invoice`, `invoice_items`, `payments`, `quotes`, `quote_items`, `deals`, `reviews`, `service_performance`. Common (48): every other table. `invoice`, `payments`, `quotes`, `quote_items`, `deals`, `reviews` and `service_performance` are trade because each holds a typed link into a trade table (`invoice.booking_id`, `invoice.order_id`, `invoice.customer_package_id`; `payments.invoice_id`, `payments.booking_id`, `payments.order_id`, `payments.customer_package_id`; `quotes.invoice_id`; `deals.quote_id`; `reviews.booking_id`, `reviews.service_id`; `service_performance.service_id`); a common table may not.

**Decisions.** Common: the five-app model, client instances and the no-forms rule, the Vault, SES email, R2 storage and exit, management endpoints and the heartbeat, observability, webhooks and background tasks, roles and access, system currency and timezones, compliance and offboarding, the design system, translation, the notification engine, demo mode, tiers and no-trial, the onboarding architecture, lead capture, the single contacts table, the clause builder. Trade: the service catalogue and its pricing models, availability and resources, the booking flow and calendar, group sessions, recurring series, orders, packages, time tracking, invoice and payment records with their trade-source links, quotes and deals, review requests after appointments.

Judgement calls recorded as open question OQ-1 (settled at Step 16): `service_performance` is a trade table while the dashboard feature (F-04) is common; the accounts reports (F-28) and the customer portal (F-24) are common features whose reports and panels read trade tables.

## 15. Canonical Sources

Every canonical file read, with what this design doc takes from it.

**`mb-3-cs-project-context/adr-local`** — `blank-client-template.md`: App 3's contents and the schema responsibility. `crm-package-replaces-marketing.md`: the client package is `crm/`, the server package `server_marketing/`. `schema-scope-reduction.md`: which tables are in and out, and why.

**`mb-3-cs-project-context/docs-local`** — `authoritative-schema.md`: all 68 tables, the Rules (including money in cents), column types, and the Notifications group. `schema-migration-log.md`: Part A renames, prompts P1–P13, the Part C check. `mb-3-cs-concept.md`: sections 1–16 — the problem, market, positioning, every feature, pricing, go-to-market, brand, support, out-of-scope, metrics, risks. `mb-3-cs-feature-list.md`: the client-facing feature list, §§1–28. `mb-3-cs-architecture-reference.md`: §§0–16, the build standard and every feature's architecture. `mb-3-cs-architecture-explainer.md`: the plain-language account of the same architecture. `user-flows.md`: the 44 flows, actors, terms and flow design principles. `DESIGN.md`: the design system, tokens, palettes governance, forms and pages, templates, deviations, status badges, empty states. `palette-tokens.md`: the four palettes' source colours and every value, light and dark. `custom-component-requirements-matrix.md`: the 12 shared components, 1 local, the not-built list. `calendar-feature-spec.md`: the booking calendar's forms, views, data and rules. `mybizz-ai-implementation-spec.md`: the AI helper, the ten assists, voice tiers 1–5, sample data, costs and guardrails. `team-inbox-spec.md`: SES, the domain records, addresses, receiving, the Team Inbox screen, sending, storage. `sm-syndication-spec.md`: the share panel, AI captions, previews, `?ref=` tracking. `mb-3-cs-vault-implementation.md`: what the Vault stores, the VaultForm, the build sequence. `vault-recovery-procedure.md`: both recovery paths. `offboarding-procedure.md`: the exit steps and rules. `mb-3-cs-test-specification.md`: the E2E spec, error paths, RBAC matrices, feature tests.

**`mb-3-cs-project-context/docs-standard-local`** — `anvil-ui-build-standard.md`: the hard prohibitions, tokens, naming, mapping, writeback, build process. `business-requirements-document.md`: BO-1–8, BR-1–26, AC-1–14, KPIs, risks. `deployment-procedures.md`: the release flow, migration types, rollback and triggers. `globals-contract.md`: every shared state variable and the defensive pattern. `integration-architecture.md`: every outside service, the task schedule, timezones, domain, offboarding. `observability-requirements.md`: logging, metrics, alerts, heartbeat, retention. `product-requirements-document.md`: US-1–41, FR-01–36, NFRs, OQ-1–2. `scaffold-spec.md`: packages, forms, server modules, navigation, startup, dependencies. `software-requirements-specification.md`: SR-01–58, interfaces, performance, constraints.

**`mb-3-cs-project-context/policy-local`** — `development-policy.md`: the Anvil-first mandate and its four approved exceptions, tooling, enforcement.

**`project-library-global/adr-global` (41)** — `adr-anvil-extras-exclusion`: Extras and third-party packages excluded. `adr-anvil-platform-constraints`: designed within Anvil's parameters. `adr-brevo-email-and-crm`: SES everywhere; the CRM built in; Brevo and Zoho are errors. `adr-client-data-management-rights-and-mybizz-retention-boundary`: owner data rights and Mybizz's retention boundary. `adr-client-instance-architecture`: what lives where; the two proven Anvil behaviours. `adr-client-instance-readme-five-app-system`: the mandatory README in every client instance. `adr-client-r2-storage-and-exit-handover`: one bucket per client; the exit handover. `adr-common-and-trade-modules`: the common/trade marking this design doc records. `adr-common-architecture-across-mybizz-applications`: what is common stays common. `adr-dark-mode`: dark mode from the first release. `adr-data-access-patterns`: search, never iterate; pre-computed summaries. `adr-dependency-based-not-multi-tenant`: the prohibited and required patterns. `adr-dependency-update-model`: the release flow and its discipline. `adr-design-rules`: the wireframe rules for later phases. `adr-document-frontmatter-and-identity`: document identity (governs this document's own frontmatter). `adr-form-architecture-and-state`: the content panel, `globals.py`, the batched booking flow. `adr-htmltemplate-use`: HTMLTemplate banned. `adr-lead-capture-simultaneous-creation`: lead and contact created together. `adr-legal-policy-responsibility-acknowledgement-and-clause-builder-architecture`: the owner's responsibility and the clause builder. `adr-material-3-theme-component-scope`: the permitted components. `adr-mybizz-management-visibility`: the three communication patterns and the token model. `adr-navigation-lambda-link-open-form`: the navigation standard. `adr-no-free-trial`: the two demo apps instead. `adr-observability-architecture`: instance-level metrics and the heartbeat. `adr-onboarding-data-schema-alignment`: onboarding uses the schema's exact names. `adr-onboarding-finality`: onboarding revisitable; append-only records. `adr-onboarding-resumability`: progress preserved. `adr-onboarding-vs-settings-boundary`: what belongs where. `adr-payment-gateway-configuration-is-a-settings-function-and-is-rbac-governed`: gateway configuration in Settings. `adr-payment-gateway-mutability`: one active gateway, changeable under rules. `adr-payment-security-boundary-vault`: secret keys only in the Vault; the enforcement pattern. `adr-pdf-invoice-generation`: reportlab, server-side. `adr-real-time-and-background-tasks`: polling intervals, the task inventory, retry policy. `adr-responsive-behaviour-mechanism`: `wrap_on`, never breakpoints. `adr-role-property-assignment-mechanism`: roles set in code. `adr-single-contacts-table`: one `contacts` table, no `customers`. `adr-system-currency-selection-and-immutability`: currency at onboarding, locked after the first transaction (this app takes the display-currency-only model, decision D3). `adr-tiers-model`: one price, $150; assisted setup $200. `adr-timezone-utc-storage-display-conversion`: UTC storage, IANA string, display-time conversion. `adr-ui-customization-approach`: M3 tokens, `theme.css`, native layouts. `adr-webhook-architecture`: the one webhook pattern and reconciliation.

**`project-library-global/policy-global` (5)** — `pol-common-aspects-for-mybizz-anvil-applications`: the 14 aspects, all shared (section 7). `pol-anvil-first-development`: the checking order and severity. `pol-documentation-and-decisions`: the four document kinds; no duplicate sources of truth. `pol-security-and-data-governance`: Vault-only secrets, mandatory RBAC, client data ownership. `pol-testing-and-quality`: the test suite governs; local first; sign-off.

**`project-library-global/specifications-global` (25)** — `spec-five-app-architecture-model`: the five apps and the critical constraint. `spec-anvil-platform-standards`: form files, server pattern, three tiers, background tasks, Uplink, rate limiting, shared numeric standards, data access, the constraints checklist, Skulpt, media, `app_tables` failure behaviour. `spec-anvil-spec-table`: the consolidated reference; its navigation rows follow the ADR (decision D5). `spec-api-specification`: webhook and management endpoints, rate limits, errors. `spec-client-activation-runbook`: provisioning steps 1–16 and the rollback. `spec-component-properties`: Designer vs code properties. `spec-deployment`: release workflow, migration strategy, account failures. `spec-integration`: SES, Stripe, Paystack, PayPal, the service layer, the three email tiers. `spec-m3-design-standards`: prefixes, button hierarchy, typography, states, status colours, accessibility, empty states, the MUI overlay. `spec-m3_component_mapping`: the mapping and writeback table. `spec-mastertemplate-notification-system`: the notification engine; its three tables are in the schema (D2). `spec-material-3-theme`: theme architecture and roles. `spec-mybizz-management-app-notification-system`: the platform-to-client console. `spec-nomenclature`: names for everything common. `spec-observability`: the log tables and structured logging. `spec-onboarding-implementation-plan`: the onboarding architecture (this app takes the 10-step wizard of its own documents). `spec-regulatory-compliance-baseline`: POPIA, GDPR, PCI DSS, retention, offboarding. `spec-screen-and-wireframe-production-standards` and `spec-screen-production-standard`: screen production rules for later phases. `spec-security`: the security standards this app follows (decision D4). `spec-security-architecture`: role limits, sign-in, tokens, protection levels. `spec-testing-methodology-standards`: levels, safety protocol, RBAC verification. `spec-translation`: phrase keys, the module, tests, adding a language. `spec-ui-standards`: the M3 mandate, naming, binding. `spec-vault-system`: the two-level model, enforcement, enrolment and reset.

**`mb-3-cs-project-library`** — its `adr-local`, `docs-local`, `docs-standard-local` and `policy-local` files were read in full (`README.md` files, the GLOSSARY skeleton, the design-folder README, the releases and strategy placeholders, the schema-migration-log skeleton, and the docs-standard-local section skeletons). They hold structure and pointers only; every fact in this design doc comes from the two folders named at its head.
