---
title: "ServicesPage-requirement"
status: draft
doc-id: "mb-9f83dd82"
updated: "2026-11-25"
---
# ServicesPage — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `public_pages.ServicesPage` |
| Purpose | The public services listing: the active services grouped by category, with their prices and the video section (ticket 33, F-06, F-10) |
| Screen type and width | Public page — 900 px, full width: content sections |
| Layout | `layouts.PublicLayout` |
| Route | `/services` — Anvil Routing; `Route.meta` sets the share tags. The booking and order flows' entry point (UF-05–UF-10) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page container | Anvil `ColumnPanel` (`col_services_page`) | The page's one content column | — | mobile | — |
| Video section | `components.VideoDisplayComponent` | The services page's video from `business_profile.services_video_url`; hidden when empty | — | mobile | — |
| Category section | `m3.Heading` (`hdg_category_*`) | One section per category (ticket 21: the public listing is grouped) | `headline-small` | — | — |
| Service card | `m3.InteractiveCard` (`icard_public_service_*`, outlined) per service | The service's name, price (in the display currency if set), and a short description; the card opens the service's detail page | `surface-card` | mobile (320 px card in a grid) | — |
| Service image | Anvil `Image` (`img_public_service_*`) | The service's first image | — | — | — |
| Footer | `components.FooterComponent` | The shared footer | — | — | — |
| Edit mode host | `components.EditModeComponent` | Placed once on the page (ticket 11) | — | — | `editmode.button` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Services | `services` | the active services' public subset: `name`, `slug`, `description`, `category`, `price`, `display_currency`, `images` (ticket 21: the public subset) |
| Page content | `pages` | `components` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the page | the route handler | `page_content_service` public read | The active services' public subset arrives; `Route.meta` sets the share tags |
| Open a service | `icard_public_service_*_click` | None | `public_pages.ServiceDetailPage` opens for the service |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Service icon (`mi:spa`) |
| Heading | `public.services.empty.title` |
| Body text | `public.services.empty.body` — when the business has no active services yet |
| Primary action | None — the message says services are on their way |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | all |
| SignedOut | all |

## 7. Text

| Phrase key | English text |
|---|---|
| `public.services.empty.title` | Our services are on their way |
| `public.services.empty.body` | We're preparing our services. Come back soon. |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The public page explains itself |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-05–UF-10 Bookings, orders and packages | The entry point of the flows |
| UF-16 Content management and edit mode | The page's edit mode |
