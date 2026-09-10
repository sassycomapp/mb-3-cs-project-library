---
document: "Mybizz AI and Voice — V3 Specification"
doc-id: mybizz-ai-implementation-spec-v3
state: Live
date-created: 2026-07-22
---

# Mybizz AI and Voice — V3 Specification

**Version:** 1.1 (Wave Accounting added)
**Status:** Planning — not yet scheduled for development
**Prerequisite:** mybizz-ai-implementation-spec-v2.md fully implemented and stable in production
**Purpose:** Captures all confirmed V3 AI and voice decisions made during V2 planning. To be expanded into a full implementation spec when V3 development begins.

---

## Context

This document captures the V3 AI and voice roadmap as defined during V2 planning sessions. Items here were deliberately excluded from V2 for one of two reasons:

1. They depend on the V2 foundation being stable and proven before being built on top of it.
2. They depend on meaningful production data volume existing before their outputs are credible and useful.

Nothing in this document should be built until V2 is fully live and operating in production.

---

## V3 Voice Tiers

V2 delivered Tiers 1, 2, and 3. V3 adds Tiers 4 and 5.

### Tier 4 — Voice-driven form pre-fill

The user speaks a complete record. Mybizz parses the spoken input into structured data and pre-fills the relevant form for the user to review and confirm. The user does not type anything — they speak, review, and confirm.

**Examples:**
- "New booking — Sarah Johnson, Swedish massage, Friday 2pm" → booking form pre-filled, user confirms
- "New contact — Thabo Nkosi, thabo at gmail dot com, referred by Naledi" → contact form pre-filled
- "Create invoice for 3 hours consulting at R800 per hour for Makena Ltd" → invoice pre-filled

**What makes this different from Tier 2:**
Tier 2 (task creation) is simple — one field, one action. Tier 4 requires entity extraction from natural speech and mapping extracted values to specific form fields. Each form type (booking, contact, invoice) needs its own entity extraction prompt design.

**Build requirements:**
- Per-form entity extraction prompt for each supported form type
- Pre-fill logic mapping extracted entities to Anvil form fields
- Confirmation step before saving — the user always reviews before committing
- Graceful fallback if extraction is incomplete — present a partially filled form, not an error

### Tier 5 — Aggregated data queries and intelligence

Complex queries requiring cross-referencing, trend analysis, or multi-step reasoning across the user's data. Deferred from V2 because they require meaningful data volume to produce credible answers, and because the AI query layer needs to be mature before adding this complexity.

**Revenue intelligence:**
- "What is my best performing service by revenue this month?"
- "How does this month's revenue compare to last month?"
- "Which service has the most cancellations?"
- "What is my average booking value?"

**Client intelligence:**
- "Who are my top 5 clients by spend?"
- "Which clients haven't booked in more than 60 days?"
- "What is my client retention rate?"
- "Which clients came from my landing page?"

**Marketing intelligence:**
- "How many leads came in this week?"
- "Which campaign had the best open rate?"
- "How many blog posts have I published this month?"

**Predictive and advisory:**
- "Am I on track to hit last month's revenue?"
- "Which days of the week are my busiest?"
- "What time slots are most popular?"
- "Should I open more availability on Thursdays?"

---

## V3 Additional Voice Features

Three features that go beyond query-and-response and represent a meaningfully deeper voice layer than anything in V2.

### Proactive voice briefings

Mybizz speaks to the user without being asked. On opening the app — or via the WhatsUp mobile shortcut — Mybizz delivers a short spoken morning brief covering what matters right now. No query needed. The system decides what is worth saying based on the user's data.

**Suggested brief content:**
- Today's booking schedule
- Outstanding invoices
- Overdue tasks
- New leads received since yesterday
- Any flagged items requiring action

**Design principle:** The brief should be concise — 20 to 30 seconds spoken. Not a data dump. Curated, prioritised, actionable.

**Build requirements:**
- A "brief generation" server function that queries across bookings, invoices, tasks, and leads
- A morning brief prompt that converts the data into a natural-sounding spoken summary
- TTS delivery via browser SpeechSynthesis (same as V2) or upgraded to Groq TTS if quality warrants
- A user setting to enable or disable the automatic brief

### Conversational follow-up (multi-turn voice)

V2 is single-shot — one question, one answer, session ends. V3 adds multi-turn conversation within a voice session. The user can ask a follow-up question in the context of the previous answer.

**Example:**
- "What payments came in today?" → *[Mybizz answers: R2,400 from three clients]*
- "And what's still outstanding from last week?" → *[Mybizz answers in context]*
- "Send Naledi a reminder" → *[Mybizz creates the task]*

**Build requirements:**
- Session memory within the voice interaction — a short context window held in the component state
- The AI layer receives the prior exchange as context when composing each new response
- Session terminates on explicit close, timeout, or navigation away from the current form

### TTS voice quality upgrade (optional)

V2 uses browser SpeechSynthesis — functional but robotic. If user feedback from V2 indicates the voice quality is a friction point, V3 can upgrade to Groq TTS for natural-sounding responses.

**Assessment trigger:** Review after 3 months of V2 production use. Upgrade only if user feedback specifically identifies voice quality as a barrier to using the feature.

**Cost:** Groq TTS pricing is in the same negligible range as Groq Whisper — absorbable by Mybizz at the same cost model used for V2 transcription.

---

## V3 AI Intelligence Features

These are the data-dependent AI features deferred from V2. They sit in the same shared AI helper architecture but require production data volume before their outputs are meaningful.

### Client sentiment summary (AI assist #9 from V2 spec)

Analyses stored communication history and client interaction patterns to produce a sentiment summary for a given client. Helps the business owner understand the health of a client relationship before a meeting or before sending a proposal.

**Precondition:** Sufficient stored communication or client data must exist. Not useful on a new or sparse account.

### Booking and availability insights (AI assist #10 from V2 spec)

Analyses booking history and patterns to surface insights about availability, peak demand, underused slots, and service popularity. Helps the business owner make decisions about scheduling and service offerings.

**Precondition:** Requires meaningful booking history — at minimum several months of production data.

---

---

## Wave Accounting Integration

### What it is

Wave is a free accounting platform widely used by small and solo service businesses — freelancers, consultants, coaches, trades — exactly the Mybizz target market. It handles bookkeeping, profit and loss, tax preparation, and financial reporting in a way that Mybizz does not and should not try to replicate. The two platforms are naturally complementary rather than competitive.

Mybizz manages the operational side of a service business: bookings, clients, invoices, payments, marketing. Wave manages the financial recording side: accounting, ledger entries, tax categories, and reports. The gap between them is the manual data entry a business owner currently has to do themselves — copying paid invoices from Mybizz into Wave, re-entering customer records, manually recording revenue. That is the gap the integration closes.

### The concept

The integration is intentionally lean — a one-way, event-driven push from Mybizz to Wave. Mybizz does not pull data from Wave. Mybizz does not try to replace Wave's accounting features. Mybizz simply tells Wave what happened, at the moment it happened, so the business owner does not have to.

**The three events worth pushing:**

**1. Invoice paid** — when a Mybizz invoice is marked paid (manually or via Stripe/Paystack callback), the payment is recorded in Wave automatically. The correct customer record is created or matched in Wave if it does not already exist.

**2. New customer** — when a new client is created in Mybizz, a matching customer record is created in Wave. The business owner's client database and their Wave accounting contacts stay in sync without manual duplication.

**3. Refund issued** — when a refund is processed in Mybizz, the corresponding adjustment is recorded in Wave. This is the one event that, if missed, creates the most accounting headaches.

That is the entire V3 integration scope. Three triggers. Three push events. No sync UI, no mapping configuration screens, no two-way data flow. The user connects their Wave account once, and the three events flow automatically from that point forward.

### Why this fits Mybizz's design philosophy

The integration is additive, not transformational. It does not change how Mybizz works. It does not change how Wave works. It eliminates one repetitive manual task — copying financial events between two systems — without adding a new tool, a new subscription, or a new concept for the business owner to learn.

Wave's free tier is genuinely free for accounting. This means the integration adds accounting capability to Mybizz's offering at zero additional cost to the client. The Mybizz value proposition — "everything included, one price" — extends naturally: "and if you use Wave, your accounts update themselves."

### Connection model

The client connects their Wave account once through the Mybizz Settings Hub — same pattern as the existing Vault and payment gateway configuration. Credentials are stored in the Vault. The connection is visible but the mechanics are invisible from that point on.

The business owner never needs to think about it again.

### Explicit scope boundaries

The following are deliberately out of scope and should remain so:

- Mybizz does not pull financial data from Wave — no balance displays, no Wave reports inside Mybizz
- Mybizz does not attempt to replace any Wave accounting feature
- Mybizz does not sync backwards — Wave changes do not affect Mybizz records
- The integration is optional — clients who do not use Wave are unaffected

### Why V3 and not V2

Wave has a live API and the technical path is clear. It is V3 rather than V2 purely on prioritisation grounds — the V2 AI and voice features deliver more immediate perceived value to more clients. Wave accounting integration serves clients who are already using Wave, which is a subset of the total client base. V3 is the right phase: build it after V2 is stable, when the client base is large enough that the subset using Wave justifies the build effort.

---

## V3 Prerequisites Checklist

Before any V3 development begins, confirm all of the following:

- [ ] V2 AI helper component is live and stable in production
- [ ] All ten V2 AI assists are implemented and in active use
- [ ] Groq Whisper integration is confirmed stable in production
- [ ] Voice Tiers 1, 2, and 3 are confirmed working on mobile via WhatsUp shortcut
- [ ] Meaningful data volume exists across active client accounts (minimum 3 months production use recommended before building Tier 5 and sentiment/insights features)
- [ ] User feedback on V2 voice quality has been reviewed (informs TTS upgrade decision)

---

## Cost Model for V3

| Feature | Provider | Paid by | Notes |
|---|---|---|---|
| Tier 4 entity extraction | LLM (same as V2) | Mybizz | Slightly higher token usage per interaction than V2 queries |
| Tier 5 aggregated queries | LLM (same as V2) | Mybizz | Higher token usage — data context is larger |
| Proactive briefings | LLM + Groq Whisper/TTS | Mybizz | One brief generation per user per day — manageable |
| Conversational follow-up | LLM (same as V2) | Mybizz | Minimal additional cost — context window is short |
| TTS upgrade (if triggered) | Groq TTS | Mybizz | Same negligible cost range as Groq Whisper |

No additional cost passes to the client for any V3 voice or AI feature.

---

## Document Status

This is a starter document. It captures confirmed decisions and direction established during V2 planning. It is not yet a full implementation specification. When V3 development is ready to begin, this document should be expanded to include:

- Full component specifications for each new feature
- Wireframe and screen requirements
- Data flow diagrams for Tier 4 entity extraction
- Detailed prompt designs for Tier 5 intelligence queries
- ADR entries for any new architectural decisions
- Build plan integration

*Last updated: during V2 AI and voice planning session*
*Owner: Mybizz founder*
*Supersedes: Nothing — this is a new document*
*Related: mybizz-ai-implementation-spec-v2.md*
