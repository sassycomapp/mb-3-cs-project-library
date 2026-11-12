---
title: "business-requirements-document"
doc-id: "mb-c29418ec"
updated: "2026-11-12"
---
# Mybizz CS — Business Requirements Document (BRD)

Why the business needs the application and what it must achieve for the business.

**Standard:** [[[default:docs-standard-global/docstd-business-requirements-document]]](C:/dev/project-library-global/docs-standard-global/docstd-business-requirements-document.md)

**Status:** Draft
**Date:** 2026-10-31
**Author:** Mybizz founder
**Derived from:** `mb-3-cs-concept.md` (the concept) and `mb-3-cs-architecture-reference.md`. Where this document and the concept differ on a business decision, the concept is correct. Feature numbers F-01 to F-36 are those of the concept; the client-facing feature list is `mb-3-cs-feature-list.md`.

**Certainty tags:** `[CLEAR]` stated explicitly; `[PARTIAL]` direction set, specifics not; `[INFERRED]` judgment, not yet confirmed; `[MISSING]` a known gap.

---

## 1. Executive Summary

Mybizz CS gives a consulting or service business one complete, private online platform — website, booking, payments, CRM with a sales pipeline, a team email inbox, email marketing, invoicing, accounts, time tracking, blog, analytics, reviews, AI assistance and voice — in its own isolated application, on its own domain, for £119 a month or £1,190 a year (or the fixed price in the client's currency) with unlimited staff included, month to month, with no add-ons. It replaces the five to seven separate tools such a business otherwise pays for and connects by hand. The business outcome is 100 paying clients, $118,800 a year in revenue and $97,248 a year gross margin after hosting, run by the founder with Mybizz's own management app.

---

## 2. Business Problem / Opportunity

**Problem.** A consulting or service business that wants to operate professionally online needs a website and hosting, a booking system, a payment gateway, a CRM, email marketing and invoicing. Bought separately these cost a team of six $324 a month at list prices, before any setup cost (concept section 6.2). The tools do not talk to each other, so the owner re-types data between them, holds client records in three or four places, and still has no reliable view of the business. On marketplace platforms the business's customers are effectively the platform's customers, and commissions reach 20–23.5% on new customers. `[CLEAR]`

**Opportunity.** Service businesses now operate online as a matter of course; owners expect AI; a growing group wants to own its customer relationships instead of paying a marketplace. `[CLEAR]`

**Cost of doing nothing.** Without Mybizz CS, the founder's target clients continue to pay more for a fragmented, partial setup, and the founder has no product in the market. `[CLEAR]`

---

## 3. Business Objectives

| # | Objective | How success is measured | Certainty |
|---|---|---|---|
| BO-1 | Replace the separate-tool stack of a consulting or service business with one platform at one price | Every need in concept section 4.5 is met by a built feature; the price is £119 a month or £1,190 a year, or the fixed price in the client's currency, with no per-staff fee, no commission and no add-on | [CLEAR] |
| BO-2 | Reach 100 clients | 100 paying clients; $178,800 annual recurring revenue on the monthly plan | [CLEAR] |
| BO-3 | Hold the margin | Break-even at 9 clients; net profit of £7,563.84 a month at 100 clients after every client-related and fixed cost; the highest cost of any one client held below its £119 by fair-use limits; a referring client's renewal never below its yearly cost; cost per client reviewed monthly in App 5 | [CLEAR] |
| BO-4 | The client owns the client's data and customers | Each client in a separate app and database; data exportable as CSV at any time; backups in the client's own R2 bucket; no marketplace or shared directory | [CLEAR] |
| BO-5 | A client goes live without help | Average onboarding-to-live time under 7 days through the onboarding wizard | [CLEAR] |
| BO-6 | Keep clients | Client retention from year 1 to year 2 above 90% | [CLEAR] |
| BO-7 | The founder, with part-time support from 20 clients, supports every client | Under 2 support tickets per client per month; at-risk alerts raised before a client leaves | [CLEAR] |
| BO-8 | Serve businesses across Mybizz's markets | The United States, Canada, the United Kingdom, the European Union, the European Economic Area and Switzerland, from launch: Stripe and PayPal, five app languages, and a fixed price in each market's own currency | [CLEAR] |

---

## 4. Stakeholders

| Role | Name/Group | Interest |
|---|---|---|
| Sponsor and operator | The Mybizz founder | Product, revenue, margin, support load; operates App 5 |
| Primary users | Client Owners | Run the business on Mybizz CS; configure it; own its data |
| Primary users | Client staff: Managers, Admins, Staff (providers and salespeople) | Daily operation: bookings, customers, deals, email, invoices, time |
| Secondary users | The clients' customers | Book, order, pay, review, use the customer portal |
| Visitors | Visitors to a client's public website and landing pages | Find the business, enquire, become leads |
| Prospects | Businesses trying the demo apps | See the whole product before paying |
| Platform suppliers | Anvil; DigitalOcean | Anvil's Business plan terms for Apps 1, 2, 3 and 5; DigitalOcean's terms for client instances |
| Service providers | Stripe, PayPal, Paddle, Amazon Web Services (SES), Cloudflare, Groq | Each provider's own terms and fees |
| Regulators | The Information Commissioner's Office (UK GDPR); each client's own data protection authority | Lawful processing of personal data |

---

## 5. Scope

### In Scope

- A complete platform for consulting businesses and service businesses, on the ground or in the cloud or both: teams of every size, with unlimited staff, and solo practitioners (concept section 4).
- Every way such a business delivers work: at its premises, at the customer's location, online live, as delivered work, and to groups (concept section 4.3).
- All 36 features of concept section 7 (F-01 to F-36), including AI assistance with all ten assists, WhatsUp voice tiers 1 to 5, and the sample data of concept section 8.
- One isolated application instance per client, on the client's own domain, in its own container with its own database.
- Five app languages: English at launch; French, German, Spanish and Italian added one at a time.
- Two demo apps for prospects (F-36).
- The Mybizz management app (App 5), built and running before the first paying client.
- Available from launch in the United States, Canada, the United Kingdom, the European Union, the European Economic Area and Switzerland, with the United Kingdom as Mybizz's home market.

### Out of Scope

- Product catalogue, shopping cart, inventory, shipping.
- Hotel and guest room reservations, and hospitality table bookings.
- Membership tiers, subscription access control, and recurring billing for client businesses.
- A multi-vertical feature toggle system.
- A marketplace or shared directory of Mybizz clients.
- A support-ticket system for the clients' own customers.
- A knowledge base, events management.
- A native mobile app; voice and the Team Inbox run in the phone's browser.
- Right-to-left app languages.

---

## 6. Business Requirements

| # | Requirement | Satisfies Objective | Priority | Certainty |
|---|---|---|---|---|
| BR-1 | A client's customers can book every delivery mode online — at the business, at the customer's location, video or phone call, group session, with a room or equipment where needed — without phoning | BO-1 | Must | [CLEAR] |
| BR-2 | A client can sell fixed-price delivered work and session packages online | BO-1 | Must | [CLEAR] |
| BR-3 | A client gets paid by the payment option set per service — in full, deposit, on the day, or invoice with payment terms — through Stripe, with PayPal as an option | BO-1, BO-8 | Must | [CLEAR] |
| BR-4 | A client issues professional invoices with payment terms and its own tax setting, automatically on completion, from time worked, and manually | BO-1 | Must | [CLEAR] |
| BR-5 | A client has a professional website on its own domain, with homepage templates, a blog, landing pages and legal pages, edited on the page itself without a web designer, and shares every blog post to social media in one tap | BO-1 | Must | [CLEAR] |
| BR-6 | A client keeps one record per customer, created automatically from bookings, orders, enquiries, captures and emails, with companies, segments and CSV import and export | BO-1, BO-4 | Must | [CLEAR] |
| BR-7 | A client has unlimited email addresses on its own domain, a Team Inbox for every staff member, and transactional and marketing email from its own domain, with no email account or bill of its own | BO-1 | Must | [CLEAR] |
| BR-8 | A client sees how the business performs: revenue, bookings, customers, marketing return, per service and per provider | BO-1 | Must | [CLEAR] |
| BR-9 | A client gets AI help with its writing and its data, and can run the business by voice from a phone, at no extra charge | BO-1 | Must | [CLEAR] |
| BR-10 | Every staff member of a client is included in the one price, with no limit, each with an access level that the server enforces | BO-1, BO-2 | Must | [CLEAR] |
| BR-11 | Each client runs in its own application and database; no client can reach another's data; Mybizz never takes a client's customer data for its own use | BO-4 | Must | [CLEAR] |
| BR-12 | A client can export all of its data at any time and has its backups and history in its own R2 bucket | BO-4 | Must | [CLEAR] |
| BR-13 | A client registers and sets up its instance itself through a resumable onboarding wizard with a help video on every step, including connecting its domain and importing its existing contacts; assisted setup is offered at one month's subscription, once | BO-5 | Must | [CLEAR] |
| BR-14 | Mybizz provisions, monitors, bills and updates every client from one management app, and every release reaches every client through `master_template` | BO-2, BO-3, BO-7 | Must | [CLEAR] |
| BR-15 | Mybizz is alerted when a client is at risk: no staff sign-in for 14 days, no bookings for 30 days, a ticket unanswered for 3 days | BO-6, BO-7 | Must | [CLEAR] |
| BR-16 | A client reaches Mybizz through a Help button; a problem that stops the client's business is acknowledged within 4 business hours, every other ticket answered within 24 hours on business days | BO-6, BO-7 | Must | [CLEAR] |
| BR-17 | Every feature explains itself through a tooltip that opens a short help video, in the client's app language, so a client rarely needs support | BO-5, BO-7 | Must | [CLEAR] |
| BR-18 | Each client runs in its own container with its own database, with its own memory and its own database connections | BO-3 | Must | [CLEAR] |
| BR-19 | Mybizz records each client's hosting, email and AI usage so that the cost per client is known every month | BO-3 | Must | [CLEAR] |
| BR-20 | The platform meets the UK GDPR, the client's own privacy law and PCI DSS: card data never touches the platform, credentials are encrypted, every significant action is audited | BO-4 | Must | [CLEAR] |
| BR-21 | A client keeps its accounts in Mybizz CS — invoices, payments, refunds and expenses with receipts — with profit and loss, tax and statement reports, and exports them to Wave, Xero or QuickBooks in one click | BO-1 | Must | [CLEAR] |
| BR-22 | Each client's website and app carry the client's own brand: one of eight palettes, light and dark following the device | BO-1 | Must | [CLEAR] |
| BR-23 | A client's salespeople follow every sale in a deals pipeline and send quotes that customers accept online and that become invoices | BO-1 | Must | [CLEAR] |
| BR-24 | A client uses the app in its chosen language: English, French, German, Spanish or Italian | BO-1, BO-8 | Must | [CLEAR] |
| BR-25 | A new client sees every report, assist and voice question working from the first day, on sample data the Owner clears with one button | BO-5, BO-6 | Must | [CLEAR] |
| BR-26 | A prospect tries the whole product before paying, in two demo apps that reset every night | BO-2 | Must | [CLEAR] |
| BR-27 | No client's heavy or abusive use can slow, stop or cost money for another client: every shared resource carries a per-client fair-use limit, and the platform grows each client's container and the shared database without the founder acting, telling the founder each time | BO-3, BO-7 | Must | [CLEAR] |
| BR-28 | A security incident is handled under Mybizz's insurance and the law: recorded with its timeline, the insurer called before any outside supplier, affected clients told without undue delay, and the Information Commissioner's Office told within 72 hours of a breach of personal data Mybizz controls | BO-4, BO-6 | Must | [CLEAR] |
| BR-29 | Mybizz never lets its insurance, its Cyber Essentials certificate or a statutory filing lapse: each is reminded to the founder before its deadline | BO-7 | Must | [CLEAR] |
| BR-30 | A client chooses the monthly plan or the annual plan (12 months for the price of 10) and may buy assisted setup for one month's price; an annual client that refers a new annual client earns one month off its next renewal, up to 5 months, never paid in cash | BO-2, BO-3 | Must | [CLEAR] |

Every objective BO-1 to BO-8 is satisfied by at least one requirement; every requirement cites an objective that exists.

---

## 7. Assumptions and Constraints

| # | Assumption or constraint | Certainty |
|---|---|---|
| AC-1 | **Multi-instance model:** the project uses the dependency-based five-app model — App 1 development, App 2 `master_template`, App 3 `blank_client_template`, App 4 one instance per client, App 5 management (reference section 1). This governs every downstream document | [CLEAR] |
| AC-2 | **Currency:** one system currency per instance, set at onboarding and locked once the first booking or invoice exists; an optional display currency per service, for display only | [CLEAR] |
| AC-3 | **Time zones:** one IANA time zone per instance; UTC storage; conversion at display time | [CLEAR] |
| AC-4 | **Platform:** Anvil.works, full-stack Python, with the Material 3 theme; client instances on the Anvil App Server, which builds and extends each client's tables from App 3's table definitions; a release only adds tables and nullable columns | [CLEAR] |
| AC-5 | **Hosting:** Apps 1, 2, 3 and 5 on Mybizz's Anvil Business account; every client instance in its own DigitalOcean App Platform container with its own database in Mybizz CS's Managed PostgreSQL cluster (`spec-client-instance-hosting`) | [CLEAR] |
| AC-6 | The four conditions before the first paying client of `spec-client-instance-hosting` section 10 are met: the full suite on the release image, the load measurement, the tested rebuild and the end-to-end release path | [PARTIAL] |
| AC-7 | Each client instance has 1 GiB of container memory and 3 database connections, and stores about 100 MB of data a year | [CLEAR] |
| AC-8 | Hosting costs $12 a month per client container, $260 to about $1,040 a month for the cluster by stage, with a standby throughout, and $116 a month fixed; about $1,836 a month at 100 clients (`spec-client-instance-hosting` section 11) | [CLEAR] |
| AC-9 | Groq AI text and speech cost about $1 to $3 per client per month at heavy use, and Amazon SES $0.10 per 1,000 emails; both are absorbed within the margin; App 5 measures them | [CLEAR] |
| AC-10 | Every client has its own domain, which carries its website and every email address | [CLEAR] |
| AC-11 | The client pays its payment providers' fees, its domain, and Cloudflare R2 storage above the free allowance in its own Cloudflare account; Mybizz absorbs Anvil, DigitalOcean, Amazon SES and Groq costs | [CLEAR] |
| AC-14 | Amazon grants Mybizz's AWS account SES production access before the first paying client | [PARTIAL] |
| AC-12 | Capacity has no fixed client limit; the cluster's stages hold 265 clients | [CLEAR] |
| AC-13 | Launch with no marketing budget: founder-led, the United Kingdom first, the other markets through content | [CLEAR] |

---

## 8. Success Criteria / KPIs

| KPI | Target | Measured by | Certainty |
|---|---|---|---|
| First clients onboarded | 10 | App 5 client registry | [CLEAR] |
| Clients, goal | 100 | App 5 client registry | [CLEAR] |
| Monthly revenue at 100 clients | £11,900 | App 5 billing | [CLEAR] |
| Net profit at 100 clients | £7,563.84 a month | FreeAgent, read by App 5 | [CLEAR] |
| Break-even | 9 clients | App 5 client registry and FreeAgent | [CLEAR] |
| Client retention, year 1 to year 2 | above 90% | App 5 client registry | [CLEAR] |
| Average onboarding-to-live time | under 7 days | Registration date to onboarding completion, App 5 and `business_profile.onboarding_completed_at` | [CLEAR] |
| Support tickets per client per month | under 2 | App 5 tickets | [CLEAR] |
| Problem tickets acknowledged within 4 business hours | 100% | App 5 tickets | [INFERRED] |
| Hosting, email and AI cost per client | known every month | App 5 usage records | [CLEAR] |

---

## 9. Risks

| Risk | Likelihood | Impact | Mitigation | Certainty |
|---|---|---|---|---|
| A DigitalOcean region outage or account suspension stops every client | Low | High | Standby database node from the first client; nightly R2 copies; the rebuild procedure in `deployment-procedures.md`, tested before the first paying client (AC-6) | [CLEAR] |
| The Anvil App Server behaves differently from Anvil's hosted service | Medium | High | One pinned App Server version; the full test suite on the release image before every release (AC-6) | [CLEAR] |
| Mybizz's Anvil account fails or is compromised; releases and provisioning stop | Low | Medium | Client instances keep running; account-level procedures in `deployment-procedures.md`; two-factor authentication | [CLEAR] |
| Founder capacity: every ticket lands with the founder | Medium | High | Self-service help on every screen (F-25), a target under 2 tickets per client per month, at-risk alerts (F-34) | [CLEAR] |
| Early churn: a client who sees no value in the first weeks leaves quietly | Medium | Medium | At-risk alerts, the onboarding wizard with help videos, sample data showing every report from the first day, the demo apps before purchase | [CLEAR] |
| Competitors copy features | Medium | Medium | One price for the whole team in each market's own currency, founder-direct support, an ownership architecture marketplaces cannot offer | [CLEAR] |
| Cost per client rises (hosting, email, AI, voice) | Medium | Medium | Usage recorded in App 5, monthly review | [CLEAR] |
| A client's sending harms the email reputation of others | Low | Medium | One SES tenant per client, automatic suppression of bounces and complaints, marketing only to opted-in contacts | [CLEAR] |
| The market does not name the problem | Medium | Medium | Marketing names the five-tool problem before the features | [CLEAR] |
| A schema change must be applied by hand to up to 100 instances | High | Medium | Migration discipline in `deployment-procedures.md`; schema fingerprint in every heartbeat | [CLEAR] |

---

## 10. Approval

| Role | Name | Date | Signature/Confirmation |
|---|---|---|---|
| Sponsor | Mybizz founder | | |
