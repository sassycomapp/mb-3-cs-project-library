---
title: "SetPasswordForm-requirement"
status: draft
doc-id: "mb-129edf69"
updated: "2026-11-25"
---
# SetPasswordForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `auth.SetPasswordForm` |
| Purpose | Set a password from an emailed link — a new team member, a customer, or a reset (ticket 16, F-01) |
| Screen type and width | Sign-in — 400 px, centred card |
| Layout | `layouts.BlankLayout` |
| Route | None — opened from the emailed link's address |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Centred card container | `m3.Card` (`card_setpw`, elevated) | The centred card pattern | `elevated-card` | mobile | — |
| Card content | `m3.Card.CardContentContainer` (`cc_setpw`) | The card's own content container | — | — | — |
| Heading | `m3.Heading` (`hdg_setpw`) | "Set your password", `headline-small` | `headline-small` | — | `auth.setpw.title` |
| New password field | `m3.TextBox` (`txt_new_password`, outlined, `hide_text=True`) | The new password; minimum 8 characters, no composition rules (SC-01) | `outlined-input` | — | `auth.setpw.new` |
| Confirm password field | `m3.TextBox` (`txt_confirm_password`, outlined, `hide_text=True`) | The confirmation | `outlined-input` | — | `auth.setpw.confirm` |
| Set password button | `m3.Button` (`btn_set_password`, filled) | The card's primary action; disabled until both fields hold a valid pair | `primary-button` | — | `auth.setpw.submit` |
| Field error text | `m3.Text` (`text_setpw_error`, hidden) | Says what to fix: too short, or the two fields differ | `body-small` | — | `auth.setpw.error.short`, `auth.setpw.error.mismatch` |
| Link error state | `m3.Card` (`card_setpw_link_error`, outlined, hidden) | The expired-or-used link state, with a new-link button | `surface-card` | mobile | — |
| New link button | `m3.Button` (`btn_new_link`, outlined) | Opens `auth.PasswordResetForm` (identical wording rules; B7) | `secondary-button` | — | `auth.setpw.link_error.action` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| New password | `users` | `password_hash` (set by Anvil's Users service) |
| The link | `users` | the emailed link's token (Anvil-managed) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Set the password | `btn_set_password_click` | `anvil.users.reset_password` (rate-limited; `server_auth/auth_service` wrapper) | The password is set; the user continues to `auth.TwoFactorForm` to enrol a factor, then to their landing place. A used or expired link shows the link-error state |
| Validate the pair | `txt_new_password_change`, `txt_confirm_password_change` | None | The field error states what to fix (spec-ui-standards section 13.5) |
| New link | `btn_new_link_click` | None | `auth.PasswordResetForm` opens |

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
| `auth.setpw.title` | Set your password |
| `auth.setpw.new` | New password |
| `auth.setpw.confirm` | Confirm new password |
| `auth.setpw.submit` | Set password |
| `auth.setpw.error.short` | Use at least 8 characters. |
| `auth.setpw.error.mismatch` | The two passwords don't match. |
| `auth.setpw.link_error.title` | That link has expired |
| `auth.setpw.link_error.body` | Password links work once and expire. Ask for a new one. |
| `auth.setpw.link_error.action` | Send a new link |

## 8. Help Videos

| Process | Help video |
|---|---|
| Setting a password from the link | `master_template` help entry `auth.setpw` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-01 Client registration | The welcome email's link ends here: the Owner sets a password (UF-01 step 12) |
| UF-02 Owner onboarding | The Owner's first password, then the second factor, then the wizard |
| UF-03 Staff invitation and sign-in | The invitee's link ends here |
| UF-38 Error recovery | The reset link from `auth.PasswordResetForm` ends here |
