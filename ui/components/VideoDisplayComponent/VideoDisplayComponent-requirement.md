---
title: "VideoDisplayComponent-requirement"
status: draft
doc-id: "mb-112f8d2d"
updated: "2026-11-25"
---
# VideoDisplayComponent — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `components.VideoDisplayComponent` |
| Purpose | The one video embed for the homepage, the services page, ServiceViewerForm, blog posts and the help-video dialog: display only, consent-gated for outside services, hidden when no address is set (ticket 06, F-09, F-25) |
| Screen type and width | Custom component (embedded); its width is its host's |
| Layout | A component form built from theme components; no source selector or address input inside it (custom-component-requirements-matrix, VideoDisplayComponent) |
| Route | None — a component |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Component container | Anvil `ColumnPanel` (`col_video`) | Anvil canonical container; holds the embed or the placeholder | — | mobile | — |
| Video embed | Anvil `ColumnPanel` (`col_embed`) | Holds the YouTube, Vimeo, Dailymotion or direct-video embed; `video_url` and `video_type` are properties set in code by the host form (custom-component-requirements-matrix: properties `video_url`, `video_type`) | — | — | — |
| Consent placeholder | `m3.Card` (`card_consent`, outlined) | Before consent, the placeholder names the service (YouTube, Vimeo or Dailymotion) with a Load button; a direct video address loads at once (ticket 06; decision D6: `media.mybizz.live` help videos need no consent) | `surface-card` | — | — |
| Placeholder text | `m3.Text` (`text_consent`) | Names the service whose cookies the video would set | `body-small` | — | `video.consent.placeholder` |
| Load button | `m3.Button` (`btn_load`, filled) | Loads the embed only after the visitor accepts cookies | `primary-button` | — | `video.consent.load` |
| Hidden-when-empty | component `visible` set in code | An empty or invalid address hides the component; no user-facing show/hide switch (custom-component-requirements-matrix) | — | — | — |

## 3. Data

| Element | Table | Column |
|---|---|---|
| `video_url` property | the host's record | Homepage: `business_profile.home_video_url`; services page: `business_profile.services_video_url`; blog post: `blog_posts.video_url`; ServiceViewerForm: `services.video_url`; help videos: the address held in `master_template` |
| `video_type` property | the host's record | `business_profile.home_video_type`, `business_profile.services_video_type`, `blog_posts.video_type`, `services.video_type`; detection happens where the address is saved (ticket 06) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the embed | `btn_load_click` | `record_consent` (`server_settings/policy_service`) | A `consents` row records the choice if none exists; the embed then loads. The component never sets a cookie itself (ticket 06) |
| Show the component | host code | — | An empty or invalid address hides the component |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — an empty address hides the component entirely |
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

The component plays whatever its host shows; consent gating is by visitor choice, not role.

## 7. Text

| Phrase key | English text |
|---|---|
| `video.consent.placeholder` | This video is provided by {service} and sets cookies. |
| `video.consent.load` | Load |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The component plays videos; it is not itself explained by a process video |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-16 Content management and edit mode | The host forms' video sections (homepage, services page, blog post) |
| UF-05–UF-10 Booking and orders | The services page's and service viewer's video sections |
| UF-25 Homepage customisation | The video story section of the homepage templates; the section hides when the address is empty |
| F-25 Help | The help-video dialog plays `media.mybizz.live` videos with no consent (decision D6) |
