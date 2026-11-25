---
title: "SharePanelComponent-requirement"
status: draft
doc-id: "mb-c4dc0e27"
updated: "2026-11-25"
---
# SharePanelComponent — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `components.SharePanelComponent` |
| Purpose | The share panel opened by Share on a published post: the post preview, one AI-written caption per platform (editable and regenerable), and the platform share buttons (ticket 13, F-21) |
| Screen type and width | Custom component opened as a side sheet on desktop and full width on a phone (DESIGN.md Share Panel) |
| Layout | A component form built from theme components; property: the blog post row |
| Route | None — a component |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Panel container | `m3.SidesheetContent` (`side_share`) | The share panel is a side sheet of the host layout (DESIGN.md Share Panel: "A side sheet opened by Share") | — | — | — |
| Post preview card | `m3.Card` (`card_preview`, outlined) | The featured image, title and excerpt (DESIGN.md Share Panel) | `surface-card` | mobile | — |
| Preview image | Anvil `Image` (`img_preview`) | The post's featured image | — | — | — |
| Preview title | `m3.Text` (`text_preview_title`) | The post's title | `title-medium` | — | — |
| Preview excerpt | `m3.Text` (`text_preview_excerpt`) | The post's excerpt | `body-small` | — | — |
| Platform card (one per platform) | `m3.Card` (`card_facebook`, `card_linkedin`, `card_whatsapp`, `card_instagram`, outlined) | One card per platform, each with its editable caption, a Regenerate text button and the platform's share button (DESIGN.md Share Panel) | `surface-card` | mobile | — |
| Caption field | `m3.TextArea` (`txt_caption_<platform>`) | The AI-written caption, editable (custom-component-requirements-matrix: reads and saves `blog_posts.share_captions`) | `outlined-input` | — | `share.caption.<platform>` |
| Regenerate button | `m3.Button` (`btn_regenerate`, text) | Asks the social caption assist for a new set | `tertiary-button` | — | `share.regenerate` |
| Share button (per platform) | `m3.Button` (`btn_share_<platform>`, outlined) | Copies the caption and opens the platform (spec-sm-syndication section 3) | `secondary-button` | — | `share.to.<platform>` |
| Copied confirmation | `m3.Snackbar` (Python class, in code) | "Caption copied — paste it into your post." (DESIGN.md Share Panel) | — | — | `share.copied` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Post preview | `blog_posts` | `featured_image`, `title`, `excerpt` |
| Captions | `blog_posts` | `share_captions` (one caption per platform: `facebook`, `linkedin`, `whatsapp`, `instagram`) |
| Caption generation | — | The social caption assist through the `ai_request_layer`; reads the post's title, excerpt, content, tags and category, writes in the app language (spec-sm-syndication section 4) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Open the panel | the host's Share button (`btn_share_post_click`) | `generate_share_captions` (`server_blog/share_service`) | The captions are written server-side through the `ai_request_layer` on first share; the panel opens with the preview and one caption per platform (ticket 13) |
| Edit a caption | `txt_caption_<platform>_change` | `generate_share_captions` (save path) | The edited caption is saved on the post (spec-sm-syndication: an edited caption is saved) |
| Regenerate | `btn_regenerate_click` | `generate_share_captions` (`server_blog/share_service`) | A new set of captions replaces the stored ones |
| Share to Facebook / LinkedIn | `btn_share_<platform>_click` | None | `navigator.clipboard.writeText` copies the caption; the platform's share window opens with the post's link (both platforms take only the link; the client pastes the caption) |
| Share to WhatsApp | `btn_share_whatsapp_click` | None | The caption and link are pre-filled at `wa.me` |
| Share to Instagram | `btn_share_instagram_click` | None | On a phone, `navigator.share` opens the device share menu with the featured image and the caption; on a computer the image downloads, the caption copies, and the panel says to open Instagram on the phone (spec-sm-syndication section 3) |
| If the AI is unavailable | fallback in code | None | Each caption is filled with the post's title and excerpt, so sharing still works (spec-sm-syndication section 4) |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | `mi:share` |
| Heading | `share.empty.title` |
| Body text | `share.empty.body` — shown when the post has no captions yet and generation is unavailable |
| Primary action | `share.regenerate` (Regenerate) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Owner and Manager, the two roles with access to BlogEditorForm (spec-sm-syndication section 2); caption generation is Owner/Manager only (ticket 13, B1).

## 7. Text

| Phrase key | English text |
|---|---|
| `share.panel.title` | Share this post |
| `share.caption.facebook` | Facebook caption |
| `share.caption.linkedin` | LinkedIn caption |
| `share.caption.whatsapp` | WhatsApp caption |
| `share.caption.instagram` | Instagram caption |
| `share.regenerate` | Regenerate |
| `share.to.facebook` | Share to Facebook |
| `share.to.linkedin` | Share to LinkedIn |
| `share.to.whatsapp` | Share to WhatsApp |
| `share.to.instagram` | Share to Instagram |
| `share.copied` | Caption copied — paste it into your post. |
| `share.instagram.desktop` | Open Instagram on your phone and post this image with the caption. |
| `share.empty.title` | No captions yet |
| `share.empty.body` | The captions will appear here. You can write your own in the meantime. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Sharing a post to each platform | `master_template` help entry `share.panel` — one share to each platform, played in the app language (spec-sm-syndication section 10) |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-42 Sharing a blog post | The whole share flow: preview, captions, one tap per platform |
| UF-16 Content management and edit mode | The Share button appears on publishing (UF-16) |
