---
title: "CurrencyTab-requirement"
status: draft
doc-id: "mb-ac4d6cd0"
updated: "2026-11-25"
---
# CurrencyTab — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `settings.SettingsForm.CurrencyTab` |
| Purpose | The System Currency and Display Currency tabs' content: system currency select-and-confirm while no booking or invoice exists, read-only once locked; display currency per service and business-wide, display-only (tickets 18 and 22, F-16) |
| Screen type and width | Settings panel inside `settings.SettingsForm` (1000 px) |
| Layout | Hosted by `settings.SettingsForm` |
| Route | None — a local tab of the hub |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Tab panel container | Anvil `ColumnPanel` (`col_currency_tab`) | The tab's one column | — | mobile | — |
| System currency heading | `m3.Heading` (`hdg_system_currency`) | "System Currency", `headline-small` (the onboarding plan's currency step naming) | `headline-small` | — | `currency.system.title` |
| System currency hint | `m3.Text` (`text_system_currency_hint`) | "Select the currency your business operates in. This will be used for all internal calculations, reporting, and analytics." (the onboarding plan's text) | `body-medium` | — | `currency.system.hint` |
| Lock warning | `m3.Text` (`text_system_currency_warning`) | "This selection becomes permanent once your first booking or invoice has been recorded." (the onboarding plan's warning text, adapted to the schema's lock rule) | `body-small` | — | `currency.system.warning` |
| System currency selector | `m3.DropdownMenu` (`dd_system_currency`) | The supported ISO 4217 currencies (the onboarding plan's dropdown + confirm) | `outlined-select` | — | `currency.system.select` |
| Confirm button | `m3.Button` (`btn_confirm_currency`, filled) | "Confirm Currency Selection" (the onboarding plan's button); enabled only while the currency can change | `primary-button` | — | `currency.system.confirm` |
| Locked display | `m3.Text` (`text_system_currency_locked`) | When a booking or invoice exists, the selector is replaced by the locked value and this text | `body-medium` | — | `currency.system.locked` |
| Display currency heading | `m3.Heading` (`hdg_display_currency`) | "Display Currency", `headline-small` | `headline-small` | — | `currency.display.title` |
| Display currency hint | `m3.Text` (`text_display_currency_hint`) | The optional per-service currency prices are shown in; every stored amount stays in the system currency (F-16) | `body-medium` | — | `currency.display.hint` |
| Business-wide display currency selector | `m3.DropdownMenu` (`dd_display_currency`, `allow_none=True`) | Optional; empty = none | `outlined-select` | — | `currency.display.select` |
| Help icon | `components.HelpTooltipComponent` (help entry `settings.currency`) | The tab's tooltip and help video | — | — | `help.settings.currency.tooltip` |

Per-service display currency is set on `services.ServiceEditorForm` (`services.display_currency`); the tab holds the business-wide display default (F-16; ticket 22 completes this tab).

## 3. Data

| Element | Table | Column |
|---|---|---|
| System currency | `business_profile` | `system_currency` (ISO 4217) |
| The lock | `bookings`, `invoice` | row existence — the server checks whether any booking or invoice exists (ticket 22: the lock's check is server-side) |
| Business-wide display currency | `config` | the display-currency default key |
| Per-service display currency | `services` | `display_currency` (set on the service editor) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the tab | `form_show` → `load_currency` | `settings_service` currency functions (`server_settings/settings_service`) | The current values and whether the lock holds arrive |
| Confirm the system currency | `btn_confirm_currency_click` | `settings_service` currency functions (`server_settings/settings_service`) | The currency is stored; before the first booking or invoice it can be changed again; once locked the attempt fails server-side with a clear message (adr-system-currency-selection-and-immutability; AC-S10 pattern: a client-supplied currency never changes a stored amount) |
| Set the display currency | `dd_display_currency_change` | `settings_service` currency functions | The display default is stored; it is display-only and never changes a stored amount (LG-04) |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the tab always shows its selectors |
| Heading | None — not applicable |
| Body text | None — not applicable |
| Primary action | None — not applicable |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | none |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per `access-matrix.yaml` `settings_tabs.SystemCurrency`.

## 7. Text

| Phrase key | English text |
|---|---|
| `currency.system.title` | System Currency |
| `currency.system.hint` | Select the currency your business operates in. This will be used for all internal calculations, reporting, and analytics. |
| `currency.system.warning` | This selection becomes permanent once your first booking or invoice has been recorded. |
| `currency.system.select` | System currency |
| `currency.system.confirm` | Confirm Currency Selection |
| `currency.system.locked` | Your system currency is {currency}. It is locked because your first booking or invoice exists. |
| `currency.display.title` | Display Currency |
| `currency.display.hint` | Optionally show prices to your customers in another currency. Every stored amount stays in your system currency. |
| `currency.display.select` | Display currency |

## 8. Help Videos

| Process | Help video |
|---|---|
| Choosing the system currency | `master_template` help entry `settings.currency` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-02 Owner onboarding | The wizard's step 5 sets the system currency here (dropdown plus confirm) |
| UF-44 Trying the demo apps | The demo's currency is set; the lock applies as anywhere |
