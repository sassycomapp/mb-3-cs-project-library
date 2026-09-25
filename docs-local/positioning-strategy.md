---
title: "positioning-strategy"
doc-id: "positioning-strategy"
---
# Mybizz — Business Strategy & Positioning Document
**mybizz.live**
Version 1.0 | Pre-Launch | 2026

---

## Purpose of this document

This document is the authoritative foundational reference for Mybizz. It defines what the business is, who it serves, how it is positioned, how it is priced, and how it will go to market. It is not a brainstorming document. Every statement in it is a decision unless explicitly marked otherwise.

It serves three purposes:
- Strategic guidance for AI-assisted planning and development workflows
- Foundation for all web content, marketing, and business development
- Reference document for investor or partner conversations

---

## 1. What Mybizz Is

Mybizz is a complete, standalone online business platform for consulting and services businesses, delivered as a subscription SaaS product.

Every subscriber receives their own fully operational, isolated application instance — their own website, booking system, payment processing, CRM, email marketing, invoicing, blog, and analytics — in a single platform, at a single price, with nothing extra to configure, integrate, or pay for separately.

**Domain:** mybizz.live
**Platform:** Anvil.works (full-stack Python, hosted)
**UI theme:** Material Design 3
**Schema:** 36 data tables
**Capacity:** 100 clients (V1.x)

---

## 2. The Problem Mybizz Solves

Setting up an online business presence is unnecessarily complex, expensive, and fragmented. A service business trying to operate professionally online faces:

- A website builder — one subscription, one learning curve
- A booking system — another subscription, another integration
- A payment gateway — another signup, another configuration
- A CRM — another tool, another import/export
- An email marketing platform — another account, another connection
- An invoicing tool — yet another service

Each of these is a web of decisions, technical requirements, and monthly fees. Any one of them can take weeks to set up correctly. Together, they cost hundreds of dollars a month and still do not talk to each other seamlessly. Most small service businesses never get all of it working. They end up with a partial setup — perhaps a website and a booking link — and manage everything else manually.

The result is what Mybizz was built to eliminate: eleven-minute bookings, six browser tabs open simultaneously, client data entered three times in three different systems, and no reliable view of how the business is actually performing.

**Mybizz is one setup. One platform. One price. Everything included.**

---

## 3. The Founder Story

Mybizz was not built from a whiteboard. It was built from personal experience.

The founder spent years attempting to establish an online business and encountered exactly the problem described above — not as an observer, but as the person sitting in front of the screen, overwhelmed, wasting money, and never quite getting to a complete, working system. Every service provider sold a piece of the puzzle. Nobody sold the complete picture.

After speaking with other small business owners, the pattern was consistent. The challenge was not ambition or effort — it was the sheer fragmentation of the tools required to operate a modern service business online. Most people eventually gave up on parts of it, paid someone else to figure it out, or settled for less than they needed.

Mybizz is the product the founder needed and could not find. That origin is the product's most credible differentiator: it was designed by someone who lived the problem, not by a product team optimising metrics.

---

## 4. Target Market

### 4.1 Who Mybizz serves

Mybizz serves any business that sells consulting or services — online or in person. The platform is deliberately horizontal across service types.

**Examples include but are not limited to:**
- Health and wellness: physiotherapists, chiropractors, psychologists, nutritionists, personal trainers, yoga instructors
- Beauty and lifestyle: salons, spas, nail technicians, aestheticians, massage therapists
- Professional services: consultants, coaches, business advisors, accountants, attorneys
- Creative and technical: photographers, videographers, web designers, IT consultants
- Trades and skilled services: plumbers, electricians, tutors, driving instructors, caterers

**The common thread:** they sell time or deliverables, they book clients, they need to get paid, and they need a professional online presence.

### 4.2 Business size

Mybizz V1 is optimised for:
- Solo practitioners
- Small practices of 2–10 providers

The multi-provider architecture (Staff roles, provider-specific calendars, per-provider revenue tracking) supports team-based businesses from day one.

### 4.3 Geography

Mybizz is global from launch. The platform is built for international operation:
- Stripe for global payment processing
- Paystack for South African and African clients (Stripe does not operate in South Africa)
- Multi-currency: system currency set at onboarding for all administration; display currency configurable per service or region
- UTC storage with client timezone display

South Africa is the founder's home market and the primary focus for direct, personal client relationships. International clients are acquired through content and digital marketing.

---

## 5. What Mybizz Is Not

Mybizz is not a marketplace.

Booksy, Vagaro, and Mindbody are platforms where service businesses compete for clients inside a shared discovery environment. Your competitors appear on your listing. Your clients are effectively platform clients — if you leave the platform, the client relationship stays behind. New client acquisition through the marketplace attracts commissions of up to 20–23.5% per transaction.

Mybizz does not list your business alongside competitors. It does not take a commission on bookings. It does not own your client relationships. Every client record, booking history, contact detail, and transaction belongs exclusively to the subscribing business — isolated in their own application instance, inaccessible to anyone else.

In V2, Mybizz will offer an optional marketplace presence to clients who want it. This will be a choice, not the default model.

---

## 6. Competitive Positioning

### 6.1 The real cost comparison

Marketplace platforms advertise low base prices. The full cost tells a different story.

| | Booksy | Vagaro | Mindbody | **Mybizz** |
|---|---|---|---|---|
| Base price | $29.99/mo | $23.99/mo | $99–$699/mo | **$50/mo** |
| Per-staff fee | +$20/person | +per calendar | included | **included** |
| Processing fee | 2.49–2.69% | 2.2–3.5% | 3.5% | **gateway rate only** |
| Marketplace commission | up to $49.99/mo (Boost) | none | 20% new client | **none** |
| Add-ons required | none listed | forms, SMS, storage | many | **none** |
| Website included | no | add-on | no | **yes** |
| Hosting included | no | no | no | **yes** |
| Client owns data | no | no | no | **yes** |
| Competitors on your page | yes | yes | yes | **no** |

A solo practitioner on Booksy with one staff member pays $49.99/month before processing fees, with no website and no ownership of their client data.

Mybizz at $50/month is all-in. The price is the price.

### 6.2 The ownership argument

On any marketplace platform, your clients are the platform's clients. The platform's algorithm decides your visibility. Your competitors appear next to your listing. If you build your client base through Booksy and then leave, the clients remain in Booksy's ecosystem.

On Mybizz, your client database is structurally isolated. It exists only in your application instance. No other business can access it. No algorithm affects your visibility. Your clients are your clients on day one and remain yours permanently.

---

## 7. The Platform — V1 Feature Set

### Core capabilities
- Authentication, RBAC (5 roles: Owner, Manager, Admin, Staff, Customer), full audit trail
- The Vault: encrypted secrets store for all API keys — owner-only, TOTP-protected
- Dashboard: revenue, bookings, customer totals with period comparison

### Public-facing website
- Home, About, Contact, Privacy Policy, Terms & Conditions
- 4 homepage templates: Classic, Services, Booking, Minimalist
- Landing pages: Lead Capture, Event Registration, Video Sales Letter
- Dynamic: Services listing & detail, Blog listing & posts
- Mobile responsive, SEO optimised, managed from admin panel

### Service catalogue
- Duration pricing (time-based: e.g. 60-min session at R450)
- Unit pricing (deliverable-based: e.g. logo design at R3,500)
- Per service: category, provider, pricing model, images, video, meeting type, intake form

### Bookings & appointments
- Full booking flow: service → provider → date/time → meeting type → intake form → payment → confirmation
- Calendar: month, week, day views
- Availability management: business hours, blocked dates, provider schedules
- Meeting types: In-Person, Video Call, Phone Call
- Payment at booking: full or deposit, configurable per service
- Automatic reminders: 24 hours and 1 hour before
- Recurring appointments: weekly, bi-weekly, monthly

### Payments & invoicing
- Stripe (global) and Paystack (Africa) — gateway-agnostic service layer
- Payment integrated into booking flow — no separate checkout
- Automatic invoice generation on booking completion (PDF + email)
- Manual invoice creation supported
- Multi-currency: system currency + display currency

### CRM & contacts
- Unified contacts table: Lead / Customer / Inactive status
- Full activity timeline per contact
- Tags, segments, CSV import/export
- Automatic contact creation from bookings and enquiry forms
- Lifetime value, total bookings, last contact date

### Email & campaigns
- System email: Anvil built-in (login, password reset)
- Transactional email: Brevo SMTP (confirmations, reminders, invoices)
- Marketing email: Brevo Campaigns (sequences, broadcasts, webhooks)
- Pre-built sequences: New Client Welcome, Appointment Follow-Up, Re-engagement, Referral Request

### Blog & content
- Rich text editor, SEO per post, post statuses
- View count tracking
- Public listing and individual post pages

### Tasks & segmentation
- Manual and basic auto-generated tasks
- Pre-built segments: New Leads, Active Customers, Inactive Customers, High Value
- Custom segment builder

### Analytics
- Revenue, bookings, customers — period comparison dashboard

### Security & compliance
- POPIA, GDPR, PCI DSS alignment
- Server-side role enforcement
- Encrypted secrets storage

---

## 8. V2 Roadmap (Deferred)

- Client portal (customer self-service for bookings, invoices, profile)
- Advanced analytics (revenue by service/provider, marketing ROI, PDF/CSV export, scheduled reports)
- Complex task automation
- Optional marketplace presence for clients who want discovery exposure
- Additional homepage templates
- PayPal integration

---

## 9. Pricing Strategy

### 9.1 Billing model

Annual billing only. No monthly option.

Annual billing is the correct model for Mybizz for two reasons:

First, setting up a business on Mybizz is a meaningful commitment. A business configures their website, uploads their service catalogue, connects their payment gateway, imports their contacts, and establishes their booking flow. This is not a trial. It is a business setup. The annual model reflects the nature of the commitment.

Second, annual billing simplifies operations. Revenue is received upfront. Hosting is paid upfront. There are no monthly collection cycles, no failed payment chases, and no month-to-month churn risk from clients who have not yet felt the full value of the platform.

### 9.2 Pricing tiers

| Tier | Clients | Monthly equivalent | Annual fee | Nature |
|---|---|---|---|---|
| Launch | 10 | $20/month | $240/year | Founding members — at-cost pricing, grandfathered for life |
| Grandfathered | 40 | $35/month | $420/year | Early adopter pricing, grandfathered for life |
| Full price | 50 | $50/month | $600/year | Standard pricing |

All tiers are grandfathered — clients who join at a given tier retain that price for as long as they remain subscribers.

### 9.3 Launch client terms

The 10 launch client spaces are offered at $240/year — effectively at cost. In exchange, launch clients agree to:
- Provide honest feedback during the first 90 days
- Allow Mybizz to reference them as a client (with approval on any case study content)

This is a transparent arrangement. Launch clients know they are joining a platform in active development and are compensated with the lowest price available, held for life.

### 9.4 Revenue model

| Year | Clients | Annual revenue |
|---|---|---|
| Year 1 (building to capacity) | 10 + 40 + 50 = 100 | $49,200 |
| Year 2 (full capacity, retained) | 100 | $49,200 |

At full capacity and 100% retention, annual revenue is $49,200. Direct hosting cost at 100 clients is $18,000/year. Gross margin before marketing and personal costs is $31,200/year.

This is a lean business at V1 scale. The model strengthens materially as full-price clients replace grandfathered tiers through natural attrition, and as V2 capacity expands beyond 100 clients.

### 9.5 Custom domain

Clients who want a custom domain (e.g. bookings.their-business.com) supply their own registered domain. Mybizz configures the subdomain connection. Clients without a custom domain receive a subdomain on the Mybizz/Anvil infrastructure at no additional cost.

---

## 10. Go-to-Market Strategy

### 10.1 Principles

Mybizz launches with no marketing budget. The strategy must therefore be:
- Founder-led and personal
- Built on authentic story rather than paid reach
- South Africa-first, then international by content

This is not a weakness. A founder who lived the problem, built the solution, and can speak about both honestly is a more credible voice than any ad campaign.

### 10.2 Phase 1 — Launch clients (10 clients)

The goal of Phase 1 is not revenue — it is proof. Ten real businesses using a real product and willing to say so publicly.

**Tactics:**
- Personal network first: identify 10 service business owners in the founder's direct network or warm referrals
- WhatsApp and direct outreach: brief, honest, personal — "I built this, you are exactly who I built it for, I want you to be one of ten founding members"
- LinkedIn: the founder's own profile, sharing the story of building Mybizz — not promotional content but honest building-in-public posts
- Local South African business communities: Facebook groups, local networking events, BNI chapters, professional associations

**What to offer:** $240/year (grandfathered for life) + genuine involvement in shaping the product.

**What not to do:** paid advertising, cold email blasts, or any approach that requires pretending the platform has more clients than it does.

### 10.3 Phase 2 — Grandfathered clients (40 clients)

With 10 clients using the platform, the narrative shifts from "founding member" to "proven platform." Phase 2 is about systematic outreach using real evidence.

**Tactics:**
- Case studies from launch clients: one or two detailed write-ups of how a real business uses Mybizz (with client permission)
- LinkedIn content: founder posts sharing lessons, client results, and product updates — consistent, weekly
- SEO content: blog posts targeting high-intent searches such as "how to set up online booking for a physiotherapy practice" or "best booking system for personal trainers South Africa"
- Referral programme: launch clients who refer a paying client receive a $50 credit or account extension
- Industry-specific outreach: identify professional associations and communities for target industries (beauty, wellness, consulting) and engage authentically — not as a sponsor, but as a member

**Pricing:** $420/year (grandfathered for life)

### 10.4 Phase 3 — Full price clients (50 clients)

By this stage, Mybizz has 50 real clients, documented case studies, visible reviews, and an established content presence. Phase 3 introduces paid acquisition supported by organic proof.

**Tactics:**
- Targeted LinkedIn and Facebook advertising to service business owners in South Africa and internationally
- Google Search advertising targeting booking software and business management keywords
- Partnership outreach: complementary service providers (accountants, business coaches, web designers) who serve the same clients and can refer
- Content marketing: continued SEO blog content now supported by a track record

**Pricing:** $600/year

### 10.5 International strategy

International clients are acquired primarily through content and search. A service business owner in the UK, Australia, or the US searching for "complete booking and website platform for consultants" should find Mybizz. SEO content targeting these search terms, combined with the Stripe integration for global payments, makes international acquisition possible without a local presence.

The Paystack integration is a specific advantage in African markets where Stripe does not operate. This positions Mybizz as the only complete platform built for African service businesses.

---

## 11. Brand Identity

### 11.1 Name and domain

**Brand name:** Mybizz
**Domain:** mybizz.live

The domain is the brand statement: *my business, live.* Online. Operational. Running. The .live TLD does functional work — it communicates that a business on Mybizz is active, present, and open.

### 11.2 Brand story

Mybizz exists because its founder needed it and it did not exist.

After years of attempting to build an online business presence and encountering the same fragmented, expensive, technically demanding landscape that every small service business faces, the founder built a single platform that solves the whole problem. Not part of it. All of it.

That story — "I built what I needed" — is the brand's most durable asset. It is specific, honest, and verifiable. It connects directly with the target customer who has had the same experience.

### 11.3 Brand position statement

*Mybizz is the complete online business platform for consulting and services businesses. One platform. One price. Everything included. You own your data, your clients, and your business.*

### 11.4 Tone of voice

- Direct and honest — no marketing language that overpromises
- Empathetic — speaks to the real frustration of the target customer, not a fictional ideal
- Confident but not arrogant — the product works, the founder knows it, there is no need to oversell
- Human — written by someone who ran the same race, not a corporate marketing department

### 11.5 Visual identity (in development)

The design system is being developed under the /design-consultation pipeline. Key directions established:
- Material Design 3 (M3) as the UI framework
- Dual-tone approach: management UI (clear, functional, professional) and public-facing UI (warm, inviting, trustworthy)
- Four onboarding colour palettes for client customisation: Professional Blue, Warm Coral, Calm Teal, Sophisticated Purple
- Typography: Roboto, consistent M3 scale

---

## 12. Key Messages

### Primary message
*One platform. Everything included. You own it.*

### Supporting messages

**On fragmentation:**
*Most service businesses are running their online operations across five or six separate tools. Each one costs money. None of them talk to each other properly. Mybizz replaces all of them.*

**On ownership:**
*On Booksy, your clients are Booksy's clients. On Mybizz, your client database belongs to you — structurally isolated, privately hosted, accessible only by you.*

**On pricing:**
*$50 a month sounds like more than the alternatives. Add per-staff fees, processing commissions, marketplace cuts, add-ons, and the cost of a separate website — and you are paying $80–150 a month for less. Mybizz is $50. That is the number. Nothing extra.*

**On simplicity:**
*Set up your business once. Website, booking, payments, CRM, email, invoicing — all connected, all working, all in one place. Then run your business.*

**On Africa:**
*Stripe does not operate in South Africa. Mybizz integrates Paystack natively. Your clients pay in Rands. Your money arrives in your account. No workarounds.*

**The founder message:**
*I built this because I needed it. I could not find a single platform that did everything a service business needs without costing a fortune and requiring a technical degree to set up. So I built one. That is Mybizz.*

---

## 13. Support Model

### 13.1 Self-service first

Mybizz is designed to be operated without technical knowledge. Every feature is accompanied by in-app help content linked to concise, non-technical explanations and how-to video guides. The platform is the support system.

### 13.2 Direct support

Client support is handled through a chat facility within the Mybizz management application. A client message raises a ticket and notifies the founder. Response target: within 24 hours on business days.

This system is in development and will be operational before the first paying client is onboarded.

### 13.3 Onboarding

| Tier | Onboarding | Cost |
|---|---|---|
| Standard | 30-minute guided call — founder walks the client through initial setup | Included |
| Premium | Full setup — Mybizz configures the client's instance on their behalf | $200 (one-time) |

Onboarding is personal at this stage. The founder conducts every call. This is not a scalable model beyond 100 clients but it is the right model for V1 — direct relationship, immediate feedback, high early retention.

---

## 14. Technical Architecture Summary

| Component | Detail |
|---|---|
| Platform | Anvil.works — full-stack Python, built-in HTTPS and hosting |
| Architecture | Dependency model: mb-3-cs (dev) → master_template (frozen release) → client instances |
| Data isolation | Each client instance is a separate Anvil app with its own database. Cross-client access is architecturally impossible |
| UI | Material Design 3 (M3), Dependency ID: 4UK6WHQ6UX7AKELK |
| Routing | Anvil Routing dependency (ID: 3PIDO5P3H4VPEMPL) for public pages |
| Payments | Stripe (global), Paystack (Africa) — gateway-agnostic service layer |
| Email | Anvil built-in (system), Brevo SMTP (transactional), Brevo Campaigns (marketing) |
| Authentication | Anvil Users service, RBAC via custom decorators |
| Secrets | The Vault — encrypted store, `encryption_key` in Anvil Secrets only |
| Navigation | NavigationRailLayout/NavigationDrawerLayout + plain Link components + lambda click handlers (admin); Anvil Routing (public) |

---

## 15. What Is Explicitly Out of Scope

The following are not part of Mybizz V1 and will not be built:

- Product catalogue, shopping cart, inventory management, shipping
- Room reservations and hospitality table bookings
- Membership tiers, subscription access control, recurring billing for client businesses
- Multi-vertical feature toggle system
- Support tickets, knowledge base, reviews system, events management, expense tracking
- PayPal (deferred to V2)
- Client portal (deferred to V2)

---

## 16. Success Metrics

| Metric | V1 Target |
|---|---|
| Launch clients onboarded | 10 |
| Grandfathered clients onboarded | 40 |
| Full-price clients onboarded | 50 |
| Total annual recurring revenue at capacity | $49,200 |
| Client retention (year 1 to year 2) | >90% |
| Average onboarding-to-live time | <7 days |
| Support tickets per client per month | <2 |

---

*Document owner: Mybizz founder*
*Status: Authoritative — decisions reflected herein are confirmed unless marked for review*
*To be updated after each significant product, pricing, or strategic decision*
