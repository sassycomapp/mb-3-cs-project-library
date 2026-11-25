---
title: "ReaderShareComponent-requirement"
status: draft
doc-id: "mb-48e52d9e"
updated: "2026-11-25"
---
# ReaderShareComponent — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `components.ReaderShareComponent` |
| Purpose | The reader share buttons on the public blog post: Facebook, LinkedIn, WhatsApp and Copy link, below the post title and at the end of the post (ticket 14, F-21; DESIGN.md Share Panel) |
| Screen type and width | Custom component (embedded in the blog post templates); its width is its host's |
| Layout | A component form built from theme components |
| Route | None — a component |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Component container | Anvil `FlowPanel` (`flow_reader_share`) | The icon buttons in one row that wraps on a phone | — | mobile | — |
| Facebook button | `m3.IconButton` (`ibtn_share_facebook`, icon `mi:facebook`) | The theme's icon button opens Facebook's share window with the post's link (spec-sm-syndication section 7) | `icon-button` | — | `share.reader.facebook` |
| LinkedIn button | `m3.IconButton` (`ibtn_share_linkedin`, icon `mi:linkedin`) | Opens LinkedIn's share window | `icon-button` | — | `share.reader.linkedin` |
| WhatsApp button | `m3.IconButton` (`ibtn_share_whatsapp`, icon `mi:chat`) | Opens WhatsApp with the post's link | `icon-button` | — | `share.reader.whatsapp` |
| Copy link button | `m3.IconButton` (`ibtn_copy_link`, icon `mi:link`) | Copies the post's address with `navigator.clipboard.writeText` | `icon-button` | — | `share.reader.copy` |
| Copied confirmation | `m3.Snackbar` (Python class, in code) | Confirms the copy | — | — | `share.reader.copied` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| The post's share address | `blog_posts` | `slug` — the public address `/blog/{slug}`; every reader share carries the platform's `?ref=` value (spec-sm-syndication sections 6 and 7) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Share to Facebook | `ibtn_share_facebook_click` | None | `https://www.facebook.com/sharer/sharer.php?u={post address}?ref=facebook` opens in a new tab |
| Share to LinkedIn | `ibtn_share_linkedin_click` | None | LinkedIn's share-offsite address opens with the post's link |
| Share to WhatsApp | `ibtn_share_whatsapp_click` | None | `wa.me` opens with the post's link |
| Copy link | `ibtn_copy_link_click` | None | The post's address is copied; the snackbar confirms |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the buttons always show on a published post |
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

Every reader of the public post sees the buttons.

## 7. Text

| Phrase key | English text |
|---|---|
| `share.reader.facebook` | Share on Facebook |
| `share.reader.linkedin` | Share on LinkedIn |
| `share.reader.whatsapp` | Share on WhatsApp |
| `share.reader.copy` | Copy link |
| `share.reader.copied` | Link copied. |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The buttons are self-explanatory share actions |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-20 Lead capture | A reader share lands the next visitor on the post with `?ref=`; a later booking records `contacts.source = social` (spec-sm-syndication section 6) |
| UF-42 Sharing a blog post | The reader-side half of the syndication flow |
