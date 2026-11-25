---
title: "PolicyEditorForm-requirement"
status: draft
doc-id: "mb-5026b033"
updated: "2026-11-25"
---
# PolicyEditorForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `settings.PolicyEditorForm` |
| Purpose | Settings → Legal Pages: the document type selector and the ClauseBuilder for all three documents (ticket 40, F-22; UF-28; DESIGN.md PolicyEditorForm) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels (hosted in the Legal Pages tab) |
| Layout | `layouts.AdminLayout` (opened from the Legal Pages tab) |
| Route | None — opened from `settings.SettingsForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_policy_editor`) | "Legal Pages", `headline-large` | `page-title` | — | `policies.title` |
| Document type selector | `m3.ButtonGroup` (`bgrp_document_type`, connected, single) | Privacy Policy, Terms & Conditions, Cookie Policy (DESIGN.md PolicyEditorForm) | — | mobile | `policies.doc.privacy`, `policies.doc.terms`, `policies.doc.cookie` |
| Document status row | `m3.Text` (`text_document_status`) | The document's version and last publish (`config.legal_versions`) | `body-small` | — | `policies.version` |
| Clause builder | `components.ClauseBuilder` | Embeds the ClauseBuilder component, parameterized by document type (DESIGN.md PolicyEditorForm; ticket 12) | — | mobile | — |
| Help icon | `components.HelpTooltipComponent` (help entry `settings.legal_pages`) | The feature's tooltip and help video | — | — | `help.settings.legal_pages.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| The chosen document | `privacy_policy` / `t_and_c` / `cookie_policy` | the clause tables |
| Version | `config` | `value` of `legal_versions` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Choose the document | `bgrp_document_type_change` | None | The ClauseBuilder loads the document's clauses |
| Publish | through the ClauseBuilder | `publish_policy` (`server_settings/policy_service`; Owner only) | The document is published; the public page updates immediately; the version rises; the R2 copy is kept |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the form always shows its selector and builder |
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

Per `access-matrix.yaml` `settings_tabs.LegalPages` (Owner only).

## 7. Text

| Phrase key | English text |
|---|---|
| `policies.title` | Legal Pages |
| `policies.doc.privacy` | Privacy Policy |
| `policies.doc.terms` | Terms & Conditions |
| `policies.doc.cookie` | Cookie Policy |
| `policies.version` | Version {version}, published {date} |

## 8. Help Videos

| Process | Help video |
|---|---|
| Building your legal pages | `master_template` help entry `settings.legal_pages` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-28 Clause Builder | The host of the whole flow |
| UF-02 Owner onboarding | The wizard's legal acknowledgement step links the responsibility; the documents are built here |
