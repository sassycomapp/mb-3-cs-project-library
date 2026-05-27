# Mybizz CS — Phase 1 Foundation Implementation Plan

**Date:** 2026-05-26  
**Status:** In Progress  
**Phase:** 1 — Foundation  
**Goal:** Establish authentication, navigation, settings, and the Vault

---

## Current Status

| Stage | Status | Notes |
|-------|--------|-------|
| **1.1 Project Infrastructure** | ✅ Complete | M3 theme, routing, GitHub sync verified |
| **1.2 Authentication System** | 🔄 In Progress | Email confirmation, RBAC, session management |
| **1.3 Dashboard & Navigation** | ⏸️ Pending | Awaits auth completion |
| **1.4 Settings & Configuration** | ⏸️ Pending | Awaits auth completion |
| **1.5 The Vault** | ⏸️ Pending | Awaits auth completion |
| **1.6 AdminLayout Navigation** | ⏸️ Pending | Awaits dashboard completion |

---

## Phase 1 Implementation Details

### Stage 1.2: Authentication System

**Deliverables:**
- `users` table extended with `role`, `permissions`, `account_status`, `signed_up`, `email_confirmation_key`, `mfa` fields
- `LoginForm` — email/password login with validation
- `SignupForm` — new user registration with business name capture
- `PasswordResetForm` — password reset flow
- `server_auth/service.py` — authentication server functions (email confirmation, rate limiting, session management)
- `server_auth/rbac.py` — role-based access control decorators (full coverage)
- `tests/test_auth_service.py` — auth unit tests
- `tests/test_rbac.py` — RBAC unit tests

**Completion Criteria:**
- ✅ Users can register with email/password
- ✅ Email confirmation flow works (Brevo SMTP, expiration handling, resend capability)
- ✅ Password reset flow works
- ✅ Five roles defined: Owner, Manager, Admin, Staff, Customer
- ✅ RBAC decorators functional on ALL server functions (TODO 11 verified)
- ✅ Rate limiting implemented (10 req/min unauthenticated, 100 req/min authenticated) using `rate_limits` table
- ✅ Session management working (30-minute inactivity timeout, "Remember me" V1 feature)
- ✅ Audit logging for authentication events
- ✅ All unit tests passing

**Implementation Notes:**
- Email confirmation uses Brevo SMTP (configured in Phase 0)
- Rate limiting uses `rate_limits` table for persistence across server restarts
- Session timeout is 30 minutes inactivity, with "Remember me" option for extended sessions (V1 feature)
- Full RBAC coverage required on all server functions, not just critical endpoints

---

### Stage 1.3: Dashboard & Navigation

**Deliverables:**
- `AdminLayout` — custom HtmlTemplate layout with navigation
- `DashboardForm` — main admin dashboard with metrics
- Navigation structure implemented (Dashboard, Sales & Operations, Customers & Marketing, Content & Website, Finance & Reports, Settings)
- `shared/navigation_helpers.py` — navigation utility functions (lambda click handlers, open_form wrappers)

**Completion Criteria:**
- ✅ AdminLayout renders with M3 styling
- ✅ Navigation sidebar functional with all items
- ✅ Lambda click handlers working with variable capture
- ✅ Dashboard displays metrics (revenue, bookings, customers — initially static)
- ✅ Navigation visibility respects RBAC (Settings Owner/Manager only)
- ✅ Mobile responsive layout working
- ✅ Sign out link present at bottom of sidebar

**Implementation Notes:**
- Navigation helpers prevent lambda capture bugs and ensure consistent patterns
- AdminLayout uses custom HtmlTemplate with plain Link components (not NavigationLink)
- Navigation structure follows the spec in architecture.md

---

### Stage 1.4: Settings & Configuration

**Deliverables:**
- `business_profile` table populated with business information
- `config` table for key-value configuration
- `email_config` table for email settings
- `payment_config` table for payment gateway settings
- `theme_config` table for M3 theme customization
- `SettingsForm` — main settings form with sub-sections:
  - `BusinessProfileForm` — business profile section
  - `FeatureFlagsForm` — feature activation section
  - `ThemeConfigForm` — theme customization section
  - `CurrencyForm` — currency selection section
  - `UserManagementForm` — users & permissions section
- `tests/test_settings.py` — settings unit tests

**Completion Criteria:**
- ✅ Business profile can be saved and retrieved
- ✅ Feature flags functional (bookings_enabled, services_enabled, etc.)
- ✅ Theme customization persists and applies correctly
- ✅ Currency selection working (immutable after first transaction)
- ✅ User management functional (invite, edit role, deactivate)
- ✅ All config tables initialized via `create_initial_config()`
- ✅ All unit tests passing

**Implementation Notes:**
- SettingsForm uses sub-sections for better maintainability
- Currency becomes immutable after first transaction
- Feature flags control navigation item visibility

---

### Stage 1.5: The Vault

**Deliverables:**
- `vault` table created with encrypted secrets storage
- `encryption_key` set in Anvil Secrets
- `server_shared/encryption_service.py` — encryption/decryption functions (Phase 0 complete)
- `server_shared/vault_service.py` — Vault CRUD functions (Phase 0 complete)
- `server_shared/vault_totp_service.py` — TOTP step-up authentication
- `settings/VaultForm` — owner-only Vault UI with TOTP gate
- `tests/test_vault.py` — vault unit tests

**Completion Criteria:**
- ✅ `encryption_key` set in Anvil Secrets (only item there)
- ✅ VaultForm requires TOTP step-up on every open
- ✅ No grace period — TOTP required each time
- ✅ Email notification sent to Owner on every Vault access
- ✅ Secret keys stored encrypted in `vault` table
- ✅ `get_vault_secret()` retrieves secrets correctly (TODO 1: Vault error handling implemented)
- ✅ Payment config uses Vault for secret keys (not stored in `payment_config` table)
- ✅ Secret masking in `get_payment_config()` returns `'***'` for secrets
- ✅ All unit tests passing

**Implementation Notes:**
- TOTP step-up required on EVERY Vault access (no grace period)
- Email notification sent to Owner on every Vault access
- Uses encryption_service.py and vault_service.py from Phase 0

---

### Stage 1.6: AdminLayout Navigation Refinement

**Deliverables:**
- Finalized navigation structure with feature flag visibility
- CustomerLayout created (deferred to V2 — stub for now)
- Navigation consistency verified across all forms

**Completion Criteria:**
- ✅ All navigation items have correct visibility rules
- ✅ Feature flags control navigation item visibility
- ✅ Navigation works correctly on mobile
- ✅ Active link highlighting working
- ✅ No navigation errors or broken links

**Implementation Notes:**
- CustomerLayout is a stub (deferred to V2)
- Navigation visibility controlled by feature flags
- Mobile responsive navigation tested

---

## Test Coverage Requirements

### Auth System Tests (`tests/test_auth_service.py`)
```python
test_login_success
test_login_invalid_password
test_login_rate_limiting
test_signup_success
test_signup_duplicate_email
test_password_reset_request
test_password_reset_success
test_email_confirmation
test_email_confirmation_expired
test_email_confirmation_invalid_token
```

### RBAC Tests (`tests/test_rbac.py`)
```python
test_owner_can_access_all
test_manager_can_access_admin
test_admin_can_access_settings
test_staff_cannot_access_settings
test_customer_cannot_access_admin
test_rate_limiting_unauthenticated
test_rate_limiting_authenticated
```

### Vault Tests (`tests/test_vault.py`)
```python
test_vault_totp_required
test_vault_email_notification
test_vault_secret_encryption
test_vault_secret_decryption
test_vault_secret_not_found
```

---

## Engineering Decisions

### Decision 1: Email Confirmation Flow
**Status:** ✅ Approved  
**Implementation:** Full email confirmation with Brevo SMTP (2-3 hours)  
**Rationale:** Security best practice, prevents account takeover, enables password reset

### Decision 2: RBAC Decorator Coverage
**Status:** ✅ Approved  
**Implementation:** Full coverage on ALL server functions  
**Rationale:** Complete security coverage, aligns with build-plan.md requirements

### Decision 3: Vault TOTP Implementation
**Status:** ✅ Approved  
**Implementation:** Full TOTP + email notifications on every Vault access  
**Rationale:** Owner-only security with audit trail, prevents unauthorized access

### Decision 4: SettingsForm Structure
**Status:** ✅ Approved  
**Implementation:** SettingsForm with sub-sections (BusinessProfileForm, FeatureFlagsForm, ThemeConfigForm, CurrencyForm, UserManagementForm)  
**Rationale:** Better organization, easier to maintain, follows single responsibility

### Decision 5: Navigation Helpers
**Status:** ✅ Approved  
**Implementation:** Create navigation_helpers.py upfront with lambda click handlers, open_form wrappers  
**Rationale:** Prevents lambda capture bugs, consistent patterns, easier maintenance

### Decision 6: Rate Limiting Implementation
**Status:** ✅ Approved  
**Implementation:** Use `rate_limits` table for all rate limiting  
**Rationale:** Persistent across server restarts, security-critical

### Decision 7: Session Management
**Status:** ✅ Approved  
**Implementation:** 30-minute inactivity timeout + "Remember me" V1 feature  
**Rationale:** Security best practice with UX consideration for returning users

---

## Dependencies

### Phase 0 Dependencies (Complete)
- ✅ Vault error handling (TODO 1)
- ✅ Test coverage requirements (TODO 2)
- ✅ Observability requirements (TODO 3)
- ✅ Deployment procedures (TODO 4)

### Phase 1 Internal Dependencies
- Stage 1.2 → Stage 1.3, 1.4, 1.5 (Auth must complete first)
- Stage 1.3 → Stage 1.6 (Dashboard must complete first)
- Stage 1.4 → None (can run in parallel with 1.5)
- Stage 1.5 → None (can run in parallel with 1.4)

---

## Implementation Order

**Recommended Order:**
1. Stage 1.2: Authentication System (critical path)
2. Stage 1.4: Settings & Configuration (can run in parallel with 1.5)
3. Stage 1.5: The Vault (can run in parallel with 1.4)
4. Stage 1.3: Dashboard & Navigation (depends on auth)
5. Stage 1.6: AdminLayout Navigation Refinement (depends on dashboard)

**Estimated Timeline:**
- Stage 1.2: 6-8 hours (auth + email + RBAC + tests)
- Stage 1.4: 4-6 hours (settings + tests)
- Stage 1.5: 3-4 hours (vault + TOTP + tests)
- Stage 1.3: 4-6 hours (dashboard + navigation + helpers)
- Stage 1.6: 2-3 hours (navigation refinement)

**Total Estimated Time:** 19-27 hours

---

## Next Steps

1. **Begin Stage 1.2 Implementation:**
   - Implement `server_auth/service.py` with email confirmation
   - Implement `server_auth/rbac.py` with full decorator coverage
   - Create auth tests
   - Create RBAC tests

2. **Begin Stage 1.4 & 1.5 (Parallel):**
   - Implement SettingsForm with sub-sections
   - Implement VaultForm with TOTP
   - Create settings tests
   - Create vault tests

3. **Begin Stage 1.3:**
   - Implement AdminLayout
   - Implement DashboardForm
   - Create navigation_helpers.py
   - Create navigation tests

4. **Complete Stage 1.6:**
   - Finalize navigation structure
   - Verify all navigation items
   - Test mobile navigation

5. **Update Documentation:**
   - Update devlog.md with progress
   - Update build-plan.md with completion status

---

## Notes

- All files mentioned in this plan exist as stubs or directories
- Actual implementation will happen in the build phase
- Phase 0 work (vault_service.py, encryption_service.py, exceptions.py) is already complete
- SettingsForm and VaultForm directories exist, ready for implementation
- navigation_helpers directory exists, ready for implementation

---

*End of file — Phase 1 Implementation Plan*
