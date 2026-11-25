---
title: "EditModeComponent-requirement"
status: draft
doc-id: "mb-d40a0072"
updated: "2026-11-25"
---
# EditModeComponent — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `components.EditModeComponent` |
| Purpose | In-place editing of a published public page, landing page or blog post: the Edit button, the edit-mode marks, in-place editing of text, image and video, the AI helper for text, and one server call that saves every change (ticket 11, F-06, F-08, F-21) |
| Screen type and width | Custom component (embedded once on each public page, landing page template and blog post template); its width is its host's |
| Layout | A component form built from theme components; the host form registers its editable elements and the content record they belong to |
| Route | None — a component |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Edit button | `m3.FloatingActionButton` (`fab_edit`, icon `mi:edit`, floating) | A signed-in Owner or Manager sees the floating Edit button at the bottom right of every public page, landing page and published blog post (DESIGN.md Edit Mode) | `fab-button` | — | `editmode.button` |
| Edit-mode top bar | `m3.Toolbar` (`tbar_editmode`, docked, `color_style="vibrant"`, `pin_to_screen=True`) | The slim top bar shows the editing state and holds Save and Cancel (DESIGN.md Edit Mode); visible only in edit mode | — | — | — |
| Top bar message | `m3.Text` (`text_editing`) | "Editing — changes are not live until you save" | `body-medium` | — | `editmode.bar.message` |
| Save button | `m3.Button` (`btn_save_changes`, filled) | Save publishes at once | `primary-button` | — | `editmode.bar.save` |
| Cancel button | `m3.Button` (`btn_cancel_changes`, outlined) | Cancel discards every change | `secondary-button` | — | `editmode.bar.cancel` |
| Editable-text outline | role `editmode-editable` | In edit mode every editable text element gets a dashed `outline` border and a small edit icon on hover and focus (DESIGN.md Edit Mode); the host registers the element and the component marks it | — | — | — |
| Text editor | `m3.TextArea` (`txt_edit_text`) | Opens in place over the selected text element for typing, with the AI helper button beside it (DESIGN.md Edit Mode) | `outlined-input` | — | `editmode.text.editor` |
| AI helper | `components.AIHelperComponent` (the host's trigger) | Offers write / rewrite shorter / warmer / more professional / translate for the selected text (spec-ai-implementation, Website text) | — | — | `ai.open` |
| Image upload dialog | `m3.Dialog` (`dlg_upload_image`) | Selecting an image opens an upload dialog | — | — | `editmode.image.upload` |
| Upload field | `m3.FileLoader` (`fu_image`) | The theme's file loader takes the new image; it is stored in `files` | — | — | `editmode.image.choose` |
| Video address field | `m3.TextBox` (`txt_video_url`) | Selecting a video opens its address field (DESIGN.md Edit Mode) | `outlined-input` | — | `editmode.video.address` |
| Confirmation snackbar | `m3.Snackbar` (Python class, in code) | Save publishes at once and shows the theme's snackbar confirmation (DESIGN.md Edit Mode) | — | — | `editmode.saved` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Registered content record | the host's record | Public page: `pages.components`; homepage: `config` `home_config`; landing page: `landing_pages.config`; blog post: `blog_posts.content` (the column blog posts hold their content in) |
| Uploaded images | `files` | `file`, `path` |
| Video addresses | the host's record | the address field the host registers |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Enter edit mode | `fab_edit_click` | None | The marks appear; the top bar shows |
| Edit a text element | `txt_edit_text_change` | None | The element holds the typed text in place |
| Ask the AI for text | the AI trigger inside the text editor | `request(assist, inputs, context)` (`server_ai/ai_request_layer`) | The result replaces the selected text when the user accepts it; nothing is published until Save |
| Upload an image | `fu_image_change` | None | The image is staged for the save |
| Set a video address | `txt_video_url_change` | None | The address is staged |
| Save | `btn_save_changes_click` | `save_page_content` (`server_settings/page_content_service`) | Every change saves in one server call and publishes at once; the save is recorded in `activity_log` (ticket 11; SC-12, IN-15); the snackbar confirms |
| Cancel | `btn_cancel_changes_click` | None | Every change is discarded; the published content stays |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the component is the Edit button and the edit-mode chrome |
| Heading | None — not applicable |
| Body text | None — not applicable |
| Primary action | None — not applicable |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Visible only to a signed-in Owner or Manager (custom-component-requirements-matrix, EditModeComponent); the save function carries exactly those roles (ticket 11, B1).

## 7. Text

| Phrase key | English text |
|---|---|
| `editmode.button` | Edit this page |
| `editmode.bar.message` | Editing — changes are not live until you save |
| `editmode.bar.save` | Save |
| `editmode.bar.cancel` | Cancel |
| `editmode.text.editor` | Edit text |
| `editmode.image.upload` | Replace image |
| `editmode.image.choose` | Choose an image |
| `editmode.video.address` | Video address |
| `editmode.saved` | Changes are live. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Editing a page in place | `master_template` help entry `editmode.page` — played in the app language |
| Writing text with the AI helper | `master_template` help entry `editmode.ai_text` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-16 Content management and edit mode | The whole edit-mode flow: Edit → edit in place → Save (publishes at once) or Cancel (discards) |
| UF-17 Homepage customisation | The homepage editor's alternative path: edit the page itself |
| UF-18 Landing page creation | Edit mode on the landing page (UF-18 step 3) |
| UF-42 Sharing a blog post | Correcting a published post in edit mode |
