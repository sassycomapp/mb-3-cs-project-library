# Mybizz CS — Integration Architecture

**Authority:** Mandatory — all integration implementation must conform

---

## 1. Payment Gateway Architecture

Two payment gateways supported in V1 (Stripe + Paystack). One gateway per client instance, configured at onboarding.

### Gateway-Agnostic Service Layer
Client code never calls a gateway directly. All payment operations go through a gateway-agnostic service layer in `server_payments/`. The service layer determines which gateway to call based on the `active_gateway` value in `payment_config`.

### Gateway Rationale
- **Stripe** — Anvil-native integration, primary international gateway
- **Paystack** — South African and African clients (Stripe does not operate in SA)

### Webhooks
Webhooks from all gateways are received at dedicated HTTP endpoints and verified by signature before processing. Payment events are logged to `webhook_log`. Idempotency must be enforced — duplicate webhook delivery must not create duplicate records.

### Payment Model
Payment is integrated into the booking flow. No separate checkout. Central-account only — all payments go to the business's single account. Per-provider payment accounts are not supported.

System currency is set at onboarding and immutable after the first transaction.

**See:** ADR-038 (Webhook Architecture), ADR-034 (Anvil Platform Constraints)

---

## 2. Three-Tier Email Architecture

### Anvil Built-in Email
System-level platform events: login confirmations, password reset links, account notifications. Accessed via `anvil.email.send()`. Quota: approximately 1,000 emails/month per client instance. Comes from an Anvil-hosted address.

### Brevo SMTP — Transactional Email
Booking confirmations, appointment reminders, invoice delivery, contact form auto-replies. Originates from the client's business identity.

Connection constants (hardcoded in `transactional_email_service.py`):
- Host: `smtp-relay.brevo.com`
- Port: `587` (STARTTLS)
- Authentication: Brevo SMTP key from Vault (`brevo_smtp_key`)
- From address/name: from `email_config` table

Brevo is client-connected: each client creates their own Brevo account. Free tier: 300 emails/day, SMTP included.

### Brevo Campaigns — Marketing Email
Campaign sequences, broadcast emails, newsletter communications. Subject to unsubscribe management, open/click tracking.

Brevo API key stored in Vault (`brevo_api_key`). Separate from SMTP key — two distinct credentials per client.

**See:** `1-brevo-replaces-zoho-email.md`

---

## 3. Brevo CRM Integration

Optional convenience provided to clients. Each Mybizz client instance connects to its own separate Brevo organisation. Mybizz does not operate a shared Brevo org across all clients.

Free tier capabilities: 100,000 contacts, full API access, marketing automation workflows, contact history, pipeline. No per-user limitation.

CRM is automatically updated whenever a booking or service transaction is created via `contact_service.py`'s `update_contact_from_transaction()` function.

---

## 4. Background Tasks

Seven scheduled and event-driven background tasks (see ADR-036):

| Task | Schedule | Purpose |
|------|----------|---------|
| Health heartbeat | Every 15 minutes | Write health metrics to `health_log` table |
| Campaign enrollment | Hourly | Process active campaign enrollments, send next email |
| Appointment reminders (24h) | Hourly check | Send reminders for appointments in next 24 hours |
| Appointment reminders (1h) | Hourly check | Send reminders for appointments in next 1 hour |
| Lifecycle stage updates | Daily 02:00 | Recalculate contact lifecycle stages |
| Automated task generation | Daily 03:00 | Create follow-up tasks based on upcoming bookings |
| Webhook processing | On event | Defer webhook payload processing from HTTP handler |

Any operation expected to exceed 22 seconds must use `@anvil.server.background_task`. Background tasks have no timeout limit.

**See:** ADR-036 (Real-Time and Background Task Architecture)

**See:** ADR-039 (PDF Invoice Generation) for invoice PDF approach.

**See:** ADR-040 (Data Access Patterns) for denormalization and pre-computed aggregation patterns.

---

## 5. Timezone Architecture

Each client instance configures its own timezone at onboarding, stored as an IANA timezone string in `business_profile.timezone`. All datetimes are stored in the database as UTC. Conversion to the client's local timezone occurs at display time in server functions.

**See:** `5-timezone-utc-storage-display-conversion.md`

---

## 6. Domain and Provisioning

Mybizz owns the domain `mybizz.live`. Client subdomains are issued from this domain. Cloudflare is used for all DNS, CDN, and security.

**Scenario A:** Client has no domain → receives `businessname.mybizz.live`
**Scenario B:** Client brings their own domain → Mybizz points nameservers to Cloudflare

Business email is not provisioned by Mybizz. Clients use whatever email provider they choose.

---

## 7. Offboarding

On offboarding, the client receives a full export of all Data Tables, their `encryption_key`, and Vault contents in decrypted form. Mybizz retains nothing of the client's data after offboarding.

---

*End of file*
