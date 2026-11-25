---
title: "ClauseBuilder-requirement"
status: draft
doc-id: "mb-109bd9b5"
updated: "2026-11-25"
---
# ClauseBuilder — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `components.ClauseBuilder` |
| Purpose | The shared clause editor for the three legal documents: category selection, seeded clauses (editable, not deletable), custom clauses (editable and deletable), clause editing, drag reordering and switching clauses on or off (ticket 12, F-22) |
| Screen type and width | Custom component hosted by `settings.PolicyEditorForm` (Settings → Legal Pages, 1000 px) |
| Layout | A component form built from theme components; parameterised by document type (`privacy_policy`, `t_and_c`, `cookie_policy`) |
| Route | None — a component |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Component container | Anvil `ColumnPanel` (`col_builder`) | Anvil canonical container; the builder's one column | — | mobile | — |
| Category selector | `m3.DropdownMenu` (`dd_category`) | The theme's dropdown selects the clause grouping (DESIGN.md ClauseBuilder) | `outlined-select` | — | `clauses.category.label` |
| Clauses list | Anvil `ColumnPanel` (`col_clauses`) | The document's clauses, grouped by category in category order then `sort_order` | — | mobile | — |
| Clause row | `m3.Card` (`card_clause`, outlined) | One clause as an outlined card: title, text, its switches | `surface-card` | mobile | — |
| Clause title | `m3.Text` (`text_clause_title`) | The clause's title | `title-medium` | — | — |
| Clause text | `m3.Text` (`text_clause_body`) | The clause's text | `body-medium` | — | — |
| Clause active switch | `m3.Switch` (`sw_clause_active`) | Switches the clause on or off; a `feature_key` clause is active exactly while its feature is in use (ticket 12) | — | — | `clauses.active` |
| Edit link | `m3.Link` (`lnk_edit_clause`) | "Edit" on each clause reveals the edit form (DESIGN.md ClauseBuilder) | `tertiary-button` | — | `clauses.edit` |
| Seeded marker | `m3.Badge` (no count) | Marks a seeded clause (editable, not deletable) | — | — | `clauses.seeded` |
| Delete button (custom clauses only) | `m3.IconButton` (`ibtn_delete_clause`, icon `mi:delete`) | Custom clauses are editable and deletable; seeded clauses are not | `icon-button` | — | `clauses.delete` |
| Edit form | Anvil `ColumnPanel` (`col_clause_editor`, hidden) | Title, text and category inputs for the clause being edited | — | mobile | — |
| Edit title field | `m3.TextBox` (`txt_clause_title_edit`) | The clause's title | `outlined-input` | — | `clauses.title.label` |
| Edit text field | `m3.TextArea` (`txt_clause_text_edit`) | The clause's text | `outlined-input` | — | `clauses.text.label` |
| Edit save button | `m3.Button` (`btn_save_clause`, filled) | Saves the clause and publishes the document | `primary-button` | — | `clauses.save` |
| Edit cancel button | `m3.Button` (`btn_cancel_clause`, text) | Discards the edit | `tertiary-button` | — | `editmode.bar.cancel` |
| Add custom clause button | `m3.Button` (`btn_add_clause`, filled) | "+ Add Custom Clause" reveals the add form (DESIGN.md ClauseBuilder) | `primary-button` | — | `clauses.add` |
| Reorder handles | Anvil `ColumnPanel` drag behaviour | Drag reordering of clauses (custom-component-requirements-matrix, ClauseBuilder) | — | — | `clauses.reorder` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Clauses | `privacy_policy` / `t_and_c` / `cookie_policy` | `category`, `title`, `clause`, `sort_order`, `is_seeded`, `is_active` |
| Cookie feature link | `cookie_policy` | `feature_key` (`maps` / `video` / `payments`; a clause with a `feature_key` cannot be switched off while its feature is in use) |
| Published version | `config` | `legal_versions` (raised by one at each publish) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Choose the document | host code (`PolicyEditorForm`) | None | The component loads the document's clauses grouped by category |
| Switch a clause | `sw_clause_active_change` | None (staged) | The clause's active state is staged for the save |
| Edit a clause | `lnk_edit_clause_click` | None | The edit form reveals with the clause's values |
| Save a clause | `btn_save_clause_click` | `publish_policy` (`server_settings/policy_service`) | Saves the document, publishes the public page immediately, raises the document's version and keeps the published text in R2 at `legal/{document}/v{version}.html` (ticket 12) |
| Add a custom clause | `btn_add_clause_click` | `publish_policy` | The new clause is saved with the document |
| Delete a custom clause | `ibtn_delete_clause_click` | `publish_policy` | The custom clause is removed; seeded clauses cannot be deleted |
| Reorder | the drag handler | `publish_policy` | The new `sort_order` is saved with the document |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | `mi:description` |
| Heading | `clauses.empty.title` |
| Body text | `clauses.empty.body` — the seeded clause set loads per document (ticket 40), so this state appears only before the first load |
| Primary action | `clauses.add` (+ Add Custom Clause) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | none |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

The builder is hosted by the Owner-only Legal Pages tab (ticket 12, B1; access matrix `settings_tabs.LegalPages`).

## 7. Text

| Phrase key | English text |
|---|---|
| `clauses.category.label` | Category |
| `clauses.active` | Published |
| `clauses.seeded` | Standard clause |
| `clauses.edit` | Edit |
| `clauses.delete` | Delete |
| `clauses.add` | + Add Custom Clause |
| `clauses.reorder` | Drag to reorder |
| `clauses.title.label` | Clause title |
| `clauses.text.label` | Clause text |
| `clauses.save` | Save and publish |
| `clauses.empty.title` | No clauses yet |
| `clauses.empty.body` | The standard clauses for this document load here. You can edit them and add your own. |
| `clauses.publish.warn` | Publishing updates the public page straight away and keeps a copy of this version. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Building a legal document | `master_template` help entry `clauses.builder` — played in the app language |
| The client's responsibility for the wording | `master_template` help entry `clauses.responsibility` (the Owner is responsible for the final wording being right for the client's business and jurisdiction; UF-28) |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-28 Clause Builder | The whole flow: choose the document, edit clauses, add custom ones, reorder, switch on or off, save and publish |
| UF-19 Contact form submission | The published Privacy Policy the contact form links to |
| UF-41 Customer accepts a quote | The acceptance box links to the published Terms and Privacy Policy |
| Cookie choice first | The published Cookie Policy the banner's More info opens |
