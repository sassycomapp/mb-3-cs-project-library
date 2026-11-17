---
title: "33 — F-06 Public website"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-d21be74d"
updated: "2026-11-17"
---

## Description

**Feature:** F-06 Public website (UF-16, UF-19; `spec-sm-syndication.md` Route.meta tags; SR-12, SR-12a; FR-06).
**Depends on:** 01 (PublicLayout), 05 (FooterComponent), 11 (EditModeComponent), 02 (shared client), 20 (contacts for the enquiry).

### Forms

- `public_pages.HomePage` (the form; its four templates are ticket 34), `public_pages.AboutPage`, `public_pages.ContactPage`, `public_pages.ServicesPage`, `public_pages.ServiceDetailPage`, `public_pages.PrivacyPolicyPage`, `public_pages.TermsConditionsPage`, `public_pages.CookiePolicyPage`
- `public_pages.PageListForm` — admin: name, SEO title, address, published
- `public_pages.MediaLibraryForm` — admin: Content & Website → Media

### Server functions

- `server_settings/page_content_service` — the public content readers (the intentionally public functions), page save (with ticket 11), media library
- The contact-form submission handling (UF-19): saves to `contact_submissions`, creates or updates the contact, adds the enquiry to the timeline, creates the follow-up task, notifies the Owner, sends the auto-reply — one bounded public write
- Every public route sets title, description and Open Graph tags in `Route.meta`

### Tables

- `pages`, `files`, `business_profile`, `config`, `contact_submissions`, `contacts`, `tasks`, `activity_log`

### Acceptance tests

- `SR-12`, `SR-12a`; UF-16, UF-19; `ST-11`, `ST-14`
- The contact form: name and email required, message ≤ 1,000 characters; the thank-you message and reset

### Security criteria

- B1 — edit mode Owner/Manager only; the public readers return the public subset only
- B8 — the contact-form write is rate-limited (10/min unauthenticated per IP) and bounded
- AC-S12 — every edit and submission audited; no personal data in logs beyond ids
- The cookie banner and consent records are ticket 41's; the pages host them

## Comments
