---
title: "31 — F-19 Lead capture and marketing funnel"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-adbcd84e"
updated: "2026-11-17"
---

## Description

**Feature:** F-19 Lead capture and marketing funnel (UF-20; ADR `lead-capture-simultaneous-creation`; SR-33; FR-19).
**Depends on:** 30 (landing pages host the capture forms), 29 (the welcome sequence enrolment), 20 (contacts).

### Forms

- `crm.LeadCaptureForm` — the admin lead-capture form editor: `lead_form`, `event_registration`, `vsl` kinds; trigger settings, form fields, offer text

### Server functions

- `server_marketing/lead_capture_service` — the simultaneous creation: the `leads` and `contacts` records created together with `leads.converted_to_contact_id` set at once; the conversion count incremented; the contact enrolled in the welcome sequence

### Tables

- `leads`, `lead_captures`, `contacts`, `contact_campaigns`

### Acceptance tests

- `IN-05` — the simultaneous creation (exactly one lead and one contact); `SR-33`; UF-20
- The funnel: blog call to action → landing page → booking flow with the source carried (UF-20 funnel note)

### Security criteria

- B1, B8 — the capture write is a public, rate-limited endpoint (ST-02; the consent and capture endpoints are rate-limited and tested)
- AC-S2 — the created records carry only what the form collected

## Comments
