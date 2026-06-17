# Technical Handover — Mybizz CS Platform
## Session Date: 2026-06-13
## Prepared for: OpenCode
## Prepared by: Architecture and planning session (Claude / mybizz dev)

---

## 1. Purpose of This Document

This handover captures all architectural decisions, verified findings, implementation approaches, dependencies, constraints, and unresolved items from the planning session of 2026-06-13. It is the single source of truth for continuing development with minimal additional context.

OpenCode should read this document in full before touching any code. Every decision here has a rationale. Do not deviate from finalized decisions without creating a new ADR.

---

## 2. Project Identity

**Platform:** Mybizz CS — a dependency-based SaaS business management platform for the consulting and services vertical.

**Model:** White-label. Clients are unaware of Anvil. Each client receives a dedicated hosted application instance. Mybizz owns all infrastructure.

**Developer:** Solo. All decisions are made with a solo dev constraint in mind.

**Target capacity:** 100 clients (V1 cap).

**Current build state:**
- Planning: complete (with additions from this session)
- Data Tables: defined and set up (36 tables)
- App: scaffolded in mb-3-cs
- UI: to be built
- Server modules: to be coded feature by feature as UI is built

**Primary development app:** `mb-3-cs` — all feature work happens here.

---

## 3. The Five-App Structure (Finalized)

This is the confirmed platform architecture. All five apps live on the single mybizz Anvil account.

| App | Role | Contains | Updated via |
|---|---|---|---|
| `mb-3-cs` | Development workspace | All code + all forms + all server modules + 36 tables | Daily development; GitHub `develop` branch |
| `master_template` | Published dependency | All server modules + all forms + all UI | Merge `develop` → `stable` in GitHub |
| `blank_client_template` | Provisioning clone source | 36 tables + one startup module + master_template dependency | When schema changes only |
| `[client-name]-cs` | Client instances (one per client) | 36 tables + one startup module + master_template dependency (active) | Auto — server logic, UI, forms all propagate via dependency; schema changes manual only |
| `Mybizz_management` | Platform operator tooling | Client registry, monitoring, billing, update distribution | Deferred — built post-launch |

**Critical rules:**
- Client instances must never contain server modules — Anvil runs the instance's own code instead of the dependency, permanently breaking update propagation
- Client instances must never contain forms — same precedence problem applies to UI
- The startup module is the only client-side code a client instance contains

---

## 4. Verified Architectural Finding — app_tables Resolution

**Test conducted:** 2026-06-13  
**Status:** Confirmed ✅

`app_tables` called from within a master_template server module resolves to the **calling client instance's Data Tables** — not master_template's own tables.

**Test method:**
- `datatable-test-host-app` (acting as dependency): server module called `app_tables.table_1.add_row()`
- `datatable-test-client-app` (acting as client instance): depended on host app; had its own `table_1`
- Result: row written to client app's `table_1` only; host app's `table_1` unchanged

**Implication:** Data isolation between client instances is structural and automatic. No application-level enforcement required. All 36 tables in each client instance are exclusively that client's data. One client's server calls cannot access another client's tables.

**Documented in:** `docs/platform-overview.md` Section 7.4 (already appended during this session)

---

## 5. Finalized Decisions — ADR Summary

### ADR-028 — Client Instance Architecture
**File:** `adr/028-client-instance-architecture.md`

**Decision:** Client instances contain only 36 Data Tables and one startup module. No server modules. No forms. All server logic and all UI/forms are served from master_template via the dependency. The startup module is the only client-side code a client instance holds.

**Verified by:** Test A (app_tables resolution) and Test B (startup form from dependency) — both 2026-06-13.

**Implementation approach:**
- All server modules written in mb-3-cs, published via master_template
- All forms and UI written in mb-3-cs, published via master_template
- Client instances receive all logic and UI exclusively through the dependency
- `blank_client_template` is the provisioning source (36 tables + startup module + dependency pre-wired)

**Constraints:**
- Schema changes after client provisioning require manual migration per client instance
- Client instances must never have server modules or forms added

---

### ADR-029 — Dependency Update Model
**File:** `adr/029-dependency-update-model.md`

**Decision:** mb-3-cs develops on GitHub `develop` branch. master_template tracks GitHub `stable` branch. Merging `develop` → `stable` releases an update. All client instances on `stable` branch update automatically. Version tags (`v1.0.0`, `v1.1.0`) enable staged rollout.

**Rationale:** Anvil has no provisioning API; no mechanism exists to update a dependency version from within an app. Branch-tracking means the merge IS the release action. No per-client manual steps required.

**Implementation approach:**
- mb-3-cs: connected to GitHub `develop` branch
- master_template: connected to same GitHub repo, `stable` branch
- All client instances: dependency set to master_template `stable` branch at provisioning
- Release process: test in mb-3-cs → merge `develop` → `stable` → send release notification email via Anvil built-in email
- Staged rollout: tag commit in master_template repo (e.g. `v1.1.0`) → update subset of clients to that tag → roll remaining clients to `stable` once confirmed

**Client notification:** Anvil built-in email (system email tier — not client's Brevo). Notification is informational; update has already propagated at time of sending. In-app notification system deferred to Mybizz_management phase.

**Test required before first client onboarded:**
1. Create `test-dependency-app` connected to GitHub `stable` branch
2. Create `test-client-app` depending on `test-dependency-app` stable branch
3. Make a change in `test-dependency-app`, merge to `stable`
4. Verify `test-client-app` reflects the change without any action on the client app side
5. ADR status updates to Fully Confirmed on successful test

**Constraints:**
- Merge to `stable` is a production action — must be tested fully in mb-3-cs first
- Data Table schema changes do not propagate — must be applied manually to all existing client instances and to blank_client_template

---

### ADR-030 — blank_client_template as Provisioning Clone Source
**File:** `adr/030-blank-client-template.md`

**Decision:** A dedicated `blank_client_template` app is maintained on the mybizz Anvil account. It is the sole clone source for new client instances. It contains all 36 Data Tables (correct schema, no data), one startup module, and master_template as dependency. No server modules. No forms.

**Rationale:** Anvil cannot create tables programmatically. A fresh blank app has zero tables. Cloning master_template would copy all server module code and forms into the client instance, breaking the update model (ADR-028). Manual table creation per client (36 steps) is not workable. Test B confirmed that forms do not need to live in the client instance — they are served from master_template at runtime.

**Implementation approach:**
- Create `blank_client_template` in mybizz Anvil account
- Add all 36 Data Tables with correct schema (copy schema from mb-3-cs), including health_log and error_log
- Add one startup module that opens first form from master_template
- Set master_template as dependency (stable branch)
- Add zero forms, zero server modules
- Leave Anvil Secrets empty (set per instance at provisioning)
- For every new client: clone `blank_client_template`, rename, proceed with activation runbook

**Schema migration responsibility:**
- Every schema change must be applied to `blank_client_template` (future clients)
- Every schema change must be applied manually to all existing client instances (running clients)
- A schema change must never be merged to `stable` without a documented migration plan

**Constraints:**
- `blank_client_template` is a maintained asset for Data Tables only — schema changes are the only maintenance trigger
- Must be kept in sync with mb-3-cs schema at all times

---

### ADR-031 — 30-Day Free Trial Abandoned
**File:** `adr/031-free-trial-abandoned.md`

**Decision:** No free trial. Replaced by: live guided demo on mybizz's own configured instance + 30-minute onboarding session + 30-day money-back guarantee.

**Rationale:** Product requires Vault setup, gateway configuration, Brevo connection, and business profile before it functions usefully. Anvil free tier disables background tasks, scheduled tasks, and persistent server — making core features (appointment reminders, campaign enrollment) non-functional in trial. A crippled demo creates a misleading first impression.

**Implementation approach:**
- Maintain mybizz's own fully configured demo instance
- All paying clients provisioned on Anvil Business plan from day one
- Money-back guarantee terms to be defined in subscription agreement
- No free-tier app provisioning logic required anywhere in the codebase

**Constraints:** None technical. Requires a well-maintained demo instance.

---

### ADR-032 — Observability Architecture
**File:** `adr/032-observability-architecture.md`

**Decision:** Application-level metrics written to designated tables in each client instance. Mybizz_management is the consolidated monitoring layer. External tools (Prometheus, Grafana) rejected. Anvil's built-in per-app tools used for deep diagnostic work only.

**Rationale:** Anvil's monitoring tools are per-app and IDE-bound — not accessible from a management application, and require separate logins for each of 100 client apps. External tools require infrastructure not consistent with a hosted-only model.

**Implementation approach:**

Master_template must include server-side logic that writes to the following tables in each client instance:

| Table | Event type | Trigger |
|---|---|---|
| `health_log` | Health heartbeat | Scheduled background task |
| `audit_log` | Background task completion/failure | On task end |
| `audit_log` | Payment events | On payment success/failure |
| `audit_log` | Vault access | On every Vault open (existing — ADR-003) |
| `audit_log` | User sessions | On login/logout |
| `error_log` | Unhandled exceptions | On error threshold |

Mybizz_management queries these tables via management HTTP endpoints (ADR-033) and displays a consolidated dashboard:
- Last active per client
- Background task health (last run, success/failure)
- Error count and last error per client
- Subscription and billing status

**Dependencies:**
- `health_log` and `error_log` tables must be added to the 36-table schema in mb-3-cs AND to `blank_client_template`
- Mybizz_management must be built before consolidated view is available (manual IDE monitoring until then)
- Heartbeat background task adds to per-client scheduled task load — verify against Anvil Business plan limits

**Anvil built-in tools remain in use for:**
- Deep per-app diagnostic investigation (App Logs)
- Performance bottleneck analysis (Profiling and Tracing — available from early 2026)
- CPU and memory investigation (CPU/Memory tabs — available from February 2026)

---

### ADR-033 — Mybizz_management Visibility and Control
**File:** `adr/033-mybizz-management-visibility.md`

**Decision:** Bidirectional HTTP endpoints secured with per-client Bearer tokens. Three communication patterns: on-demand reads/control, background polling, and event push from client instances to Mybizz_management.

**Rationale:** Anvil has no native cross-app data access outside the dependency model. Mybizz_management cannot depend on client instances (inverted architecture). Uplink is for external Python processes, not Anvil-to-Anvil. HTTP endpoints are the correct Anvil-native mechanism.

**Implementation approach:**

**Part A — Management endpoints in master_template**

Create `server_modules/management_endpoints.py` in master_template. Ships with every client instance automatically.

```
GET  /management/health              → health metrics, last active, error summary
GET  /management/status              → subscription status, feature flags
POST /management/suspend             → write suspended flag to client tables
POST /management/unsuspend           → clear suspended flag
POST /management/push-notification   → write notification row to client notifications table
POST /management/push-update-alert   → write update available notice to client tables
```

All endpoints validate Bearer token from `Authorization` header. Reject with HTTP 403 on invalid token.

**Part B — Event receiver in Mybizz_management**

```
POST /events/receive    → log incoming client event to Mybizz_management event table
```

Master_template server modules call this endpoint (non-blocking) when:
- Payment success / failure
- Background task failure
- Unhandled exception / error threshold
- Vault access
- Subscription lapse trigger

**Part C — Background polling in Mybizz_management**

Scheduled background task in Mybizz_management calls each client's `/management/health` endpoint. Stores results. Alerts if client fails to respond within threshold. Suggested polling frequency: every 15 minutes.

**Security model:**

| Direction | Token type | Storage |
|---|---|---|
| Mybizz_management → Client | Unique Bearer token per client | Mybizz_management client registry (encrypted); client instance management config table |
| Client → Mybizz_management | Shared event push token | Each client's Vault |

**Client registry table in Mybizz_management:**

| Field | Purpose |
|---|---|
| `client_id` | UUID |
| `app_name` | Anvil app name |
| `base_url` | Client instance base URL |
| `management_token` | Encrypted Bearer token |
| `subscription_status` | active / suspended / churned |
| `plan` | Pricing tier |
| `onboarded_date` | Activation date |
| `last_health_check` | Timestamp of last successful health poll |
| `last_active` | Last user session (from health endpoint) |

**Timing constraint:** `management_endpoints.py` must be built into master_template before the first client is provisioned, even though Mybizz_management itself is deferred. All client instances provisioned from day one must already have the endpoints in place.

---

## 6. Client Instance Activation — Runbook Summary

**Full runbook:** `implementation/client-activation-runbook.md`

**14-step process in 5 phases:**

| Phase | Steps | Summary |
|---|---|---|
| 1 — Provision | 1–3 | Clone `blank_client_template`, set `encryption_key`, configure domain |
| 2 — Management integration | 4–6 | Generate management token, register in client registry, verify health endpoint |
| 3 — Owner account | 7 | Create Owner user with correct role |
| 4 — Guided setup session | 8–12 | Vault (gateway keys, Brevo SMTP), webhook registration, Brevo config, business profile, initial services |
| 5 — Completion | 13–14 | Send welcome email, confirm billing active, file checklist |

**Gateway webhook URLs per client instance:**
- Stripe: `[base_url]/api/webhooks/stripe`
- Paystack: `[base_url]/api/webhooks/paystack`

**Client never has Anvil IDE access.** All management via application UI (Owner role) or mybizz operator tooling.

---

## 7. Email Tier Architecture (Clarified)

Three separate email concerns. Do not conflate them.

| Tier | System | Purpose | Scope |
|---|---|---|---|
| System / platform email | Anvil built-in email | mybizz → client: release notifications, platform alerts, welcome emails | mybizz use only |
| Transactional email | Client's Brevo SMTP account | Client app → client's customers: booking confirmations, invoices, receipts | Client's business use |
| Marketing campaigns | Client's Brevo marketing account | Client → client's customer marketing campaigns | Out of scope for this session |

Anvil built-in email is the mechanism for all update notifications. Brevo is never used for platform-level communication.

---

## 8. Billing and Hosting Model (Partially Resolved)

**Decision:** All client instances hosted as separate apps on one mybizz Anvil account. One Business plan account, not per-client accounts.

**Anvil Business plan: ~$89/month (annual)**

Features confirmed required:
- Unlimited apps ✅
- Scheduled tasks ✅ (required for campaign enrollment, appointment reminders)
- Unlimited background task timeout ✅
- Persistent server ✅ (required for performance)
- Multiple custom domains ✅
- Private GitHub repos ✅
- 1,000,000 Data Table rows
- 100 GB Data Table storage

**Unresolved — action required:**
The pricing page does not explicitly state whether row/storage limits are per-account or per-app. This is a critical question.

**Action:** Email Anvil sales with the specific question: *"On the Business plan with multiple apps on one account — do the Data Table row limits and storage limits apply per account or per app?"*

- If **per app**: no issue at any scale
- If **per account**: 1,000,000 rows ÷ 100 clients = 10,000 rows per client — potentially tight for active businesses over time. Extra Server Resources add-on ($599/month, includes isolated database) is the mitigation.

**Platform-overview.md update required:** The $18,000/year direct hosting cost figure is based on the old per-account model and must be revised once the Anvil billing question is answered.

---

## 9. Unresolved Dilemmas — Not Yet Addressed

The following 6 dilemmas were identified at the start of the session but not yet resolved. They must be addressed before build continues past the current stage.

| # | Dilemma | Why it matters |
|---|---|---|
| 4 | Background Task Limits per client instance | Multiple background tasks per client (reminders, campaign enrollment, health heartbeat). Anvil Business plan limits apply per account or per app — unknown. At 100 clients, this may be a hard ceiling. |
| 5 | Webhook Architecture per client | Stripe, Paystack, and Brevo all POST webhooks. Each client is a separate app with a separate URL. Webhook handler must return HTTP 200 immediately and defer processing to background task or gateway times out. |
| 6 | State Management Between Forms | Navigation uses lambda + `open_form()`. Every form switch is a fresh instantiation — state does not carry across automatically. No defined pattern yet for shared state across forms at scale (globals.py vs other patterns). |
| 7 | PDF Invoice Generation | No native Anvil PDF library. Server-side only. Library choice not yet made. Must be decided before the invoicing feature is built. |
| 8 | Skulpt Client-Side Limitations | Not all Python 3 stdlib works in Anvil's client-side runtime. Date arithmetic, currency formatting, and number formatting have known quirks. Rule of thumb: anything touching money or dates lives in server modules only. |
| 9 | Server Call Latency in Booking Flow | Booking flow has multiple steps (service → provider → date/time → meeting type → intake → payment → confirmation). Each server round-trip adds latency. Requires deliberate design to avoid sluggish UX. |

**Do not build features touching dilemmas 4–9 without resolving them first.**

---

## 10. Tests Required Before First Client

### Test A — Branch-based Update Propagation (ADR-029) — PENDING
1. Create `test-dependency-app` connected to GitHub `stable` branch
2. Create `test-client-app` depending on `test-dependency-app` stable branch, no code of its own
3. Make a change in `test-dependency-app` server module
4. Merge change to `stable` in GitHub
5. Call the function from `test-client-app` — verify new behaviour without any action on client app

Pass criteria: client app reflects the updated dependency behaviour immediately after merge, with no manual action on the client app.

### Test B — Startup Form from Dependency — ✅ COMPLETED 2026-06-13

`test-client-no-forms` (startup module only, no forms) successfully loaded and displayed `MainForm` from `test-dependency-forms` via dependency. Passed without error.

**Consequence:** Forms live in master_template. Client instances require only a startup module and Data Tables. ADR-028, ADR-030 updated accordingly.

---

## 11. Assumptions and Constraints

| Item | Assumption / Constraint |
|---|---|
| Anvil account structure | All apps on single mybizz Business plan account |
| Client awareness | Clients never know Anvil is involved; never have IDE access |
| Solo developer | All decisions optimised for solo maintenance; no team tooling required |
| V1 client cap | 100 clients maximum; architecture does not need to scale beyond this |
| Schema changes post-launch | Manual migrations to existing client instances; constrain schema changes in production |
| Server module location | All server modules live in master_template only — never in client instances |
| Form and UI location | All forms and UI live in master_template only — never in client instances |
| Client instance contents | 36 Data Tables + one startup module only — nothing else |
| Vault | `encryption_key` is the only item in Anvil Secrets; all other secrets in the Vault |
| Background tasks | Anvil Business plan required; free tier background task limits make core features non-functional |
| Data residency | All client data stays in the client instance; never centralised |
| Mybizz_management | Deferred until after first client is manually onboarded and master_template is launch-ready |

---

## 12. Document Registry — All Relevant Files

### Newly Created This Session

| File | Path | Type |
|---|---|---|
| Client Instance Architecture | `adr/028-client-instance-architecture.md` | ADR |
| Dependency Update Model | `adr/029-dependency-update-model.md` | ADR |
| blank_client_template | `adr/030-blank-client-template.md` | ADR |
| Free Trial Abandoned | `adr/031-free-trial-abandoned.md` | ADR |
| Observability Architecture | `adr/032-observability-architecture.md` | ADR |
| Mybizz_management Visibility | `adr/033-mybizz-management-visibility.md` | ADR |
| Client Activation Runbook | `implementation/client-activation-runbook.md` | Runbook |
| Platform Overview Addendum | `docs/platform-overview-addendum.md` | Doc update |

### Pre-Existing Documents Referenced

| File | Path | Notes |
|---|---|---|
| Platform Overview | `docs/platform-overview.md` | Section 7.4 already updated; addendum to be appended |
| Vault System | `docs/vault-system.md` | Confirmed unchanged; audit log pattern extended by ADR-032 |
| Payment Security Boundary | `adr/3-payment-security-boundary-vault.md` | Confirmed unchanged; referenced by ADR-032 |
| Build Plan | `implementation/build-plan.md` | Not modified this session; verify Stage 1.5 (Vault) dependency on ADR-033 management endpoints |

### Platform Overview Addendum — Append Instructions

`docs/platform-overview-addendum.md` contains 5 self-contained sections with explicit placement instructions. Append each section to platform-overview.md at the position indicated in the addendum file. Do not modify the addendum file itself — append from it.

---

## 13. Immediate Next Actions for OpenCode

In priority order:

1. **Append** `docs/platform-overview-addendum.md` sections to `docs/platform-overview.md` at indicated positions

2. **Create** `blank_client_template` app in Anvil IDE:
   - Clone mb-3-cs table schema (all 36 tables, empty data)
   - Add `health_log` and `error_log` tables to schema (required by ADR-032) — also add these to mb-3-cs
   - Set master_template as dependency (stable branch)
   - Confirm zero server modules and zero forms

3. **Run Test A** — branch propagation (see Section 10)

4. **Run Test B** — startup form from dependency (see Section 10)

5. **Create** `server_modules/management_endpoints.py` in mb-3-cs:
   - Implement the 6 HTTP endpoints defined in ADR-033
   - Bearer token validation on all endpoints
   - This must ship in master_template before the first client is provisioned

6. **Email Anvil** re: per-account vs per-app row/storage limits (see Section 8) — unblock billing model finalisation

7. **Resolve Dilemmas 4–9** (see Section 9) before building any features that touch those areas

8. **Update build-plan.md** to reflect:
   - `blank_client_template` creation as a pre-launch task
   - `management_endpoints.py` as a Stage 1 deliverable
   - `health_log` and `error_log` table additions to schema
   - Two pre-launch tests (A and B above)

---

## 14. What Has Not Changed

The following areas were reviewed and confirmed unchanged. No action required:

- **Vault system** (`vault-system.md`) — architecture is sound; no modifications
- **Payment security boundary** (ADR-003) — confirmed unchanged
- **36-table schema** — unchanged except addition of `health_log` and `error_log` (action item 2 above)
- **TOTP step-up for Vault access** — unchanged
- **UTC storage with IANA timezone display** — confirmed as existing decision; not revisited
- **Single contacts table model** — confirmed as existing decision; not revisited

---

*End of HANDOVER-opencode-2026-06-13.md*
