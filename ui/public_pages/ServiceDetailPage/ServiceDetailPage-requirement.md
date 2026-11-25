---
title: "ServiceDetailPage-requirement"
status: draft
doc-id: "mb-8a47cd59"
updated: "2026-11-25"
---
# ServiceDetailPage — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `public_pages.ServiceDetailPage` |
| Purpose | A service's public page: everything a visitor needs before booking or ordering (ticket 33, F-06, F-10) |
| Screen type and width | Public page — 900 px, full width: content sections |
| Layout | `layouts.PublicLayout` |
| Route | `/services/{slug}` — Anvil Routing; `Route.meta` sets the share tags |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page container | Anvil `ColumnPanel` (`col_service_detail`) | The page's one content column | — | mobile | — |
| Service images | `m3.Carousel` (`car_service_images`, layout `multi_browse`) | The service's images (`services.images`) | — | mobile | — |
| Title | `m3.Heading` (`hdg_service_detail_title`) | The service's name | `headline-large` | — | — |
| Price display | `m3.Text` (`text_service_price`) | The price, in the display currency if set | `title-medium` | — | — |
| Description | `m3.Text` (`text_service_description`) | The service's description | `body-large` | mobile | — |
| Video section | `components.VideoDisplayComponent` | The service's video from `services.video_url`; hidden when empty | — | mobile | — |
| Details section | `m3.Card` (`card_service_details`, outlined) | Duration, meeting types, provider information, what's included | `surface-card` | mobile | — |
| Book button | `m3.Button` (`btn_book_service`, filled) | Opens `bookings.BookingFlowForm` with the service preselected (a `duration` service) | `primary-button` | — | `public.service.book` |
| Order button | `m3.Button` (`btn_order_service`, filled) | Opens `orders.OrderFlowForm` (a `unit` service) | `primary-button` | — | `public.service.order` |
| Package purchase button | `m3.Button` (`btn_buy_package`, outlined) | A package's purchase (UF-10 step 1) | `secondary-button` | — | `public.service.buy_package` |
| Footer | `components.FooterComponent` | The shared footer | — | — | — |

## 3. Data

| Element | Table | Column |
|---|---|---|
| The service | `services` | the active service's public subset: `name`, `slug`, `description`, `price`, `duration_minutes`, `pricing_model`, `display_currency`, `images`, `video_url`, `meeting_types` |
| Packages | `packages` | the active packages of this service |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the page | the route handler | `page_content_service` public read | The service's public subset arrives; `Route.meta` sets the share tags |
| Book | `btn_book_service_click` | None | The booking flow opens with the source carried (UF-20 funnel) |
| Order | `btn_order_service_click` | None | The order flow opens |
| Buy a package | `btn_buy_package_click` | the package purchase path | The package is purchased (UF-10) |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Service icon (`mi:spa`) |
| Heading | `public.service.unavailable.title` |
| Body text | `public.service.unavailable.body` — the service is not active |
| Primary action | `public.nav.services` |

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
| `public.service.book` | Book this service |
| `public.service.order` | Order this service |
| `public.service.buy_package` | Buy a package |
| `public.service.unavailable.title` | This service isn't available |
| `public.service.unavailable.body` | It may have been retired. See all our services. |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The public page explains itself |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-05–UF-08 Bookings | The Book button starts the flow |
| UF-09 Ordering delivered work | The Order button starts the order |
| UF-10 Buying a session package | The Buy a package button |
