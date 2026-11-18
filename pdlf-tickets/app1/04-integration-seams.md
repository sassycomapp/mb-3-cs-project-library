---
title: "04 — Integration seams: SES, R2, payments, AI route"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-0f97960f"
updated: "2026-11-18"
---

## Description

**Feature:** The outside-service seams — common aspects 6, 7, 8 and the AI route (design doc §23.2, §23.4, §23.5).
**Depends on:** 03 (server foundation).

### Forms

- None.

### Server functions / modules

- `server_shared/email_service` — `send(template, recipients, attachments)`; Amazon SES is the only adapter; tests replace it with the mail catcher; demo mode redirects at this seam. (The Brevo module `server_marketing/brevo_campaigns_integration.py` is deleted at ticket 29, the build step that touches marketing email — C1.)
- `server_storage/r2_service` — `put/get/delete` per key; the only module that touches the client's bucket; the retry queue sits behind it
- `server_payments/payment_service` — create/verify/refund per record, gateway-agnostic; the single gateway-agnostic payment service (common aspect 6)
- `server_payments/stripe_gateway` — the Stripe adapter (renamed from `stripe_service` at this step, per the divergence inventory — this is the payments seam step)
- `server_payments/paypal_gateway` — the PayPal adapter; captures and refunds carry a `PayPal-Request-Id`
- `server_payments/webhook_handlers` — the gateway webhook endpoints `/_/api/webhooks/{stripe,paypal}`; the one §23.5 pattern: verify signature against the raw body (invalid → HTTP 400, `error_log`, no row) → refuse a seen `event_id` → write the `webhook_log` row → dispatch the background task → HTTP 200 within 5 s; reconciliation every 5 minutes (D4)
- `server_ai/ai_request_layer` — `request(assist, inputs, context)`; the one route to App 5 and Groq; no AI key in the instance; failure handling in one place

### Tables

- `webhook_log`, `payment_config` (read), `vault` (keys, via `vault_service`)

### Acceptance tests

- `IN-07` — webhook security and exactly-once; test-plan §5
- `IN-06` — server-side amounts; test-plan §6
- `ST-09` — bounded outbound calls; `SC-10` — no card data held; SAQ A script list
- `IN-08` — SES send failures queued and retried; `IN-10` — R2 writes queued on bucket failure

### Security criteria

- B3 — payment-gateway boundary: amounts read from the database, never a parameter; signature verified before any payload is used; idempotency by the gateway's event id; card data never held (AC-S3, AC-S9, AC-S10)
- B4 — email boundary: SES with SNS-signed events, never Brevo; scoped AWS keys
- B5 — the AI route passes content unstored and unlogged (AC-S13)
- B6 — the bucket token is scoped to the client's own bucket and read only through `vault_service` (AC-S14)

### Notes

- `boto3` pinned in `requirements.txt` (B11; pip-audit at the build and release steps).

## Comments
