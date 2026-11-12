---
title: "software-requirements-specification"
doc-id: "mb-f157e3fc"
updated: "2026-11-12"
---
# Mybizz CS — Software Requirements Specification (SRS)

The testable requirements the software must meet.

**Standard:** [[[default:docs-standard-global/docstd-software-requirements-specification]]](C:/dev/project-library-global/docs-standard-global/docstd-software-requirements-specification.md)

**Status:** Draft
**Date:** 2026-10-31
**Author:** Mybizz founder
**Derived from:** `product-requirements-document.md`. Every requirement traces to a functional requirement there. The build detail of each feature is `mb-3-cs-architecture-reference.md` section 12; where this document differs from it, the architecture reference is correct.

**Certainty tags:** `[CLEAR]` / `[PARTIAL]` / `[INFERRED]` / `[MISSING]`, as in the BRD and PRD.

---

## 1. Introduction

### 1.1 Purpose
This document states the testable requirements of Mybizz CS for developers, for the agents that build it, and for testing. It is the technical companion of the PRD.

### 1.2 Scope
App 1 (`mb-3-cs-app1`) and what it releases into App 2 (`master_template`) and App 3 (`blank_client_template`) for the client instances, and the interfaces of the client instances with App 5. App 5's own internals are specified in App 5's own run. Product scope: PRD sections 2 and 7.

### 1.3 Definitions and Acronyms

Domain terms are defined in `GLOSSARY.md` once Step 16 produces it; until then, `user-flows.md` (Terms and Actors) holds them. Terms specific to this SRS:

| Term | Meaning |
|---|---|
| Client instance | App 4: one client's container running the release image (App 3 on App 2, on the Anvil App Server), with its own database |
| Release image | App 3, App 2 and `m3` on the pinned Anvil App Server, built once per release in `mb-3-cs-app4` |
| Role decorator | The server-side check in `server_auth/rbac` that refuses a call from a role not allowed |
| Payment hold | The 15-minute `pending` state of a booking whose online payment has not yet succeeded |
| Heartbeat | The 15-minute report from a client instance to App 5 |
| Schema fingerprint | A hash of an instance's table and column names, sent in the heartbeat |
| R2 | Cloudflare R2 object storage; one bucket per client |
| SES | Amazon Simple Email Service; every email received and sent; one tenant per client |
| Phrase key | The key of one piece of screen text in `shared/translations/` |
| Sample data | The three months of example records loaded at provisioning and recorded in `sample_rows` |

### 1.4 References

- `business-requirements-document.md`, `product-requirements-document.md`
- `mb-3-cs-concept.md`, `mb-3-cs-architecture-reference.md`, `user-flows.md`
- `scaffold-spec.md`, `globals-contract.md`, `integration-architecture.md`, `observability-requirements.md`, `deployment-procedures.md`
- `authoritative-schema.md`, `DESIGN.md`, `custom-component-requirements-matrix.md`, `spec-calendar-feature.md`, `spec-ai-implementation.md`, `spec-team-inbox.md`, `spec-sm-syndication.md`, `vault-implementation.md`, `spec-test-specification.md`, `mb-3-cs-feature-list.md`
- [[[default:specifications-global/spec-regulatory-compliance-baseline|Regulatory Compliance Baseline]]](C:/dev/project-library-global/specifications-global/spec-regulatory-compliance-baseline.md), [[[default:specifications-global/spec-security|Security Specification]]](C:/dev/project-library-global/specifications-global/spec-security.md), [[[default:specifications-global/spec-api-specification|API Specification (HTTP Endpoints)]]](C:/dev/project-library-global/specifications-global/spec-api-specification.md)

---

## 2. Overall Description

### 2.1 Product Perspective
Mybizz CS is a dependent-instance application of the five-app model ([[[default:specifications-global/spec-five-app-architecture-model|Five-App Architecture Model (Specification)]]](C:/dev/project-library-global/specifications-global/spec-five-app-architecture-model.md)). App 1 is developed; each release is pushed into App 2; every client instance depends on App 2 and holds only the schema, the startup module and its data. App 5, the Mybizz management app, is a separate application that the instances call through HTTP endpoints. `[CLEAR]`

### 2.2 Product Functions
The 37 features F-01 to F-37 of `mb-3-cs-concept.md` section 7, summarised in PRD section 5. `[CLEAR]`

### 2.3 User Characteristics
- Owners and staff are business people, not technical users; every screen must be usable without training, with tooltips that open help videos (F-25), in the app language.
- Staff use the admin area on desktop and phone; WhatsUp is used on a phone, often on the move.
- Customers and visitors use the public website and portal on any device, mostly phones.
- Users include people who rely on keyboard, screen reader or high contrast (WCAG 2.1 AA). `[CLEAR]`

### 2.4 Constraints
Anvil's platform constraints apply by default: the App Server builds and extends each client's tables from App 3's table definitions, and a release only adds tables and nullable columns; one environment and one database per app; the Skulpt client runtime (money and date logic on the server only); no WebSocket push (client polling); lazy-loaded rows. Beyond the platform default:
- App 3 holds no form or server module; one would silently override `master_template` for every client (reference section 2).
- Every client instance runs in its own container with its own database on DigitalOcean (`spec-client-instance-hosting`).
- The only Anvil Secret is `encryption_key`.
- Agents never read or write `.env`, never write `anvil.yaml`, and never commit. `[CLEAR]`

### 2.5 Assumptions and Dependencies
- The Anvil App Server version Mybizz pins runs every feature as Anvil's hosted service runs it; the full test suite on the release image proves it before each release. `[PARTIAL]`
- An app can schedule its dependency's background tasks and serve its dependency's HTTP endpoints (PRD OQ-2). `[MISSING]`
- Outside services as in `integration-architecture.md`. `[CLEAR]`
- Amazon grants SES production access to Mybizz's AWS account (BRD AC-14). `[PARTIAL]`
- `boto3` runs on Anvil's server Python. `[MISSING]`

---

## 3. Specific Requirements

### 3.1 Functional Requirements

| # | Requirement | Traces to PRD | Verification Method | Certainty |
|---|---|---|---|---|
| SR-01 | Every callable server function carries a role decorator; a call from a role not allowed returns `{'success': False}` and writes nothing | FR-01, FR-30 | Access-matrix test (IN-02) over every function and role | [CLEAR] |
| SR-02 | No password is sent by email; new users set a password from a single-use, time-limited link | FR-01 | Test: invitation and provisioning emails contain no password; a used link is refused | [CLEAR] |
| SR-03 | Sign-in is rate-limited per identifier in `rate_limits`; repeated failures lock the account as the Security Specification sets out | FR-01, FR-30 | Test: repeated failed sign-ins are refused | [CLEAR] |
| SR-03a | Every account holder signs in with a second factor (Anvil's "Require 2-factor authentication"); the Owner's high-value functions are refused while any non-FIDO2 factor is enrolled; a lost second factor is reset by the user through Anvil's reset by email | FR-01 | Test SC-02; settings check ST-12 | [CLEAR] |
| SR-04 | A session idle for 30 minutes requires sign-in again and then returns the user to the same form | FR-01 | Test with the clock advanced | [CLEAR] |
| SR-05 | Every significant action writes an `activity_log` row: user, action type, description, metadata, time, IP address | FR-01, FR-29 | Test: role change, Vault access, payment, refund and status changes each write a row | [CLEAR] |
| SR-06 | `vault_service` is the only module that reads `encryption_key`; every credential is stored encrypted in `vault` | FR-02 | Code scan: no other module reads the secret; table scan: no value in clear | [CLEAR] |
| SR-07 | Every Vault view requires a valid TOTP code, with no remembered session; every access emails the Owner; 5 wrong codes lock the Vault for 15 minutes and email the Owner | FR-02 | Test of each condition | [CLEAR] |
| SR-08 | Onboarding saves after each step in `business_profile.onboarding_status`; reopening goes to the next unfinished step; steps 5 and 6 cannot be passed until confirmed | FR-03 | Test: leave at each step and return | [CLEAR] |
| SR-08a | The contact import maps the columns of the HubSpot, Mailchimp and Acuity exports without manual mapping, matches existing contacts by email, and creates no duplicate | FR-03, FR-17 | Test with an export of each tool | [CLEAR] |
| SR-08b | The domain step reports each DNS record live or not; the MX record is written only after the Owner confirms | FR-03, FR-18 | Test against App 5 with a test domain | [CLEAR] |
| SR-09 | The legal acknowledgement is recorded once with its timestamp, in the instance and in App 5, and cannot be reversed | FR-03 | Test: second acknowledgement attempt changes nothing; App 5 record present | [CLEAR] |
| SR-10 | Dashboard and analytics read `daily_summary`, `service_performance`, `provider_performance` and `campaign_metrics` only, never aggregate source tables on load | FR-04 | Code review and query count on dashboard load | [CLEAR] |
| SR-11 | Settings tabs are filtered on the server by role: Owner all 14; Manager Business Profile, Brand Palette, Display Currency, Email, Theme | FR-05 | Access-matrix test | [CLEAR] |
| SR-12 | Public pages are served by Anvil Routing at the addresses of `scaffold-spec.md` (Public routes); each route's `Route.meta` sets title, description and Open Graph tags in the first response | FR-06, FR-21 | Route test of every address, read without running scripts | [CLEAR] |
| SR-12a | Edit-mode saves are accepted from Owner and Manager only, write the page's content in one call, and record an `activity_log` row | FR-06, FR-08, FR-21 | Access test; test of a save | [CLEAR] |
| SR-13 | The homepage renders its sections in the order and visibility of the selected template; a section with no content does not render | FR-07, FR-09 | Test each template with and without optional content | [CLEAR] |
| SR-14 | A landing page view and a landing page submission each increment its counters once | FR-08 | Test | [CLEAR] |
| SR-15 | `get_available_slots` returns only slots where the provider is available, any required resource is free, travel buffers for customer-location bookings are respected, and a group session has places left | FR-11 | Unit tests over availability rules, exceptions, resources, buffers and places | [CLEAR] |
| SR-16 | `confirm_booking` re-checks the slot, resource, places and price inside one transaction; a clash returns the nearest free slots and creates nothing | FR-11 | Concurrent-confirmation test | [CLEAR] |
| SR-17 | A booking with an online payment option is `pending` until the payment succeeds; a `pending` booking older than 15 minutes is set to `cancelled` and its slot and resource freed | FR-11, FR-15 | Test with the hold task | [CLEAR] |
| SR-17a | A PayPal order is created and captured only by the server, with the amount read from the database; a repeated capture call or webhook records the payment once; PayPal is unavailable when the system currency is not one PayPal supports | FR-15 | Test with PayPal's sandbox: tampered amount, double capture, closed browser after approval, ISK instance | [CLEAR] |
| SR-17b | Every gateway payment row carries the gateway's fee from the gateway's response; the accounts show it as an expense | FR-15, FR-28 | Test with each gateway in test mode | [CLEAR] |
| SR-18 | Bookings on pay on the day, invoice terms or a package session are `confirmed` at once | FR-11 | Test each option | [CLEAR] |
| SR-19 | Booking status changes follow `DESIGN.md` (Booking Status Lifecycle); no booking row is ever deleted | FR-11 | State-transition test; no delete call on `bookings` in code | [CLEAR] |
| SR-20 | Marking a booking `completed` issues the invoice (or the deposit's balance invoice), creates the review task and sends the review request, in one server call | FR-11, FR-15, FR-23 | Test | [CLEAR] |
| SR-21 | Each 24-hour and 1-hour reminder is sent once, with the call link for a video call | FR-11 | Test with the reminder task over a booking's life | [CLEAR] |
| SR-22 | Staff calling `get_calendar_items` receive only their own bookings, whatever filter they send | FR-11 | Access test (`spec-calendar-feature.md` section 11) | [CLEAR] |
| SR-23 | An order's status moves only ordered → in progress → delivered; each change emails the customer | FR-12 | State-transition test | [CLEAR] |
| SR-24 | Booking with a package draws one session; cancelling within the allowed window returns it; an expired package's sessions lapse | FR-13 | Test | [CLEAR] |
| SR-25 | Converting time entries to invoice lines marks them invoiced in the same call; an invoiced entry is refused | FR-14 | Test | [CLEAR] |
| SR-26 | Invoice numbers are `INV-{year}-{sequence}`, sequential per instance with no gap or repeat under concurrent creation | FR-15 | Concurrency test | [CLEAR] |
| SR-27 | Tax = subtotal × the rate at issue, stored on the invoice; with rate 0 no tax line is produced | FR-15 | Calculation tests | [CLEAR] |
| SR-28 | The nightly task sets every unpaid invoice past its due date to `overdue` and emails one reminder | FR-15 | Test with the clock advanced | [CLEAR] |
| SR-29 | Every payment and refund, by gateway or manual, is a `payments` row linked to its invoice, booking or order | FR-15 | Test | [CLEAR] |
| SR-30 | A webhook is verified by signature; an `event_id` already in `webhook_log` is refused; the payload is written to R2; HTTP 200 is returned before the event is applied by a background task | FR-15, FR-18, FR-29 | Duplicate and invalid-signature tests | [CLEAR] |
| SR-31 | The system currency cannot change once a booking or invoice exists; all stored amounts are in it | FR-16 | Test | [CLEAR] |
| SR-32 | A booking, order, enquiry or capture with an email that matches a contact updates that contact; otherwise it creates one | FR-17 | Test | [CLEAR] |
| SR-33 | A lead capture creates the `leads` row and the linked `contacts` row in one call and enrols the contact in the welcome sequence | FR-19 | Test | [CLEAR] |
| SR-34 | Every outgoing email is sent by `server_shared/email_service` through SES from an address on the client's domain, DKIM-signed, in the client's tenant; no email is sent to an address in `email_suppressions` within its scope | FR-18 | Test with the mail catcher; SES sandbox test of DKIM | [CLEAR] |
| SR-35 | A one-click unsubscribe stops all marketing email to that contact and nothing else; a hard bounce or complaint suppresses every email to that address | FR-18 | Test | [CLEAR] |
| SR-35a | A received email is stored once per `Message-ID`, filed to its address or to Unassigned, linked to the contact with the sender's address, its attachments in R2; a failed virus verdict stores nothing; a failed spam or DMARC verdict files it in Spam | FR-18 | Test with recorded SES notifications | [CLEAR] |
| SR-35b | A staff member's Team Inbox calls return only the addresses the staff member belongs to; an Owner's return every address | FR-18 | Access test | [CLEAR] |
| SR-35c | Email bodies older than 90 days are moved to R2 nightly and the row kept as an index; Trash older than 30 days is deleted | FR-18, FR-29 | Test with the clock advanced | [CLEAR] |
| SR-36 | Each automation run is recorded in `automation_runs`; an automation runs once per triggering event | FR-20 | Test | [CLEAR] |
| SR-37 | A blog post with `scheduled_at` in the past is published by the next run of the 15-minute task | FR-21 | Test | [CLEAR] |
| SR-37a | Publishing a post writes four captions to `blog_posts.share_captions`; every shared link carries its `?ref=`; `/_/api/blog-image/{slug}` returns the image of a published post only | FR-21 | Test | [CLEAR] |
| SR-38 | Seeded clauses can be edited and switched off but not deleted; publishing regenerates the public page immediately | FR-22 | Test | [CLEAR] |
| SR-38a | Every publish raises the document's version and writes `legal/{document}/v{version}.html`; every agreement point requires the unticked acceptance box and writes a `consents` row for `t_and_c` and for `privacy_policy`; a Cookie Policy clause with a `feature_key` is active exactly while its feature is in use | FR-22 | Test | [CLEAR] |
| SR-38b | No embedded map or video loads before the visitor accepts cookies or clicks Load; every cookie choice writes a `consents` row with no contact; rows older than 30 days move nightly to `consents/{document}/{year}/{month}/`, kept 2 years for cookies and 7 years for acceptances | FR-22, FR-30 | Test | [CLEAR] |
| SR-39 | A review is not public until approved; the homepage shows at most 10 testimonials | FR-23 | Test | [CLEAR] |
| SR-40 | Every server function the customer portal calls filters to the signed-in customer's own records | FR-24 | Access test with two customers | [CLEAR] |
| SR-41 | Tooltips and the morning briefing are switched per user in `users.preferences` | FR-25, FR-27 | Test | [CLEAR] |
| SR-41a | Every visible screen text is set from a phrase key; no form's Designer text holds visible words; a missing phrase in a language falls back to English and is logged | FR-25 | Code scan of every `form_template.yaml`; test with a phrase removed | [CLEAR] |
| SR-42 | Every AI and speech request goes through `server_ai/ai_request_layer` to App 5 with the instance's Bearer token; no AI provider key exists in the instance | FR-26, FR-27 | Code scan; Vault and secrets scan | [CLEAR] |
| SR-43 | An AI or voice failure returns a clear message (spoken for voice) and leaves the form usable | FR-26, FR-27 | Test with App 5 unreachable | [CLEAR] |
| SR-44 | Assists 9 and 10 and voice tier 5 run on the sample data while it exists; on real data below the thresholds in the AI specification they answer with what is needed | FR-26, FR-27 | Test with sample data, then with thin real data | [CLEAR] |
| SR-44a | Clear sample data deletes every row recorded in `sample_rows`, empties links from real rows to sample rows, empties `sample_rows`, recomputes the summary tables and writes an `activity_log` row; it is refused for every role but Owner | FR-26 | Test (test specification 4.10) | [CLEAR] |
| SR-44b | No email, reminder, campaign or payment is sent for a sample record, and no sample record is exported | FR-26, FR-28 | Test (test specification 4.11) | [CLEAR] |
| SR-45 | Voice tier 3 and tier 5 questions are read-only; no voice question changes data; voice tier 4 writes nothing until the user confirms the filled-in form | FR-27 | Test: every voice query function performs no write; tier 4 test | [CLEAR] |
| SR-45a | A voice tier 5 request to App 5 carries figures and placeholder labels only, never a person's name | FR-27 | Payload inspection | [CLEAR] |
| SR-46 | Profit and loss, outstanding invoices, tax and statements are computed in server code from `invoice`, `payments` and recorded `expenses`; each export button writes one ZIP in that package's import format, as `integration-architecture.md` (Accounting Export) sets out, leaving out void expenses, draft and cancelled invoices and sample records | FR-28 | Calculation tests; an import test of each ZIP into the package's free trial | [CLEAR] |
| SR-47 | The daily backup writes every table to the client's R2 bucket; `activity_log` holds no row older than 30 days after the nightly archive | FR-29 | Test | [CLEAR] |
| SR-48 | Export all data produces a CSV of every table as a background task | FR-29 | Test | [CLEAR] |
| SR-49 | No card number, CVV or full card data is stored in any table or log | FR-30 | Table and log scans | [CLEAR] |
| SR-50 | Every colour on every screen comes from the Colour Scheme; `m3x_theming` (`m3-custom-theme`) sets the palette from `business_profile.palette` and follows the device's light or dark setting | FR-31 | UI conformance scans (Step 29); visual check in light and dark | [CLEAR] |
| SR-51 | A Help message is sent to App 5 with the Bearer token and returns a reference number; App 5's replies appear in the Help view | FR-32 | Test against App 5 | [CLEAR] |
| SR-52 | Usage (memory, storage, email volume) is reported to App 5 daily; above 80% of a container's memory App 5 raises it to 2 GiB once, and raises the Advanced cluster to 16 GiB at 60 active clients and to 32 GiB at 130, each in the next maintenance window; App 5 never reduces a size and messages Mybizz at each resize | FR-33 | Test against a simulated DigitalOcean API (RL-13) | [CLEAR] |
| SR-53 | The heartbeat reaches App 5 every 15 minutes with release version and schema fingerprint and no customer personal data | FR-34 | Test against App 5; payload inspection | [CLEAR] |
| SR-53a | AI and speech requests pass through App 5 without their content being stored or logged; App 5 records only each client's usage counts; Mybizz's Groq organisation has Zero Data Retention on | FR-26, FR-34 | Test against App 5: after an AI and a speech request, no App 5 table or log holds their content; Groq's Data Controls show Zero Data Retention on | [CLEAR] |
| SR-54 | The usage report carries the last staff sign-in and last booking date, from which App 5 raises the at-risk alerts | FR-34 | Test against App 5 | [CLEAR] |
| SR-55 | A service with pricing model `duration` offers time slots sized by its duration; a service with `unit` offers Order and no slot; only active services appear publicly | FR-10 | Test both pricing models and an inactive service | [CLEAR] |
| SR-56 | Quote numbers are `QT-{year}-{sequence}` with no gap or repeat; accepting a quote at `/quote/{token}` creates its invoice, links it and moves a linked deal to `won` in one transaction; a second acceptance changes nothing; a quote past `valid_until` is `expired` the next morning | FR-35 | Concurrency and state tests | [CLEAR] |
| SR-57 | Staff list and change only the deals they own | FR-35 | Access test | [CLEAR] |
| SR-58 | With `config.demo_mode` on, `email_service` delivers every email to the signed-in visitor's own address only, payments use test keys, and the nightly reset returns the demo to its starting state and removes demo sign-ins | FR-36 | Test in a demo instance | [CLEAR] |
| SR-59 | Each client database user is created with a connection limit of 3, a statement timeout of 60 seconds and an idle-in-transaction timeout of 120 seconds | FR-37 | Cluster check (RL-16) | [CLEAR] |
| SR-60 | The heartbeat carries the client database's size and the month's AI and voice spend; App 5 warns the Owner at 1 GiB and £5.60, and at 2 GiB switches off bulk imports and at £16.80 pauses AI and natural voice until the next month | FR-37 | Test against App 5 with simulated heartbeats (RL-14) | [CLEAR] |
| SR-61 | The instance refuses a 21st AI request within a minute and makes it wait; with AI paused, voice falls back to typing and the browser's built-in voice and every form keeps working | FR-37 | Test in App 1 (RL-14) | [CLEAR] |
| SR-62 | App 5 pauses a client's marketing email at 25,000 sent in a month and all email except sign-in at 50,000, resuming on the first of the next month; a paused SES tenant is reported to the Owner and Mybizz | FR-37 | Test against simulated SES (RL-14) | [CLEAR] |
| SR-63 | An incident record in App 5 holds the times of detection, containment, the insurer's call, the ICO report and each client notice; a client notice is sent from App 5 and recorded | FR-38 | Incident drill on the test client instance (RL-22) | [CLEAR] |
| SR-64 | App 5 sends each business reminder at its lead time, reads statutory deadlines from FreeAgent with read requests only, and messages Mybizz when no Wise transaction has reached FreeAgent for 7 days | FR-39 | Simulated clock and FreeAgent sandbox (RL-20, RL-21) | [CLEAR] |
| SR-65 | App 5 records every referral in its `referrals` table as `pending`, `confirmed`, `used` or `void`; a referred client's first annual payment confirms its referral and a refund of it returns it to pending; App 5 sets on the referring client's Paddle subscription one flat discount for the next billing period of one month's price per confirmed credit, at most 5, keeps further confirmed credits in reserve and carries them forward, and marks the credits taken as used once the renewal is paid | FR-40 | Paddle test environment (RL-23) | [CLEAR] |
| SR-66 | App 5 writes `plan_summary` through `/management/plan-summary` daily and after every change, its total equal to Paddle's next-transaction preview; the Plan and billing tab is shown to the Owner only; every referral kit item carries `plan_summary.referral_link` and the discount line; the badge and pop-up appear on the public website only when `business_profile.referral_badge_on` or `referral_popup_on` is True, and the pop-up sets no cookie | FR-40 | Test client instance (RL-23, BR-10) | [CLEAR] |

Every functional requirement FR-01 to FR-40 of the PRD appears in this table at least once.

### 3.2 External Interface Requirements

- **User interfaces:** the Anvil Material 3 theme with the project deviations (`DESIGN.md`); screens built to `spec-ui-standards` (project-library-global); the admin area, customer portal and public website as in `scaffold-spec.md` (Navigation Architecture); WhatsUp on the phone's browser.
- **Hardware interfaces:** the device microphone for WhatsUp, through `anvil.js`, and the device camera for expense receipts, through the file upload, each with the user's permission.
- **Software interfaces:** Stripe, PayPal, Amazon SES, SNS and S3, Cloudflare R2, and App 5 (for Groq, management, DNS and SES provisioning), as `integration-architecture.md` sets out.
- **Communication interfaces:** HTTPS only. Inbound HTTP endpoints: one webhook endpoint per gateway; `/_/api/email/inbound`, `/_/api/email/events`, `/_/api/email/unsubscribe` for SES; `/_/api/blog-image/{slug}`; the management endpoints for App 5, with the instance's Bearer token. Outbound: HTTPS to SES and every other service; SMTP with STARTTLS to the SES SMTP endpoint for Anvil's Users service email.

### 3.3 Performance Requirements

| Requirement | Value | Certainty |
|---|---|---|
| Booking flow server round trips | at most 3 (`get_booking_context`, `get_available_slots`, `confirm_booking`) | [CLEAR] |
| Calendar | one server call per visible period; refresh every 60 seconds while open | [CLEAR] |
| Any server call | progress indicator shown; timeout with retry after 30 seconds | [CLEAR] |
| Long operations | any operation expected to exceed 22 seconds runs as `@anvil.server.background_task` (Anvil platform constraint) | [CLEAR] |
| Webhook handler | returns HTTP 200 before the event is applied | [CLEAR] |
| Morning briefing | 20–30 seconds spoken | [CLEAR] |
| Capacity per client | 1 GiB container memory and 3 database connections; about 100 MB of data a year | [CLEAR] |
| Page response under normal load | screen usable within 3 seconds on a phone connection | [INFERRED] |

### 3.4 Design Constraints

- Five-app model and the no-forms-in-instances rule: reference sections 1 and 2; [[[default:adr-global/adr-client-instance-readme-five-app-system|Mandatory README in Every Client Instance Documenting the Five-App System]]](C:/dev/project-library-global/adr-global/adr-client-instance-readme-five-app-system.md) ADR.
- Form and state architecture: [[[default:adr-global/adr-form-architecture-and-state|Form Architecture and State Management]]](C:/dev/project-library-global/adr-global/adr-form-architecture-and-state.md) ADR; `globals-contract.md`.
- Navigation: [[[default:adr-global/adr-navigation-lambda-link-open-form|Navigation Standard: Lambda/Link/open_form]]](C:/dev/project-library-global/adr-global/adr-navigation-lambda-link-open-form.md) ADR, [[[default:adr-global/adr-htmltemplate-use|HTMLTemplate Use]]](C:/dev/project-library-global/adr-global/adr-htmltemplate-use.md) ADR.
- Webhooks: [[[default:adr-global/adr-webhook-architecture|Webhook Architecture]]](C:/dev/project-library-global/adr-global/adr-webhook-architecture.md) ADR.
- Background tasks and polling: [[[default:adr-global/adr-real-time-and-background-tasks|Real-Time Updates and Background Task Architecture]]](C:/dev/project-library-global/adr-global/adr-real-time-and-background-tasks.md) ADR.
- Data access: [[[default:adr-global/adr-data-access-patterns|Data Access Patterns and Query Limitations]]](C:/dev/project-library-global/adr-global/adr-data-access-patterns.md) ADR.
- Dark mode: [[[default:adr-global/adr-dark-mode|Dark Mode]]](C:/dev/project-library-global/adr-global/adr-dark-mode.md) ADR.
- Platform: [[[default:adr-global/adr-anvil-platform-constraints|Anvil Platform Constraints and Design Boundaries]]](C:/dev/project-library-global/adr-global/adr-anvil-platform-constraints.md) ADR.
- Imports between modules are relative, so that code runs from `master_template`'s package in a client instance (`scaffold-spec.md`).

### 3.5 Non-Functional Requirements

- **Roles:** five roles in `users.role` — `owner`, `manager`, `admin`, `staff`, `customer`. Every callable server function carries one decorator from `server_auth/rbac` naming the roles allowed; the decorator reads the signed-in user from `anvil.users.get_user()` and refuses the call before the function body runs. The interface hides what a role cannot use, from `globals.session_user`, but that is never relied on. `[CLEAR]`
- **Customer isolation:** functions allowed to `customer` take no contact or customer id from the client; they derive it from the signed-in user. `[CLEAR]`
- **Secrets:** `encryption_key` is the only Anvil Secret; credentials are in `vault`, encrypted by `vault_service`; App 5's Bearer token for the instance is held in the Vault. `[CLEAR]`
- **Reliability:** outside-service calls that fail are queued in `server_shared/retry_queue` and retried every 15 minutes; the Owner is notified after repeated failure. Webhook application is idempotent by `event_id`. `[CLEAR]`
- **Recoverability:** daily backups and pre-release and pre-migration backups to the client's R2 bucket; restore by Mybizz from R2 (UF-33). `[CLEAR]`
- **Observability:** structured logging, `health_log`, `error_log` and `activity_log`, heartbeat to App 5, as `observability-requirements.md` sets out. `[CLEAR]`
- **Maintainability:** feature-based packages, never renamed; one module per responsibility (`scaffold-spec.md`); every public function typed and documented; response envelope `{'success', 'data' | 'error'}`. `[CLEAR]`
- **Accessibility:** WCAG 2.2 AA (`spec-ui-standards`, project-library-global). `[CLEAR]`

---

## 4. Appendices

- Package and module structure, navigation and routes: `scaffold-spec.md`.
- Tables and fields: `authoritative-schema.md`; the changes that bring App 1 to it: `schema-migration-log.md`.
- Booking and invoice status lifecycles: `DESIGN.md` (Booking Design, Invoice Design).
- Calendar data model and server function: `spec-calendar-feature.md` section 7.
- Background task schedule: `integration-architecture.md` (Background Tasks).
