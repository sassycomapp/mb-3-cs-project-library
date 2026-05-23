# Mybizz CS — Programmatic Task List

**Purpose:** All tasks that must be done programmatically by AI  
**Excludes:** Developer tasks (drag-and-drop UI work in Anvil Designer)  
**Related:** `09-developer-task-list.md` — Developer-only tasks

---

## Phase 0: Pre-Implementation (Must Complete Before Phase 1)

### TODO 1: Vault Error Handling (P1, Critical)

**Status:** ⏳ Pending  
**Location:** `server_code/server_shared/`

**Tasks:**
1. Create `server_code/server_shared/exceptions.py`
   - Define `VaultError` base exception class
   - Define `VaultSecretNotFoundError` exception
   - Define `VaultDecryptionError` exception

2. Update `server_code/server_shared/vault_service.py`
   - Add rescue block for `KeyNotFoundError` in `get_vault_secret()`
   - Raise `VaultSecretNotFoundError` with message: "Vault secret not configured. Please contact the business owner."
   - Add logging for vault secret retrieval failures
   - Add rescue block for `VaultDecryptionError`

3. Update `server_code/server_shared/encryption_service.py`
   - Add rescue block for encryption key retrieval failures
   - Add validation for 32-character encryption key

4. Create `tests/test_vault_service.py`
   - Unit test for `get_vault_secret()` with missing secret
   - Unit test for `get_vault_secret()` with valid secret
   - Unit test for `save_vault_secret()` with new secret
   - Unit test for `save_vault_secret()` with existing secret
   - Unit test for `delete_vault_secret()` with existing secret
   - Unit test for `delete_vault_secret()` with missing secret

**Completion Criteria:**
- ✅ All vault operations have proper error handling
- ✅ User sees friendly error message, not 500 error
- ✅ All unit tests passing

---

### TODO 2: Test Coverage Framework (P1, Pre-Implementation)

**Status:** ⏳ Pending  
**Location:** `tests/` directory

**Tasks:**
1. Create `pytest.ini`
   - Configure test paths, markers, coverage settings

2. Create `tests/conftest.py`
   - Define `mock_vault_table` fixture
   - Define `mock_encryption_key` fixture
   - Define `mock_brevo_client` fixture
   - Define `mock_stripe_client` fixture
   - Define `mock_paystack_client` fixture

3. Create `tests/server_shared/`
   - `test_exceptions.py` — Test custom exception classes
   - `test_encryption_service.py` — Test encryption/decryption
   - `test_vault_service.py` — Test vault operations

4. Create `tests/server_auth/`
   - `test_rbac.py` — Test RBAC decorators
   - `test_service.py` — Test authentication functions

5. Create `tests/server_payments/`
   - `test_stripe_service.py` — Test Stripe integration
   - `test_paystack_service.py` — Test Paystack integration
   - `test_webhook_handlers.py` — Test webhook processing

6. Create `tests/server_bookings/`
   - `test_service.py` — Test booking functions
   - `test_availability_service.py` — Test availability logic

7. Create tests for all server modules following same pattern

**Completion Criteria:**
- ✅ 100% coverage for pure logic functions
- ✅ 80% coverage for server functions
- ✅ All critical flows have E2E tests
- ✅ Test data cleanup procedures defined

---

### TODO 3: Observability Framework (P2, Pre-Implementation)

**Status:** ⏳ Pending  
**Location:** `server_code/server_shared/`

**Tasks:**
1. Create `server_code/server_shared/logging_config.py`
   - Configure structlog for structured JSON logging
   - Create `get_logger()` helper function
   - Define log format with required fields

2. Create `server_code/server_shared/metrics.py`
   - Define metrics counters (bookings_created, payments_processed, etc.)
   - Define metrics histogram (api_latency_seconds)
   - Create `track_metrics()` decorator

3. Create `server_code/server_shared/alerts.py`
   - Define alert thresholds
   - Create `check_alerts()` function
   - Create `send_alert()` function for email/Slack

4. Update all server functions to include logging at entry/exit

5. Create `docs/observability-requirements.md`
   - Document logging standards
   - Document metrics definitions
   - Document alert thresholds
   - Document log retention policy (2 years)

**Completion Criteria:**
- ✅ All server functions have structured logging
- ✅ Metrics are being collected
- ✅ Alerts configured for critical failures
- ✅ Documentation complete

---

### TODO 4: Deployment Procedures (P2, Pre-Implementation)

**Status:** ⏳ Pending  
**Location:** `docs/`

**Tasks:**
1. Create `docs/deployment-procedures.md`
   - Document expand-and-contract migration pattern
   - Define feature flag strategy (10%, 50%, 100% rollout)
   - Create post-deploy verification checklist
   - Document rollback procedures (Git, database, feature flags)

2. Create `scripts/migrate.py`
   - Implement migration up command
   - Implement migration down command
   - Implement migration status command
   - Support rollback to specific version

3. Define `feature_flags` table schema in `authoritative-schema.md`

4. Create `docs/rollback-procedures.md`
   - Git revert procedure
   - Database rollback procedure
   - Feature flag disable procedure
   - Post-rollback verification checklist

**Completion Criteria:**
- ✅ Migration procedures documented and tested
- ✅ Feature flag strategy defined
- ✅ Rollback procedures tested in staging
- ✅ Post-deploy verification checklist created

---

## Phase 1: Foundation

### TODO 5: Authentication System

**Status:** ⏳ Pending  
**Location:** `server_code/server_auth/`

**Tasks:**
1. Update `users` table schema in `authoritative-schema.md`
   - Add `role` field (owner, manager, admin, staff, customer)
   - Add `account_status` field (active, suspended, deleted)
   - Add `permissions` field (simpleObject)
   - Add `email_confirmation_key` field
   - Add `mfa` field (simpleObject)

2. Create `server_code/server_auth/rbac.py`
   - Define `@require_owner()` decorator
   - Define `@require_manager()` decorator
   - Define `@require_admin()` decorator
   - Define `@require_staff()` decorator
   - Define `@require_any_role()` decorator

3. Create `server_code/server_auth/service.py`
   - Implement `register_user()` function
   - Implement `login_user()` function
   - Implement `logout_user()` function
   - Implement `reset_password()` function
   - Implement `confirm_email()` function
   - Implement `resend_email_confirmation()` function

4. Create unit tests for all authentication functions

**Completion Criteria:**
- ✅ All server functions have RBAC decorators
- ✅ Authentication flows working
- ✅ RBAC enforcement verified

---

### TODO 6: Settings & Configuration

**Status:** ⏳ Pending  
**Location:** `server_code/server_settings/`

**Tasks:**
1. Update `business_profile` table schema
   - Add `system_currency` field (ISO 4217)
   - Add `timezone` field (IANA string)
   - Add `logo` field (media)

2. Update `config` table schema
   - Add `category` field
   - Add index on `key` field

3. Create `server_code/server_settings/service.py`
   - Implement `get_business_profile()` function
   - Implement `update_business_profile()` function
   - Implement `get_config()` function
   - Implement `update_config()` function
   - Implement `set_system_currency()` function
   - Implement `set_timezone()` function

4. Implement onboarding flow to set system_currency and timezone
   - system_currency immutable after first transaction
   - timezone set during onboarding

**Completion Criteria:**
- ✅ Business profile can be configured
- ✅ System currency set at onboarding
- ✅ Timezone configured for display conversion

---

### TODO 7: Vault Integration

**Status:** ⏳ Pending  
**Location:** `server_code/server_settings/`

**Tasks:**
1. Update `vault` table schema in `authoritative-schema.md`
   - Verify `name` field is indexed and unique
   - Verify `value` field stores encrypted data
   - Add `updated_by` field (link → users)

2. Create `server_code/server_settings/vault_totp_service.py`
   - Implement TOTP generation
   - Implement TOTP verification
   - Implement TOTP registration

3. Update `vault_service.py` to integrate with TOTP
   - Require TOTP step-up for Vault access
   - Send email notification on Vault access

4. Create unit tests for TOTP service

**Completion Criteria:**
- ✅ Vault requires TOTP step-up on every access
- ✅ Email notification sent on Vault access
- ✅ No grace period — TOTP required each time

---

### TODO 8: Database Indexes

**Status:** ⏳ Pending  
**Location:** Database tables

**Tasks:**
1. Add index on `contacts.email`
2. Add index on `bookings.contact_id`
3. Add index on `bookings.service_id`
4. Add index on `bookings.start_datetime`

**Completion Criteria:**
- ✅ All indexes created
- ✅ Query performance verified with EXPLAIN

---

## Phase 2-5: Feature Implementation

### TODO 9: Services Module
**Tasks:**
- Implement service CRUD operations
- Implement service category management
- Implement provider assignment
- Implement meeting type configuration

### TODO 10: Bookings Module
**Tasks:**
- Implement booking creation flow
- Implement availability management
- Implement recurring appointments
- Implement video call link generation

### TODO 11: Payments Module
**Tasks:**
- Implement Stripe payment processing
- Implement Paystack payment processing
- Implement webhook signature verification
- Implement webhook idempotency handling

### TODO 12: CRM Module
**Tasks:**
- Implement contact CRUD operations
- Implement contact activity timeline
- Implement tags and segmentation
- Implement CSV import/export

### TODO 13: Email Module
**Tasks:**
- Implement Brevo SMTP integration
- Implement Brevo Campaigns integration
- Implement campaign sequence processing
- Implement webhook handling for opens/clicks

### TODO 14: Blog Module
**Tasks:**
- Implement blog post CRUD operations
- Implement rich text editor integration
- Implement SEO field management
- Implement view count tracking

### TODO 15: Public Pages Module
**Tasks:**
- Implement homepage with 4 templates
- Implement About, Contact, Privacy, Terms pages
- Implement landing pages (3 templates)
- Implement lead capture with simultaneous contacts creation

### TODO 16: Analytics Module
**Tasks:**
- Implement dashboard metrics
- Implement revenue reporting
- Implement booking analytics
- Implement customer analytics

---

## Multi-Currency Implementation (ADR-012)

### TODO 17: System Currency
**Status:** ⏳ Pending  
**Tasks:**
1. Add `system_currency` field to `business_profile` table
2. Implement onboarding flow to set system_currency
3. Make system_currency immutable after first transaction
4. Add `local_currency_price` and `local_currency` to `services` table
5. Implement currency conversion logic (Option A: Fixed rates for V1)

---

## Timezone Implementation (ADR-005)

### TODO 18: UTC Storage Pattern
**Status:** ⏳ Pending  
**Tasks:**
1. Add `timezone` field to `business_profile` table (IANA string)
2. Ensure all datetimes stored in UTC
3. Create timezone conversion helper function
4. Update all server functions to convert UTC to local timezone at display

---

## Task Execution Notes

- **Read ADRs first:** Always read relevant ADRs before implementing
- **Follow patterns:** Use existing code patterns from scaffold
- **Test thoroughly:** Write tests for all new functionality
- **Document decisions:** Add comments for non-obvious implementation choices
- **Reference CEO review:** Use CEO review findings to guide priorities

---

*End of file — 07-programmatic-task-list.md*
