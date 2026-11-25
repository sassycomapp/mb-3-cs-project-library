---
title: "OnboardingForm-requirement"
status: draft
doc-id: "mb-7aee072a"
updated: "2026-11-25"
---
# OnboardingForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `onboarding.OnboardingForm` |
| Purpose | The resumable 10-step wizard with a progress indicator and a short help video on every step; each step saves when completed; bidirectional navigation; resume at the next unfinished step (ticket 51, F-03; UF-02) |
| Screen type and width | Onboarding — 600 px, centred card with a step indicator (spec-ui-standards section 7) |
| Layout | `layouts.AdminLayout` (the wizard runs on the rail layout with a Sidesheet per architecture reference F-03) |
| Route | None — opened at the Owner's first sign-in while `business_profile.onboarding_status` is `in_progress`, and from Settings' Return to Onboarding |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Wizard container | Anvil `ColumnPanel` (`col_onboarding`) | The wizard's one column, the centred card | — | mobile | — |
| Step card | `m3.Card` (`card_onboarding_step`, elevated) | The step's content, on the Sidesheet per architecture reference section 11 (guided flows carry each step's data in `self.item`) | `elevated-card` | mobile | — |
| Progress indicator | `m3.LinearProgressIndicator` (`lpi_onboarding`, determinate) | Progress indicator visible throughout (DESIGN.md OnboardingForm) | — | — | — |
| Step indicator | `m3.Text` (`text_onboarding_step`) | "Step {n} of 10" | `body-small` | — | `onboarding.step_of` |
| Step 1 — Welcome and orientation | `m3.Heading` + `m3.Text` | Informational only; the Continue button is always enabled (DESIGN.md per-step logic) | `headline-small`, `body-medium` | mobile | `onboarding.welcome.title`, `onboarding.welcome.body` |
| Step 2 — App Language | `m3.DropdownMenu` (`dd_ob_language`) | The choice of the languages whose translation is complete; English preselected; saves `business_profile.app_language` | `outlined-select` | — | `profile.app_language` |
| Step 3 — Business Profile | `m3.Button` (`btn_ob_business_profile`, filled) | Links to Settings → Business Profile; the AI helper offers the first website text | `primary-button` | — | `onboarding.step3.configure` |
| Step 4 — Brand Palette | `m3.Button` (`btn_ob_palette`, filled) | Links to Settings → Brand Palette; Professional Blue applies until a palette is chosen | `primary-button` | — | `onboarding.step4.configure` |
| Step 5 — System Currency | `m3.DropdownMenu` (`dd_ob_currency`) + `m3.Button` (`btn_confirm_currency`, filled) | Dropdown plus confirm button. Required. Locked once the first booking or invoice exists | `outlined-select` | — | `currency.system.select` |
| Step 6 — Legal Acknowledgement | `m3.Text` (`cb_legal_ack` on `m3.Checkbox`), `m3.Checkbox` (`cb_data_governance_ack`), `m3.Button` (`btn_confirm_legal`, filled) | The two checkboxes plus confirm (DESIGN.md Step 6); the button is enabled only when both are ticked; recorded once, cannot be reversed | — | mobile | the checkboxes' texts in section 7 |
| Step 7 — Payment Gateway | `m3.Button` (`btn_ob_gateway`, filled) | Links to Settings → Payment Gateways; bookings that take payment stay unavailable until a gateway is connected | `primary-button` | — | `onboarding.step7.configure` |
| Step 8 — Connect the Domain | `onboarding.OnboardingForm.DomainStep` (local step form) | The DNS records with a live status per record; the MX confirmation dialog appears before the domain's email moves into the Team Inbox | — | mobile | — |
| Step 9 — Import Contacts | `onboarding.OnboardingForm.ImportStep` (local step form) | CSV upload, source tool selector, column mapping table, preview, Import. Skippable | — | mobile | — |
| Step 10 — Completion | `m3.Heading` + `m3.Text` + `m3.Button` (`btn_go_to_dashboard`, filled) | "Go to Dashboard": sets `onboarding_status = "completed"`, `onboarding_completed = True`, navigates to the Dashboard | `headline-small` | mobile | `onboarding.complete.title` |
| Back button | `m3.IconButton` (`ibtn_ob_back`, icon `mi:arrow_back`) | Back and forward navigation between steps (DESIGN.md) | `icon-button` | — | `booking.flow.back` |
| Continue button | `m3.Button` (`btn_ob_continue`, filled) | The per-step Continue button logic of DESIGN.md (enabled, or disabled until confirmed, per step) | `primary-button` | — | `onboarding.continue` |
| Help icon | `components.HelpTooltipComponent` (per step's help entry) | A short help video on every step (ticket 51) | — | — | `help.onboarding.step{n}.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Step state | `business_profile` | `onboarding_status`, `onboarding_completed`, `onboarding_completed_at`, `app_language`, `system_currency`, `legal_ack_accepted`, `legal_ack_accepted_at`, `data_governance_ack_accepted`, `data_governance_ack_accepted_at` |
| The wizard's state | `server_settings/onboarding_service` | the step state and completion |
| Resume | `business_profile.onboarding_status` | an Owner who leaves returns to the next unfinished step (UF-02) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Resume | `form_show` → `resume_onboarding` | `onboarding_service` state (`server_settings/onboarding_service`) | The wizard opens at the next unfinished step |
| Save a step | `btn_ob_continue_click` | `onboarding_service` save | The step is saved when completed; the wizard moves forward |
| Confirm the currency | `btn_confirm_currency_click` | `settings_service` currency functions | The currency is confirmed; step 5's Continue enables after confirmation |
| Confirm the acknowledgement | `btn_confirm_legal_click` | the acknowledgement recording (SR-09) | Both flags and their timestamps are recorded; cannot be reversed; recorded in the instance and in App 5 |
| Open Settings | `btn_ob_business_profile_click` / `btn_ob_palette_click` / `btn_ob_gateway_click` | None | The Settings tab opens; the Owner returns with Return to Onboarding |
| Complete | `btn_go_to_dashboard_click` | `onboarding_service` complete | `onboarding_status = "completed"`; the Dashboard opens, where the sample data banner shows |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the wizard always shows its step card and progress |
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

Owner only (the wizard's scope in the access matrix).

## 7. Text

| Phrase key | English text |
|---|---|
| `onboarding.step_of` | Step {n} of 10 |
| `onboarding.continue` | Continue |
| `onboarding.welcome.title` | Welcome to your Mybizz |
| `onboarding.welcome.body` | Ten short steps and your business is set up. Take them in order, or come back later — nothing is lost. |
| `onboarding.step3.configure` | Configure in Settings |
| `onboarding.step4.configure` | Choose a palette in Settings |
| `onboarding.step7.configure` | Connect a gateway in Settings |
| `onboarding.cb_legal_ack` | I understand I must configure my own Privacy Policy, Terms & Conditions and Cookie Policy in Settings → Legal Pages. Mybizz is not responsible for the content of those pages. |
| `onboarding.cb_data_governance_ack` | I understand that Mybizz retains certain records for legal and compliance purposes as described above. |
| `onboarding.confirm_legal` | Confirm Acknowledgement |
| `onboarding.complete.title` | You're all set |
| `onboarding.complete.body` | Your business is ready. Your sample data is waiting on the dashboard. |
| `onboarding.complete.action` | Go to Dashboard |
| `onboarding.mx_confirm.title` | Move your email to the Team Inbox? |
| `onboarding.mx_confirm.body` | Switching this record moves all email for your domain into your Team Inbox. |
| `onboarding.skip` | Skip this step |
| `onboarding.step9.import` | Import contacts |

## 8. Help Videos

| Process | Help video |
|---|---|
| Every step of onboarding | One video per step (`onboarding.step1` … `onboarding.step10`), one version per language in `master_template`, played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-02 Owner onboarding | The whole wizard, resumable, with bidirectional Settings navigation |
| UF-01 Client registration | The instance side begins at the Owner's first sign-in, here |
