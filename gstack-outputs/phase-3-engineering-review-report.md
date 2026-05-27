# Mybizz CS — Phase 3 Engineering Review Report

**Date:** 2026-05-26  
**Review Type:** Engineering Review (Phase 3 CRM + Email)  
**Branch:** N/A (project-library documentation review)  
**Reviewer:** Build Agent (gstack)

---

## Executive Summary

**Verdict:** ✅ **APPROVED** — All architectural decisions validated, ready for implementation

Phase 3 Engineering Review completed for CRM + Email. 2 key decisions made and approved:

1. ✅ **Brevo API rate limiting** — Full queue-based rate limiting (2-3 hours)
2. ✅ **Contact deduplication** — Email-based deduplication with manual merge (3-4 hours)

**Phase Scope:** 5 Stages (Contacts, Campaigns, Segments, Tasks, Landing Pages)  
**Files to Implement:** ~10-12 files  
**Estimated Implementation Time:** 20-30 hours

---

## Issues Found & Decisions Made

### Issue 1: Brevo API Rate Limiting ✅ DECIDED

**Finding:** [P2] (confidence: 9/10) `server_marketing/brevo_campaigns_integration.py` — No rate limiting pattern specified

**Problem:**
Brevo has API rate limits (typically 100 requests/minute for free tier, higher for paid). Without rate limiting:
- API calls rejected with 429 errors
- Campaign processing failures
- Contact enrollment failures
- Unsubscribe handling failures

**Decision:** **Full queue-based rate limiting** (2-3 hours)

**Rationale:** Email marketing is a core feature — if campaigns fail, customers lose trust. Rate limiting is a two-way door decision (can be added later, but better to get it right from day one).

**Implementation Plan:**
- Implement message queue for Brevo API calls (Redis or Anvil database queue)
- Rate limiter: 100 requests/minute burst, 50 requests/minute sustained
- Retry logic with exponential backoff (max 3 retries)
- Webhook handling with idempotency (similar to Phase 2 payments)
- Background task for campaign processing with progress tracking
- Error logging and alerting for API failures

---

### Issue 2: Contact Deduplication ✅ DECIDED

**Finding:** [P2] (confidence: 9/10) `server_customers/contact_service.py` — Deduplication strategy not specified

**Problem:**
Build-plan.md requires "No duplicate contacts (email-based deduplication)" but doesn't specify implementation:
- How to detect duplicates?
- What happens when duplicates found?
- How to merge duplicate data?
- Should users be able to manually merge?

**Decision:** **Email-based deduplication with manual merge** (3-4 hours)

**Rationale:** Email-based deduplication is the most reliable method (emails are unique identifiers). Manual merge option gives users control over edge cases (e.g., same email for different people in same household).

**Implementation Plan:**
- Check for existing contact by email on creation
- If duplicate found:
  - Show merge dialog to user (manual merge)
  - Auto-merge if confidence > 90% (same name + phone)
  - Log all merge actions in `contact_events` table
- Merge strategy:
  - Keep most recent contact record
  - Combine activity history from both contacts
  - Combine tags and segments
  - Preserve all booking history
- Add `contacts.email` index for fast duplicate checking
- Add `contacts.email` unique constraint (with exception for null emails)

---

## Architecture Review Summary

**Issues Found:** 2  
**Issues Resolved:** 2 (all decisions approved)  
**Overall Architecture Rating:** 8/10 → 9/10 (after fixes)

### Section 2: Code Quality Review

**Phase 3 Deliverables Analysis:**

| Stage | Files | Complexity | DRY Violations | Error Handling |
|-------|-------|------------|----------------|----------------|
| 3.1 Contacts | 3 forms + 1 service | Medium | ✅ None | ✅ Complete |
| 3.2 Email Marketing | 3 forms + 1 service | High | ⚠️ Brevo duplication | ✅ Complete |
| 3.3 Segmentation | 1 form + 1 service | Medium | ✅ None | ✅ Complete |
| 3.4 Tasks | 1 form + 1 service | Low | ✅ None | ✅ Complete |
| 3.5 Landing Pages | 3 forms + 1 service | Medium | ✅ None | ✅ Complete |

**Recommendations:**
- ✅ **No DRY violations** — Each service has clear responsibility
- ⚠️ **Brevo duplication** — Consider abstracting common Brevo calls to `server_marketing/brevo_base.py`
- ✅ **Complete error handling** — All stages include error handling in completion criteria

---

### Section 3: Test Review

**Test Coverage Requirements for Phase 3:**

#### Stage 3.1: Contact Management Tests
```python
tests/test_contact_service.py:
test_contact_creation
test_contact_auto_creation_from_booking
test_contact_email_deduplication
test_contact_merge
test_contact_activity_timeline
test_contact_csv_import
test_contact_csv_export
test_contact_lifecycle_stages
test_contact_lifetime_value_calculation
```

#### Stage 3.2: Email Marketing Tests
```python
tests/test_campaign_service.py:
test_campaign_sequence_creation
test_campaign_contact_enrollment
test_campaign_broadcast
test_campaign_open_tracking
test_campaign_click_tracking
test_campaign_unsubscribe_handling
test_brevo_rate_limiting
test_brevo_retry_logic
```

#### Stage 3.3: Segmentation Tests
```python
tests/test_segment_service.py:
test_prebuilt_segments
test_custom_segment_builder
test_segment_contact_count_accuracy
test_segment_csv_export
test_segment_campaign_targeting
```

#### Stage 3.4: Tasks Tests
```python
tests/test_task_service.py:
test_manual_task_creation
test_auto_generated_tasks
test_task_completion_tracking
test_task_due_date_reminders
test_task_visibility_in_dashboard
```

#### Stage 3.5: Landing Pages Tests
```python
tests/test_landing_page_service.py:
test_landing_page_creation
test_lead_capture_flow
test_lead_to_contact_conversion
test_landing_page_analytics
test_landing_page_slug_uniqueness
test_welcome_campaign_auto_enrollment
```

**Total Test Cases:** ~25 test functions

---

### Section 4: Performance Review

**Performance Considerations for Phase 3:**

| Concern | Impact | Mitigation |
|---------|--------|------------|
| **Contact deduplication** | Medium | Email index + caching |
| **Brevo API calls** | High | Rate limiting + queue |
| **Segment calculations** | Medium | Cached segment counts |
| **Campaign processing** | High | Background task + batching |
| **Lead capture** | Low | Async contact creation |

**Recommendations:**
- ✅ **Contact deduplication** — Add `contacts.email` index for fast lookups
- ✅ **Brevo API calls** — Rate limiting with queue (100 req/min burst)
- ✅ **Segment calculations** — Cache segment counts, recalculate hourly
- ✅ **Campaign processing** — Background task with batching (100 contacts/batch)
- ✅ **Lead capture** — Async contact creation to avoid blocking

---

## Engineering Decisions Summary

| Decision | Status | Time Estimate |
|----------|--------|---------------|
| Brevo API rate limiting | ✅ Approved | 2-3 hours |
| Contact deduplication | ✅ Approved | 3-4 hours |
| Brevo abstraction (recommended) | ⏸️ Pending | 1 hour |
| Segment caching (recommended) | ⏸️ Pending | 30 min |

**Total Estimated Time:** 20-30 hours

---

## Test Coverage Diagram

```
Phase 3 Code Paths:
├── Contact Management (3.1)
│   ├── Contact creation (manual + auto)
│   ├── Email deduplication
│   ├── Contact merge
│   ├── Activity timeline
│   ├── CSV import/export
│   └── Lifecycle stages
│
├── Email Marketing (3.2)
│   ├── Campaign sequence creation
│   ├── Contact enrollment
│   ├── Campaign broadcast
│   ├── Open/click tracking
│   ├── Unsubscribe handling
│   └── Brevo API (rate limited)
│
├── Segmentation (3.3)
│   ├── Pre-built segments
│   ├── Custom segment builder
│   ├── Segment contact counts
│   ├── Segment CSV export
│   └── Segment campaign targeting
│
├── Tasks (3.4)
│   ├── Manual task creation
│   ├── Auto-generated tasks
│   ├── Task completion tracking
│   ├── Due date reminders
│   └── Dashboard visibility
│
└── Landing Pages (3.5)
    ├── Landing page creation
    ├── Lead capture
    ├── Lead to contact conversion
    ├── Landing page analytics
    ├── Slug uniqueness
    └── Welcome campaign enrollment
```

**Every branch in this diagram needs a test.**

---

## Data Flow Diagrams

### Contact Creation Flow
```
User/Booking/Form
    ↓
ContactService.create_contact()
    ↓
Check duplicate by email (with index)
    ↓
[Duplicate Found?] → Show merge dialog → Merge contacts
    ↓
[No Duplicate] → Create new contact
    ↓
Log contact_event (created)
    ↓
Return contact
```

### Campaign Processing Flow
```
Hourly background task
    ↓
Fetch active campaigns
    ↓
For each campaign:
    ↓
Get contacts in segment
    ↓
[Rate limit check] → Queue if exceeded
    ↓
Send email via Brevo API
    ↓
Log open/click events (webhooks)
    ↓
Update campaign stats
```

### Lead Capture Flow
```
Landing page form submission
    ↓
Validate form data
    ↓
Create lead record
    ↓
Create contact record (email deduplication)
    ↓
Set lead.converted_to_contact_id
    ↓
Enroll in welcome campaign
    ↓
Return success
```

---

## Next Steps

**Before Implementation:**
1. ✅ Brevo API rate limiting — Full queue-based rate limiting
2. ✅ Contact deduplication — Email-based with manual merge

**Implementation Order:**
1. Stage 3.1: Contact Management (5-7 hours)
2. Stage 3.2: Email Marketing (6-8 hours)
3. Stage 3.3: Segmentation (3-4 hours)
4. Stage 3.4: Tasks (2-3 hours)
5. Stage 3.5: Landing Pages (4-5 hours)

**Total Estimated Time:** 20-27 hours

---

## Sign-Off

**Engineering Review Completed:** 2026-05-26  
**Review Status:** ✅ APPROVED  
**Issues Found:** 2 (all resolved)  
**Files to Implement:** ~10-12 files  
**Estimated Implementation Time:** 20-30 hours  
**Next Review:** Phase 4 Engineering Review (Content + Website)

---

*End of file — Phase 3 Engineering Review Report*
