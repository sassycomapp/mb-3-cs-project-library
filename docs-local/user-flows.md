---
document: Mybizz CS — User Flows
doc-id: user-flows
state: Live
date-created: 2026-07-09T144411+0200
---

# Mybizz CS — User Flows

**Authority:** Reference — defines intended behaviour for all user flows  
**Related ADRs:** `form-architecture-and-state` ADR (Form Architecture and State Management), `webhook-architecture` ADR (Webhook Architecture)

---

## Actors

| Actor | Description |
|---|---|
| New Subscriber | A new client signing up for the Mybizz platform |
| Business Owner | The client instance's Owner-role user |
| Manager | Manager-role user — operational management access |
| Admin / Staff | Day-to-day operational roles |
| Customer | The client's end users — booking appointments, accessing the portal |
| Visitor | An unauthenticated user browsing the public website |
| Campaign Visitor | A visitor arriving from a marketing campaign |

---

## Flow Index

| # | Flow | Actor | Entry Point |
|---|------|-------|-------------|
| 1 | Client Onboarding | New Subscriber | Mybizz marketing site |
| 2 | Services Appointment Booking | Customer | `/services` |
| 3 | Admin Daily Operations | Business Owner / Staff | `/admin` |
| 4 | Content Management | Business Owner / Admin | `/admin/blog` |
| 5 | Customer Support | Customer / Staff | `/contact` |
| 6 | Error Recovery | All Users | Inline |
| 7 | Contact Management | Business Owner | `/admin/customers` |
| 8 | Email Campaign Setup | Business Owner | `/admin/campaigns` |
| 9 | Marketing Dashboard | Business Owner | `/admin/marketing` |
| 10 | Homepage Customisation | Business Owner | `/admin/settings/website` | V1 |
| 11 | Landing Page Creation | Business Owner | `/admin/landing-pages` | V1 |
| 12 | Contact Form Submission | Visitor | `/contact` | V1 |
| 13 | Lead Capture via Landing Page | Campaign Visitor | `/landing/:slug` | V1 |

---

## §1 Client Onboarding

**Two flows:**

### Mybizz_management Provisioning (internal)
1. **Create instance** — Clone master_template, configure dependency, initialise tables
2. **Create Owner account** — Owner role, credentials
3. **Set onboarding state** — `business_profile.onboarding_status = "in_progress"`
4. **Create client record** — Mybizz_management Client Details Table
5. **Send welcome email** — Login credentials via Brevo

### Owner Onboarding (Onboarding Form — guided first-run)

> **Note:** Onboarding is a separate form from Settings. It links to Settings for specific configuration tasks. See `specifications-global/spec-onboarding-implementation-plan.md` for full details.

1. **Owner logs in** → Onboarding form opens with welcome and progress indicator
2. **Business Profile** — Link to Settings → Business Profile tab
3. **Palette Selection** — Link to Settings → Palette tab
4. **System Currency** — Select currency from dropdown + confirm (immutable after first transaction)
5. **Legal Acknowledgement** — Read critical excerpts, tick checkbox, confirm (logged in Mybizz_management)
6. **Onboarding complete** → Owner directed to Dashboard

**Bidirectional navigation:** Settings form includes "Return to Onboarding" button (visible while onboarding is in progress).

**Post-onboarding:** Owner may return to Onboarding at any time to change credentials. Settings form accessible at any time for ongoing configuration (tabbed, RBAC-governed).

---

## §2 Services Appointment Booking

1. Browse service categories.
2. Select service — description, duration, price, provider(s).
3. Select provider (specific or "any available") — photo, specialisations, availability.
4. Select date and time slot — calendar showing available slots.
5. Select meeting type — In-Person / Video Call / Phone Call. Video call link generated automatically.
6. Enter client details. Complete intake form if required.
7. Add appointment notes.
8. Review & payment — configurable: pay now / optional / deposit only.
9. Confirmation — immediate email, 24h reminder, 1h reminder (optional), calendar invite.

**Post-confirmation:** Offer recurring appointment option (weekly / bi-weekly / monthly).

**Performance note:** The booking flow has multiple steps, each potentially requiring a server call. To avoid sluggish UX, data is pre-loaded where possible and related operations are batched. See `form-architecture-and-state` ADR (Form Architecture — booking flow latency management).

**See:** `anvil-platform-constraints` ADR (Anvil Platform Constraints — Skulpt limits on date/money logic)

---

## §3 Admin Daily Operations

**Navigation structure:**
```
Dashboard
▼ Sales & Operations: Bookings, Services
▼ Customers & Marketing: Contacts, Campaigns, Broadcasts, Segments, Tasks
▼ Content & Website: Blog, Pages, Media
▼ Finance & Reports: Payments, Invoices, Reports, Analytics, Time Entries
Settings (Owner / Manager only)
Vault (Owner only)
```

**Morning routine:** View today's metrics and alerts → process pending items → check today's calendar.

**Processing a booking:** Open detail → verify details and payment status → Confirm (PENDING → CONFIRMED) → system sends confirmation email and creates calendar event.

**Payment webhooks:** Stripe and Paystack POST webhooks to the client instance. Handlers return HTTP 200 immediately and defer processing to background tasks. See `webhook-architecture` ADR (Webhook Architecture).

**See:** `real-time-and-background-tasks` ADR (Real-Time and Background Tasks — polling intervals)
**See:** `pdf-invoice-generation` ADR (PDF Invoice Generation) for invoice creation on booking completion.

---

## §4 Content Management

**Update website content:** Pages → select page → edit text/images inline → Preview → Publish.

**Publish blog post:** Blog → New Post → write with rich text editor → set metadata (excerpt, category, tags, featured image, author) → SEO settings → Publish Now / Save Draft / Schedule.

---

## §5 Customer Support

**Customer raises ticket:** Help → search knowledge base first → if unresolved, submit ticket (subject, category, priority, description, attachments) → ticket number assigned, confirmation email sent.

**Staff responds:** Ticket queue (colour-coded by priority) → open ticket → write response → Send / Send & Mark Resolved.

---

## §6 Error Recovery

**Payment failure:** Display error → retry same card → if still failing, offer alternative method → hold booking 15 minutes (save as Payment Pending, send payment link) → release slot on expiry.

**Session timeout (30 minutes):** Prompt re-login. Restore user context automatically.

---

## §7 Contact Management

**Entry:** Dashboard → Contacts → ContactListForm

**Overview:** Total contact count, status breakdown, pre-built segments, search bar, contact DataGrid.

**Actions:** View segment, search, view contact (stats, activity timeline, active campaigns, quick actions), add contact, bulk actions (add tag, enroll in campaign, CSV export).

**Automatic contact creation:** Contacts created or updated automatically from bookings, enquiry forms, and landing page lead captures.

---

## §8 Email Campaign Setup

**Entry:** Marketing → Email Campaigns → [+ New Campaign]

1. View campaigns dashboard — active campaigns with enrolled count and performance.
2. Click [+ New Campaign] → EmailCampaignEditorForm.
3. Choose campaign type — pre-built templates: New Client Welcome, Appointment Follow-Up, Re-engagement, Referral Request.
4. Review email sequence — preview each email with configurable delay.
5. Set trigger — event type and delay before first email.
6. Activate — confirm settings → [Activate Campaign].
7. Campaign runs automatically — contacts auto-enrolled, emails sent on schedule, opens/clicks tracked via Brevo webhooks.

**Background process:** Hourly background task processes active enrollments.

---

## §9 Marketing Dashboard

**Metrics:** Total contacts and new this month, revenue (current vs previous), email performance (sent, open rate, click rate, conversions), top lead sources by revenue, actionable alerts.

**Alert logic:** Nightly background task at 2am calculates metrics and generates alerts.

**Actions:** Drill into any metric, one-click enroll contacts from alert, view detailed reports, change time period, export to PDF/CSV/scheduled email.

---

## §10 Homepage Customisation

1. Select homepage template — four options: Classic, Services, Booking, Minimalist.
2. Configure hero — headline, subheadline, hero image, CTA.
3. Configure features — up to 5 items (icon, title, 100-char description).
4. Configure services showcase — toggle, count (1–6), display style.
5. Configure testimonials — up to 10 (name, text, rating, photo).
6. Configure final CTA — headline, subheadline, button text + link.
7. Preview → Save → live immediately.

Auto-saves every 30 seconds. Configuration stored in `config` table with `key = 'home_config'`.

---

## §11 Landing Page Creation

**Three templates:** Lead Capture, Event Registration, Video Sales Letter.

1. Choose template.
2. Set basic settings — title, URL slug (auto-generated, unique), status (Draft / Published).
3. Configure template-specific content.
4. Preview (BlankLayout — no header/footer).
5. Publish → validates, saves to `landing_pages`, displays shareable URL.
6. Share — copy URL, send test email, social media, embed, add to email campaign.

Analytics tracked: views, conversions, conversion rate, referrer, device.

---

## §12 Contact Form Submission

**Entry:** `/contact`

**Fields:** Name (required), Email (required), Phone (optional), Message (required, max 1000 chars).

**On submission:** Saves to `contact_submissions`, sends notification to business owner, sends auto-reply to visitor if configured. Form resets on success.

---

## §13 Lead Capture via Landing Page

**Entry:** `/landing/:slug` — BlankLayout

1. Visitor arrives from campaign → page view tracked.
2. Views page content — headline, benefits, hero image, email capture form.
3. Submits email address.
4. System: validates email, creates records in both `leads` and `contacts` simultaneously, sets `leads.converted_to_contact_id` immediately, increments conversion count, triggers welcome email via Brevo Campaigns, enrolls in welcome sequence.
5. Thank you message displayed.
6. Lead receives welcome email with content, CTA, unsubscribe link.

**Business owner:** receives "New lead captured" notification. Lead appears in CRM with status 'new'.

---

## §14 Flow Design Principles

- **Progressive disclosure** — show only relevant info per step
- **Clear CTAs with progress indication**
- **Real-time input validation**
- **Recovery paths for every error** — no dead-ends
- **Automatic progress saving**
- **Email confirmation for all transactions**

---

*End of file*
