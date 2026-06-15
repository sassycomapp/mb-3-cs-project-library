# ADR-038 — Webhook Architecture

**Status:** Approved  
**Date:** 2026-06-14  
**Authority:** Derived from platform-overview.md, ADR-036, integration-architecture.md

---

## Context

Three external services POST webhooks to client instances:

| Service | Events | Criticality |
|---------|--------|-------------|
| Stripe | `payment_intent.succeeded`, `payment_intent.payment_failed`, subscription updates | High — payment status must be accurate |
| Paystack | `charge.success`, `subscription.disable`, `invoice.payment_failed` | High — payment status must be accurate |
| Brevo | Open tracking, click tracking, unsubscribe events | Medium — campaign analytics |

Each client instance is a separate Anvil app with a separate URL. Webhook URLs are:
- Stripe: `[base_url]/api/webhooks/stripe`
- Paystack: `[base_url]/api/webhooks/paystack`
- Brevo: `[base_url]/api/webhooks/brevo`

**Critical constraint:** Gateway webhook handlers must return HTTP 200 within a few seconds or the gateway times out and retries. Processing must be deferred.

---

## Decision

### Webhook Handler Pattern: Immediate 200 + Background Task

All webhook handlers follow the same pattern:

1. **Receive** the webhook payload
2. **Validate** the signature (prevents spoofing)
3. **Log** the raw payload to `webhook_log` table
4. **Dispatch** to a background task for processing
5. **Return HTTP 200** immediately

```python
@anvil.server.http_handler(path='webhooks/stripe')
def handle_stripe_webhook():
    payload = anvil.server.request.body
    sig_header = anvil.server.request.headers.get('stripe-signature', '')
    
    # 1. Validate signature
    if not verify_stripe_signature(payload, sig_header):
        return anvil.server.HttpResponse(
            status=400,
            headers={'Content-Type': 'application/json'},
            body='{"error": "Invalid signature"}'
        )
    
    # 2. Log raw payload
    event_row = app_tables.webhook_log.add_row(
        source='stripe',
        payload=payload,
        status='pending',
        received_at=datetime.now(TZ.UTC)
    )
    
    # 3. Dispatch to background task
    anvil.server.launch_background_task('process_stripe_webhook', event_row.get_id())
    
    # 4. Return 200 immediately
    return anvil.server.HttpResponse(
        status=200,
        headers={'Content-Type': 'application/json'},
        body='{"received": true}'
    )
```

### Background Task Processing

```python
@anvil.server.background_task
def process_stripe_webhook(event_row_id):
    event_row = app_tables.webhook_log.get_by_id(event_row_id)
    
    try:
        event = json.loads(event_row.payload)
        event_type = event.get('type', '')
        
        if event_type == 'payment_intent.succeeded':
            handle_payment_success(event)
        elif event_type == 'payment_intent.payment_failed':
            handle_payment_failure(event)
        elif event_type == 'customer.subscription.updated':
            handle_subscription_update(event)
        elif event_type == 'customer.subscription.deleted':
            handle_subscription_cancellation(event)
        
        event_row.status = 'processed'
        event_row.processed_at = datetime.now(TZ.UTC)
        
    except Exception as e:
        event_row.status = 'failed'
        event_row.error_message = str(e)
        # Push error event to Mybizz_management (ADR-033)
        push_management_event('webhook_processing_error', {
            'source': 'stripe',
            'event_id': event_row_id,
            'error': str(e)
        })
```

### Idempotency

Webhook delivery is not guaranteed to be exactly-once. Gateways may retry on timeout. Idempotency must be enforced:

- **Stripe:** Use `event.id` as idempotency key — check `webhook_log` before processing
- **Paystack:** Use `event.id` or `reference` as idempotency key
- **Brevo:** Use webhook event ID or timestamp-based dedup

```python
def handle_payment_success(event):
    payment_intent_id = event['data']['object']['id']
    
    # Idempotency check
    existing = app_tables.bookings.search(
        payment_intent_id=payment_intent_id
    )
    if existing:
        return  # Already processed
    
    # Process payment...
```

### Webhook Log Table

| Column | Type | Purpose |
|--------|------|---------|
| `id` | UUID | Primary key |
| `source` | Text | `stripe`, `paystack`, or `brevo` |
| `payload` | Text | Raw JSON payload |
| `status` | Text | `pending`, `processed`, `failed` |
| `received_at` | DateTime | When the webhook was received |
| `processed_at` | DateTime | When processing completed |
| `error_message` | Text | Error details if failed |
| `idempotency_key` | Text | Deduplication key |

---

## Consequences

- ✅ Webhook handlers return 200 immediately — no gateway timeouts
- ✅ Processing failures are captured in `webhook_log` with full payload for retry
- ✅ Idempotency prevents duplicate records from webhook retries
- ✅ Background task processing is non-blocking — client UI is not affected
- ⚠️ Webhook processing is asynchronous — payment status update is not instant (seconds delay)
- ⚠️ `webhook_log` table grows over time — retention policy needed (90 days recommended)
- ⚠️ Background task failures must be monitored — failed webhooks require manual retry or investigation

---

## Related Documents

| Document | Relationship |
|---|---|
| `adr/036-real-time-and-background-tasks.md` | Background task architecture and limits |
| `adr/033-mybizz-management-visibility.md` | Error events pushed to Mybizz_management |
| `integration-architecture.md` | Section 1: Payment Gateway Architecture; Section 3: Brevo CRM Integration |
| `docs/client-activation-runbook.md` | §4.9: Webhook URL registration during provisioning |

---

*End of ADR-038*
