# Mybizz CS — ADR Index

**Last Updated:** 2026-06-01
**Total ADRs:** 23 files (18 Accepted, 1 Pending, 1 Proposed, 1 Superseded, 1 Cancelled, 1 Consolidated)

---

| ADR File | Title | Status | Date |
|---|---|---|---|
| `01-brevo-replaces-zoho-email.md` | Brevo Replaces All Zoho Products | Accepted | 2026-03-21 |
| `02-navigation-lambda-link-open-form.md` | Navigation Standard: Lambda/Link/open_form | Accepted | 2026-03-18 |
| `03-payment-security-boundary-vault.md` | Payment Security Boundary: Vault | Accepted | 2026-03-15 |
| `04-lead-capture-simultaneous-creation.md` | Lead Capture: Simultaneous Creation | Accepted | 2026-03-21 |
| `05-timezone-utc-storage-display-conversion.md` | Client Timezone: UTC Storage, Display Conversion | Accepted | 2026-03-17 |
| `06-crm-package-replaces-marketing.md` | Package Name: crm/ Replaces marketing/ | Accepted | 2026-03-18 |
| `09-multi-vertical-to-single-vertical-conversion.md` | Multi-Vertical to Single-Vertical Conversion | Accepted | 2026-04-04 |
| `10-single-contacts-table.md` | Single Contacts Table Replaces Dual contacts/customers | Accepted | 2026-05-03 |
| `11-schema-scope-reduction.md` | Schema Scope Reduction | Accepted | 2026-05-03 |
| `012-superseded-consolidated-build-sequence.md` | Consolidated Build Sequence | **Superseded** | 2026-05-03 |
| `013-superseded-system-currency-setting.md` | System Currency Setting | **Superseded** | 2026-05-10 |
| `14-anvil-extras-exclusion.md` | Anvil Extras Exclusion | Accepted | 2026-05-21 |
| `15-material-3-theme-component-scope.md` | Material 3 Theme Component Scope | Accepted | 2026-05-21 |
| `16-system-currency-selection-and-immutability.md` | System Currency, Display Currency, and Immutability | Accepted (Consolidated) | 2026-05-29 |
| `17-onboarding-vs-settings-boundary.md` | Onboarding vs Settings Boundary | Accepted | 2026-05-29 |
| `18-legal-policy-responsibility-acknowledgement-and-clause-builder-architecture.md` | Legal Policy Responsibility Acknowledgement and Clause-Builder Architecture | Accepted | 2026-05-29 |
| `19-payment-gateway-configuration-is-a-settings-function-and-is-rbac-governed.md` | Payment Gateway Configuration Is a Settings Function and Is RBAC-Governed | Accepted | 2026-05-29 |
| `20-onboarding-data-schema-alignment.md` | Onboarding Data Schema Alignment | Accepted | 2026-05-29 |
| `21-client-data-management-rights-and-mybizz-retention-boundary.md` | Client Data Management Rights and Mybizz Retention Boundary | Accepted | 2026-05-29 |
| `22-tiers-model.md` | Tiers Model | Accepted | 2026-05-29 |
| `23-onboarding-resumability.md` | Onboarding Resumability | Accepted | 2026-05-29 |
| `24-payment-gateway-mutability.md` | Payment Gateway Mutability | Accepted | 2026-05-29 |
| `25-onboarding-finality.md` | Onboarding Finality | Accepted | 2026-06-01 |

---

## Notes

- **ADR-07 and ADR-08:** These numbers were assigned to obsolete decisions (Continue.dev Front Matter CoP and Rules File Line Limit). No files exist for these numbers. They are permanently skipped.
- **`012-superseded-consolidated-build-sequence.md`:** Superseded by `implementation/build-plan.md` (Definitive Build Plan, 2026-05-26).
- **`013-superseded-system-currency-setting.md`:** Superseded by `016-system-currency-selection-and-immutability.md` (Consolidated ADR, 2026-06-01).
- **`16-system-currency-selection-and-immutability.md`:** Consolidated from ADR-13 and ADR-16. Covers system currency, display currency, immutability enforcement, and currency conversion strategy.
- **`25-onboarding-finality.md`:** Created 2026-05-31, updated 2026-06-01. Formalises that onboarding is resumable and revisitable. Owners may change any credential at any time. Mybizz_management maintains an append-only amendment log. Client data rights are defined (Owner-removable vs legally-retained).
- **`025-cancelled-onboarding-finality.md`:** Old version cancelled and superseded by `25-onboarding-finality.md`.

---

*End of file — adr-index.md*
