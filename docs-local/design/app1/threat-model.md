---
title: "threat-model"
doc-id: "mb-15e2c8aa"
updated: "2026-11-07"
---

# Mybizz CS — Threat Model (App 1, run app1)

Written at Step 15 by `secure-software-engineering` at its threat-model phase, on the reviewed architecture (design doc §23). Method: the skill's lightweight threat model; STRIDE as vocabulary. **Scope note — multi-tenant guidance does not apply:** the `secure-software-engineering` skill's multi-tenant guidance (`references/multi-tenant-isolation.md`) has no target here — Mybizz CS is dependency-based, one Anvil app and one database per client instance; there is no shared database, no tenant discriminator and no tenant context anywhere (`adr-dependency-based-not-multi-tenant`). The leakage guard still runs as its own check: `sec-multi-tenant-assumption-leakage`'s verification — zero tenant-style filtering or columns — is test ST-06. Every boundary below names its owner: **Anvil** (the platform guarantees it structurally) or **the application** (code must enforce it). No credential, client host, deployment detail or open weakness is recorded here; an open weakness is a ledger finding, not a file entry.

## System Overview

A client instance receives the schema, one startup module and `master_template` as its dependency; all logic runs in server code the client's browser reaches only through server functions and HTTP endpoints. Outside services: the card gateways and PayPal, Amazon SES/SNS/S3, Cloudflare R2, App 5 (management, AI gateway, Help). The full surface: §23.4's service table and §23.7's endpoints.

## Assets

| Asset | Harm if compromised | Location or flow | Owner |
|---|---|---|---|
| Client data — personal (contacts, bookings, invoices, email) | POPIA/GDPR breach; one client seeing another's | The instance's tables; cold copies in its R2 bucket | The client (Owner) |
| Credentials — gateway keys, AWS keys, R2 token, Bearer token | Direct financial and data loss across the client's services | The Vault, encrypted by `encryption_key` | The client; the application enforces |
| Money-moving actions — payments, refunds, gateway config, prices | Fraud, double-payment, wrong amount | Server functions only; card data never touches the instance | The application |
| The audit trail | Undetectable abuse | `activity_log` + R2 archive | The application |
| The AI/speech channel | Personal-data leak to the model provider | Instance → App 5 → Groq, pass-through | The application |
| Availability | Client's business stops | Anvil hosting, per-client account | Anvil; Mybizz operates |

## Actors and Authority

| Actor | Trust basis | Current authority | Required authority | Explicitly denied |
|---|---|---|---|---|
| Visitor (signed out) | none | public pages, booking/order/quote forms, consent writes | as listed | any server function; any other person's data |
| Customer | password + 2nd factor | own records only, derived from the signed-in user | own bookings, packages, quotes, invoices, reviews | other customers' rows; any back-office function |
| Staff | password + 2nd factor | own calendar, bookings, deals, addresses | own rows only | other providers' rows; settings; Vault; financial config |
| Admin | password + 2nd factor | bookings and customers | operational forms | financial reporting, settings, Vault |
| Manager | password + 2nd factor | operational management | operational tabs + reports | Vault; gateway secret keys; user management |
| Owner | password + FIDO2 only | everything, incl. the Vault with TOTP step-up | full access | (n/a) |
| App 5 | per-client Bearer token | the six management endpoints; the AI pass-through | health, status, suspend, push, update alerts | any client table directly; any other instance |
| Background jobs | server-side | the task inventory of §23.6 | as listed | client authority; secrets in logs |
| Outside services | TLS + signatures | the webhook endpoints' data only | as listed | any other path in |

## Data Flows and Trust Boundaries

### B1 — Browser → server functions
Trust changes at every `anvil.server.call()`: the client-supplied value is untrusted input, never authority. Governing: `sec-server-side-authority`, `sec-broken-access-control-within-instance`, `sec-insecure-direct-object-reference`, `sec-injection-unsafe-input`. Owner: **the application** (Anvil guarantees only the client/server code boundary, not within-instance roles). Threats: insecure direct object reference (a guessed invoice or PDF id), broken access control (a Staff id supplied as a parameter), injection through a future non-Data-Table call path, over-broad returns. Controls: every callable carries a role decorator naming exactly its roles (`server_auth/rbac`); identity and ownership derived from `anvil.users.get_user()`, never a client-supplied id; get-by-ID functions verify ownership; all queries through Anvil's parameterized Data Tables API; functions return no more than the request needs (restricted projections for `users`, `vault`, `contacts.internal_notes`). Verification: IN-02 (matrix over every function and role), IN-03, IN-04, ST-02, ST-04, SC-05, SC-06, BR-03. Residual: none recorded.

### B2 — Role → role within the instance
Vertical escalation from a lower role to a higher one. Governing: `sec-privilege-escalation`, `spec-security-architecture` §1. Owner: **the application**. Threats: a decorator written broader than the role table; a Client-Uplink call as any role succeeding on a higher-role function. Controls: decorators match the five-role table exactly; the Owner's high-value functions refuse while a non-FIDO2 factor is enrolled; navigation hides what a role cannot use (never the enforcement). Verification: IN-02 as every role, IN-03, SC-08, ST-12, BR-03. Residual: none recorded.

### B3 — Server ⇄ payment gateways (Stripe, Paystack, PayPal)
Money boundary both directions. Governing: `sec-payment-gateway-trust-boundary`, `sec-payment-manipulation`, `adr-webhook-architecture`. Owner: **the application**; Anvil provides TLS. Threats: amount manipulation from the browser, forged or replayed webhooks, double capture or refund, card data touching the instance, payment-page script tampering. Controls: every amount read from the database, never a parameter; signature verified before any payload is used (Stripe/Paystack headers; PayPal's verify-webhook-signature call with `paypal_webhook_id`); `webhook_log` idempotency by the gateway's event id; PayPal captures and refunds carry a `PayPal-Request-Id`; card data never held — SAQ A; the payment-page script list approved and checked at every release; reconciliation every 5 minutes (D4). Verification: IN-06, IN-07, SC-10, test 4.20. Residual: gateway-side acceptance is outside the instance (test-mode gateways at Level 2/3).

### B4 — Server ⇄ Amazon SES, SNS, S3
Email boundary both directions. Governing: `sec-email-service-trust-boundary` — **the boundary is Amazon SES with SNS-signed events, never Brevo** (David's Step 15 decision; the document's Brevo wording is corrected after Step 15 and its rules apply unchanged here) — and `adr-brevo-email-and-crm`. Owner: **the application**. Threats: a forged SNS notification creating a false email or suppression, spam/virus delivery, sending-reputation abuse, address harvesting from bounces. Controls: SNS signature checked with Amazon's signing certificate; the subscription confirmation accepted only for the client's own topic; virus-failed mail never stored, spam/DMARC-failed to Spam; suppression honoured within scope; scoped AWS keys (send from and read the client's prefix only); DKIM/SPF/DMARC written at provisioning. Verification: IN-07, IN-09, ST-09, tests 4.1–4.5. Residual: none recorded.

### B5 — App 5 ⇄ instance
The platform's control, push and AI-gateway boundary. Governing: `sec-management-service-authentication`, `adr-mybizz-management-visibility`, `spec-mybizz-management-app-notification-system`. Owner: **the application** (endpoint validation); Anvil provides transit. Threats: a leaked or guessed Bearer token suspending or notifying a client; a forged platform push; the AI channel leaking personal data. Controls: the per-client Bearer token validated before any processing — HTTP 403 and no state change; one token per client, encrypted in the App 5 registry and the instance's Vault; rotation manual in V1 by re-provisioning, tracked (max age: manual, per `sec-secret-rotation-policy`); the push path is the one canonical endpoint `POST /management/push-notification` (D4); AI and speech requests pass through App 5 unstored and unlogged, Groq Zero Data Retention on, tier 5 sends placeholder labels only. Verification: IN-12, SC-14, RL-05, SR-53, SR-53a, SR-45, SR-45a. Residual: manual token rotation is a real, current limitation — tracked, not silently assumed.

### B6 — Instance ⇄ its R2 bucket
Cold-data boundary. Governing: `sec-backup-recovery-integrity`, `sec-data-exfiltration-bulk-export`, `adr-client-r2-storage-and-exit-handover`. Owner: **the application** (`r2_service` only); the bucket account is the client's. Threats: a stolen bucket token reading another client's data, bulk export abuse, tampered backups, an unrecoverable restore. Controls: one bucket per client in the client's own Cloudflare account; the token scoped to that one bucket, in the Vault; `r2_service` the only module that touches the bucket; bulk exports write an audit row (who, when, scope); backups proven by restore drills; the exit export confirmed before anything is deleted. Verification: IN-10, IN-14, RL-02, RL-08, SC-14. Residual: none recorded.

### B7 — Sessions and second factors
Authentication boundary for every human actor. Governing: `sec-authentication-session`, `spec-security` §3. Owner: **Anvil** (session mechanics, password hashing, breached-password checks) and **the application** (step-up checks, custom screens). Threats: a session kept alive past its window, a high-value action on a stale factor, account discovery through reset wording. Controls: 30-minute inactivity enforced by Anvil's session mechanism (confirmed not overridden); a second factor for every account holder; the Owner FIDO2 only, high-value functions refused while any other factor is enrolled; a factor given in the last five minutes for Vault, refunds above the limit, role changes, exports, gateway and price changes; identical wording for unknown email and wrong password; the Owner can end any user's sessions. Verification: SC-02, SC-03, SC-04, ST-12, SR-04. Residual: none recorded.

### B8 — Availability and resource exhaustion
Governing: `sec-denial-of-service-resource-exhaustion`. Owner: **the application**. Threats: request floods, unbounded queries or exports. Controls: rate limits enforced in the `rate_limits` table — 10/minute unauthenticated per IP, 100/minute authenticated per user — on every endpoint including background-triggering ones; searches filtered and bounded, never whole-table iteration; operations over 22 seconds as background tasks. Verification: ST-02, IN-16, DC-07. Residual: none recorded.

### B9 — Business-logic races
Governing: `sec-business-logic-race-conditions`. Owner: **the application**. Threats: two confirmations on one slot, double refund, double notification send, duplicated summary rows. Controls: `@tables.in_transaction` — Anvil retries a conflicting transaction up to five times — around every contended write: `confirm_booking`'s atomic re-check, the D5 reinstatement, `contact_counter`, the sequence keys, group-session places, package draw-down, the notification check-and-insert (R4), the summary period-keyed upserts (R5). Verification: IN-05, IN-06, §22 rows 2, 4, 17, 18. Residual: none recorded.

### B10 — Secrets lifecycle
Governing: `sec-master-encryption-key-protection`, `sec-secret-exposure-response`, `sec-secret-rotation-policy`, `sec-secret-access-audit`, `adr-payment-security-boundary-vault`. Owner: **the application**. Threats: the master key read outside `vault_service`, a secret logged through an exception handler, a client round-trip of a secret field, an unreviewed access pattern. Controls: exactly one module calls `anvil.secrets.get_secret()`; masked reads `'***'`; secret fields omitted from every outbound payload; exception handlers in secret-reading functions log codes, never the exception object; every Vault access audited and emailed; gitleaks over the working tree and history at every step that writes documents; the exposure response rotates immediately. Verification: ST-03, IN-13, SC-03, PR-06. Residual: none recorded.

### B11 — Dependency supply chain
Governing: `sec-dependency-skill-supply-chain`, `sec-dependency-version-currency`. Owner: **the application** (`requirements.txt`). Threats: a vulnerable or hijacked `boto3`/`reportlab` version. Controls: pinned versions; pip-audit at the build and release steps; every new dependency reviewed against the requirement before adoption. Verification: ST-13, SC-18. Residual: none recorded.

### B12 — The build process itself
Governing: `sec-ai-agent-introduced-vulnerabilities`. Owner: **the process** — Sentinel at each step's scrutiny; the developer resolves every finding. Threats: an agent writing plausible-but-wrong access control. Controls: every threat-model document's Approved/Prohibited pair checked against new code; Sentinel's findings resolved only by the developer; `triage_findings` clean before `build-verified`. Verification: the step gates; PR-04. Residual: none recorded.

## Assumptions To Test

| Assumption | Why it matters | Evidence or test | Owner | Status |
|---|---|---|---|---|
| Anvil permits client business apps on Mybizz-owned Business accounts | the whole hosting model | written confirmation from Anvil's sales team (AC-6) | David | open |
| An app can schedule its dependency's background task and serve its dependency's HTTP endpoints | the schedule and endpoints live in App 3/`master_template` | proved in App 1 before Step 31 relies on it (PRD OQ-2) | the build | open |
| `boto3` runs on Anvil's server Python | every SES and R2 call | proved on the private test instance before the first release | the build | open |
| Amazon grants SES production access | every email before launch | the request on file (AC-14) | David | open |

## Decisions And Follow-Up

| Risk or decision | Likelihood | Impact | Chosen mitigation | Owner | Delivery phase | Exception or review date |
|---|---|---|---|---|---|---|
| Management token compromise | low | high | unique per client, encrypted both sides, manual rotation via re-provisioning | Mybizz | V1 | review at first rotation |
| A summary/notification write races | low | medium | period-keyed upsert / transactional check-and-insert (R5, R4) | the application | Step 35 | none |
| SAQ A script tampering | low | high | payment-page script list approved, checked every release | the application | Steps 41–42 | annual SAQ A |

## Plan of Security Assessments

Added by `security-audit-methodology` at Step 15 (item 22). The assessments the build will receive, in order:

| When | Assessment | Level | Evidence |
|---|---|---|---|
| This step (15) | threat model, access matrix, security decisions; Sentinel at High | D | `threat-model.md`, `access-matrix.yaml`, the Security Decisions section, the `triage_findings` row |
| Step 34 (`architecture-verified`) | `ts_scan.py` static checks over the app folder; the skeleton implements this threat model's controls | 0 | the checks file |
| Step 35 (each feature) | Level 1 pure-logic tests incl. the security rules; Level 2 integration via Uplink — the server key sets up, the client key calls as each role; adversarial attempts of every Prohibited pattern above, run literally (`sec-testing-adversarial-verification`) | 1–2 | pytest XML per feature |
| Step 37–38 | independent review — `code-review`, `/cso --diff` and Sentinel, each in its own session (`sec-testing-independent-review`); regression tests for every fixed security finding (`sec-testing-regression-prevention`) | 2–3 | findings files |
| Step 41 (release-ready) | dependency audit (pip-audit); the whole suite passes; no High/Critical finding open (`sec-requirements-release-readiness`) | — | the release checklist |
| Steps 42–44 | OWASP ZAP passive scan; the payment-page script check (SC-10); post-deploy verification in the heartbeats | 3–4 | the release evidence |
| Before first launch, then annual | independent penetration test (SC-16); annual SAQ A while payments are taken | — | on file |
