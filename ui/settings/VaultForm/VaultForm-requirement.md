---
title: "VaultForm-requirement"
status: draft
doc-id: "mb-6924bf2a"
updated: "2026-11-25"
---
# VaultForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `settings.VaultForm` |
| Purpose | The Owner-only Vault UI: TOTP on every open with no remembered session, enrolment on first open, the secrets list with masked values, add and replace, and the Reset Vault code path (ticket 17, F-02; DESIGN.md VaultForm) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels (spec-ui-standards section 7) |
| Layout | `layouts.AdminLayout` (the Vault link is Owner-only) |
| Route | None — opened from the admin navigation |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| TOTP overlay container | `m3.Card` (`card_totp`, elevated) | Displayed before the vault content is visible; a prominent card at the top of the form (DESIGN.md VaultForm TOTP entry screen) | `elevated-card` | mobile | — |
| TOTP heading | `m3.Heading` (`hdg_totp`) | "TOTP Verification Required" (DESIGN.md VaultForm) | `headline-small` | — | `vault.totp.title` |
| TOTP instruction | `m3.Text` (`text_totp_hint`) | "Enter the code from your authenticator app" | `body-medium` | — | `vault.totp.hint` |
| TOTP code field | `m3.TextBox` (`txt_totp_code`, outlined) | The six-digit code | `outlined-input` | — | `vault.totp.code` |
| Verify button | `m3.Button` (`btn_verify_totp`, filled) | Verifies the code | `primary-button` | — | `vault.totp.verify` |
| TOTP error text | `m3.Text` (`text_totp_error`, hidden) | "Invalid code. Please try again." with the field cleared (DESIGN.md VaultForm); after five failed codes the Vault locks and the Owner is emailed | `body-small` | — | `vault.totp.error`, `vault.totp.locked` |
| Enrolment section | Anvil `ColumnPanel` (`col_totp_enrol`, hidden) | On first open, the QR code enrols the Vault's authenticator | — | mobile | — |
| Enrolment QR | Anvil `Image` (`img_totp_qr`) | The authenticator's QR code | — | — | — |
| Secrets panel container | Anvil `ColumnPanel` (`col_secrets`) | The vault content, visible only after verification | — | mobile | — |
| TOTP notice | `m3.Text` (`text_totp_notice`) | The TOTP verification notice displayed prominently at the top of the form (DESIGN.md VaultForm) | `body-small` | — | `vault.totp.notice` |
| Secrets heading | `m3.Heading` (`hdg_secrets`) | "Stored secrets", `headline-small` | `headline-small` | — | `vault.secrets.title` |
| Secrets list | Anvil `ColumnPanel` (`col_secret_rows`) | One row per secret: name, last updated date, usage context (DESIGN.md VaultForm) | — | mobile | — |
| Secret row | `m3.ListItem` (`li_secret_*`, clickable) | The theme's list item row for each secret: headline (name), supporting text (usage context), trailing text (updated date) | — | — | — |
| View (masked reveal) button | `m3.IconButton` (`ibtn_reveal_<name>`, icon `mi:visibility`) | The reveal shows the theme's loading indicator while the server decrypts and returns the masked value (DESIGN.md VaultForm loading state); the value shown is `***` only | `icon-button` | — | `vault.secrets.reveal` |
| Delete button | `m3.IconButton` (`ibtn_delete_secret_<name>`, icon `mi:delete`) | The row's removal is confirmed with the theme's feedback, and the row fades until deletion is confirmed (DESIGN.md VaultForm) | `icon-button` | — | `vault.secrets.delete` |
| Add Secret form | Anvil `ColumnPanel` (`col_add_secret`) | At the bottom with name and password input (DESIGN.md VaultForm) | — | mobile | — |
| Secret name field | `m3.TextBox` (`txt_secret_name`, outlined) | The credential's name (the Vault's fixed names) | `outlined-input` | — | `vault.add.name` |
| Secret value field | `m3.TextBox` (`txt_secret_value`, outlined, `hide_text=True`) | The credential's value; never displayed after saving | `outlined-input` | — | `vault.add.value` |
| Add Secret button | `m3.Button` (`btn_add_secret`, filled) | Shows its loading state while the secret is saved (DESIGN.md VaultForm loading state) | `primary-button` | — | `vault.add.submit` |
| Error banner (server error) | `m3.Text` (`text_vault_error`, hidden) | "Unable to verify TOTP. Please try again or contact support." with a Retry button (DESIGN.md VaultForm error state) | `body-small` | — | `vault.error.server` |
| Retry button | `m3.Button` (`btn_retry_vault`, outlined) | Retries the verification | `secondary-button` | — | `vault.error.retry` |
| Help icon | `components.HelpTooltipComponent` (help entry `vault.form`) | The feature's tooltip and help video | — | — | `help.vault.form.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Secrets list | `vault` | `name`, `updated_at`, `updated_by` (the value never leaves the server: reads show `***`) |
| Secret value | `vault` | `value` (encrypted with `encryption_key` by `vault_service`) |
| Vault lock | `rate_limits` | `identifier` (`vault:{user row id}`), `count`, `reset_time` (15 minutes after the 5th wrong code) |
| Access record | `activity_log` | `action_type` (`vault_opened`), `created_at` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Verify TOTP | `btn_verify_totp_click` | `vault_totp_service` check (ticket 17's `server_settings/vault_totp_service`) | On success the overlay dismisses and the vault content shows; the access is emailed to the Owner and audited; on failure the error shows and the field clears; the 5th wrong code locks the Vault for 15 minutes |
| Enrol the authenticator | the enrolment confirm | `vault_totp_service` enrolment | The QR enrolment completes; the overlay dismisses |
| Reveal a secret (masked) | `ibtn_reveal_<name>_click` | `get_vault_secret` (`server_settings/vault_service`; step-up) | The masked value `***` shows; the value never leaves the server (access matrix) |
| Delete a secret | `ibtn_delete_secret_<name>_click` | `save_vault_secret` (delete path; step-up) | The removal is confirmed with the theme's feedback; the row fades until confirmed |
| Add a secret | `btn_add_secret_click` | `save_vault_secret` (`server_settings/vault_service`; step-up) | The secret is encrypted and stored; its value is never displayed again |
| Reset Vault code | the Reset path | `vault_totp_service` reset + `sop-vault-recovery.md` | The Owner-only reset by emailed link; the next open asks for enrolment; no stored secret changes (test 4.19) |

Every Vault action is a high-value action needing a second factor given in the last five minutes and a fresh TOTP code (SC-03; AC-S5, AC-S7).

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Lock icon (`mi:lock`) |
| Heading | `vault.empty.title` (DESIGN.md: "No secrets stored yet") |
| Body text | `vault.empty.body` (DESIGN.md: "Add your API keys and credentials below. They will be encrypted at rest and never displayed after saving.") |
| Primary action | `vault.empty.action` — "Add Your First Secret" (DESIGN.md) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | none |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Owner only (access matrix `settings.VaultForm`).

## 7. Text

| Phrase key | English text |
|---|---|
| `vault.totp.title` | TOTP Verification Required |
| `vault.totp.hint` | Enter the code from your authenticator app |
| `vault.totp.code` | Code |
| `vault.totp.verify` | Verify |
| `vault.totp.error` | Invalid code. Please try again. |
| `vault.totp.locked` | The Vault is locked for 15 minutes after five wrong codes. We've emailed you. |
| `vault.totp.notice` | Every open of the Vault asks for a fresh code. Nothing is remembered. |
| `vault.secrets.title` | Stored secrets |
| `vault.secrets.reveal` | Show value |
| `vault.secrets.delete` | Delete |
| `vault.add.name` | Name |
| `vault.add.value` | Value |
| `vault.add.submit` | Add Secret |
| `vault.error.server` | Unable to verify TOTP. Please try again or contact support. |
| `vault.error.retry` | Retry |
| `vault.empty.title` | No secrets stored yet |
| `vault.empty.body` | Add your API keys and credentials below. They will be encrypted at rest and never displayed after saving. |
| `vault.empty.action` | Add Your First Secret |
| `vault.reset` | Reset Vault code |
| `vault.masked` | *** |

## 8. Help Videos

| Process | Help video |
|---|---|
| Opening the Vault | `master_template` help entry `vault.form` — played in the app language |
| Enrolling the Vault's authenticator | `master_template` help entry `vault.totp_enrol` |
| Resetting a lost Vault code | `master_template` help entry `vault.reset` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-04 Opening the Vault | The whole flow: fresh sign-in if needed, the code on every open, view/add/replace, the emailed access record |
| UF-03 Staff invitation | Not this form — the Owner configures keys here before inviting (the gateway keys) |
| UF-38 Error recovery | The server-error and lockout states; `sop-vault-recovery.md` for a lost authenticator |
| UF-01 Client registration | App 5 writes the client's keys into this Vault at provisioning; the Owner sees and replaces them here |
