---
title: "30 — F-08 Landing pages"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-b33fe6af"
updated: "2026-11-17"
---

## Description

**Feature:** F-08 Landing pages (UF-18; SR-14; FR-08).
**Depends on:** 11 (EditModeComponent), 02 (shared client), 20 (contacts for conversion counts' context).

### Forms

- `landing_pages.LandingPageListForm`, `landing_pages.LandingPageEditorForm`
- `landing_pages.LandingPage` — public, on BlankLayout, with local templates `LeadCaptureTemplate`, `EventRegistrationTemplate`, `VslTemplate`; preview on the blank layout; publish validated

### Server functions

- `server_marketing/landing_page_service` — create, configure, publish, the shareable address, view and conversion counting

### Tables

- `landing_pages`, `leads` (counts), `lead_captures`

### Acceptance tests

- `SR-14`; UF-18; `ST-11` (templates); analytics: views, conversions, conversion rate, referrer, device

### Security criteria

- B1 — Owner/Manager edit; the public page is a route with `Route.meta` tags
- AC-S12 — every edit-mode save audited (with ticket 11)

## Comments
