---
title: "PasswordResetForm-requirement"
status: draft
doc-id: "mb-7e462c30"
updated: "2026-11-25"
---
# PasswordResetForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `auth.PasswordResetForm` |
| Purpose | Request a password-reset link by email; the link opens `auth.SetPasswordForm` (ticket 16, F-01) |
| Screen type and width | Sign-in — 400 px, centred card |
| Layout | `layouts.BlankLayout` |
| Route | None — opened from `auth.LoginForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Centred card container | `m3.Card` (`card_reset`, elevated) | The centred card pattern | `elevated-card` | mobile | — |
| Card content | `m3.Card.CardContentContainer` (`cc_reset`) | The card's own content container | — | — | — |
| Heading | `m3.Heading` (`hdg_reset`) | "Reset your password", `headline-small` | `headline-small` | — | `auth.reset.title` |
| Instruction text | `m3.Text` (`text_reset_hint`) | Explains that a link goes to the email address | `body-medium` | — | `auth.reset.hint` |
| Email field | `m3.TextBox` (`txt_email`, type `email`, outlined) | The address the link is sent to | `outlined-input` | — | `auth.signin.email` |
| Send button | `m3.Button` (`btn_send_reset`, filled) | The card's primary action | `primary-button` | — | `auth.reset.send` |
| Back to sign-in link | `m3.Link` (`lnk_back`) | Returns to `auth.LoginForm` | `tertiary-button` | — | `auth.reset.back` |
| Confirmation | `m3.Text` (`text_reset_sent`, hidden) | The sent confirmation — identical wording whether or not the address exists (ticket 16, B7: reset by email only) | `body-small` | — | `auth.reset.sent` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Email field | `users` | `email` |
| Rate limit | `rate_limits` | `identifier` (`reset:{email}`), `count`, `reset_time` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Send the reset link | `btn_send_reset_click` | `anvil.users.send_password_reset_email` (rate-limited; `server_auth/auth_service` wrapper) | The link is emailed; the form shows the identical confirmation whatever the address; no password is ever sent by email (ticket 16, D7) |
| Back to sign-in | `lnk_back_click` | None | `auth.LoginForm` opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the card always shows its fields |
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

## 7. Text

| Phrase key | English text |
|---|---|
| `auth.reset.title` | Reset your password |
| `auth.reset.hint` | Tell us the email address on your account. We'll send you a link to set a new password. |
| `auth.reset.send` | Send reset link |
| `auth.reset.back` | Back to sign in |
| `auth.reset.sent` | If that address has an account, a reset link is on its way. The link works once and expires. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Resetting a password | `master_template` help entry `auth.reset` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-38 Error recovery | The reset path of a forgotten password |
| UF-02 Owner onboarding | Not this form — the welcome link is `auth.SetPasswordForm`'s |
