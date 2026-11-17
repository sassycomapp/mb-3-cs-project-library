---
title: "49 — F-32 Help, support tickets and feedback"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-e56745f7"
updated: "2026-11-17"
---

## Description

**Feature:** F-32 Help, support tickets and feedback (UF-34; SR-51; FR-32). Support data lives in App 5 (`adr-schema-scope-reduction`: no `tickets` tables in the instance).
**Depends on:** 47 (the management surface and the App 5 route), 04 (the retry queue).

### Forms

- `support.HelpForm` — problem or feedback; subject, message, optional screenshot; the reference number shown on send; replies appear in the Help view with an email when Mybizz replies

### Server functions

- `server_support/help_service` — sends the message to App 5 with the instance's Bearer token; reads replies through the management endpoints; retries through the retry queue

### Tables

- None in the instance (tickets and replies are App 5's).

### Acceptance tests

- `SR-51`; UF-34; `IN-12` — the App 5 link's failure handling (non-blocking push; a failed delivery rides the next one)

### Security criteria

- B5 — the token authenticates the instance; the message carries no credentials
- No client personal data enters App 5 beyond the message the user wrote (ADR `client-data-management-rights-and-mybizz-retention-boundary`)

## Comments
