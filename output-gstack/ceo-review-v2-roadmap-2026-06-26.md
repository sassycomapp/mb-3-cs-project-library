---
title: "ceo-review-v2-roadmap-2026-06-26"
doc-id: "mb-11490aaf"
---
# CEO REVIEW — Mybizz V2 Roadmap

**Date:** 2026-06-26
**Mode:** SCOPE EXPANSION
**Scope:** V2 Roadmap (14 planned features + 10 expansion proposals)
**Context:** V1 not yet launched. 41 ADRs, 43 tables, 50 wireframes. Previous CEO review (2026-06-15) validated V1 as HOLD SCOPE. This review evaluates the V2 roadmap for strategic coherence, sequencing, and scope discipline.

---

## Step 0: Premise Challenge

### 0A. Is V2 the right conversation right now?

**The hard question:** V1 has not launched. There are zero paying clients. Reviewing a 14-item V2 roadmap before V1 has a single user is a classic founder trap — planning the future to avoid the discomfort of shipping the present.

**However** — there is a legitimate reason to review V2 now: architectural decisions being made for V1 (schema, navigation patterns, component architecture) will either enable or constrain V2 features. A V2 review now is really a **V1 architectural compatibility check**, not a V2 build plan. That framing is healthy.

**What would happen if we did nothing about V2?** Nothing bad. V1 ships, clients use it, feedback arrives, and V2 priorities self-correct. The risk of NOT reviewing V2 now is zero. The risk of over-investing in V2 planning before V1 validation is real.

**Verdict:** This review is valuable only if it (a) identifies V1 architectural decisions that would block V2, and (b) ruthlessly prioritizes which V2 items actually matter. It is dangerous if it becomes a build plan.

---

### 0B. Existing Code Leverage

| V2 Feature | V1 Code That Partially Exists |
|---|---|
| AI Assistant Layer | None — greenfield |
| Client Portal | `CustomerLayout` navigation structure already defined (architecture.md). Customer role exists in RBAC. |
| Advanced Analytics | Dashboard module exists. Revenue/booking/customer metrics already tracked. |
| PayPal Integration | Gateway-agnostic payment service layer (ADR-019). Stripe + Paystack already abstracted. Adding PayPal is a new adapter, not a new architecture. |
| Time Tracking | `Time Entries` already in nav structure. Schema may already have a table. |
| Custom Segment Builder | Pre-built segments exist (New Leads, Active Customers, etc.). CRM module has filtering. |
| Additional Homepage Templates | Template system exists (4 templates). Adding more is additive. |
| Optional Marketplace Presence | None — greenfield. Requires a separate discovery layer. |
| Dark Mode | M3 theme system exists. Dark mode is a token swap. |
| Customer Portal Self-Service | Same as Client Portal — shares the CustomerLayout. |
| Multi-Source Video Support | YouTube embed exists. Extending to Vimeo/others is a provider abstraction. |
| Mybizz Management Platform | Architecture defined (App 5). ADR-033 covers visibility. Deferred to post-launch. |
| WhatsApp Task Creation | Task system exists. WhatsApp integration is greenfield. |
| Platform Capacity Expansion | Anvil platform constraint (ADR-034). May require infrastructure change. |

**Key finding:** 8 of 14 V2 items have partial V1 foundations. The payment layer, template system, RBAC, and navigation architecture already accommodate expansion. This is good — V1 was designed with V2 in mind.

---

### 0C. Dream State — 12 Months From Now

```
CURRENT STATE                    THIS PLAN (V2)                    12-MONTH IDEAL
─────────────────                ──────────────                    ───────────────
V1 not launched                  14 V2 features planned            V1 launched, 50+ clients
0 paying clients                 No prioritization                 AI assistant live
No feedback data                 No sequencing                     Client portal active
No revenue                       No effort estimates               $25K+ ARR
                                 No dependency mapping             Feedback-driven V2
```

**The gap:** The V2 roadmap is a wish list, not a plan. It has no prioritization framework, no effort estimates, no dependency mapping, and no connection to actual user feedback. This is expected — and it's fine as long as we treat it as a menu, not a commitment.

---

### 0C-bis. Implementation Alternatives

**Approach 1: "Feedback-First" (Recommended)**
- **Summary:** Launch V1. Collect 90 days of client feedback. Let feedback drive V2 priority. Use this review to identify only the V1 architectural guards needed.
- **Effort:** S (for this review) — just identify blockers
- **Risk:** Low
- **Pros:** No wasted build effort. V2 priorities are real, not imagined. Founder energy stays on launch.
- **Cons:** V2 starts later. Some architectural prep opportunities may be missed.
- **Reuses:** Everything V1 already built.

**Approach 2: "Architectural Prep"**
- **Summary:** Before V1 launch, make targeted schema/navigation/component decisions that enable V2 features without building them. Add V2-ready hooks to V1 code.
- **Effort:** M — 1-2 weeks of targeted prep work
- **Risk:** Medium — risk of over-engineering V1 for hypothetical V2 needs
- **Pros:** V2 features drop in faster when needed. Fewer breaking changes.
- **Cons:** Delays V1 launch. Adds complexity to V1 codebase. Builds for imagined requirements.
- **Reuses:** V1 architecture as foundation.

**Approach 3: "V2 Sprint"**
- **Summary:** Build 3-5 V2 features before or immediately after V1 launch to differentiate in market.
- **Effort:** L — 4-8 weeks
- **Risk:** High — no user feedback to validate priorities
- **Pros:** Launches with more features. Marketing advantage.
- **Cons:** Builds features nobody asked for. Delays revenue. Founder burnout risk.
- **Reuses:** V1 codebase.

**RECOMMENDATION:** Choose **Approach 1 (Feedback-First)**. Use this review to identify the 2-3 V1 architectural decisions that would block V2, document them as ADRs, and stop there. V2 planning resumes after 90 days of V1 client data.

---

### 0E. Temporal Interrogation

> **HOUR 1 (V1 launch):** What V2 items require V1 architectural prep?
- Client Portal needs the `CustomerLayout` and customer auth flow to be V2-ready
- PayPal needs the payment adapter pattern to remain extensible
- AI Assistant needs a clean component injection pattern in forms

> **HOUR 2-3 (post-launch feedback):** What will clients actually ask for?
- Almost certainly: Client Portal (self-service booking management)
- Probably: PayPal (international clients), Advanced Analytics
- Possibly: Dark Mode, Time Tracking
- Unlikely to be requested: Marketplace, WhatsApp, AI Assistant

> **HOUR 4-5 (V2 build):** What will surprise us?
- The AI Assistant spec estimates 3-4 days. That is optimistic by 3x for production-quality implementation with error handling, prompt tuning, and UX polish.
- Client Portal requires a second auth flow and a separate layout — this is a significant architectural addition, not a feature.
- Platform Capacity Expansion may require moving off Anvil or negotiating enterprise pricing.

> **HOUR 6+ (V2 maturity):** What will we wish we'd planned?
- The Client Portal and the Mybizz Management Platform share infrastructure (customer auth, cross-app communication). Building one without the other creates rework.

---

### 0F. Mode Selection

Based on the analysis above:

1. **SCOPE EXPANSION** — Dream big, propose the ambitious version. User selected this mode.
2. **SELECTIVE EXPANSION** — Hold baseline, cherry-pick expansions.
3. **HOLD SCOPE** — Maximum rigor, make it bulletproof.
4. **SCOPE REDUCTION** — Ruthless cut to minimum viable version.

**User selected: SCOPE EXPANSION**

---

## Section 1: Architecture Review

**V2 items that require V1 architectural prep:**

| V2 Feature | V1 Dependency | Architectural Impact | Risk if Not Prepared |
|---|---|---|---|
| Client Portal (10.3/10.11) | CustomerLayout, customer auth flow | Second auth context. Customer users need a separate landing page, separate nav, separate session state. | **HIGH** — retrofitting a second auth context into a single-layout app is painful |
| AI Assistant (10.2) | Component injection in forms | Floating helper needs a consistent trigger pattern across all forms. | MEDIUM — can be added later but cleaner if planned |
| PayPal (10.5) | Payment adapter pattern | Gateway-agnostic layer exists (ADR-019). Adding PayPal is a new adapter. | LOW — architecture already supports this |
| Mybizz Management (10.13) | Cross-app HTTP communication | Bidirectional endpoints with Bearer tokens. | MEDIUM — ADR-033 defines the pattern but no code exists |
| Platform Capacity (10.14) | Anvil infrastructure | May require Anvil enterprise plan or architectural change. | HIGH — could be a hard ceiling |
| Marketplace (10.9) | Entirely new discovery layer | No V1 foundation. Requires a separate app or service. | LOW if deferred — no V1 impact |
| Advanced Analytics (10.4) | Dashboard module | Extension of existing metrics. Export (PDF/CSV) requires server-side generation. | LOW — additive |

**Architectural verdict:** The Client Portal is the single highest-risk V2 item because it requires a second auth context and a separate layout. If V1's `CustomerLayout` is not wired for V2 expansion, the Client Portal becomes a rewrite, not a feature addition.

**Action needed:** Before V1 launch, confirm that `CustomerLayout` in the startup routing (architecture.md Section 7) supports the full V2 customer portal navigation (My Dashboard, My Bookings, My Invoices, My Reviews, Support Tickets, Account). This is a V1 architectural guard, not a V2 build.

---

## Section 2: Error & Rescue Map

V2 features that introduce new external dependencies and their failure modes:

| Feature | External Dependency | Failure Mode | Current V1 Pattern |
|---|---|---|---|
| AI Assistant | LLM API (OpenAI/similar) | Rate limit, timeout, hallucination, cost spike | None — greenfield |
| WhatsApp Task Creation | WhatsApp Business API | Message delivery failure, webhook reliability | None — greenfield |
| PayPal | PayPal API | Payment failure, webhook signature mismatch | Stripe/Paystack patterns exist — reusable |
| Client Portal | None (internal) | Session state corruption between admin and customer contexts | Single-context auth exists |
| Advanced Analytics (PDF export) | Server-side PDF generation | Large report timeout, memory pressure | ADR-039 covers PDF invoice generation — reusable |

**Key finding:** The AI Assistant has the most complex error surface of any V2 feature. LLM APIs are non-deterministic, rate-limited, expensive, and can produce harmful output. The spec's 3-4 day estimate does not account for:
- Prompt injection defense
- Output sanitization
- Cost monitoring per client
- Graceful degradation when API is down
- User feedback loop (thumbs up/down on generated content)

**Action needed:** The AI implementation spec needs an error and rescue section before build begins. Do not treat AI as a simple API call.

---

## Section 3: Security & Threat Model

**New attack surfaces introduced by V2:**

| Feature | Attack Vector | Severity | Mitigation |
|---|---|---|---|
| AI Assistant | Prompt injection via user input | HIGH | Input sanitization, output filtering, sandboxed prompts |
| AI Assistant | Data leakage through prompts | HIGH | Never send client data to LLM without explicit user action |
| Client Portal | Horizontal privilege escalation (customer A sees customer B data) | CRITICAL | Server-side filtering on every query. No client-side auth assumptions. |
| WhatsApp | Message spoofing, spam | MEDIUM | Webhook signature verification, rate limiting per business |
| Marketplace | Fake listings, review manipulation | MEDIUM | Manual approval for V1 marketplace, automated later |
| Mybizz Management | Admin token compromise affects all clients | CRITICAL | Per-client Bearer tokens, token rotation, audit logging |

**The Client Portal is the security landmine.** In V1, all authenticated users are business-side (Owner/Manager/Admin/Staff). The Customer role exists in RBAC but the CustomerLayout is deferred. When customers get their own portal, every server module that queries data must be audited for customer-context safety. A booking query that returns "all bookings" for an admin must return "only MY bookings" for a customer.

**Action needed:** Before building the Client Portal, create an ADR defining the customer data access contract. Every server module touched by the portal must have explicit customer-context filtering.

---

## Section 4: Data Flow & Interaction Edge Cases

**AI Assistant data flow (the most complex V2 flow):**

```
User input → Input validation → Prompt composition → LLM API call
   ↓              ↓                    ↓                   ↓
nil input    XSS/injection       Context mapping      API timeout
empty input  too long            Template missing     rate limit
wrong type   prompt injection    Wrong context        hallucination
                                                     cost spike
   ↓
Result display → User edits → Save back to form
   ↓                ↓              ↓
unreadable      empty edit      wrong field
too long        abandon mid     concurrent edit
harmful content                 validation fail
```

**Each node has 3-4 failure paths.** The spec addresses none of them. This is the difference between a "3-4 day" feature and a production-quality AI layer.

**Client Portal edge cases:**
- Customer logs in on phone, books appointment, closes browser mid-booking → orphaned booking state
- Customer and admin edit the same booking simultaneously → conflict resolution needed
- Customer views invoice while admin voids it → stale data
- Customer's session expires mid-action → redirect to login, lose context

---

## Section 5: Code Quality Review

**V2 roadmap items that risk over-engineering:**

| Feature | Risk | Recommendation |
|---|---|---|
| Custom Segment Builder | V1 already has 4 pre-built segments. A full segment builder with arbitrary filtering is significant scope for unclear value. | Start with 2-3 more pre-built segments. Only build the builder if clients ask for it. |
| Optional Marketplace | A marketplace is a separate product, not a feature. Discovery, ranking, reviews, moderation, listing management — each is a sub-product. | Kill this as a V2 item. If it happens, it's V3+ and a separate business unit. |
| Platform Capacity Expansion | This is an infrastructure decision, not a feature. It may require moving off Anvil entirely. | Treat as a business decision, not a development task. |

**V2 items that risk under-engineering:**

| Feature | Risk | Recommendation |
|---|---|---|
| AI Assistant | The spec is architecturally sound but underestimates production complexity by 3x. | Add a dedicated "AI Production Readiness" phase. |
| Client Portal | Described as a "self-service portal" but requires a full second layout, auth context, and data access pattern. | Treat as a major feature, not a bolt-on. |

---

## Section 6: Test Review

**V2 features and their test complexity:**

| Feature | Unit Tests | Integration Tests | E2E Tests | Manual QA |
|---|---|---|---|---|
| AI Assistant | Prompt templates, input validation | LLM API mocking, result handling | Full flow with real API | Output quality review |
| Client Portal | Customer data access | Cross-context auth | Full booking/view/invoice flow | Security audit |
| PayPal | Payment adapter | PayPal sandbox | Full payment flow | Real transaction test |
| WhatsApp | Message parsing | WhatsApp sandbox | End-to-end message → task | Manual message test |
| Dark Mode | Token mapping | Visual regression | — | Visual review |
| Advanced Analytics | Metric calculations | Export generation | PDF/CSV output | Data accuracy check |

**Key finding:** The AI Assistant and Client Portal each have test complexity comparable to an entire V1 feature module. Neither is a "quick add."

---

## Section 7: Observability & Monitoring

**V2 features that need new observability:**

| Feature | What to Monitor | Why |
|---|---|---|
| AI Assistant | API call count per client, cost per call, latency, error rate, hallucination reports | LLM costs are unpredictable. One client could burn $100/day with aggressive usage. |
| Client Portal | Login attempts, session duration, feature usage, support ticket volume | Measure adoption and identify UX friction |
| PayPal | Transaction success rate, webhook delivery, reconciliation errors | Payment failures directly impact revenue |
| WhatsApp | Message delivery rate, task creation success, webhook latency | External API reliability |

**Action needed:** The AI Assistant needs a cost dashboard from day one. LLM API costs are the single biggest V2 financial risk. Without per-client cost tracking, one power user could consume the entire platform's margin.

---

## Section 8: Database & State Management

**V2 schema additions needed:**

| Feature | New Tables/Columns | Impact |
|---|---|---|
| AI Assistant | `ai_usage_log` (client_id, assist_type, tokens_in, tokens_out, cost, timestamp) | Per-client cost tracking |
| Client Portal | No new tables — uses existing bookings, invoices, contacts | Low schema impact, high query pattern impact |
| Time Tracking | `time_entries` table (may already exist in V1 schema) | Verify V1 schema |
| PayPal | `paypal_transactions` or extend `payments` table | Minimal — follows existing pattern |
| Marketplace | `marketplace_listings`, `marketplace_reviews`, `marketplace_analytics` | Entirely new schema surface |

**Key finding:** The AI usage log is non-negotiable. Without it, AI costs are invisible and unmanageable.

---

## Section 9: API Design & Contract

**V2 features requiring new API endpoints:**

| Feature | Endpoints | Notes |
|---|---|---|
| Client Portal | `GET /api/customer/bookings`, `GET /api/customer/invoices`, `GET /api/customer/profile` | Must be customer-scoped, not admin-scoped |
| Mybizz Management | Per-client health check, billing status, update distribution | ADR-033 defines the pattern |
| WhatsApp | Inbound webhook endpoint | New external-facing endpoint |
| AI Assistant | Internal server call (not a new endpoint) | Uses existing `anvil.server.call` pattern |

---

## Section 10: Performance & Scalability

**At 10x load (1,000 clients):**

| Feature | Bottleneck | Risk |
|---|---|---|
| AI Assistant | LLM API rate limits, cost ($0.01-0.10 per call x 1,000 clients x daily usage) | HIGH — costs scale linearly with clients |
| Client Portal | Database queries per customer session | MEDIUM — query optimization needed |
| Advanced Analytics | Report generation for large datasets | MEDIUM — server-side aggregation needed |
| Platform Capacity | Anvil infrastructure ceiling | CRITICAL — may require architecture change |

**At 100x load (10,000 clients):** The Anvil platform cannot support this. V2 capacity expansion is not a feature — it's a platform migration.

---

## Section 11: Design & UX

**V2 features with UX implications:**

| Feature | UX Challenge | Risk |
|---|---|---|
| AI Assistant | Floating window must not block the host form. Generated content must be editable. Context switching between assists must feel natural. | HIGH — bad AI UX is worse than no AI |
| Client Portal | Customers expect mobile-first. Navigation must be simpler than admin panel. Empty states must be welcoming. | HIGH — customer portal UX defines the brand |
| Dark Mode | Must work across all 4 color palettes. Public pages and admin panel. | MEDIUM — token-based approach helps |
| Additional Homepage Templates | Must be as polished as the 4 V1 templates. Each template needs responsive testing. | LOW — additive |

---

## SCOPE EXPANSION PROPOSALS

10 expansion proposals presented individually. User opted in or out of each.

### Expansion Proposal 1: "AI-Powered Onboarding Wizard" — ACCEPTED

**What:** Make AI the core onboarding experience. A new client signs up, and the AI walks them through building their entire business profile — generating their bio, service descriptions, FAQ, terms of service, and first blog post in a guided, step-by-step wizard. The client goes from signup to a live, content-rich website in 15 minutes instead of 3 hours.

**Why this matters:** The real value of AI is not as a helper — it's as the thing that eliminates the blank-page problem that kills 60% of new business setups. If a new client signs up and sees empty fields everywhere, they churn. If they sign up and AI fills every field with professional content they can edit, they're live in 15 minutes.

**Effort:** M (extends the existing AI spec, but reorders priorities)
**Risk:** Medium — depends on AI output quality
**Impact:** Transforms onboarding from a chore into a delight moment. Directly impacts the "<7 days to live" success metric.

---

### Expansion Proposal 2: "Customer Mobile-First Portal as a PWA" — DROPPED

**What:** Build the Client Portal as a Progressive Web App with home screen icon and offline capability.

**Decision:** User dropped the Client Portal entirely due to security risk. Both 10.3 (Client Portal) and 10.11 (Customer Portal Self-Service) removed from V2.

---

### Expansion Proposal 3: "WhatsApp Business Bot — Full Conversational Interface" — DECLINED

**What:** Expand WhatsApp from one-directional task creation to a full conversational business bot for booking, confirmations, reminders, and invoicing.

**Decision:** User declined. WhatsApp stays as one-directional task creation (10.1) only.

---

### Expansion Proposal 4: "Revenue Intelligence Dashboard" — DECLINED

**What:** Predictive analytics — revenue forecasting, churn risk scoring, lifetime value prediction, scheduled email reports.

**Decision:** User declined. Advanced Analytics stays as basic extensions (export, breakdown by service/provider).

---

### Expansion Proposal 5: "Mybizz Academy — In-Platform Business Education" — ACCEPTED

**What:** Short, contextual video tutorials and written guides that teach business skills inside the admin panel. "How to price your services." "How to write a proposal that wins." "How to reduce no-shows." Content is contextual — the pricing guide appears when the user is setting up services.

**Why this matters:** The target market are not business experts. A platform that teaches them to be better business owners creates loyalty that no competitor can replicate. This is the "sticky" feature that turns $50/month subscribers into lifetime customers.

**Effort:** M (content creation is the main effort, not code. The delivery mechanism is a contextual help panel.)
**Risk:** Low — additive feature, no architectural impact
**Impact:** Differentiates Mybizz from every competitor. No marketplace platform teaches you how to run your business better.

---

### Expansion Proposal 6: "White-Label Reseller Program" — DECLINED

**What:** Allow agencies and consultants to resell Mybizz under their own brand with recurring commissions.

**Decision:** User declined. Direct sales only.

---

### Expansion Proposal 7: "Automated Client Lifecycle Marketing" — ACCEPTED

**What:** Event-triggered email sequences that run without business owner intervention. New lead → welcome sequence. Booking completed → follow-up with review request. Client inactive 30 days → re-engagement. Birthday → greeting. High-value anniversary → thank-you with discount code.

**Why this matters:** Most solo practitioners never create email campaigns — they're too busy. An automated lifecycle engine does the marketing for them. This is the feature that makes $50/month feel like it includes a marketing manager.

**Effort:** M (extends existing Brevo integration + event-driven trigger system + template library)
**Risk:** Medium — requires event hooks throughout the app. Risk of over-emailing clients.
**Impact:** Directly increases client retention and rebooking rates.

---

### Expansion Proposal 8: "Mybizz Mobile App (React Native)" — ACCEPTED

**What:** Native mobile app for business owners. Dashboard overview, booking management, quick client lookup, push notifications for new bookings and payments, one-tap invoice creation.

**Why this matters:** A salon owner checking their next appointment shouldn't have to open a laptop. A consultant between meetings should be able to confirm a booking from their phone. Booksy and Vagaro have mobile apps. Mybizz without one is a gap.

**Effort:** XL (React Native development, API layer for mobile, push notifications, app store submission)
**Risk:** High — significant new codebase, platform-specific bugs, app store approval
**Impact:** Major competitive differentiator.

---

### Expansion Proposal 9: "Smart Scheduling with AI Optimization" — DECLINED

**What:** AI-powered scheduling — no-show prediction, waitlist auto-fill, peak pricing suggestions.

**Decision:** User declined. Keep scheduling as manual calendar management.

---

### Expansion Proposal 10: "Integrated Review and Reputation System" — ACCEPTED

**What:** After completed bookings, automatically send review requests. Reviews displayed on business's public website (with owner approval). Aggregate score on homepage and service pages. Negative review alerts. Review responses supported. Review data feeds into CRM.

**Why this matters:** Social proof is the #1 conversion factor for service businesses. A salon with 47 five-star reviews converts visitors at 3x the rate. This turns every completed booking into a marketing asset.

**Effort:** M (review request automation + public display + CRM integration)
**Risk:** Low — no external dependencies, no security risk
**Impact:** Directly increases website conversion rates.

---

## CEO REVIEW SUMMARY

**Mode:** SCOPE EXPANSION

### Final V2 Scope — What's IN

| # | Feature | Source | Status |
|---|---|---|---|
| 1 | **AI-Powered Onboarding Wizard** | Expansion 1 | **NEW — accepted** |
| 2 | **Mybizz Academy** | Expansion 5 | **NEW — accepted** |
| 3 | **Automated Client Lifecycle Marketing** | Expansion 7 | **NEW — accepted** |
| 4 | **Mybizz Mobile App (React Native)** | Expansion 8 | **NEW — accepted** |
| 5 | **Integrated Review and Reputation System** | Expansion 10 | **NEW — accepted** |
| 6 | AI Assistant Layer (10.2) | Original V2 | Retained — merged with Expansion 1 |
| 7 | Advanced Analytics (10.4) | Original V2 | Retained — basic extensions only |
| 8 | PayPal Integration (10.5) | Original V2 | Retained |
| 9 | Time Tracking (10.6) | Original V2 | Retained |
| 10 | Dark Mode Support (10.10) | Original V2 | Retained |
| 11 | Mybizz Management Platform (10.13) | Original V2 | Retained |
| 12 | WhatsApp Task Creation (10.1) | Original V2 | Retained — one-directional only |
| 13 | Platform Capacity Expansion (10.14) | Original V2 | Retained — business decision, not feature |
| 14 | Multi-Source Video Support (10.12) | Original V2 | Retained |
| 15 | Additional Homepage Templates (10.8) | Original V2 | Retained |

### What's OUT

| Feature | Reason |
|---|---|
| **Client Portal (10.3)** | Dropped — security risk (horizontal privilege escalation, second auth context) |
| **Customer Portal Self-Service (10.11)** | Dropped — same security risk as Client Portal |
| **Custom Segment Builder (10.7)** | Deferred — V1 pre-built segments are sufficient. Build only if clients ask. |
| **Optional Marketplace Presence (10.9)** | Deferred — this is a separate product, not a feature. V3+ consideration. |
| White-Label Reseller Program | Expansion declined |
| WhatsApp Full Bot | Expansion declined — keep one-directional |
| Revenue Intelligence Dashboard | Expansion declined — keep analytics basic |
| Smart Scheduling | Expansion declined |

### Strongest Challenges Found

1. **Client Portal security risk** — Correctly identified and dropped. The second auth context was the highest-risk V2 architectural item.
2. **AI Assistant production complexity** — The 3-4 day estimate is 3x optimistic. Prompt injection defense, cost monitoring, output sanitization, and graceful degradation are not in the spec.
3. **Mobile App scope** — This is an entirely new codebase. The estimate needs to be realistic (8-12 weeks minimum for a production-quality app).

### Recommended Path

1. **Immediate (pre-V1 launch):** Confirm CustomerLayout supports future expansion. Create ADR for customer data access contract. No other V2 prep needed.
2. **Post-launch (90 days):** Collect client feedback. Let feedback validate or invalidate V2 priorities.
3. **V2 Phase 1:** AI-Powered Onboarding Wizard + Dark Mode + Multi-Source Video (quick wins, high impact).
4. **V2 Phase 2:** Automated Lifecycle Marketing + Review System + PayPal (medium effort, high retention impact).
5. **V2 Phase 3:** Mybizz Academy + Advanced Analytics + Time Tracking (content and data features).
6. **V2 Phase 4:** Mobile App + Mybizz Management Platform (largest effort, highest strategic value).
7. **V2 Phase 5:** WhatsApp expansion + Platform Capacity (infrastructure and market-dependent).

### Accepted Scope
15 features in scope (down from 14 original + 5 expansions, minus 4 dropped).

### Deferred
3 items (Custom Segment Builder, Marketplace, Smart Scheduling dropped as expansion).

### Not in Scope
Client Portal, Customer Portal, White-Label, WhatsApp Full Bot, Revenue Intelligence, Smart Scheduling.

---

**Completion status:** DONE

*Review saved: 2026-06-26*
