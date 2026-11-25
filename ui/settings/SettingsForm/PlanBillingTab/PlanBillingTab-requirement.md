---
title: "PlanBillingTab-requirement"
status: draft
doc-id: "mb-736c419c"
updated: "2026-11-25"
---
# PlanBillingTab — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `settings.SettingsForm.PlanBillingTab` |
| Purpose | The Settings → Plan and billing tab: the plan and next renewal, the mock-up invoice, and the referral kit on the annual plan (tickets 18 and 48, F-37; UF-45, UF-46) |
| Screen type and width | Settings panel inside `settings.SettingsForm` (1000 px) |
| Layout | Hosted by `settings.SettingsForm` |
| Route | None — a local tab of the hub |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Tab panel container | Anvil `ColumnPanel` (`col_plan_tab`) | The tab's one column | — | mobile | — |
| Plan card | `m3.Card` (`card_plan`, elevated) | The plan and the next renewal date (UF-45 step 1) | `elevated-card` | mobile | — |
| Plan name | `m3.Text` (`text_plan_name`) | The plan (`monthly` / `annual`) | `title-medium` | — | `plan.name` |
| Renewal date | `m3.Text` (`text_renews_on`) | The next renewal date, from `plan_summary.renews_on` | `body-medium` | — | `plan.renews` |
| Mock-up invoice card | `m3.Card` (`card_mockup`, outlined) | The mock-up of the next annual invoice (UF-45 step 2) | `surface-card` | mobile | — |
| Mock-up heading | `m3.Heading` (`hdg_mockup`) | "Your next renewal", `headline-small` | `headline-small` | — | `plan.mockup.title` |
| Mock-up lines | Anvil `ColumnPanel` (`col_mockup_lines`) | The lines as stored — the page renders the stored text and never reformats it (adr-plan-summary-display-text) | — | mobile | — |
| Mock-up line | `m3.Text` (`text_mockup_line_*`) | One line of the stored `plan_summary.lines` | `body-medium` | — | — |
| Mock-up total | `m3.Heading` (`hdg_mockup_total`) | The stored `plan_summary.total`, before tax, as Paddle previews it | `title-medium` | — | — |
| Pending referrals | `m3.Text` (`text_pending_referrals`) | Each pending referral by the referred business's name, below the total (UF-45 step 3) | `body-small` | — | `plan.pending.label` |
| Reserve | `m3.Text` (`text_reserve`) | The confirmed referrals in reserve for the next renewal | `body-small` | — | `plan.reserve.label` |
| Monthly-plan offer | `m3.Card` (`card_annual_offer`, outlined) | On the monthly plan, the annual plan's offer and how to move to it through Paddle's customer portal (UF-45 step 4) | `surface-card` | mobile | — |
| Referral kit section | Anvil `ColumnPanel` (`col_referral_kit`) | The referral kit, annual plan only (UF-46) | — | mobile | — |
| Referral link row | `m3.TextBox` (`txt_referral_link`, outlined, read-only) + `m3.IconButton` (`ibtn_copy_referral`, icon `mi:content_copy`) | The client's referral link with a copy button (`plan_summary.referral_link`; empty on the monthly plan) | `outlined-input` | — | `plan.kit.link`, `plan.kit.copy` |
| Social post buttons | `m3.Button` (`btn_kit_linkedin`, `btn_kit_facebook`, `btn_kit_x`, `btn_kit_whatsapp`, outlined) | The ready-made posts; each copies the post and opens the platform's share window as the blog's Share panel does (F-21) | `secondary-button` | — | `plan.kit.share.<platform>` |
| Email banner switch | `m3.Switch` (`sw_referral_banner`) | Adds the email banner to campaign emails, sequences and broadcasts (`business_profile.referral_banner_in_emails`) | — | — | `plan.kit.banner` |
| Website badge switch | `m3.Switch` (`sw_referral_badge`) | Shows the badge in the public website's footer (`business_profile.referral_badge_on`) | — | — | `plan.kit.badge` |
| Pop-up switch | `m3.Switch` (`sw_referral_popup`) | Shows the pop-up once per visit on the public website (`business_profile.referral_popup_on`) | — | — | `plan.kit.popup` |
| Kit hint | `m3.Text` (`text_kit_hint`) | Every kit item carries the referral link and the line that the client gains a discount when someone joins (F-37) | `body-small` | — | `plan.kit.hint` |
| Help icon | `components.HelpTooltipComponent` (help entry `settings.plan`) | The tab's tooltip and help video | — | — | `help.settings.plan.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| The whole display | `plan_summary` | `plan`, `renews_on`, `currency`, `lines`, `total`, `pending`, `reserve`, `referral_link` — written only by App 5 through `/management/plan-summary` |
| Kit switches | `business_profile` | `referral_banner_in_emails`, `referral_badge_on`, `referral_popup_on` |

The plan summary's amounts are display text in the subscription's currency, exactly as Paddle states them — they are not money records of the client's business, and the whole-cents rule does not apply to them (adr-plan-summary-display-text).

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the tab | `form_show` → `load_plan` | `get_plan_summary` (`server_settings/plan_service`) | The stored row arrives and is rendered as stored (UF-45) |
| Copy the referral link | `ibtn_copy_referral_click` | None | `navigator.clipboard.writeText` copies the link |
| Share a kit post | `btn_kit_<platform>_click` | None | The ready-made post is copied and the platform's own share window opens with the link (UF-46) |
| Switch the email banner | `sw_referral_banner_change` | `update_referral_kit` (`server_settings/plan_service`) | `business_profile.referral_banner_in_emails` is stored; the banner rides the client's campaign emails |
| Switch the badge | `sw_referral_badge_change` | `update_referral_kit` | `business_profile.referral_badge_on` is stored; the badge appears on the public website at once |
| Switch the pop-up | `sw_referral_popup_change` | `update_referral_kit` | `business_profile.referral_popup_on` is stored; the pop-up shows once per visit and sets no cookie |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | `mi:receipt_long` |
| Heading | `plan.empty.title` |
| Body text | `plan.empty.body` — shown before App 5 has written the first `plan_summary` row |
| Primary action | None — the tab has no primary action of its own; the state says App 5's row is on its way |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | none |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per `access-matrix.yaml` `settings_tabs.PlanAndBilling`; a Manager does not see the tab (test 4.22).

## 7. Text

| Phrase key | English text |
|---|---|
| `plan.name` | Your plan |
| `plan.renews` | Renews on {date} |
| `plan.mockup.title` | Your next renewal |
| `plan.pending.label` | Pending referrals |
| `plan.reserve.label` | In reserve for the renewal after |
| `plan.kit.link` | Your referral link |
| `plan.kit.copy` | Copy link |
| `plan.kit.share.linkedin` | Share on LinkedIn |
| `plan.kit.share.facebook` | Share on Facebook |
| `plan.kit.share.x` | Share on X |
| `plan.kit.share.whatsapp` | Share on WhatsApp |
| `plan.kit.banner` | Email banner in your campaigns |
| `plan.kit.badge` | Badge on your website |
| `plan.kit.popup` | Pop-up on your website |
| `plan.kit.hint` | Every item carries your link and the line that you gain a discount when a business joins. |
| `plan.empty.title` | Your plan is on its way |
| `plan.empty.body` | Your plan and renewal appear here as soon as Mybizz's billing records them. |
| `plan.saved` | Referral kit saved. |

## 8. Help Videos

| Process | Help video |
|---|---|
| The plan and billing tab | `master_template` help entry `settings.plan` — played in the app language |
| The referral kit | `master_template` help entry `plan.kit` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-45 Plan and billing | The plan, the renewal and the mock-up invoice |
| UF-46 Sharing the referral kit | The link, the posts, the banner and the badge and pop-up switches |
