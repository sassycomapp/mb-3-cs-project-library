# Settings Wireframes Production Report

**Date:** 2026-06-20  
**Package:** settings  
**Wireframes Produced:** 7/7  
**ADR-27 Compliance:** All wireframes now compliant

---

## Summary

All 7 settings wireframes have been recreated for ADR-27 compliance. The existing wireframes had multiple violations that have been corrected.

---

## Wireframes Produced

| # | Form | Status | Key Changes |
|---|------|--------|-------------|
| 1 | EmailSetupForm | ✅ Complete | Removed `lbl_` prefix, cleaned explanatory text |
| 2 | PaymentGatewayForm | ✅ Complete | Removed `lbl_` prefix, cleaned explanatory text |
| 3 | ThemeConfigForm | ✅ Complete | Removed `lbl_` prefix, cleaned explanatory text |
| 4 | VaultForm | ✅ Complete | Removed `lbl_` prefix, sample data, emoji icons; added Material Icons |
| 5 | UserManagementForm | ✅ Complete | Removed sample data, emoji icons; added Material Icons |
| 6 | SettingsForm | ✅ Complete | Removed extra sections (RBAC Governance, end note), `lbl_` prefix, explanatory text |
| 7 | PolicyEditorForm | ✅ Complete | Removed extra sections (Behaviour Notes, ADR References), sample data, explanatory text |

---

## ADR-27 Compliance Corrections

### Violations Found and Fixed

| Violation | Wireframes Affected | Fix Applied |
|-----------|---------------------|-------------|
| Legacy `lbl_` prefix (Section 3.2) | EmailSetupForm, PaymentGatewayForm, ThemeConfigForm, VaultForm, SettingsForm | Replaced with `txt_`, `hdg_`, or `txt_` as appropriate |
| Extra sections beyond mandatory 3 (Section 2) | SettingsForm, PolicyEditorForm | Removed RBAC Governance, Behaviour Notes, ADR References sections |
| Sample data in wireframe (Section 4.2) | UserManagementForm, VaultForm, PolicyEditorForm | Removed user names, emails, secret names, dates, clause text |
| Explanatory/behaviour text (Section 4.3) | Multiple wireframes | Removed descriptive text from wireframe diagram |
| Emoji icons instead of Material Icons | UserManagementForm, VaultForm | Replaced with Material Icons (`<span class="material-icons">`) |

### Compliance Checklist (Section 7)

- [x] Section 0 reading completed (ADR-027, nomenclature.md, m3_component_mapping.md, scaffold-spec.md)
- [x] Document contains exactly three sections, in the correct order (Section 2.1–2.3)
- [x] Document is NOT formatted as a "Screen" type document
- [x] No legacy `Label` (`lbl_`) component is used anywhere
- [x] No sample data in wireframes
- [x] No explanatory/behaviour text in wireframe diagram
- [x] Material Icons imported where icon components are used

---

## Component Naming Conventions

All wireframes follow the naming conventions from `nomenclature.md`:

| Component Type | Prefix | Example |
|----------------|--------|---------|
| ColumnPanel | `col_` | `col_email_config` |
| FlowPanel | `flow_` | `flow_sender_fields` |
| Card | `card_` | `card_sender` |
| Heading | `hdg_` | `hdg_email_title` |
| TextBox | `txt_` | `txt_from_email` |
| TextArea | `txt_` | `txt_clause_body` |
| DropdownMenu | `dd_` | `dd_timezone` |
| Button | `btn_` | `btn_save` |
| CheckBox | `cb_` | `cb_gateway_connected` |
| Switch | `sw_` | `sw_test_mode` |
| RepeatingPanel | `rp_` | `rp_secrets` |
| DataGrid | `dg_` | `dg_users` |
| Text | `txt_` | `txt_email_subtitle` |
| Icon | `ic_` | `ic_empty_vault` |

---

## Design Rules Applied

### Section Structure (ADR-27 Section 2)

All wireframes contain exactly 3 sections:
1. **Introduction** — Page title, subtitle, legend explaining conventions
2. **Wireframe Diagram** — Component layout with annotations
3. **Properties Table** — Designer-mandatory properties only

### Component Usage (ADR-27 Section 3)

- All components are approved Anvil Material 3 Theme components
- No legacy `Label` (`lbl_`) components used
- Material Icons imported for icon components
- Correct component-purpose usage (FlowPanel for horizontal, ColumnPanel for vertical)

### Text and Content (ADR-27 Section 4)

- Every visible text element is its own component
- No sample data in wireframes (sample data belongs in screens only)
- No behaviour, data, or explanatory text in wireframe diagram

### Container Drawing (ADR-27 Section 5)

- All containers drawn as containers with components inside
- Container names inset in container border
- No border on non-bordered components (Heading, Text)

### Properties Table (ADR-27 Section 6)

- Only Designer-mandatory properties listed
- `writeback (W)` specified for all text input components
- `hide_text = True` specified for password fields

---

## Form-Specific Notes

### EmailSetupForm
- SMTP host, port, encryption are platform constants (Brevo SMTP) — not user-configurable
- SMTP credentials stored in Vault, not in form
- Test Connection button verifies SMTP configuration

### PaymentGatewayForm
- Gateway selector with Stripe and Paystack options
- Test Mode toggle for sandbox/live key switching
- Publishable Key input — safe to expose in client-side code
- Secret Key input NOT present — stored exclusively in Vault (ADR-003)

### ThemeConfigForm
- Primary Color and Accent Color with color picker + hex text input
- Font Family dropdown (Google Fonts selection)
- Header Style dropdown (Standard, Centered, Minimal)
- Live Preview section showing theme applied to sample content
- Reset to Default button

### VaultForm
- TOTP verification required on every open (no grace period)
- Stored secrets listed with name, last updated date, and usage context
- Each secret has View (masked reveal) and Delete actions
- Add Secret form at bottom with name and password input
- Secret values encrypted at rest and never displayed after saving
- Empty state shown when no secrets stored

### UserManagementForm
- Role filter dropdown (All, Owner, Manager, Admin, Staff, Customer)
- Invite User button (primary action)
- User table with Name, Email, Role, Status, Actions columns
- Role badges with semantic color coding per role
- Status indicators (Active, Inactive, Pending)
- Row actions: Edit (all), Deactivate (active users), Reactivate (inactive users)

### SettingsForm
- Single form with 11 tabs using M3 buttons
- Tab visibility is RBAC-governed
- "Return to Onboarding" button visible only when onboarding in progress
- Services and Users & Permissions tabs are link-only (navigate to respective forms)
- PaletteSettingsForm is a local component used only within Palette tab

### PolicyEditorForm
- Document type selector (Privacy Policy vs Terms & Conditions)
- Embeds ClauseBuilder component parameterized by document type
- Category dropdown for clause grouping
- Seeded clauses (green border, editable, not deletable)
- Custom clauses (blue border, editable and deletable)
- Publish/update button writes to `privacy_policy` or `t_and_c` table
- Version history shows previous versions with timestamps

---

## RBAC Governance

| Tab | Owner | Manager | Admin | Staff | Customer |
|-----|-------|---------|-------|-------|----------|
| Business Profile | ✓ | ✓ | ✗ | ✗ | ✗ |
| Palette | ✓ | ✓ | ✗ | ✗ | ✗ |
| System Currency | ✓ | ✗ | ✗ | ✗ | ✗ |
| Legal Acknowledgement | ✓ | ✗ | ✗ | ✗ | ✗ |
| Display Currency | ✓ | ✓ | ✗ | ✗ | ✗ |
| Payment Gateway | ✓ | ✗ | ✗ | ✗ | ✗ |
| Services | ✓ | ✓ | ✓ | ✗ | ✗ |
| Users & Permissions | ✓ | ✗ | ✗ | ✗ | ✗ |
| Email Configuration | ✓ | ✗ | ✗ | ✗ | ✗ |
| Theme | ✓ | ✓ | ✗ | ✗ | ✗ |
| Feature Flags | ✓ | ✗ | ✗ | ✗ | ✗ |

---

## Files Produced

| Wireframe | Path |
|-----------|------|
| EmailSetupForm | `wireframes/settings/EmailSetupForm/wireframe-settings-EmailSetupForm.html` |
| PaymentGatewayForm | `wireframes/settings/PaymentGatewayForm/wireframe-settings-PaymentGatewayForm.html` |
| ThemeConfigForm | `wireframes/settings/ThemeConfigForm/wireframe-settings-ThemeConfigForm.html` |
| VaultForm | `wireframes/settings/VaultForm/wireframe-settings-VaultForm.html` |
| UserManagementForm | `wireframes/settings/UserManagementForm/wireframe-settings-UserManagementForm.html` |
| SettingsForm | `wireframes/settings/SettingsForm/wireframe-settings-SettingsForm.html` |
| PolicyEditorForm | `wireframes/settings/PolicyEditorForm/wireframe-settings-PolicyEditorForm.html` |

---

## Next Steps

1. **Commit and push** settings wireframes to repository
2. **Sync GBrain** to update knowledge base
3. **Proceed to next package** (bookings, crm, blog, etc.)

---

*Report generated by Build Agent — 2026-06-20*
