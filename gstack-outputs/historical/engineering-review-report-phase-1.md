# Mybizz CS — Engineering Review Report

**Date:** 2026-05-26  
**Review Type:** Engineering Review (Phase 1 Foundation)  
**Branch:** N/A (project-library documentation review)  
**Reviewer:** Build Agent (gstack)

---

## Executive Summary

**Verdict:** ✅ **APPROVED** — All architectural decisions validated, ready for implementation

The Phase 1 Foundation engineering review identified 7 key issues and made 7 corresponding decisions. All decisions have been approved and documented. The implementation plan is complete with test coverage requirements, dependencies, and estimated timeline.

**Engineering Review Rating:** 8/10 → 9/10 (after fixes)

**Screens Reviewed:**
- Auth screens (LoginForm, SignupForm, PasswordResetForm)
- Dashboard (main admin dashboard with metrics)
- Settings (business profile, feature flags, theme customization)
- Vault (owner-only encrypted secrets store with TOTP gate)
- Navigation (AdminLayout with sidebar navigation)

---

## Review Scope

### Documents Reviewed

| Document | Status | Notes |
|----------|--------|-------|
| `build-plan.md` | ✅ Reviewed | 6-phase sequence validated |
| `authoritative-schema.md` | ✅ Reviewed | 36 tables validated |
| `architecture.md` | ✅ Reviewed | Dependency model validated |
| `design-direction.md` | ✅ Reviewed | M3 design validated |
| `phase-0-implementation.md` | ✅ Reviewed | TODOs validated |
| `devlog.md` | ✅ Reviewed | Progress tracking validated |
| `server_auth/service.py` | ⚠️ Empty stub | Needs full implementation |
| `server_auth/rbac.py` | ⚠️ Empty stub | Needs full implementation |
| `client_code/settings/` | ✅ Directory exists | Ready for implementation |
| `client_code/shared/navigation_helpers` | ✅ Directory exists | Ready for implementation |

### Issues Identified

| Issue | Severity | Status | Resolution |
|-------|----------|--------|------------|
| Email confirmation flow | P2 | ✅ Resolved | Implement with Brevo SMTP |
| RBAC decorator coverage | P1 | ✅ Resolved | Full coverage on all server functions |
| Vault TOTP implementation | P1 | ✅ Resolved | Full TOTP + email notifications |
| SettingsForm structure | P2 | ✅ Resolved | Create with sub-sections |
| Navigation helpers | P1 | ✅ Resolved | Create upfront |
| Rate limiting implementation | P2 | ✅ Resolved | Use rate_limits table |
| Session management | P2 | ✅ Resolved | 30-minute timeout + "Remember me" |

---

## Detailed Findings

### Issue 1: Email Confirmation Flow (P2, confidence: 9/10)

**Finding:** `server_auth/service.py` — No email confirmation flow specified

**Current Plan:**
- `users` table extended with `email_confirmation_key` field
- LoginForm, SignupForm, PasswordResetForm exist
- Email confirmation mentioned in completion criteria

**Problem:**
The build-plan.md mentions email confirmation but doesn't specify:
- How the confirmation email is sent (Brevo SMTP?)
- What happens if the confirmation link expires
- What happens if the user requests a new confirmation link
- Rate limiting on confirmation requests

**Tradeoffs:**
- **Option A:** Implement full email confirmation flow with Brevo SMTP
  - ✅ Complete, production-ready authentication
  - ✅ Follows security best practices
  - ❌ Adds 2-3 hours of implementation
- **Option B:** Skip email confirmation for V1
  - ✅ Faster implementation (saves 2-3 hours)
  - ❌ Security risk (no email verification)
  - ❌ Poor user experience (no password reset without email)

**Decision:** ✅ **Option A** — Implement full email confirmation flow with Brevo SMTP

**Rationale:** The 2-3 hours investment prevents security vulnerabilities and provides a complete authentication experience. Email confirmation is a one-way door decision — once implemented, it's hard to add later without data migration.

**User Confirmation:** Approved by user

---

### Issue 2: RBAC Decorator Coverage (P1, confidence: 10/10)

**Finding:** `server_auth/rbac.py` — RBAC coverage not specified

**Current Plan:**
- Build-plan.md requires "All server functions have correct RBAC decorators verified" (TODO 11)
- No implementation details provided

**Problem:**
The requirement states "all server functions" but doesn't clarify:
- Does this mean ALL functions or just critical endpoints?
- What happens if a function is missed?
- How to verify coverage?

**Tradeoffs:**
- **Option A:** Implement RBAC for ALL server functions
  - ✅ Complete security coverage
  - ✅ Aligns with build-plan.md requirements
  - ❌ More implementation work upfront
- **Option B:** Implement RBAC only for critical endpoints
  - ✅ Faster implementation
  - ❌ Security gaps in non-critical endpoints
  - ❌ Violates build-plan.md requirement

**Decision:** ✅ **Option A** — Implement RBAC for ALL server functions

**Rationale:** The build-plan.md explicitly states "All server functions have correct RBAC decorators verified." This is a security-critical requirement that cannot be compromised. Full coverage ensures no security gaps and aligns with the documented requirements.

**User Confirmation:** Approved by user

---

### Issue 3: Vault TOTP Implementation (P1, confidence: 10/10)

**Finding:** `settings/VaultForm` — TOTP step-up not specified

**Current Plan:**
- Build-plan.md requires "VaultForm requires TOTP step-up on every open"
- Build-plan.md requires "Email notification sent to Owner on every Vault access"
- No implementation details provided

**Problem:**
The requirements mention TOTP and email notifications but don't specify:
- How TOTP is implemented (Google Authenticator? Authy?)
- What happens if TOTP fails
- How email notifications are sent
- Can TOTP be disabled?

**Tradeoffs:**
- **Option A:** Implement full TOTP + email notifications
  - ✅ Complete security with audit trail
  - ✅ Follows security best practices
  - ❌ Adds 1-2 hours of implementation
- **Option B:** Simplify Vault access to just owner-only without TOTP
  - ✅ Faster implementation
  - ❌ Security risk (no 2FA)
  - ❌ Violates build-plan.md requirement

**Decision:** ✅ **Option A** — Implement full TOTP + email notifications on every Vault access

**Rationale:** The Vault contains sensitive API keys and secrets. TOTP step-up on every access ensures that even if the owner's session is compromised, the attacker cannot access the Vault without the TOTP code. Email notifications provide an audit trail for security monitoring.

**User Confirmation:** Approved by user

---

### Issue 4: SettingsForm Structure (P2, confidence: 9/10)

**Finding:** `client_code/settings/` — Form structure not specified

**Current Plan:**
- Build-plan.md requires `SettingsForm` with sections: Business Profile, Feature Activation, Theme Customization, Currency, Users & Permissions
- Directory exists with sub-forms: BusinessProfileForm, FeatureFlagsForm, ThemeConfigForm, CurrencyForm, UserManagementForm, EmailSetupForm, PaymentGatewayForm, SettingsForm, VaultForm

**Problem:**
The requirements mention "sections" but don't specify:
- Should SettingsForm be a single large form?
- Should it use sub-sections/components?
- How to handle navigation between sections?

**Tradeoffs:**
- **Option A:** Create SettingsForm as a single large form (1000+ lines)
  - ✅ Faster implementation
  - ❌ Hard to maintain, violates single responsibility
  - ❌ Difficult to test
- **Option B:** Create SettingsForm with sub-sections
  - ✅ Better organization, easier to maintain
  - ✅ Follows single responsibility principle
  - ❌ Slightly more setup time (+30 min)

**Decision:** ✅ **Option B** — Create SettingsForm with sub-sections

**Rationale:** SettingsForm with sub-sections (BusinessProfileForm, FeatureFlagsForm, ThemeConfigForm, CurrencyForm, UserManagementForm) follows the three-tier module pattern and makes the codebase maintainable. Each sub-section can be tested independently and updated without affecting others. The 30-minute upfront investment pays off in fewer bugs and easier future changes.

**User Confirmation:** Approved by user

---

### Issue 5: Navigation Helpers (P1, confidence: 9/10)

**Finding:** `shared/navigation_helpers.py` — Not implemented

**Current Plan:**
- Build-plan.md mentions `shared/navigation_helpers.py` in Stage 1.3
- Directory exists at `client_code/shared/navigation_helpers/`
- No implementation details provided

**Problem:**
The requirements mention navigation helpers but don't specify:
- What functions should be included?
- How to prevent lambda capture bugs?
- When to use helpers vs inline code?

**Tradeoffs:**
- **Option A:** Create navigation helpers now
  - ✅ Consistent navigation patterns across app
  - ✅ Prevents lambda capture bugs
  - ❌ +1 hour upfront cost
- **Option B:** Add navigation helpers later if needed
  - ✅ Saves time initially
  - ❌ Risk of inconsistent navigation patterns
  - ❌ Harder to fix later if bugs found

**Decision:** ✅ **Option A** — Create navigation helpers upfront

**Rationale:** Navigation helpers prevent common Anvil navigation bugs (lambda capture issues, inconsistent patterns) and make the codebase more maintainable. The 1-hour investment pays off in fewer bugs and easier future navigation changes. Common patterns include:
- Lambda click handlers with variable capture
- `open_form()` wrappers
- Navigation visibility helpers

**User Confirmation:** Approved by user

---

### Issue 6: Rate Limiting Implementation (P2, confidence: 8/10)

**Finding:** `server_auth/service.py` — Rate limiting not specified

**Current Plan:**
- Build-plan.md requires "Rate limiting implemented (10 req/min unauthenticated, 100 req/min authenticated)"
- `rate_limits` table exists in schema
- No implementation details provided

**Problem:**
The requirements mention rate limiting but don't specify:
- Where to store rate limit data (table vs cache)?
- How to handle rate limit violations?
- Should rate limits persist across server restarts?

**Tradeoffs:**
- **Option A:** Use `rate_limits` table for all rate limiting
  - ✅ Persistent across sessions
  - ✅ Can query rate limit history
  - ❌ Database overhead on every request
- **Option B:** Use in-memory cache + periodic DB sync
  - ✅ Faster (no DB on every request)
  - ❌ Lost on server restart
  - ❌ More complex implementation

**Decision:** ✅ **Option A** — Use `rate_limits` table

**Rationale:** The database overhead is minimal for auth endpoints (10-100 req/min), and persistent rate limiting is critical for security (prevents rate limit bypass on server restart). The `rate_limits` table is already defined in the schema, making this the natural choice.

**User Confirmation:** Approved by user

---

### Issue 7: Session Management (P2, confidence: 8/10)

**Finding:** `server_auth/service.py` — Session timeout not specified

**Current Plan:**
- Build-plan.md requires "Session management working (30-minute inactivity timeout)"
- No implementation details provided

**Problem:**
The requirements mention 30-minute timeout but don't specify:
- Should there be a "Remember me" option?
- How to handle session expiration?
- Should users be warned before timeout?

**Tradeoffs:**
- **Option A:** 30-minute inactivity timeout (as specified)
  - ✅ Secure, follows security best practices
  - ❌ Users may be logged out during long sessions
- **Option B:** 24-hour session with "Remember me" option
  - ✅ Better UX for returning users
  - ❌ Less secure, requires additional implementation

**Decision:** ✅ **Option A** — 30-minute inactivity timeout + "Remember me" V1 feature

**Rationale:** 30-minute inactivity timeout is a security-critical decision that follows best practices for business applications. The "Remember me" option (V1 feature) provides a compromise for users who need longer sessions. This balances security with user experience.

**User Confirmation:** Approved by user

---

## Summary of Engineering Review

### Issues Identified: 7
### Issues Resolved: 7

| Issue | Severity | Confidence | Status | Decision |
|-------|----------|------------|--------|----------|
| Email confirmation flow | P2 | 9/10 | ✅ Resolved | Option A (Brevo SMTP) |
| RBAC decorator coverage | P1 | 10/10 | ✅ Resolved | Option A (Full coverage) |
| Vault TOTP implementation | P1 | 10/10 | ✅ Resolved | Option A (Full TOTP + email) |
| SettingsForm structure | P2 | 9/10 | ✅ Resolved | Option B (Sub-sections) |
| Navigation helpers | P1 | 9/10 | ✅ Resolved | Option A (Create upfront) |
| Rate limiting implementation | P2 | 8/10 | ✅ Resolved | Option A (rate_limits table) |
| Session management | P2 | 8/10 | ✅ Resolved | Option A (30-min + Remember me) |

### Engineering Rating: 8/10 → 9/10 (after fixes)

### Files Updated

| File | Status | Changes |
|------|--------|---------|
| `docs/build-plan.md` | ✅ Updated | Stage 1.2, 1.3, 1.4 implementation details |
| `docs/devlog.md` | ✅ Updated | Phase 1 status, engineering decisions |
| `docs/phase-1-implementation-plan.md` | ✅ Created | Detailed implementation plan |

### Test Coverage Requirements

**Auth System Tests (`tests/test_auth_service.py`):**
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

**RBAC Tests (`tests/test_rbac.py`):**
```python
test_owner_can_access_all
test_manager_can_access_admin
test_admin_can_access_settings
test_staff_cannot_access_settings
test_customer_cannot_access_admin
test_rate_limiting_unauthenticated
test_rate_limiting_authenticated
```

**Vault Tests (`tests/test_vault.py`):**
```python
test_vault_totp_required
test_vault_email_notification
test_vault_secret_encryption
test_vault_secret_decryption
test_vault_secret_not_found
```

**Settings Tests (`tests/test_settings.py`):**
```python
test_business_profile_save
test_feature_flags_toggle
test_theme_customization
test_currency_selection
test_user_management
```

**Navigation Tests (`tests/test_navigation.py`):**
```python
test_lambda_capture
test_open_form
test_navigation_visibility
test_mobile_navigation
```

---

## Implementation Plan

### Recommended Order

1. **Stage 1.2: Authentication System** (6-8 hours) - Critical path
   - Implement `server_auth/service.py` with email confirmation
   - Implement `server_auth/rbac.py` with full decorator coverage
   - Create auth tests
   - Create RBAC tests

2. **Stage 1.4: Settings & Configuration** (4-6 hours) - Can run in parallel with 1.5
   - Implement SettingsForm with sub-sections
   - Create settings tests

3. **Stage 1.5: The Vault** (3-4 hours) - Can run in parallel with 1.4
   - Implement VaultForm with TOTP
   - Create vault tests

4. **Stage 1.3: Dashboard & Navigation** (4-6 hours) - Depends on auth
   - Implement AdminLayout
   - Implement DashboardForm
   - Create navigation_helpers.py
   - Create navigation tests

5. **Stage 1.6: AdminLayout Navigation Refinement** (2-3 hours) - Depends on dashboard
   - Finalize navigation structure
   - Verify all navigation items
   - Test mobile navigation

### Estimated Timeline

| Stage | Estimated Time | Dependencies |
|-------|----------------|--------------|
| 1.2: Authentication | 6-8 hours | Phase 0 complete |
| 1.4: Settings | 4-6 hours | Phase 1.2 complete |
| 1.5: Vault | 3-4 hours | Phase 1.2 complete |
| 1.3: Dashboard | 4-6 hours | Phase 1.2 complete |
| 1.6: Navigation | 2-3 hours | Phase 1.3 complete |
| **Total** | **19-27 hours** | |

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

## Recommendations

### Immediate Actions (Completed):
1. ✅ Updated `build-plan.md` with implementation details
2. ✅ Updated `devlog.md` with Phase 1 status
3. ✅ Created `phase-1-implementation-plan.md`
4. ✅ All 7 engineering decisions approved

### Next Steps:
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

## Engineering Review Metrics

| Metric | Value |
|--------|-------|
| **Screens Reviewed** | 5 (Auth, Dashboard, Settings, Vault, Navigation) |
| **Issues Identified** | 7 |
| **Issues Resolved** | 7 (100%) |
| **Decisions Made** | 7 |
| **Files Updated** | 3 (build-plan.md, devlog.md, phase-1-implementation-plan.md) |
| **Test Cases Defined** | 25+ |
| **Estimated Implementation Time** | 19-27 hours |
| **Overall Verdict** | ✅ APPROVED |

---

## Sign-Off

**Engineering Review Completed:** 2026-05-26  
**Review Status:** ✅ APPROVED  
**Implementation Status:** Ready to begin  
**Next Review:** After Phase 1 implementation completion  
**Next Skill:** `/design-html` (after Phase 1 implementation)

---

*End of file — Engineering Review Report*
