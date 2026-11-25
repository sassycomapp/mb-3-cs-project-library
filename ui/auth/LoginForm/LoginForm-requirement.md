---
title: "LoginForm-requirement"
status: draft
doc-id: "mb-1352dda1"
updated: "2026-11-25"
---
# LoginForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `auth.LoginForm` |
| Purpose | The app's own sign-in screen and the startup form: routes by role before `init_components()`; signs the user in with password and second factor (ticket 16, F-01) |
| Screen type and width | Sign-in — 400 px, centred card (spec-ui-standards section 7) |
| Layout | `layouts.BlankLayout` |
| Route | None — the startup form named in `anvil.yaml` (ticket 16) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Centred card container | `m3.Card` (`card_signin`, elevated) | The sign-in card is the screen's one centred card (spec-ui-standards section 7: centred card) | `elevated-card` | mobile | — |
| Card content | `m3.Card.CardContentContainer` (`cc_signin`) | The card's own content container (spec-ui-standards section 7.7) | — | — | — |
| Heading | `m3.Heading` (`hdg_signin`) | "Sign in", `headline-small` (spec-ui-standards section 8.2: section title) | `headline-small` | — | `auth.signin.title` |
| Email field | `m3.TextBox` (`txt_email`, type `email`, outlined) | The theme's text box with the outlined appearance (spec-ui-standards section 8.3) | `outlined-input` | — | `auth.signin.email` |
| Password field | `m3.TextBox` (`txt_password`, outlined, `hide_text=True`) | A password field hides its text (spec-ui-standards section 8.5) | `outlined-input` | — | `auth.signin.password` |
| Remember-me switch | `m3.Switch` (`sw_remember`) | The theme's switch for the remember-me choice (at most 7 days per `ts-config.yaml` `max_remember_me_days`) | — | — | `auth.signin.remember` |
| Sign in button | `m3.Button` (`btn_sign_in`, filled) | The screen's one filled button, its primary action | `primary-button` | — | `auth.signin.submit` |
| Forgot-password link | `m3.Link` (`lnk_forgot`) | Opens `auth.PasswordResetForm` | `tertiary-button` | — | `auth.signin.forgot` |
| Error text | `m3.Text` (`text_signin_error`, hidden) | The failed sign-in message; identical wording for unknown email and wrong password (ticket 16, B7) | `body-small` | — | `auth.signin.error` |
| Lockout notice | `m3.Text` (`text_lockout`, hidden) | Shown while the sign-in rate limiter holds the account or address | `body-small` | — | `auth.signin.locked` |
| Help icon | `components.HelpTooltipComponent` (help entry `auth.signin`) | The feature's tooltip and help video beside the heading (F-25) | — | — | `help.auth.signin.tooltip` |

Routing in `__init__` (before `init_components()`, architecture reference section 10): team member → `dashboard.DashboardForm` in AdminLayout (Owner → `onboarding.OnboardingForm` while `business_profile.onboarding_status` is `in_progress`); Customer → the portal in CustomerLayout; a public address → the route; signed out → this form. `open_form()` in `__init__` is always followed by `return`. Startup theming calls (`m3x_theming.apply_saved()`, `set_palette`, `set_mode("system")`) are completed at ticket 19 (ticket 16).

## 3. Data

| Element | Table | Column |
|---|---|---|
| Email field | `users` | `email` |
| Password field | `users` | `password_hash` (checked by Anvil's Users service; never read by the form) |
| Remember-me | `users` | Anvil's remembered logins (`remembered_logins`) |
| Routing role | `users` | `role` |
| Onboarding routing | `business_profile` | `onboarding_status` |
| Error state | `users` | `account_status` (a deactivated user cannot sign in) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Sign in | `btn_sign_in_click` (and `txt_password_pressed_enter`) | `anvil.users.login_with_email` with the second-factor step | The user signs in; the routing above applies. A refusal shows `auth.signin.error` with identical wording for unknown email and wrong password |
| Second factor | `auth.TwoFactorForm` | `anvil.users.mfa` | The code or the security key completes the sign-in; the Owner's high-value functions refuse while any non-FIDO2 factor is enrolled (SC-02) |
| Forgot password | `lnk_forgot_click` | None | `auth.PasswordResetForm` opens |
| Lockout | the rate limiter (`rate_limits`) | — | Ten failures lock the account; the notice shows and clears when the window ends |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the sign-in card always shows its fields |
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

The screen is reachable by everyone; sign-up is off (ticket 16: sign-up is off; `auth.SignupForm` is deleted).

## 7. Text

| Phrase key | English text |
|---|---|
| `auth.signin.title` | Sign in |
| `auth.signin.email` | Email |
| `auth.signin.password` | Password |
| `auth.signin.remember` | Remember me on this device |
| `auth.signin.submit` | Sign in |
| `auth.signin.forgot` | Forgot your password? |
| `auth.signin.error` | That email and password don't match an account. |
| `auth.signin.locked` | Too many attempts. Try again in a few minutes. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Signing in | `master_template` help entry `auth.signin` — played in the app language |
| Setting up your second factor | `master_template` help entry `auth.twofactor` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-02 Owner onboarding | The first sign-in routes the Owner to the onboarding wizard |
| UF-03 Staff invitation and sign-in | The invitee signs in here and lands on the Dashboard |
| UF-29 Customer portal | The customer's sign-in; the portal opens |
| UF-38 Error recovery | A session timeout returns the user here; sign-in returns them to where they were |
| UF-44 Trying the demo apps | The demo sign-in starts here with the demo visitor's email |
