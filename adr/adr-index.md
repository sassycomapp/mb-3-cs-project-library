# Mybizz CS — ADR Index

**Last Updated:** 2026-06-28
**Total ADRs:** 42 files (25 Accepted, 7 Confirmed, 2 Superseded, 1 Cancelled, 7 Approved)

---

| ADR File | Title | Status | Date |
|---|---|---|---|
| `1-brevo-replaces-zoho-email.md` | Brevo Replaces All Zoho Products | Accepted | 2026-03-21 |
| `2-navigation-lambda-link-open-form.md` | Navigation Standard: Lambda/Link/open_form | Accepted | 2026-03-18 |
| `3-payment-security-boundary-vault.md` | Payment Security Boundary: Vault | Accepted | 2026-03-15 |
| `4-lead-capture-simultaneous-creation.md` | Lead Capture: Simultaneous Creation | Accepted | 2026-03-21 |
| `5-timezone-utc-storage-display-conversion.md` | Client Timezone: UTC Storage, Display Conversion | Accepted | 2026-03-17 |
| `6-crm-package-replaces-marketing.md` | Package Name: crm/ Replaces marketing/ | Accepted | 2026-03-18 |
| `9-multi-vertical-to-single-vertical-conversion.md` | Multi-Vertical to Single-Vertical Conversion | Accepted | 2026-04-04 |
| `10-single-contacts-table.md` | Single Contacts Table Replaces Dual contacts/customers | Accepted | 2026-05-03 |
| `11-schema-scope-reduction.md` | Schema Scope Reduction | Accepted | 2026-05-03 |
| `12-consolidated-build-sequence.md` | Consolidated Build Sequence | **Superseded** | 2026-05-03 |
| `13-system-currency-setting.md` | System Currency Setting | **Superseded** | 2026-05-10 |
| `14-anvil-extras-exclusion.md` | Anvil Extras Exclusion | Accepted | 2026-05-21 |
| `15-material-3-theme-component-scope.md` | Material 3 Theme Component Scope | Accepted | 2026-05-21 |
| `16-system-currency-selection-and-immutability.md` | System Currency, Display Currency, and Immutability | Accepted | 2026-05-29 |
| `17-onboarding-vs-settings-boundary.md` | Onboarding vs Settings Boundary | Accepted | 2026-05-29 |
| `18-legal-policy-responsibility-acknowledgement-and-clause-builder-architecture.md` | Legal Policy Responsibility Acknowledgement and Clause-Builder Architecture | Accepted | 2026-05-29 |
| `19-payment-gateway-configuration-is-a-settings-function-and-is-rbac-governed.md` | Payment Gateway Configuration Is a Settings Function and Is RBAC-Governed | Accepted | 2026-05-29 |
| `20-onboarding-data-schema-alignment.md` | Onboarding Data Schema Alignment | Accepted | 2026-05-29 |
| `21-client-data-management-rights-and-mybizz-retention-boundary.md` | Client Data Management Rights and Mybizz Retention Boundary | Accepted | 2026-05-29 |
| `22-tiers-model.md` | Tiers Model | Accepted | 2026-05-29 |
| `23-onboarding-resumability.md` | Onboarding Resumability | Accepted | 2026-05-29 |
| `24-payment-gateway-mutability.md` | Payment Gateway Mutability | Accepted | 2026-05-29 |
| `25-onboarding-finality.md` | Onboarding Finality (old) | **Cancelled** | 2026-05-31 |
| `26-onboarding-finality.md` | Onboarding Finality | Accepted | 2026-06-01 |
| `27-design-rules.md` | Design Rules | Accepted | 2026-06-10 |
| `28-client-instance-architecture.md` | Client Instance Architecture | Confirmed | 2026-06-13 |
| `29-dependency-update-model.md` | Dependency Update Model | Confirmed | 2026-06-13 |
| `30-blank-client-template.md` | blank_client_template as Provisioning Clone Source | Confirmed | 2026-06-13 |
| `31-free-trial-abandoned.md` | 30-Day Free Trial Abandoned | Confirmed | 2026-06-13 |
| `32-observability-architecture.md` | Observability Architecture | Confirmed | 2026-06-13 |
| `33-mybizz-management-visibility.md` | Mybizz_management Visibility and Control | Confirmed | 2026-06-13 |
| `34-anvil-platform-constraints.md` | Anvil Platform Constraints and Design Boundaries | Approved | 2026-06-14 |
| `35-form-architecture-and-state.md` | Form Architecture and State Management | Approved | 2026-06-14 |
| `36-real-time-and-background-tasks.md` | Real-Time Updates and Background Task Architecture | Approved | 2026-06-14 |
| `37-ui-customization-approach.md` | UI Customization Approach | Approved | 2026-06-14 |
| `38-webhook-architecture.md` | Webhook Architecture | Approved | 2026-06-14 |
| `39-pdf-invoice-generation.md` | PDF Invoice Generation | Approved | 2026-06-14 |
| `40-data-access-patterns.md` | Data Access Patterns and Query Limitations | Approved | 2026-06-14 |
| `41-dependency-based-not-multi-tenant.md` | Dependency-Based Architecture, Not Multi-Tenant | Confirmed | 2026-06-13 |
| `42-client-instance-readme-five-app-system.md` | Mandatory README in Every Client Instance Documenting the Five-App System | Accepted | 2026-06-28 |
| `43-role-property-assignment-mechanism.md` | Role Property Assignment Mechanism (Designer vs. Code) | Accepted | 2026-06-27 |
| `44-responsive-behaviour-mechanism.md` | Responsive Behaviour Mechanism | Accepted | 2026-06-27 |
| `45-htmltemplate-use.md` | HTMLTemplate Use | Accepted | 2026-06-27 |

---

## Notes

- **ADR-07 and ADR-08:** Permanently skipped — numbers assigned to obsolete decisions, files never created.
- **`12-consolidated-build-sequence.md`:** Superseded by `implementation/build-plan.md` (Definitive Build Plan, 2026-05-26).
- **`13-system-currency-setting.md`:** Superseded by `16-system-currency-selection-and-immutability.md` (Consolidated ADR, 2026-06-01).
- **`16-system-currency-selection-and-immutability.md`:** Consolidated from ADR-13 and ADR-16. Covers system currency, display currency, immutability enforcement, and currency conversion strategy.
- **`25-onboarding-finality.md`:** Cancelled. Original decision that onboarding was a one-time non-replayable process. Reversed and superseded by `26-onboarding-finality.md`.
- **`26-onboarding-finality.md`:** Created 2026-05-31, updated 2026-06-01. Onboarding is resumable and revisitable. Owners may change any credential at any time. Mybizz_management maintains an append-only amendment log with three data tables.
- **`28-client-instance-architecture.md`:** Defines the dependency-based architecture. Confirmed by live testing (Test A: app_tables resolution, Test B: forms from dependency). Foundation for all data access patterns.
- **`29-dependency-update-model.md`:** Four-app update flow (mb-3-cs → master_template → client instances). Branch propagation confirmed.
- **`34-anvil-platform-constraints.md` through `40-data-access-patterns.md`:** Created 2026-06-14. Consolidated decisions from the "Issues to be resolved" section of platform-overview.md.
- **`41-dependency-based-not-multi-tenant.md`:** Definitive statement that Mybizz CS is a dependency-based SaaS platform, not a multi-tenant application. Prohibits tenant discriminator columns and tenant-filtered queries.
- **`42-client-instance-readme-five-app-system.md`:** Every client instance must contain a standardized README documenting the five-app architecture and the constraint against adding forms or modules directly to client instances.
- **`43-role-property-assignment-mechanism.md`:** Resolves contradiction in design-direction.md. `role` property is set via the Designer Properties Panel, not in code. Derived from Anvil documentation analysis.
- **`44-responsive-behaviour-mechanism.md`:** Resolves contradicting breakpoint models in design-direction.md. Responsive behaviour uses `wrap_on` per-container, not CSS breakpoint tables. Nav collapse is automatic and separate.
- **`45-htmltemplate-use.md`:** `HTMLTemplate` is banned. All current use cases have native M3 alternatives. Wireframes using it require reworking.

---

*End of file — adr-index.md*
