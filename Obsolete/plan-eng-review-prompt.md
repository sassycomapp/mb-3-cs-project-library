# /plan-eng-review Prompt — Post-Batch 1 Execution Readiness Lock

**Date:** 2026-06-14
**Context:** CEO review audit completed. Batch 1 (25 doc edits) applied. Batch 2 (Anvil Designer spec) produced. Schema now documents 43 tables. All ADR-034 through ADR-040 references integrated into implementation-facing docs.
**Goal:** Lock execution readiness. Validate that the plan, schema, and architecture are now consistent enough to begin Phase 1 implementation.

---

## What to Review

### 1. Schema Consistency

Verify that `docs/authoritative-schema.md` is internally consistent after the Batch 1 edits:

- Table Groups Overview lists 7 groups totaling 43 tables
- Each table section has correct field count and types
- `blog_comments` (new) correctly links to `blog_posts` and `users`
- 5 summary tables (Section 7) correctly reference their source tables via link columns
- `business_profile` has all onboarding fields + design/website fields
- `blog_posts` has all SEO + video fields
- `webhook_log` has the ADR-038 reconciliation note
- `time_entries` has the V2 infrastructure note
- Footer says "43 tables"

### 2. ADR Integration Completeness

Verify that ADR-034 through ADR-040 are referenced in the correct implementation-facing docs:

| ADR | Should appear in |
|-----|-----------------|
| ADR-034 (Anvil Constraints) | build-plan.md (Cross-Phase Dependencies, Risk Mitigation), architecture.md (§7), integration-architecture.md (§1), coding-standards.md (§2), testing-standards.md (§1), user-flows.md (§2) |
| ADR-035 (Form Architecture) | build-plan.md (Stages 1.3, 2.3), architecture.md (§5), coding-standards.md (§5) |
| ADR-036 (Background Tasks) | build-plan.md (Stages 2.4, 3.2), integration-architecture.md (§4), testing-standards.md (§3), user-flows.md (§3) |
| ADR-037 (UI Customization) | build-plan.md (Stage 1.4), architecture.md (§6), design-direction.md (Design Tokens) |
| ADR-038 (Webhook Architecture) | build-plan.md (Stage 2.4), integration-architecture.md (§1), authoritative-schema.md (webhook_log note) |
| ADR-039 (PDF Invoice) | build-plan.md (Stage 2.5), integration-architecture.md, user-flows.md (§3) |
| ADR-040 (Data Access) | build-plan.md (Stages 3.1, 5.1), architecture.md (§5 implied), coding-standards.md (§5), integration-architecture.md, authoritative-schema.md (Section 7) |
| ADR-027 (Design Rules) | design-direction.md (Anvil Design Rules) |

### 3. Cross-Document Consistency

Check for contradictions between:

- **authoritative-schema.md** vs **anvil.yaml** (mb-3-cs): Do the 38 existing tables match? Are the 5 new tables (not yet in anvil.yaml) clearly documented as "to be created"?
- **build-plan.md** vs **authoritative-schema.md**: Does every table referenced in a build stage exist in the schema?
- **integration-architecture.md** background task list (7 tasks) vs **ADR-036** task inventory (7 tasks): Do they match?
- **platform-overview.md** §13.1 table count vs **authoritative-schema.md** footer: Both should say 43

### 4. Execution Readiness Assessment

For each build-plan phase, assess:

| Phase | Question | Status to verify |
|-------|----------|-----------------|
| Phase 1 | Are all Foundation tables (users, vault, business_profile, config, etc.) fully specified in the schema? | Check column completeness |
| Phase 1 | Does the schema support the onboarding flow (ADR-017, 023, 025, 026)? | Check business_profile onboarding fields |
| Phase 2 | Are bookings, services, availability tables fully specified? | Check bookings has 16 columns (documented) |
| Phase 2 | Are payment tables (payment_config, webhook_log) consistent with ADRs? | Check ADR-038 reconciliation |
| Phase 2 | Are invoice tables (invoice, invoice_items) specified? | Check column completeness |
| Phase 3 | Are CRM tables (contacts, campaigns, segments, tasks, leads) specified? | Check column completeness |
| Phase 3 | Are summary tables (daily_summary, etc.) documented for analytics? | Check Section 7 exists |
| Phase 4 | Are content tables (blog_posts, blog_comments, pages, landing_pages) specified? | Check column completeness |
| Phase 5 | Are analytics tables (summary tables) documented? | Check Section 7 |
| Phase 5 | Is health_log documented for observability? | Check Section 7.5 |

### 5. Blocker Identification

Identify any remaining blockers before Phase 1 implementation can start:

- Schema gaps (missing tables or columns)
- ADR contradictions (two ADRs saying different things)
- Doc contradictions (two docs saying different things)
- Missing wireframes or design specs
- Missing server code files
- Anvil Designer changes not yet applied (Batch 2 tasks 1-10)

### 6. Data Flow Validation

Trace the key data flows through the schema to verify they work:

```
Flow 1: Onboarding
  Owner created → business_profile.onboarding_status = "in_progress"
  → Currency selected → business_profile.system_currency set
  → Legal acknowledged → business_profile.legal_ack_accepted = True
  → Onboarding complete → business_profile.onboarding_status = "completed"

Flow 2: Booking
  Service selected → Provider selected → Date/time chosen
  → bookings row created (16 columns)
  → Payment processed → bookings.payment_status updated
  → Invoice generated → invoice row created
  → Contact created/updated → contacts row + contact_events row

Flow 3: Lead Capture
  Landing page form submitted → leads row + contacts row created simultaneously
  → leads.converted_to_contact_id set → Welcome campaign enrollment

Flow 4: Analytics
  Daily background task → reads bookings, contacts, campaigns
  → Writes daily_summary, service_performance, provider_performance, campaign_metrics
  → Dashboard reads summary tables
```

Verify each flow has all required tables and columns in the schema.

---

## Expected Output

Produce an engineering review report with:

1. **Schema Consistency Score** — pass/fail per section
2. **ADR Integration Score** — pass/fail per ADR
3. **Cross-Document Consistency Score** — pass/fail per pair
4. **Execution Readiness Score** — pass/fail per phase
5. **Blockers** — list any remaining blockers with severity
6. **Data Flow Validation** — pass/fail per flow
7. **Recommendation** — proceed to implementation, or fix remaining issues first

---

## Source Files to Read

| File | Purpose |
|------|---------|
| `docs/authoritative-schema.md` | Primary schema — verify internal consistency |
| `implementation/build-plan.md` | Build sequence — verify ADR refs present |
| `docs/architecture.md` | Architecture — verify ADR refs present |
| `docs/integration-architecture.md` | Integration patterns — verify ADR refs + task list |
| `docs/coding-standards.md` | Coding standards — verify ADR refs present |
| `docs/testing-standards.md` | Testing standards — verify ADR refs present |
| `docs/user-flows.md` | User flows — verify ADR refs present |
| `docs/design-direction.md` | Design direction — verify ADR refs + no duplicates |
| `docs/platform-overview.md` | Platform overview — verify table count |
| `docs/clarifications.md` | Clarifications — verify table count + path fix |
| `implementation/devlog.md` | Devlog — verify June entries present |
| `adr/034-anvil-platform-constraints.md` | ADR source — reference for validation |
| `adr/035-form-architecture-and-state.md` | ADR source |
| `adr/036-real-time-and-background-tasks.md` | ADR source |
| `adr/037-ui-customization-approach.md` | ADR source |
| `adr/038-webhook-architecture.md` | ADR source |
| `adr/039-pdf-invoice-generation.md` | ADR source |
| `adr/040-data-access-patterns.md` | ADR source |
| `gstack-outputs/ceo-review-fix-plan.md` | CEO review audit — source of 18 gaps |
| `gstack-outputs/build-batch-2-anvil-spec.md` | Anvil Designer spec — Batch 2 tasks |

---

## Constraints

- This is a read-only review. Do not edit files.
- Focus on consistency and completeness, not code quality.
- Flag every contradiction, even minor ones.
- The review must confirm whether Phase 1 implementation can begin.

---

*End of /plan-eng-review prompt*
