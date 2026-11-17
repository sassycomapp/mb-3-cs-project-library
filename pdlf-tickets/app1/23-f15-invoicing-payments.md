---
title: "23 — F-15 Invoicing and payments"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-49be0b95"
updated: "2026-11-17"
---

## Description

**Feature:** F-15 Invoicing and payments (UF-14, UF-15; `integration-architecture.md` Payment Gateways, PayPal, Recording payments and refunds, Currency; `spec-integration`; DESIGN.md Invoice Design; ADRs `pdf-invoice-generation`, `payment-gateway-mutability`, `payment-gateway-configuration-is-a-settings-function-and-is-rbac-governed`, `payment-security-boundary-vault`; SR-17, SR-17a, SR-17b, SR-26–SR-30; FR-15). Trade feature; the gateway service layer itself is common aspect 6 (ticket 04).
**Depends on:** 04 (seams), 16 (auth), 18 (settings), 20 (contacts), 22 (F-16 currency).

### Forms

- `invoices.InvoiceListForm`, `invoices.InvoiceEditorForm`, `invoices.InvoiceViewerForm` (includes Record Payment and Refund), `invoices.PaymentListForm`
- `settings.PaymentGatewayForm` — the Payment Gateways tab: connect Stripe, optionally PayPal; secret keys go into the Vault; test mode on

### Server functions

- `server_payments/invoice_service` — `create_invoice` (Owner/Manager), lines, tax fixed at issue, due date from terms, numbering through the `config` sequence keys inside a transaction
- `server_payments/invoice_pdf` — reportlab, server-side (ADR `pdf-invoice-generation`)
- `server_payments/refund_service` — `approve_refund` (Owner/Manager, step-up; full or part; through the original gateway where the payment was online; never exceeds the payment)
- `server_payments/invoice_tasks` — the nightly overdue task with the reminder
- Payment recording (manual methods: cash, bank transfer, other) and the webhook application logic completing the handlers of ticket 04; Stripe completion beyond the seam; PayPal completion beyond ticket 15's buttons; the `payment_config` hard-fail guard

### Tables

- `invoice`, `invoice_items`, `payments`, `payment_config`, `webhook_log`, `time_entries` (pull-in link), `contacts`, `config` (sequence keys)

### Acceptance tests

- `IN-06`, `IN-07`, `SC-10`; test-plan §5 (webhooks), §6 (server-side amounts); test 4.20–4.21
- `SR-17`, `SR-17a`, `SR-17b`, `SR-26`–`SR-30`; UF-14, UF-15; the invoice lifecycle (test-plan §19)

### Security criteria

- B3 — every amount server-side; webhooks verified, idempotent, reconciled every 5 minutes (AC-S9, AC-S10)
- B10 — gateway secret keys only in the Vault; publishable keys in `payment_config` (AC-S4)
- AC-S3 — SAQ A: card data never held; the payment-page script list approved and checked at every release
- AC-S7 — refunds above the set limit need a factor given in the last five minutes
- AC-S12 — every payment, refund and gateway change writes an audit row

### Notes

- `stripe_service.py` → `stripe_gateway` rename lands here if not already taken by ticket 04 (the divergence inventory assigns it to the payments build step).

## Comments
