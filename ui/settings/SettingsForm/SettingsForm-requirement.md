---
title: "SettingsForm-requirement"
status: draft
doc-id: "mb-8e3ab5cc"
updated: "2026-11-25"
---
# SettingsForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `settings.SettingsForm` |
| Purpose | The single tabbed form for all configuration after onboarding: 15 tabs shown by role per the access matrix `settings_tabs`, with "Return to Onboarding" in the header while onboarding is in progress (ticket 18, F-05; DESIGN.md SettingsForm) |
| Screen type and width | Settings — 1000 px, tabs and panels (spec-ui-standards section 7) |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation and from the onboarding wizard's links |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_settings`) | "Settings", `headline-large` (spec-ui-standards section 8.2: page title) | `page-title` | — | `settings.title` |
| Return to Onboarding button | `m3.Button` (`btn_return_onboarding`, outlined) | Visible only while onboarding is in progress (DESIGN.md SettingsForm; UF-02) | `secondary-button` | — | `settings.return_onboarding` |
| Tab bar | `m3.Tabs` (`tabs_settings`, type `primary`) | The theme's primary tabs switch the hub's main views (DESIGN.md SettingsForm: tab-based navigation using the theme's Tabs); tab visibility is RBAC-governed | — | — | one key per tab, section 7 |
| Business Profile tab panel | `settings.SettingsForm.BusinessProfileTab` (local form) | The Business Profile tab's content (ticket 18) | — | — | — |
| Brand Palette tab panel | `settings.SettingsForm.PaletteSettingsForm` (local form) | The Brand Palette tab's content (ticket 18; the local component per the custom-component matrix) | — | — | — |
| System Currency tab panel | `settings.SettingsForm.CurrencyTab` (local form) | The System Currency and Display Currency tabs' content; completed at ticket 22 | — | — | — |
| Legal Acknowledgement panel | Anvil `ColumnPanel` (`col_legal_ack`) | The read-only acknowledgement history (DESIGN.md SettingsForm: Legal Acknowledgement is read-only; the action belongs to onboarding) | — | mobile | — |
| Acknowledgement row | `m3.ListItem` (`li_ack_*`) | One row per recorded acknowledgement: what, when | — | — | — |
| Payment Gateways tab panel | `settings.PaymentGatewayForm` (opened in the tab) | Built with its feature (ticket 23); this ticket creates the tab host that opens it (ticket 18) | — | — | — |
| Invoicing tab panel | `settings.SettingsForm.InvoicingTab` (local form) | The Invoicing tab's content (ticket 18) | — | — | — |
| Users and Permissions tab panel | `settings.UserManagementForm` (opened in the tab) | A link tab that opens UserManagementForm (DESIGN.md SettingsForm) | — | — | — |
| Email tab panel | `settings.EmailSettingsForm` (opened in the tab) | Built with its feature (ticket 29); this ticket creates the tab host | — | — | — |
| Domain tab panel | `settings.DomainSettingsForm` (opened in the tab) | Built with its feature (ticket 29); this ticket creates the tab host | — | — | — |
| Theme tab panel | `settings.ThemeConfigForm` (opened in the tab) | Built with its feature (ticket 33); this ticket creates the tab host | — | — | — |
| Feature Switches tab panel | `settings.SettingsForm.FeatureSwitchesTab` (local form) | The Feature Switches tab's content (ticket 18) | — | — | — |
| Legal Pages tab panel | `settings.PolicyEditorForm` (opened in the tab) | Built with its feature (ticket 40); this ticket creates the tab host | — | — | — |
| Data tab panel | `settings.SettingsForm.DataTab` (local form) | The Data tab's content; built as a placeholder at ticket 18, completed at ticket 46 | — | — | — |
| Plan and billing tab panel | `settings.SettingsForm.PlanBillingTab` (local form) | Built as a placeholder at ticket 18, completed at ticket 48 | — | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `settings.hub`) | The feature's tooltip and help video beside the title | — | — | `help.settings.hub.tooltip` |

The tab list is the one in `mb-3-cs-architecture-reference.md` F-05; a tab a role may not use is hidden, never disabled (ticket 18, B1).

## 3. Data

| Element | Table | Column |
|---|---|---|
| Tab visibility | `users` | `role` (per `access-matrix.yaml` `settings_tabs`) |
| Return-to-onboarding visibility | `business_profile` | `onboarding_status` |
| Legal acknowledgement history | `business_profile` | `legal_ack_accepted`, `legal_ack_accepted_at`, `data_governance_ack_accepted`, `data_governance_ack_accepted_at` |
| Everything else | the tab's own tables | the tab's own columns (each tab's requirement) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Switch tab | `tabs_settings_change` | None | The selected tab's panel shows; hidden tabs are not in the list for that role |
| Return to onboarding | `btn_return_onboarding_click` | None | `onboarding.OnboardingForm` opens at the next unfinished step |
| Open a link tab's form | the tab's open handler | None | The tab's form opens in the panel |
| Read the settings state | `form_show` → `load_settings` | `settings_service` reads (`server_settings/settings_service`) | The tab's data arrives through the tab's own server calls; settings forms that read and write through their own server calls use no data bindings (ticket 18) |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the hub always shows its tabs |
| Heading | None — not applicable |
| Body text | None — not applicable |
| Primary action | None — not applicable |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

The shell opens for Owner and Manager; each tab's scope is the access matrix's `settings_tabs` row of the same name.

## 7. Text

| Phrase key | English text |
|---|---|
| `settings.title` | Settings |
| `settings.return_onboarding` | Return to Onboarding |
| `settings.tab.business_profile` | Business Profile |
| `settings.tab.brand_palette` | Brand Palette |
| `settings.tab.system_currency` | System Currency |
| `settings.tab.display_currency` | Display Currency |
| `settings.tab.legal_ack` | Legal Acknowledgement |
| `settings.tab.payment_gateways` | Payment Gateways |
| `settings.tab.invoicing` | Invoicing |
| `settings.tab.users` | Users and Permissions |
| `settings.tab.email` | Email |
| `settings.tab.domain` | Domain |
| `settings.tab.theme` | Theme |
| `settings.tab.feature_switches` | Feature Switches |
| `settings.tab.legal_pages` | Legal Pages |
| `settings.tab.data` | Data |
| `settings.tab.plan_billing` | Plan and billing |
| `settings.legal_ack.title` | Your acknowledgements |
| `settings.legal_ack.recorded` | Recorded {date} — this acknowledgement is kept and cannot be reversed. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Finding a setting | `master_template` help entry `settings.hub` — played in the app language |
| Roles and the tabs they see | `master_template` help entry `settings.roles` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-02 Owner onboarding | The Settings tabs the wizard links to; "Return to Onboarding" while in progress |
| UF-03 Staff invitation and sign-in | The Users and Permissions tab (UF-03 step 1) |
| UF-04 Opening the Vault | The gateway keys' home is the Vault; the Payment Gateways tab explains where secrets live |
| UF-39 Email addresses and desks | The Email tab |
| UF-28 Clause Builder | The Legal Pages tab |
| UF-33 Restoring or viewing data | The Data tab |
| UF-45 / UF-46 Plan and billing | The Plan and billing tab |
| UF-44 Trying the demo apps | In demo mode the Vault, Users and Permissions and Domain tabs are read-only (architecture reference F-36) |
