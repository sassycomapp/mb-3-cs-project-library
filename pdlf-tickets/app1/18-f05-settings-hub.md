---
title: "18 — F-05 Settings Hub"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-7a5cc03f"
updated: "2026-11-17"
---

## Description

**Feature:** F-05 Settings Hub (DESIGN.md SettingsForm — 15 tabs, RBAC visibility; scaffold-spec settings package; SR-11; FR-05). Configuration, never code, in a client instance (common aspect 3; design doc §23.3).
**Depends on:** 16 (auth and roles), 03 (foundation).

### Forms

- `settings.SettingsForm` — the 15-tab hub, tabs shown by role per the access matrix `settings_tabs`
- Local tabs (in SettingsForm's folder): `PaletteSettingsForm` (Brand Palette, F-31), `BusinessProfileTab` (incl. app language), `CurrencyTab` (system currency read-only, local currencies — completed at ticket 22), `InvoicingTab` (payment terms, tax, numbering, quote validity), `FeatureSwitchesTab`, `DataTab` (placeholder — completed at ticket 46), `PlanBillingTab` (placeholder — completed at ticket 48)
- `settings.UserManagementForm` — Users and Permissions: invite, role change, deactivate/reactivate (UF-03)
- `settings.ThemeConfigForm`, `settings.EmailSettingsForm`, `settings.DomainSettingsForm`, `settings.PaymentGatewayForm`, `settings.PolicyEditorForm` are built with their features (tickets 33, 29, 29, 23, 40); this ticket creates the tab hosts that open them

### Server functions

- `server_settings/settings_service` (renamed from `server_settings/service.py` at this step, per the divergence inventory) — settings reads and writes as data; `get_layout_context` lives here (ticket 01)
- `auth_service` invitation functions are called by UserManagementForm (from ticket 16)

### Tables

- `business_profile`, `config`, `theme_config`, `users` (via auth_service)

### Acceptance tests

- `SR-11` — the Settings Hub's behaviour; UF-03 (Users and Permissions)
- `IN-02` — every tab and function as every role, per the access matrix `settings_tabs` and writable_fields
- `ST-14` — every visible text a phrase key

### Security criteria

- B1 — tab visibility is the interface only; the server functions enforce (AC-S1)
- B2 — tab access matches the access matrix exactly (Owner-only tabs refuse Manager)
- Settings forms that read and write through their own server calls use no data bindings (scaffold M3 standards)

## Comments
