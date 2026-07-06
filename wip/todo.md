# Todo — Open Items Only

**Last cleaned:** 2026-07-06
**Source:** Extracted from legacy todo.md (530 lines → this file). All [Done] items removed.

---

## Docs Reorganization — Deferred (Phase 2)

1. **README.md rewrite** — still describes old structure, old `C:\mybizz\` paths, references unresolved devlog/todo system
2. **Populate 17 empty spec placeholder files** — zero-byte files in `specifications/`: spec-adr.md, spec-anvil.md, spec-api-specification.md, spec-build-plan.md, spec-custom-components.md, spec-data.md, spec-deployment.md, spec-design.md, spec-gates.md, spec-integration.md, spec-navigation.md, spec-observability.md, spec-pdlf-preparation.md, spec-scaffold.md, spec-screens.md, spec-ui.md, spec-wireframes.md
3. **Audit all `instance_id` references** — `dependency-based-not-multi-tenant` ADR prohibits tenant discriminator patterns. Found in `spec-anvil-platform-standards.md` §10 and §13. GBrain query needed to find all instances across all documents
4. **Checklist templates** — no equivalent list ever produced for the checklists folder (handover §5 flagged this)

---

## Planned — Phase 2 (Transactional Core)

### Service Catalogue Setup
- **Priority:** P2
- **Build Stage:** 2.1
- **Details:** Clients may have multiple services (consulting or product). Two pricing models: `duration` (time-based) and `unit` (deliverable-based). Per service: name, description, category, provider, pricing model, price, duration, images, video URL, meeting type. Configured in Settings → Services after onboarding.

---

## Planned — Phase 4 (Public Website)

### ClauseBuilder Component Implementation
- **Priority:** P2-high
- **Build Stage:** 4.2
- **Details:** Standalone reusable clause editor, parameterised by document type (`privacy_policy` or `t_and_c`). Category selector, seeded clauses (non-deletable, editable), custom clauses (add/edit/delete), ordering by sort_order, save to relevant table.
- **Wireframe:** `wireframes/components/ClauseBuilder/wireframe-components-ClauseBuilder.html`
- **ADR:** `18-legal-policy-responsibility-acknowledgement-and-clause-builder-architecture.md`

### Policy Editor Screen Implementation
- **Priority:** P2-medium
- **Build Stage:** 4.2
- **Details:** Settings → Website screen with document type selector (Privacy Policy vs T&Cs), embedded ClauseBuilder, publish/update button, version history.
- **Wireframe:** `wireframes/settings/PolicyEditorForm/wireframe-settings-PolicyEditorForm.html`
- **ADR:** `18-legal-policy-responsibility-acknowledgement-and-clause-builder-architecture.md`

### Privacy Policy Public Page
- **Priority:** P2-medium
- **Build Stage:** 4.2
- **Details:** Public page reading from `privacy_policy` table. Clauses ordered by sort_order, grouped by category. Only active clauses displayed.

### Terms & Conditions Public Page
- **Priority:** P2-medium
- **Build Stage:** 4.2
- **Details:** Public page reading from `t_and_c` table. Same structure as Privacy Policy page.

---

## Planned — Phase 5 (Compliance & Polish)

### Client Data Export / Rights Interface
- **Priority:** P2-medium
- **Build Stage:** 5.3
- **Details:** Dashboard entry point for Owner to export client data as PDF. Separation of Mybizz-held records vs client-instance data. GDPR/POPIA compliance.
- **ADR:** `21-client-data-management-rights-and-mybizz-retention-boundary.md`

---

## Deferred (Out of Scope for V1)

### Mybizz_management Amendment Log
- **Priority:** P3-low
- **Details:** Append-only amendment log for credential/legal changes in Mybizz_management. Three tables: Client Details, Retained Records, Amendment Log. Deferred to post-V1.

---

## Blocked

### Anvil Billing Model Clarification
- **Priority:** P2
- **Status:** Blocked — requires answer from Anvil support
- **Details:** Resolve per-account vs per-app limits. Questions documented in `docs/questions-to-raise-with-anvil.md` Section 3-4. Graceful degradation design added to ADR-036 as mitigation.

---

*End of todo.md*
