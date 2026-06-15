# Mybizz CS — Platform Overview

**Vertical:** Consulting & Services
**Architecture:** Single vertical, dependency-based (master_template → client instances)
**UI Theme:** Material Design 3 (M3)
**Platform Capacity:** 100 clients (V1.x)
**Schema Date:** 2026-05-03 (43 tables)

---

## 1. What Mybizz Is

Mybizz is a complete online business management platform for consulting and services businesses, delivered as a subscription SaaS product. Each subscriber receives a fully operational, isolated Anvil application instance — their own online presence and business management system, from public website through to CRM, bookings, payments, and reporting.

**Target businesses:** salons, spas, therapists, personal trainers, physiotherapists, consultants, coaches, clinics, studios, and any business that sells services — either as timed appointments or as fixed-price deliverables.

---

## 2. The Strategic Decision — Single Vertical

Mybizz was originally designed as a multi-vertical open platform supporting four business types: Consulting & Services, E-commerce, Hospitality, and Membership. The decision was made to narrow to a single vertical — Consulting & Services only. This was a deliberate strategic choice made on the grounds of complexity, maintenance burden, and focus.

The entire codebase, database schema, and planning documents are scoped to Consulting & Services. Products, orders, shopping carts, shipping, room reservations, membership tiers, subscription billing, and hospitality-specific features are explicitly out of scope.

**See:** `9-multi-vertical-to-single-vertical-conversion.md`

---

## 3. The Five-App Structure

The platform consists of five Anvil applications, all hosted on the mybizz Anvil account:

| App | Role | Updated via |
|---|---|---|
| `mb-3-cs` | Development workspace | Daily development; GitHub `develop` branch |
| `master_template` | Published dependency — all server logic | Merge `develop` → `stable` in GitHub |
| `blank_client_template` | Provisioning clone source — tables only, no code | When schema changes |
| `[client instances]` | One per client — UI forms, 43 tables, dependency | Automatically via master_template stable branch |
| `Mybizz_management` | Platform operator tooling | Separate; built post-launch |

`blank_client_template` is a maintained asset. All new client instances are cloned from it. It contains all 43 Data Tables with correct schema, master_template set as dependency, and no server modules or forms. See ADR-030.

**Note:** mb-2-cs was the original development application and has been deprecated. All development has migrated to mb-3-cs.

---

## 4. Client Instances

Each subscriber receives their own independent Anvil application. This instance contains no code of its own — it depends on master_template as a versioned library. The client instance contains only the client's Data Tables and their configuration.

- **Data isolation is structural.** Each client instance is a separate Anvil app with a separate database. Cross-client data access is architecturally impossible.
- **The master template is stateless.** It contains zero client data. All state lives in the client's Data Tables.
- **Clients never see source code.** The master template is published as a compiled dependency.

---

## 5. Core Features

### 5.1 Authentication & Administration
- Email/password login, password reset
- RBAC roles: Owner, Manager, Admin, Staff, Customer
- Rate-limited login, account lockout, full audit trail
- Dashboard: revenue, booking count, customer totals — with period comparison
- Settings: business profile, theme, currency, user & permissions management
- The Vault: encrypted secrets store for all API keys — owner-only access
- **Onboarding:** Onboarding is a separate form with required elements (currency selection, legal acknowledgement). Settings is a single tabbed form for all configuration. Bidirectional navigation between Onboarding and Settings. **[`26-onboarding-finality.md`]**

**See:** `22-tiers-model.md`, `26-onboarding-finality.md`

### 5.2 Public-Facing Website
- Standard pages: Home, About, Contact (with enquiry form), Privacy Policy, Terms & Conditions
- Dynamic pages: Services listing & detail, Blog listing & posts
- Home page: Four configurable templates (Classic, Services, Booking, Minimalist) — all use same components, zero additional coding burden
- Landing pages: Lead Capture, Event Registration, Video Sales Letter — template-based static pages with CTA
- All pages: mobile responsive, SEO optimised, managed from the admin panel

### 5.3 Service Catalogue
Two pricing models — both fully supported:

| Model | Description | Example |
|---|---|---|
| `duration` | Price tied to a time block | 60-minute massage at R450 |
| `unit` | Price tied to a deliverable | Logo design at R3,500 |

Per service: name, description, category, provider, pricing model, price, duration (if applicable), images, video URL, meeting type, intake form (optional), is_active.

### 5.4 Bookings & Appointments
- Service selection → provider selection → date & time → meeting type → intake form → payment → confirmation
- Calendar view: month, week, day
- Availability management: business hours by day, blocked dates, provider-specific availability
- Meeting types: In-Person, Video Call, Phone Call
- Payment at point of booking: full payment or deposit — configurable per service
- Automatic reminders: 24 hours before and 1 hour before; video call link included
- Recurring appointments: weekly, bi-weekly, monthly

**See:** ADR-035 (Form Architecture and State Management — booking flow latency management)

### 5.5 Payments & Invoicing
- Payment gateways: Stripe (global), Paystack (Africa) — one per client instance
- Payment model: All payments to the business's central account. No per-provider payment accounts.
- No shopping cart. Payment is integrated into the booking flow.
- Invoicing: Automatic invoice generation on booking completion — PDF and email delivery. Manual invoice creation also supported. PDF generated server-side using reportlab.
- Multi-currency: System currency (set at onboarding, immutable after first transaction) plus optional display currency.

**See:** ADR-039 (PDF Invoice Generation), ADR-038 (Webhook Architecture)

### 5.6 CRM & Contacts
- Contact database: Lead / Customer / Inactive status
- Full activity timeline per contact — bookings, emails, notes, form submissions
- Tags and segmentation — simple tagging at capture, filter-based segments
- CSV import/export
- Automatic contact creation from bookings and enquiry forms
- Contact detail: lifetime value, total bookings, last contact date

### 5.7 Email & Campaigns
Three distinct email systems:

| System | Purpose | Technology |
|---|---|---|
| Anvil built-in | System emails (login, password reset) | `anvil.email.send()` |
| Brevo SMTP | Transactional (booking confirmations, reminders, invoices) | SMTP relay |
| Brevo Campaigns | Marketing (campaign sequences, broadcasts) | REST API |

Campaign features (V1):
- Pre-built campaign sequences: New Client Welcome, Appointment Follow-Up, Re-engagement, Referral Request
- Campaign editor with configurable delays and event-based triggers
- Hourly background task for enrollment processing
- Open and click tracking via Brevo webhooks

### 5.8 Blog & Content
- Full blog with rich text editor
- SEO control per post: title, meta description, URL slug
- Post statuses: Draft, Published, Archived
- View count tracking
- Public blog listing and individual post pages

### 5.9 Tasks & Segmentation
- Manual task creation against contacts
- Basic auto-generated tasks (follow-ups, review requests)
- Pre-built segments: New Leads, Active Customers, Inactive Customers, High Value
- Custom segment builder using contact tags, status, spend, and booking history

---

## 6. Technical Stack

| Layer | Technology | Notes |
|---|---|---|
| Platform | Anvil.works | Full-stack Python, built-in hosting & HTTPS |
| UI | Material Design 3 (M3) | Dependency ID: 4UK6WHQ6UX7AKELK |
| Database | Anvil Data Tables | PostgreSQL-backed, 43 tables |
| Authentication | Anvil Users service | Built-in, RBAC via custom decorators |
| Anvil Secrets | `encryption_key` only | One item only — used by vault_service.py |
| The Vault | All client API keys and credentials | Encrypted store, retrieved via `get_vault_secret()` |
| Transactional email | Brevo SMTP | Via Anvil email service (SMTP) |
| Marketing email | Brevo Campaigns | Sequences, webhooks, native Brevo integration |
| Payments | Stripe + Paystack | One gateway per client instance |
| Routing | Anvil Routing dependency | ID: 3PIDO5P3H4VPEMPL — public pages |
| PDF generation | reportlab | Server-side only; ADR-039 |

**Platform constraints:** The platform is designed to work within Anvil's specific parameters — not around them. This includes Skulpt client-side limitations, no native URL routing, lazy-loaded Data Table rows, server call latency, background task limits, and Anvil Secrets size limits. See ADR-034 (Anvil Platform Constraints and Design Boundaries).

---

## 7. Architecture

### 7.1 Dependency Model
Each client instance is an independent Anvil app containing only the client's Data Tables and configuration. It depends on master_template as a versioned library.

### 7.2 Data Isolation
Each client instance is a separate Anvil app with its own Data Tables. `app_tables` always resolves to the current client's data. Cross-client data access is architecturally impossible.

### 7.3 Navigation Architecture

| Area | System |
|---|---|
| Admin & customer portal | Custom HtmlTemplate layout + plain Link components + lambda click handlers |
| Public website pages | Anvil Routing dependency (@router.route) |

**Navigation ruling — admin and customer portal:**
- Use plain `Link` components in the sidebar — not `NavigationLink`
- Navigation is triggered via lambda click handlers: `lambda e, fn=form_name, a=attr_name: self._nav_click(fn, a)`
- Loop variable capture is mandatory — always use `fn=` and `a=` keyword arguments
- `NavigationDrawerLayoutTemplate` is not used in the current architecture
- `navigate_to` is not used

**Note:** Native M3 navigation components (`NavigationDrawerLayoutTemplate`, `NavigationLink`, `NavigationRailLayout`) are fully supported by the M3 theme dependency but are not used in the current Mybizz architecture. This is a usage decision based on the custom layout approach. See `2-navigation-lambda-link-open-form.md`.

**See:** `2-navigation-lambda-link-open-form.md`

## 7.4 Dependency app_tables Resolution — Verified

**Test date:** 2026-06-13
**Status:** Confirmed ✅

### The Question
The architecture assumes that `app_tables` called from within a master_template server module
resolves to the client instance's Data Tables — not the dependency's own tables. This was
an untested assumption at the time of initial design.

### Test Conducted
Two identical Anvil apps were created:
- `datatable-test-host-app` — acting as the dependency (master_template equivalent)
- `datatable-test-client-app` — acting as the client instance

Both apps contained an identical Data Table (`table_1`) with identical columns. A server
module in the host app wrote a row via `app_tables.table_1.add_row()`. The client app
depended on the host app and called that server function.

### Findings
The row was written to the **client app's** `table_1`. The host app's `table_1` received
nothing.

### Architectural Implication
`app_tables` inside a dependency server module resolves to the **calling client instance's**
Data Tables at runtime. This confirms that master_template is genuinely stateless — it
holds no data of its own. All 43 tables, all server modules, and all integrations operate
against the client instance's data automatically. Data isolation between client instances
is structural and requires no application-level enforcement.

This finding is the verified foundation of the entire dependency-based architecture.

### 7.5 Client Instance Architecture — Confirmed

The following structural decisions are confirmed and tested. They supersede any earlier assumptions about the client instance model.

**Client instances contain no server modules.** All server logic — business rules, integrations, background tasks, Data Table operations — lives exclusively in master_template. Client instances contain:

- UI forms (cloned from master_template at provisioning)
- All 43 Data Tables (data never leaves the instance)
- master_template set as dependency (stable branch)
- No server modules of their own

If a client instance were to contain server modules with the same function names as master_template, Anvil would run the instance's version and ignore the dependency — permanently breaking update propagation. This must never occur. See ADR-028.

**Consequences for UI updates:** Because UI forms reside in the client instance clone, updates to forms in master_template do not automatically propagate. Server logic updates propagate automatically. Form updates require a separate consideration and process. See ADR-029.

---

## 8. Provider Model

Providers (therapists, trainers, consultants, practitioners) are **Staff-role users** in the system.

- Solo business: owner is the only provider
- Multi-provider business: each provider has a Staff user account with their own schedule and services
- All payments go to the business's central payment account
- Revenue per provider is tracked on bookings and reported in Analytics

---

## 9. The Vault

API keys and credentials are stored in The Vault — an encrypted secrets store built into each client instance. The encryption key is stored in Anvil Secrets (the only item there). Server functions retrieve secrets via `get_vault_secret()` at runtime without exposing them to client code.

Access to the Vault is restricted to the Owner role and requires TOTP step-up authentication on every open.

**See:** `3-payment-security-boundary-vault.md`

---

## 10. Payments

Two payment gateways are supported. Each client instance uses one gateway, configured at onboarding.

- **Stripe** — primary international gateway, Anvil-native integration
- **Paystack** — South African and African clients (Stripe does not operate in South Africa)

Both gateways use the same gateway-agnostic service layer. Payment is integrated into the booking flow — no separate checkout. System currency is set at onboarding and immutable after the first transaction.

---

## 11. Email and Communication

Three distinct email systems serve different purposes and must not be conflated. Brevo replaces all Zoho products (`1-brevo-replaces-zoho-email.md`). Each client connects their own Brevo account. The free tier provides 300 emails/day with SMTP access included.

**See:** `1-brevo-replaces-zoho-email.md`

---

## 12. The User Roles

| Role | Access |
|---|---|
| Owner | Full access including Vault, financial data, user management |
| Manager | Operational management — bookings, customers, marketing, reporting. No Vault or financial config |
| Admin | Booking and customer management. No financial reporting, settings, or Vault |
| Staff | Own calendar and bookings only. No access to other staff data or settings |
| Customer | Own bookings, invoices, and account in the customer portal |

Role enforcement is server-side. Client-side navigation visibility is a UX convenience only.

---

## 13. Data Architecture

### 13.1 Schema Overview
43 tables across 7 groups: Core (10), Bookings (6), CRM (9), Email (3), Content (5), Policy (2), Finance (3), Summary (5).

**See:** `10-single-contacts-table.md`, `11-schema-scope-reduction.md`

### 13.2 Single Contacts Table
A single `contacts` table serves as the unified person record. All customers are contacts but not all contacts are customers. All foreign keys point to `contacts`.

### 13.3 UTC Storage & Timezone Display
All datetimes stored in UTC. Display conversion uses `business_profile.timezone` (IANA string).

**See:** `5-timezone-utc-storage-display-conversion.md`

### 13.4 Lead Capture
When a lead is captured from a landing page, both `leads` and `contacts` rows are created simultaneously. `leads.converted_to_contact_id` is set immediately. No manual conversion step required.

**See:** `4-lead-capture-simultaneous-creation.md`

### 13.5 Data Access Patterns
Anvil Data Tables do not support JOINs, complex aggregations, or raw SQL. All data access is server-side. Frequently accessed related data is denormalized. Complex aggregations are pre-computed by background tasks and stored in summary tables. See ADR-040 (Data Access Patterns and Query Limitations).

---

## 14. V1 Scope vs Deferred

### V1 (MVP)
- Authentication, RBAC, Vault, settings
- Service catalogue (duration + unit pricing)
- Bookings & appointments (calendar, availability, booking flow, reminders, recurring)
- Payments & invoicing (Stripe + Paystack)
- CRM & contacts (tags, pre-built segments, activity timeline, CSV import/export)
- Email campaigns (Brevo — pre-built sequences, triggers, broadcasts)
- Tasks (manual creation + basic auto-generation)
- Blog (rich text editor, SEO, public listing)
- Public website (Home, About, Contact, Privacy, Terms — 4 home page templates)
- Landing pages (Lead Capture, Event Registration, VSL)
- Dashboard analytics (revenue, bookings, customers — period comparison)
- Security & compliance (POPIA, GDPR, PCI DSS)

### Deferred to V2
- Client portal (customer self-service for bookings, invoices, profile)
- Advanced analytics (revenue by service/provider, marketing ROI, export to PDF/CSV, scheduled reports)
- Complex task automation
- Additional homepage templates beyond the four V1 templates
- PayPal integration
- Time tracking (logging billable hours against bookings/services)
- Custom segment builder (filter by tags, status, spend, booking history)

## 15. What Is Out of Scope

The following are explicitly not part of the Consulting & Services platform:

- Product catalogue, shopping cart, inventory management, shipping
- Room reservations and hospitality table bookings
- Membership tiers, subscription access control, recurring billing (for client businesses)
- Multi-vertical feature toggle system
- Support tickets, knowledge base, reviews, events, expense tracking
- PayPal (deferred to post-V1)

---

## 16. Business Model

- Maximum clients: 100 (V1.x)
- First 10 clients (Launch): $25/month, grandfathered for life
- Clients 11–50 (Pioneer): $35/month, grandfathered for life
- Clients 51+ (Business): $50/month, price not fixed for life
- Direct costs at capacity (Anvil hosting): $18,000/year (subject to Anvil billing clarification — see Section 18)

**Onboarding:**
- All tiers: 30-minute guided setup session included
- $100 DFY (Done For You) setup option available
- Onboarding is a separate form with required elements (currency selection, legal acknowledgement)
- Settings is a single tabbed form for all configuration

**Free Trial — Abandoned (ADR-031):**
The 30-day free trial on Anvil's free tier has been abandoned. Replaced by:
- Live guided demo on mybizz's own fully configured instance
- 30-minute guided onboarding session for all new paying clients
- 30-day money-back guarantee post-signup

All paying clients are provisioned on the Anvil Business plan from day one. No client instance runs on the Anvil free tier.

**See:** `22-tiers-model.md`, `26-onboarding-finality.md`, ADR-031

---

## 17. Mybizz_management

> **Note:** Mybizz_management is outside the scope of this project. The following is recorded as an architectural intention.

### 17.1 Data Architecture

Mybizz_management maintains three data tables for client records:

**Client Details Table (Owner-Modifiable):**
- Contains client details that the Owner has the right to change at any time
- The Owner may remove entries from this table
- Updates append new values; old values are retained in the amendment log

**Retained Records Table (Legal/Financial):**
- Contains client details retained by Mybizz for legal or financial purposes
- The Owner has no right to remove these
- Held separately and immutable from the Owner's perspective

**Amendment Log (Append-Only):**
- Full history of all credential and configuration changes
- Never overwritten — always holds the latest values plus the full history
- Outside the client instance scope but a system-level behaviour

### 17.2 Visibility and Control

Mybizz_management communicates with client instances via bidirectional HTTP endpoints secured with per-client Bearer tokens. Three patterns operate simultaneously:

**Mybizz → Client (on demand and polling):**
Master_template exposes a `management_endpoints` module with secured HTTP routes for health reads, suspension, notification push, and configuration control. Mybizz_management calls these from its UI and via a scheduled polling background task.

**Client → Mybizz (event push):**
Client instances push critical events to a Mybizz_management receiver endpoint — payment failures, background task errors, Vault access, error thresholds. Calls are non-blocking.

**Security:** Each client instance has a unique management Bearer token. Compromise of one instance does not affect others.

Mybizz_management is deferred until after master_template reaches launch readiness and the first client is manually onboarded. Until then, Anvil's per-app IDE tools provide operator visibility. See ADR-033.

**See:** `26-onboarding-finality.md`, `21-client-data-management-rights-and-mybizz-retention-boundary.md`, ADR-033

---

## 18. Update Deployment Model

Updates flow as follows:

```
mb-3-cs (develop branch)
    ↓ merge when tested and ready
master_template (stable branch)
    ↓ automatically
All client instances tracking stable branch
```

There is no manual per-client update action. Merging to `stable` is a production action. Release discipline — full testing in mb-3-cs before merge — is the quality gate.

For high-risk releases, version tags (`v1.0.0`, `v1.1.0`) allow staged rollout: a subset of clients is updated first; remaining clients follow once stability is confirmed. All managed from the single mybizz Anvil IDE.

Clients receive a release notification via Anvil built-in email. The notification is informational — the update has already been applied. An internal in-app notification system is planned for the Mybizz_management phase.

**See:** ADR-029 (Dependency Update Model)

---

## 19. Observability Architecture

Client instance observability is delivered through application-level metrics. Master_template writes key events and health data to designated tables within each client instance. Mybizz_management queries these via the management HTTP endpoints and presents a consolidated dashboard across all client instances.

Anvil's built-in observability tools (App Logs, CPU/Memory tabs, Profiling and Tracing — available from early 2026) remain in use for per-app diagnostic investigation. They are complementary to, not a replacement for, the consolidated Mybizz_management monitoring layer.

External monitoring tools (Prometheus, Grafana) were evaluated and rejected as unnecessary at this scale and inconsistent with the hosted-only infrastructure model.

**See:** ADR-032 (Observability Architecture), ADR-033 (Mybizz_management Visibility and Control)

---

## 20. Platform Constraints Summary

The platform is designed to work within Anvil's specific parameters. Key constraints:

- **Skulpt client-side runtime:** Not all Python 3 stdlib works. Money and date logic lives in server modules only. See ADR-034.
- **No native URL routing:** Public pages use the community routing library. Admin portal uses lambda/Link/open_form. See ADR-002, ADR-037.
- **Lazy-loaded Data Table rows:** Never iterate without `.search()` or `.limit_to()`. See ADR-040.
- **Server call latency:** Every `anvil.server.call()` is an HTTP round-trip. Minimize in user-facing flows. See ADR-035.
- **Background task limits:** Business plan required. Per-account limits must be verified at 100 clients. See ADR-036.
- **No WebSocket push:** Real-time updates use client-side timer polling. See ADR-036.
- **Anvil Secrets limits:** One item only (`encryption_key`). All other secrets in the Vault. See ADR-003.
- **BlobMedia row size limits:** Store file references, not contents, for large files. See ADR-034.
- **Git integration quirks:** All branching/merging happens in GitHub, not the Anvil IDE. See ADR-034.

**See:** ADR-034 (Anvil Platform Constraints and Design Boundaries)

---

*End of file — platform-overview.md*