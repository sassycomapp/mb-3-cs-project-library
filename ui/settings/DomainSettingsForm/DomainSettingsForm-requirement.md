---
title: "DomainSettingsForm-requirement"
status: draft
doc-id: "mb-ce514344"
updated: "2026-11-25"
---
# DomainSettingsForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `settings.DomainSettingsForm` |
| Purpose | Settings → Domain: the status of the client's domain for the website and email — one row per DNS record with its live status, Check Now, and the refresh while open (ticket 29, F-18, F-03; DESIGN.md DomainSettingsForm) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` (opened from the Domain tab) |
| Route | None — opened from `settings.SettingsForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Records list | Anvil `ColumnPanel` (`col_dns_records`) | One row per record — website A, MX, three DKIM, MAIL FROM MX, SPF, DMARC (DESIGN.md DomainSettingsForm; spec-team-inbox section 3) | — | mobile | — |
| Record row | `m3.ListItem` (`li_dns_record_*`) | Type, name, value with a copy button, status badge with its word | — | — | — |
| Copy value button | `m3.IconButton` (`ibtn_copy_dns_*`, icon `mi:content_copy`) | Copies the record's value (DESIGN.md DomainSettingsForm) | `icon-button` | — | `plan.kit.copy` |
| Status badge | `m3.Chip` (`chip_dns_status_*`, type `input`, removable=False) | Live, Waiting, Wrong value — the word carries the meaning | — | — | `domain.status.live`, `domain.status.waiting`, `domain.status.wrong` |
| Check Now button | `m3.Button` (`btn_check_dns`, outlined) | The status refreshes on demand (DESIGN.md DomainSettingsForm) | `secondary-button` | — | `domain.check_now` |
| Auto-refresh timer | Anvil `Timer` (`tmr_dns_refresh`) | The status refreshes every few minutes while the form is open (DESIGN.md DomainSettingsForm) | — | — | — |
| Domain display | `m3.Text` (`text_domain_name`) | The client's domain from `email_config.domain` | `body-medium` | — | — |
| Verified summary | `m3.Text` (`text_domain_summary`) | What is verified: website, email receiving, DKIM, MAIL FROM (from `email_config`'s flags) | `body-small` | — | `domain.summary` |
| Help icon | `components.HelpTooltipComponent` (help entry `settings.domain`) | The feature's tooltip and help video | — | — | `help.settings.domain.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Record status | `email_config` | `dns_verified`, `dkim_verified`, `mail_from_verified`, `mx_verified`, `verified_at` |
| The live checks | the live DNS check from App 5 (DESIGN.md DomainSettingsForm Architecture) | the records' statuses |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the tab | `form_show` → `load_domain` | `domain_service` read (`server_settings/domain_service`) | The records and their statuses arrive |
| Check now | `btn_check_dns_click` | `domain_service` check | The statuses refresh |
| Auto-refresh | `tmr_dns_refresh_tick` | `domain_service` check | The statuses refresh every few minutes while the form is open |
| Copy a value | `ibtn_copy_dns_*_click` | None | `navigator.clipboard.writeText` copies the record's value |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Globe icon (`mi:language`) |
| Heading | `domain.empty.title` |
| Body text | `domain.empty.body` — before the domain step has run |
| Primary action | `domain.check_now` |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | none |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per `access-matrix.yaml` `settings_tabs.Domain`.

## 7. Text

| Phrase key | English text |
|---|---|
| `domain.check_now` | Check Now |
| `domain.status.live` | Live |
| `domain.status.waiting` | Waiting |
| `domain.status.wrong` | Wrong value |
| `domain.summary` | Website {state}. Email receiving {state}. DKIM {state}. |
| `domain.record.website` | Website |
| `domain.record.mx` | Email receiving |
| `domain.record.dkim` | Signing (DKIM) |
| `domain.record.mail_from` | Return address |
| `domain.record.spf` | Sender permission (SPF) |
| `domain.record.dmarc` | Fraud protection (DMARC) |
| `domain.empty.title` | No domain connected yet |
| `domain.empty.body` | Your domain's records appear here once you connect it in onboarding. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Reading your domain's records | `master_template` help entry `settings.domain` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-02 Owner onboarding | The wizard's domain step writes the records; this tab shows their live status afterwards |
| UF-39 Email addresses and desks | The Domain tab shows whether every DNS record is live (UF-39 step 5) |
