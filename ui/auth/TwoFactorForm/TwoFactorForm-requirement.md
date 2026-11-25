---
title: "TwoFactorForm-requirement"
status: draft
doc-id: "mb-28a71416"
updated: "2026-11-25"
---
# TwoFactorForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `auth.TwoFactorForm` |
| Purpose | Enrol a second factor; enter the code or use the security key at sign-in (ticket 16, F-01; SC-02) |
| Screen type and width | Sign-in — 400 px, centred card |
| Layout | `layouts.BlankLayout` |
| Route | None — opened by the sign-in step and by the enrolment step of first sign-in |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Centred card container | `m3.Card` (`card_2fa`, elevated) | The centred card pattern | `elevated-card` | mobile | — |
| Card content | `m3.Card.CardContentContainer` (`cc_2fa`) | The card's own content container | — | — | — |
| Heading | `m3.Heading` (`hdg_2fa`) | "Second factor", `headline-small` | `headline-small` | — | `auth.2fa.title` |
| Instruction text | `m3.Text` (`text_2fa_hint`) | Explains the factor in use | `body-medium` | — | `auth.2fa.hint` |
| Code field | `m3.TextBox` (`txt_code`, outlined) | The six-digit code from the authenticator app | `outlined-input` | — | `auth.2fa.code` |
| Verify button | `m3.Button` (`btn_verify`, filled) | The card's primary action | `primary-button` | — | `auth.2fa.verify` |
| Security-key action | `m3.Button` (`btn_security_key`, outlined) | Starts the browser's FIDO2/security-key prompt for the Owner's phishing-resistant sign-in (SC-02) | `secondary-button` | — | `auth.2fa.security_key` |
| Enrolment choice (first sign-in) | `m3.RadioGroupPanel` (`rg_factor`) with two `m3.RadioButton` (`rb_authenticator`, `rb_key`) | The theme's radio group offers the two kinds of factor: an authenticator app, or a security key (the Owner is offered the key; a non-Owner may choose either) | — | mobile | `auth.2fa.choose.authenticator`, `auth.2fa.choose.key` |
| Enrolment QR (authenticator) | Anvil `Image` (`img_qr`) | The authenticator's QR code | — | — | — |
| Enrolment confirm button | `m3.Button` (`btn_confirm_enrol`, filled) | Confirms the enrolment | `primary-button` | — | `auth.2fa.enrol_confirm` |
| Error text | `m3.Text` (`text_2fa_error`, hidden) | A wrong code's message; the field clears | `body-small` | — | `auth.2fa.error` |
| Lost-factor link | `m3.Link` (`lnk_lost_factor`) | Anvil's reset by email (architecture reference F-01: a lost second factor is reset by the user through Anvil's reset by email) | `tertiary-button` | — | `auth.2fa.lost` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Second factor | `users` | `mfa` (Anvil-managed: the user's two-factor sign-in methods) |
| Owner's factor restriction | `users` | `role` — the Owner's high-value functions refuse while any non-FIDO2 factor is enrolled (SC-02) |
| Rate limit | `rate_limits` | the sign-in identifier's counter |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Verify a code | `btn_verify_click` (and `txt_code_pressed_enter`) | `anvil.users.mfa` | The sign-in completes when the code is right; a wrong code shows the error and clears the field |
| Use the security key | `btn_security_key_click` | `anvil.users.mfa` (FIDO2) | The browser's prompt completes the sign-in (the Owner's path) |
| Enrol an authenticator | `btn_confirm_enrol_click` | `anvil.users.mfa` | The factor is enrolled; the sign-in completes |
| Lost factor | `lnk_lost_factor_click` | Anvil's two-factor reset | Anvil's reset page and emailed link take over (its own language per spec-translation section 7) |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the card always shows the step it is on |
| Heading | None — not applicable |
| Body text | None — not applicable |
| Primary action | None — not applicable |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | all |
| SignedOut | all |

Every account holder signs in with a second factor (SC-02).

## 7. Text

| Phrase key | English text |
|---|---|
| `auth.2fa.title` | Second factor |
| `auth.2fa.hint` | Enter the code from your authenticator app, or use your security key. |
| `auth.2fa.code` | Code |
| `auth.2fa.verify` | Verify |
| `auth.2fa.security_key` | Use security key or passkey |
| `auth.2fa.choose.title` | Set up a second factor |
| `auth.2fa.choose.authenticator` | Authenticator app |
| `auth.2fa.choose.key` | Security key or passkey |
| `auth.2fa.enrol_confirm` | Confirm |
| `auth.2fa.error` | That code isn't right. Try the next one. |
| `auth.2fa.lost` | I've lost my second factor |

## 8. Help Videos

| Process | Help video |
|---|---|
| Setting up a second factor | `master_template` help entry `auth.twofactor` — played in the app language |
| Using a security key | `master_template` help entry `auth.security_key` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-02 Owner onboarding | The Owner enrols FIDO2 at first sign-in, before the wizard (ticket 51) |
| UF-03 Staff invitation and sign-in | The invitee enrols a factor and signs in |
| UF-29 Customer portal | The customer's sign-in and portal sign-up second factor |
| UF-38 Error recovery | The lost-factor reset path |
