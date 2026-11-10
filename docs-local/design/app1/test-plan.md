---
title: "test-plan"
doc-id: "mb-2a09f0d3"
updated: "2026-11-10"
---

# Mybizz CS — Test Plan (App 1, run app1)

Written at Step 15 in the shape of `spec-testing-methodology-standards` §6, from GStack's test plan, the review's test decisions (D5–D8) and §22 What Must Be Tested. Each critical flow carries its preconditions, numbered steps with expected results, performance budget and data checks; edge cases and error paths follow; each lifecycle is a state machine; every flow names the catalogue test IDs it uses. The conformance checks that apply are listed at the end. The access matrix is `access-matrix.yaml`; the RBAC verification procedure is `spec-testing-methodology-standards` §7. Roles run against the matrix's six actors: Owner, Manager, Admin, Staff, Customer, SignedOut.

## 1. E2E booking flow (§22 row 1; UF-05–UF-08)

**Preconditions:** instance provisioned with all 70 tables; a `duration` service with a provider and availability rules; a gateway in test mode with its keys in the Vault; `business_profile` complete.

| Step | Action | Expected result |
|---|---|---|
| 1 | Open `/book/:slug` | service loaded in call 1 (`get_booking_context`) |
| 2 | Select service | providers for the service displayed |
| 3 | Select provider | date range displayed |
| 4 | Select date | slots from call 2 (`get_available_slots`); a required resource free in the slot |
| 5 | Select slot | meeting types from the cached context — no server call |
| 6 | Select meeting type | intake form from `booking_metadata_schemas` |
| 7 | Fill intake; untick-checked acceptance box stays disabled until ticked | review step displayed |
| 8 | Confirm (call 3, `confirm_booking`) | booking confirmed; one `payments` row with the gateway reference; invoice generated |
| 9 | Confirmation screen | booking id, date, time, service, provider; calendar invite attached to the email |

**Performance gate:** steps 1–9 under 3 seconds on a standard connection.
**Data checks:** amount = the service's price in whole cents; currency = `business_profile.system_currency`; times stored UTC, displayed in the client's IANA zone; the contact created or matched, never duplicated.

**Edge cases:** existing contact books (no duplicate); provider unavailable (clear message, alternatives); double-booking and the payment hold have their own sections (§2 and §3); a full group session; a resource clash.

**Tests:** test specification §1; `BR-01`, `BR-06`, `DM-01`; `IN-05`; SR-15–SR-18.

## 2. Booking contention — one slot, two customers (§22 row 2)

**Preconditions:** a `duration` service with one provider whose availability rules open a specific slot (date and time); no other booking on that slot; a required resource free where the service needs one.

| Step | Action | Expected result |
|---|---|---|
| 1 | Customer A calls `confirm_booking` for the slot | the booking is created (confirmed at once, or `pending` for an online payment) and the slot is taken |
| 2 | Customer B calls `confirm_booking` for the same slot at the same moment — the concurrent case | **exactly one of the two calls succeeds**: one `bookings` row exists for that slot and time |
| 3 | The losing call's response | "slot no longer available" with the nearest free slots offered; nothing written — no booking, no payment, no hold |

**Data checks:** exactly one `bookings` row for the slot and time; the winner's row holds the slot (and resource where required); no orphan `payments` row for the loser; each customer sees only their own booking afterwards.

**Tests:** `IN-05`, `DM-01`, `SR-16`; test specification §1 (double-booking prevention).

## 3. The payment hold and its expiry (§22 row 3)

**Preconditions:** a booking with an online payment option whose payment has not succeeded — status `pending`, `hold_expires_at` set 15 minutes after creation; its slot and, where the service requires one, its resource are reserved.

| Step | Action | Expected result |
|---|---|---|
| 1 | Before expiry: the 5-minute payment-hold task runs | the booking is untouched — still `pending`, the slot still held |
| 2 | The task runs with the clock past `hold_expires_at` | the booking's status is set to `cancelled` — the row is never deleted; `cancelled_at` is set |
| 3 | The slot and the resource | freed — a new customer can book the same slot immediately |
| 4 | The customer | told: the hold-expired email, with the payment link having worked until expiry |

**Data checks:** the booking's row remains with `status = cancelled` and `cancelled_at` set; the availability query offers the slot again; exactly one expiry notification; no `payments` row for the abandoned hold.

**Tests:** `SR-17`; test specification 2.1; `IN-11`.

## 4. The D5 late-payment race (§22 row 4)

**Preconditions:** a `pending` booking whose 15-minute hold has expired (slot and resource freed); a webhook for its payment succeeds afterwards; a second customer booking the same slot.

| Step | Action | Expected result |
|---|---|---|
| 1 | Late webhook arrives | the payment is recorded as a `payments` row |
| 2 | Reinstatement re-check runs **inside the same transaction** as the confirmation | slot, resource and places still free → booking reinstated to `confirmed`; taken → full refund through the original gateway |
| 3 | Either way | the customer and the Owner are notified |
| 4 | The late-payment webhook races a new booking for the same slot | **exactly one succeeds** |

**Tests:** new cases in this test plan; `IN-05`, `IN-06`, `IN-07`; SR-16, SR-17.

## 5. Payment webhooks — security and exactly-once (§22 row 5)

**Preconditions:** the gateway webhook endpoints configured; test keys in the Vault.

| Step | Action | Expected result |
|---|---|---|
| 1 | Valid webhook | HTTP 200 within 5 s; `webhook_log` row; payload to R2; applied once by a background task |
| 2 | Forged signature | HTTP 400, no row, `error_log` entry |
| 3 | Duplicate `event_id` | refused idempotently; no second state change |
| 4 | Dispatch failure | the row stays unprocessed; the 5-minute reconciliation task applies it (D4) |

**Tests:** `IN-07`; test specification 2.2–2.3; SR-30.

## 6. Server-side amounts (§22 row 6)

Every gateway and PayPal amount is read from the database, never the browser; a repeated PayPal capture records one payment (`PayPal-Request-Id`); a second quote acceptance changes nothing (`/quote/{token}`); PayPal is unavailable for ISK. **Tests:** `IN-06`; SR-17a, SR-26, SR-56; test 4.20–4.21.

## 7. Role enforcement (§22 row 7)

The access matrix (`access-matrix.yaml`) generated into IN-02: every server function called as every role and signed out through the client Uplink key; each call succeeds or is refused exactly as the matrix says; a refused call writes nothing. Every restricted link hidden, not disabled (BR-03). Staff see only their own bookings, deals and addresses; customers only their own records, derived from the signed-in user. **Tests:** `IN-02`, `IN-03`, `BR-03`, `ST-02`; the RBAC matrices of the test specification §3.

## 8. The Vault (§22 row 8; UF-04)

| Step | Action | Expected result |
|---|---|---|
| 1 | Open the Vault twice in a row | the TOTP code is asked twice — no remembered session |
| 2 | Read a secret | `'***'`; the value never leaves the server |
| 3 | Enter 5 wrong codes | the Vault locks 15 minutes; the Owner is emailed |
| 4 | Every access | an email and an `activity_log` row |
| 5 | Reset Vault code | the emailed link clears `vault_totp_secret`; no stored secret changed |

**Tests:** `IN-13`, `SC-03`; tests 4.18–4.19; test specification 2.5; SR-06, SR-07.

## 9. Second factors and sessions (§22 row 9)

Sign-in refused without the second factor for every role; the Owner with only an authenticator app is refused the Vault and refunds (FIDO2 only); 30-minute idle returns the user to the same place; the Owner can end any user's sessions. **Tests:** `SC-02`, `SC-03`, `SC-04`, `ST-12`; test 4.18.

## 10. Email through SES (§22 row 10)

A confirmation from the client's domain, DKIM-signed, with the calendar invite; a desk email reaches every member and no one else; a hard bounce or complaint suppresses all email, an unsubscribe marketing email only; the 90-day archive to R2; a failed virus verdict stores nothing. **Tests:** `IN-09`; test specification 4.1–4.5; SR-34, SR-35, SR-35a–c.

## 11. Sample data isolation (§22 row 11)

No email, payment, reminder or export touches a sample record; Clear sample data removes exactly the `sample_rows` records, empties the links, recomputes the summaries, and is refused for every role but Owner. **Tests:** `SR-44a`, `SR-44b`; tests 4.10–4.11.

## 12. Exit export (§22 row 12)

Every table in full, `encryption_key` and the decrypted Vault into the client's bucket; proven on the test client instance before the first release. **Tests:** `IN-14`, `RL-08`; `offboarding-procedure.md` §4.

## 13. Structural isolation (§22 row 13)

Two instances cannot reach each other's data; per-client tokens and keys unique; no tenant column or filter anywhere (grep). **Tests:** `SC-14`, `ST-06`.

## 14. Data-subject rights and retention (§22 row 14)

Export, correction and erasure of one person's data; transactions kept for the 7-year tax period; retention deletes on time — audit 30 days in-instance, email bodies 90 days, consent records per D4. **Tests:** `IN-14`.

## 15. The legal-documents and consent system (§22 row 15)

A publish raises the version and writes `legal/{document}/v{version}.html`; an agreement does not proceed without the unticked box and writes one `consents` row per document; a new version is asked at the next agreement or portal sign-in; a form collecting personal details writes one `shown` row; the banner sets no embedded-content cookie before Accept all or Load; every choice and Load writes one row; a new Cookie Policy version shows the banner again; the nightly archive moves rows older than 30 days to `consents/{document}/{year}/{month}/` and deletes cookie files at 2 years, acceptance files at 7 years; a `feature_key` clause is active exactly while its feature is on. **Tests:** `spec-legal-documents-and-consent` §12; `ST-02` on the endpoints; SR-38a, SR-38b.

## 16. The AI/speech channel (§22 row 16)

No AI or speech content stored or logged in the instance or App 5; Groq ZDR on; voice tier 5 sends placeholder labels only; tier 4 saves nothing before confirmation. **Tests:** `SR-53a`, `SR-45a`, `SR-45`; tests 4.8–4.9.

## 17. The notification engine (§22 row 17 — D8)

**Preconditions:** `notifications`, `user_preferences`, `delivery_logs` present; a test user with preferences set.

| Step | Action | Expected result |
|---|---|---|
| 1 | Two identical sends at the same moment (same idempotency key) | **exactly one** `notifications` row — the transactional check-and-insert (D5, R4) |
| 2 | Send with the user's channel disabled | no delivery attempt; `delivery_logs` records the preference check |
| 3 | Send inside the quiet-hours window | held until the window ends |
| 4 | Every dispatch | one `delivery_logs` row per channel attempted |
| 5 | App 5 pushes via `POST /management/push-notification` with the Bearer token | one `notifications` row, `source: platform`, shown in the feed |
| 6 | Push with a bad token | HTTP 403, nothing written |
| 7 | The bell badge | matches `get_unread_count`; mark-read updates both |

**Tests:** new cases in this test plan; `IN-02` over the notification functions; `ST-02`; SR-44a applies to sample users.

## 18. The summary tasks (§22 row 18 — D8)

A summary task run twice for the same period leaves **exactly one row per period** with the recalculated figures — for `daily_summary`, `service_performance`, `provider_performance` and `campaign_metrics` (the period-keyed upsert inside the transaction, R5). Dashboards read these tables only (SR-10). **Tests:** new case in this test plan; `IN-11`.

## 19. Lifecycles as state machines

Each lifecycle's allowed and forbidden transitions, tested by state-transition tests: booking (`DESIGN.md`, Booking Status Lifecycle — the D5 `cancelled → confirmed` path only through the transactional reinstatement); payment hold (15 minutes); invoice (draft → sent → paid / overdue → cancelled); order (ordered → in progress → delivered; cancelled); quote (draft → sent → accepted/declined; expired nightly); customer package (active → used/expired/cancelled); lead (new → converted/archived); review (requested → awaiting_approval → approved/declined); campaign and enrolment; group session and series; email message folders (Trash deleted after 30 days). Rows of `bookings`, `orders`, `invoice`, `payments`, `quotes` and `expenses` are never deleted.

## 20. Conformance checks that apply (DC-08)

Selected from `ts-conformance-checks` for this plan: five-app structure and never multi-tenant (ST-06, SC-14); client-instance contents (ST-10, RL-10); Anvil-first (DC-03, DC-07); table access and data-table creation (ST-07, DC-05, IN-02); schema up front and retention (ST-07, DC-14, RL-03); row ids and `created_at`/`updated_at` (ST-07); bounded searches and pre-computed aggregates (ST-09, IN-05, IN-17); one `contacts` table and lead creation (IN-05); UTC, currency, cents (ST-08, LG-04, LG-05); retention defaults (IN-14); access rules and least privilege (ST-02, IN-02, SC-13); server-side authority (IN-03, IN-06, SC-05); no over-broad returns (IN-04); sessions and passwords (SC-04, SC-01, ST-12); rate limits (ST-02, IN-16); responsibility assignment (DC-06); injection and races (ST-04, SC-06, SC-11); expensive functions bounded (DC-07); secrets and Vault (ST-03, IN-13, DC-07, RL-11); payments (IN-02, SC-12, IN-06, SC-10); webhooks and integrations (ST-09, IN-07, IN-08); SES everywhere (IN-09); invoice PDFs (IN-03); R2 and exit (IN-10, IN-14, SC-14, RL-08, RL-02); management (IN-12, RL-05, SC-14, RL-10); logs (ST-07, IN-15); background work (IN-11, DC-07); code rules (ST-05, ST-10, LG-06); forms and anvil.yaml through Anvil's skills (ST-01); M3 and interface rules (ST-11, DC-12, DC-13, BR-03); translation (ST-14); onboarding rules (BR-01); release requirements (RL-01, RL-09); testing discipline (SC-15, SC-18, LG-07).
