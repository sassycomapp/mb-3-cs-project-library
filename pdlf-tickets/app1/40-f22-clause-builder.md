---
title: "40 — F-22 Clause Builder (the feature)"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-d97d7179"
updated: "2026-11-17"
---

## Description

**Feature:** F-22 Clause Builder (UF-28; ADR `legal-policy-responsibility-acknowledgement-and-clause-builder-architecture`; SR-38; FR-22). One legal-documents system covers the Cookie Policy, the Privacy Policy and the Terms and Conditions (decision D8).
**Depends on:** 12 (ClauseBuilder component), 18 (the Legal Pages tab host).

### Forms

- `settings.PolicyEditorForm` — the Legal Pages tab; hosts `components.ClauseBuilder` (ticket 12) for all three documents

### Server functions

- `server_settings/policy_service` — publication, versioning and the R2 copies are ticket 12's; this ticket completes the feature: the document list and status, the public legal pages' rendering from the published clauses (with ticket 33's pages), and the pre-written seeded clauses loaded per document

### Tables

- `privacy_policy`, `t_and_c`, `cookie_policy`, `config` (`legal_versions`)

### Acceptance tests

- `SR-38`, `SR-38a`; UF-28; spec-legal-documents-and-consent §12 (publish, versions, kept copies)
- The seeded clause sets load per document; a `feature_key` clause is active exactly while its feature is on

### Security criteria

- B1 — Owner only (the tab and the publish)
- The instance does not go live until all three documents are published under the recorded acknowledgement (the acknowledgement itself is ticket 51's step 6)
- AC-S12 — publications audited

## Comments
