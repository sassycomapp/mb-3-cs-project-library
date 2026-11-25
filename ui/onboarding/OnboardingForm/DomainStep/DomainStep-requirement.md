---
title: "DomainStep-requirement"
status: draft
doc-id: "mb-6566675c"
updated: "2026-11-25"
---
# DomainStep — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `onboarding.OnboardingForm.DomainStep` |
| Purpose | Onboarding step 8: the list of website and email DNS records with a live status per record; App 5 writes them in the client's Cloudflare DNS, or the Owner copies them into another provider; the MX confirmation dialog (ticket 51, F-03; UF-02 step 8) |
| Screen type and width | Onboarding — 600 px centred card (the wizard's step panel; the record list may widen to the card's content) |
| Layout | Hosted by `onboarding.OnboardingForm`'s step card |
| Route | None — a local step of the wizard |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Step container | Anvil `ColumnPanel` (`col_domain_step`) | The step's one column | — | mobile | — |
| Records list | Anvil `ColumnPanel` (`col_dns_records_ob`) | The list of website and email DNS records with a live status per record (DESIGN.md OnboardingForm step 8) | — | mobile | — |
| Record row | `m3.ListItem` (`li_ob_dns_*`) | Type, name, value with a copy button, status badge | — | — | — |
| Copy value button | `m3.IconButton` (`ibtn_copy_ob_dns_*`, icon `mi:content_copy`) | The Owner copies the records into another provider | `icon-button` | — | `plan.kit.copy` |
| Status badge | `m3.Chip` (`chip_ob_dns_status_*`, type `input`, removable=False) | Live, Waiting, Wrong value | — | — | `domain.status.live` |
| Check timer | Anvil `Timer` (`tmr_ob_dns`) | The status updates every few minutes (DESIGN.md OnboardingForm step 8) | — | — | — |
| MX confirmation dialog | `m3.Dialog` (`dlg_mx_confirm`) | A confirmation dialog appears before the MX record moves the domain's email into the Team Inbox (DESIGN.md OnboardingForm step 8) | — | — | — |
| Email-created notice | `m3.Text` (`text_ob_email_created`) | The Owner's personal address and the default sender address are created (UF-02 step 8) | `body-small` | — | `onboarding.domain.email_created` |
| Provider choice note | `m3.Text` (`text_ob_provider_note`) | App 5 writes the records, or the Owner copies them | `body-small` | — | `onboarding.domain.provider` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Record status | `email_config` | `dns_verified`, `dkim_verified`, `mail_from_verified`, `mx_verified` |
| The live checks | the live checks through `domain_service` (ticket 51) | the records' statuses |
| Addresses created | `email_addresses` | the Owner's personal address and the default sender |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the records | `form_show` → `load_ob_dns` | `domain_service` read (`server_settings/domain_service`) | The records and their statuses arrive |
| Refresh the statuses | `tmr_ob_dns_tick` | `domain_service` check | The statuses update every few minutes; the wizard checks until every one is live |
| Confirm the MX move | `dlg_mx_confirm`'s confirm action | `domain_service` MX write | The MX record moves the domain's email into the Team Inbox |
| Copy a value | `ibtn_copy_ob_dns_*_click` | None | The record's value is copied |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Globe icon (`mi:language`) |
| Heading | `onboarding.domain.empty.title` |
| Body text | `onboarding.domain.empty.body` |
| Primary action | `onboarding.continue` (Continue — the step can be completed later) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | none |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

## 7. Text

| Phrase key | English text |
|---|---|
| `onboarding.domain.title` | Connect your domain |
| `onboarding.domain.hint` | These records put your website and your email on your own domain. |
| `onboarding.domain.provider` | We write these in your Cloudflare DNS. If your DNS lives elsewhere, copy each record across. |
| `onboarding.domain.email_created` | Your personal address and your default sender address are ready. |
| `onboarding.domain.empty.title` | No records yet |
| `onboarding.domain.empty.body` | Your records appear here once we prepare your domain. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Connecting your domain | `master_template` help entry `onboarding.domain` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-02 Owner onboarding | Step 8 of the wizard |
| UF-39 Email addresses and desks | The Domain tab continues this work after onboarding |
