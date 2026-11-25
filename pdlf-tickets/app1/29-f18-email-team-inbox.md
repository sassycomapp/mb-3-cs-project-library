---
title: "29 — F-18 Email and the Team Inbox"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-13b0b9f6"
updated: "2026-11-25"
---

## Description

**Feature:** F-18 Email (UF-35, UF-39; `spec-team-inbox.md` in full; `integration-architecture.md` Email; ADR `brevo-email-and-crm` — Amazon SES for every email, the CRM built in, Brevo and Zoho are errors; SR-34, SR-35, SR-35a–c; FR-18).
**Depends on:** 04 (the `email_service` seam), 20 (contacts — Add as lead), 16 (auth).

### Forms

- `inbox.TeamInboxForm` — addresses and folders, message list, conversation (DESIGN.md Team Inbox Design); each member sees their own address and desks, Owners see every address
- `inbox.ComposeForm` — new email, reply, forward; drafts saved automatically; AI draft reply in the app language
- `crm.EmailCampaignListForm`, `crm.EmailCampaignEditorForm`, `crm.EmailBroadcastForm` — campaigns and broadcasts (UF-23, UF-24; the access matrix lists all three): the campaigns dashboard, the campaign editor (type, trigger, sequence preview, activate) and the broadcast editor (recipients, sender, editor, schedule). Added at Step 22 by David's decision — the approved ticket list had no campaign ticket while the flows and the access matrix require the forms
- `settings.EmailSettingsForm` (renamed from `EmailSetupForm` at this step, per the divergence inventory) — the Email tab: addresses, desks, signatures, auto-replies
- `settings.DomainSettingsForm` — the Domain tab: DNS record status; `domain_service`

### Server functions

- `server_inbox/inbox_service` — folders, search, star, archive, move, Trash with 30-day retention, Unassigned
- `server_inbox/address_service` — `list_addresses` (Owner sees every address)
- `server_inbox/inbound_endpoint`, `server_inbox/events_endpoint` — `/_/api/email/inbound`, `/_/api/email/events`: SNS signature checked with Amazon's signing certificate; the subscription confirmation accepted only for the client's own topic; virus-failed mail never stored; spam/DMARC-failed to Spam
- `server_inbox/inbound_tasks` — the 90-day body archive to R2 with the row kept as an index
- `email_service` completion — templates (`email_templates`, one per type and language), suppression honouring within scope
- C1 completed here: `server_marketing/brevo_campaigns_integration.py` is deleted

### Tables

- `email_addresses`, `email_messages`, `email_suppressions`, `email_config`, `email_templates`, `email_log`, `contacts`

### Acceptance tests

- `IN-09`, `IN-07`; test-plan §10; tests 4.1–4.5; `SR-34`, `SR-35`, `SR-35a`–`SR-35c`; UF-35, UF-39

### Security criteria

- B4 — the email trust boundary is Amazon SES with SNS-signed events, never Brevo; scoped AWS keys (send from and read the client's prefix only); suppression honoured within scope (hard bounce or complaint suppresses all; unsubscribe marketing only)
- B1 — address visibility per role; `writable_fields.email_messages` (is_read, is_starred, folder) for own addresses and desks only
- AC-S12 — no email body content in any log

## Comments
