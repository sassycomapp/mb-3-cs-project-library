---
title: "AccountForm-requirement"
status: draft
doc-id: "mb-fc03bc35"
updated: "2026-11-25"
---
# AccountForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `portal.AccountForm` |
| Purpose | The customer's account: profile, password, email preferences, morning briefing switch (ticket 39, F-24; UF-29 step 8) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.CustomerLayout` |
| Route | None — opened by the portal routing |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_account`) | "Account", `headline-large` | `page-title` | — | `portal.nav.account` |
| Profile panel | `m3.Card` (`card_profile`, outlined) | Name, phone, email (the signed-in user's) | `surface-card` | mobile | — |
| Name field | `m3.TextBox` (`txt_account_name`, outlined) | The customer's name (`users.full_name`) | `outlined-input` | — | `booking.flow.name` |
| Phone field | `m3.TextBox` (`txt_account_phone`, type `tel`, outlined) | The phone (`users.phone`) | `outlined-input` | — | `booking.flow.phone` |
| Email display | `m3.Text` (`text_account_email`) | The email (read-only; it is the sign-in) | `body-medium` | — | `booking.flow.email` |
| Password panel | `m3.Card` (`card_password`, outlined) | Change password | `surface-card` | mobile | — |
| Change password button | `m3.Button` (`btn_change_password`, outlined) | Opens the password change (the current password required; credential changes send a notice, SC-04) | `secondary-button` | — | `account.change_password` |
| Preferences panel | `m3.Card` (`card_preferences`, outlined) | Email preferences | `surface-card` | mobile | — |
| Morning briefing switch | `m3.Switch` (`sw_briefing`) | The spoken morning briefing, switched on or off per user (F-27; `users.preferences.morning_briefing`) | — | — | `account.briefing` |
| Notification preferences note | `m3.Text` (`text_pref_note`) | The notification preferences are the engine's per-channel choices (F-18) | `body-small` | — | `account.preferences_note` |
| Save button | `m3.Button` (`btn_save_account`, filled) | Saves the profile | `primary-button` | — | `profile.save` |
| Help icon | `components.HelpTooltipComponent` (help entry `portal.account`) | The feature's tooltip and help video | — | — | `help.portal.account.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Profile | `users` | `full_name`, `phone`, `email` |
| Preferences | `users` | `preferences` (`morning_briefing`) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the account | `form_show` → `load_account` | the portal's account read (own only) | The fields arrive |
| Save | `btn_save_account_click` | the portal's account save | The profile is saved |
| Change password | `btn_change_password_click` | Anvil's password change (the current password required; a notice is sent, SC-04) | The password changes |
| Switch the briefing | `sw_briefing_change` | `update_preferences` (`server_shared/notification_service`; own rows only) | `preferences.morning_briefing` is saved |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the form always shows its fields |
| Heading | None — not applicable |
| Body text | None — not applicable |
| Primary action | None — not applicable |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | none |
| Customer | all |
| SignedOut | none |

## 7. Text

| Phrase key | English text |
|---|---|
| `account.change_password` | Change password |
| `account.briefing` | Morning briefing |
| `account.preferences_note` | Choose which emails you get in your notifications. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Your account | `master_template` help entry `portal.account` — played in the app language |
| The morning briefing | `master_template` help entry `account.briefing` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-29 Customer portal | Account |
| UF-31 WhatsUp voice | The briefing switch |
