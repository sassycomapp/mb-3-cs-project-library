---
title: "PaymentGatewayForm-requirement"
status: draft
doc-id: "mb-16ac32db"
updated: "2026-11-25"
---
# PaymentGatewayForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `settings.PaymentGatewayForm` |
| Purpose | The Payment Gateways tab: connect the card gateway (Stripe), optionally PayPal; test/live mode; publishable keys on the form, secret keys only in the Vault (ticket 23, F-15; adr-payment-gateway-configuration-is-a-settings-function-and-is-rbac-governed) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` (opened from the Payment Gateways tab of Settings) |
| Route | None — opened from `settings.SettingsForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Stripe connection card | `m3.Card` (`card_stripe`, outlined) | Publishable key, connection status and test/live mode (DESIGN.md PaymentGatewayForm) | `surface-card` | mobile | — |
| Stripe publishable key field | `m3.TextBox` (`txt_stripe_publishable`, outlined) | Safe to expose in client-side code (DESIGN.md PaymentGatewayForm) | `outlined-input` | — | `gateways.stripe.publishable` |
| Stripe status | `m3.Chip` (`chip_stripe_status`, type `input`, removable=False) | Connected / Not connected, as an outlined pill with the word | — | — | `gateways.stripe.connected` |
| Stripe test mode switch | `m3.Switch` (`sw_test_mode`) | Sandbox and live keys (DESIGN.md PaymentGatewayForm) | — | — | `gateways.test_mode` |
| PayPal card | `m3.Card` (`card_paypal`, outlined) | PayPal switch with its own connection status (DESIGN.md PaymentGatewayForm) | `surface-card` | mobile | — |
| PayPal switch | `m3.Switch` (`sw_paypal`) | PayPal alongside the card gateway (DESIGN.md PaymentGatewayForm) | — | — | `gateways.paypal.enable` |
| PayPal client id field | `m3.TextBox` (`txt_paypal_client_id`, outlined) | The public client id (`payment_config.paypal_client_id`) | `outlined-input` | — | `gateways.paypal.client_id` |
| PayPal test mode switch | `m3.Switch` (`sw_paypal_test_mode`) | Sandbox and live for PayPal | — | — | `gateways.paypal.test_mode` |
| PayPal status | `m3.Chip` (`chip_paypal_status`, type `input`, removable=False) | Connected / Not connected | — | — | `gateways.paypal.connected` |
| PayPal unavailable notice | `m3.Text` (`text_paypal_unavailable`, hidden) | PayPal is offered only when the system currency is one PayPal supports (not ISK) (SR-26; architecture reference F-15) | `body-small` | — | `payment.paypal.unavailable` |
| Vault notice | `m3.Card` (`card_vault_notice`, outlined) | A Vault notice explains where secret keys are stored (DESIGN.md PaymentGatewayForm): "No secret key input on this form" | `surface-card` | mobile | — |
| Vault notice text | `m3.Text` (`text_vault_notice`) | The notice | `body-small` | — | `gateways.vault_notice` |
| Open the Vault link | `m3.Link` (`lnk_open_vault`) | Opens `settings.VaultForm` (Owner only) | `tertiary-button` | — | `gateways.open_vault` |
| Save button | `m3.Button` (`btn_save_gateways`, filled) | Saves the publishable configuration | `primary-button` | — | `gateways.save` |
| Saved confirmation | `m3.Snackbar` (Python class, in code) | Confirms the save | — | — | `gateways.saved` |
| Help icon | `components.HelpTooltipComponent` (help entry `settings.gateways`) | The feature's tooltip and help video | — | — | `help.settings.gateways.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Stripe | `payment_config` | `stripe_publishable_key`, `stripe_connected`, `stripe_connected_at`, `test_mode`, `configured_at` |
| PayPal | `payment_config` | `paypal_client_id`, `paypal_test_mode`, `paypal_connected`, `paypal_connected_at` |
| Secret keys | `vault` | `stripe_secret_key`, `stripe_webhook_secret`, `paypal_client_secret`, `paypal_webhook_id` — entered only in the Vault (adr-payment-security-boundary-vault) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the form | `form_show` → `load_gateways` | `settings_service` read (`server_settings/settings_service`) | The publishable values and statuses arrive; secret fields never appear (the hard-fail guard before any broader handler, adr-payment-security-boundary-vault) |
| Save | `btn_save_gateways_click` | `settings_service` save (the payment-config save path) | The publishable keys and modes are saved; gateway changes are audited (AC-S12) |
| Switch PayPal | `sw_paypal_change` | `settings_service` save | PayPal is enabled alongside the card gateway |
| Open the Vault | `lnk_open_vault_click` | None | `settings.VaultForm` opens (Owner only; TOTP on every open) |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the form always shows its cards |
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

Per `access-matrix.yaml` `settings_tabs.PaymentGateways`; gateway configuration is Owner only and RBAC-governed.

## 7. Text

| Phrase key | English text |
|---|---|
| `gateways.stripe.publishable` | Stripe publishable key |
| `gateways.stripe.connected` | Connected |
| `gateways.stripe.not_connected` | Not connected |
| `gateways.test_mode` | Test mode |
| `gateways.paypal.enable` | Offer PayPal |
| `gateways.paypal.client_id` | PayPal client id |
| `gateways.paypal.test_mode` | PayPal test mode |
| `gateways.paypal.connected` | Connected |
| `gateways.vault_notice` | Your secret keys are stored only in the Vault, encrypted, and are never shown on this form. |
| `gateways.open_vault` | Open the Vault |
| `gateways.save` | Save |
| `gateways.saved` | Payment gateways saved. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Connecting your payment gateways | `master_template` help entry `settings.gateways` — played in the app language |
| Where the secret keys live | `master_template` help entry `gateways.vault` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-02 Owner onboarding | The wizard's step 7 links here to connect Stripe and optionally PayPal |
| UF-05–UF-10 Bookings, orders and packages | The gateways these flows pay through |
| UF-04 Opening the Vault | The secret keys' home |
| UF-44 Trying the demo apps | In demo mode payments run on the gateways' test keys |
