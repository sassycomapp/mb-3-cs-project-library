---
title: "43 — F-26 AI assistance"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-87c74219"
updated: "2026-11-17"
---

## Description

**Feature:** F-26 AI assistance (UF-30; `spec-ai-implementation.md` in full — architectural model, ten assists, rollout phases, data-dependent assists, sample data, further AI functions, data flow, technical approach, risks and guardrails, development sequence; SR-42–SR-44b; FR-26).
**Depends on:** 09 (AIHelperComponent and the request layer's contract), 25 (the dashboard's summaries the data-dependent assists read).

### Forms

- None new. The AIHelperComponent (ticket 09) is integrated by its host forms' own tickets (Business Profile at 18, ServiceEditorForm at 21, QuoteEditorForm at 38, BlogEditorForm at 36, Team Inbox compose at 29, InvoiceEditorForm at 23, ContactViewerForm at 20, EditModeComponent at 11, SharePanelComponent at 13).

### Server functions

- `server_ai/assists` — the ten assists behind the request layer; the website-text and social-caption functions
- `server_settings/sample_data_service` — the sample data loaded for the client's trade (loaded by App 5 at provisioning; this instance-side service records every row in `sample_rows`, sample contacts at `example.com`), and Clear sample data: removes every recorded row, empties the links from real rows, recomputes the summaries, refused for every role but Owner (UF-43)

### Tables

- `sample_rows` and every table the sample data populates; the summary tables (recompute)

### Acceptance tests

- `SR-42`–`SR-44b`; tests 4.10–4.11 (sample-data isolation: no email, payment, reminder or export ever touches a sample record)
- Assists 9 and 10 give results on the sample data from the first day and on the client's own data once cleared; on thin real data they say what they need
- UF-30, UF-43

### Security criteria

- B5, AC-S13 — no AI content stored or logged in the instance or App 5; Groq Zero Data Retention on; no AI key in any instance
- Sample data: never emailed, never paid, never exported (SR-44b); Clear is Owner-only and audited

## Comments
