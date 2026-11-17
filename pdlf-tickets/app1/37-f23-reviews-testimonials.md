---
title: "37 — F-23 Reviews and testimonials"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-2794637e"
updated: "2026-11-17"
---

## Description

**Feature:** F-23 Reviews and testimonials (UF-27; SR-39; FR-23). Trade feature (the review of a booking).
**Depends on:** 24 (bookings completed trigger the request), 08 (StarRatingComponent), 20 (contacts).

### Forms

- `reviews.ReviewListForm` — approve or decline; awaiting-approval queue
- `reviews.ReviewViewerForm`
- `reviews.ReviewSubmitForm` — public at `/review/:token`: star rating (1–5) and comment

### Server functions

- `server_reviews/review_service` — the request sent when a booking is completed (with the automatic task for the business), the token page, approval and decline
- `server_reviews/testimonial_service` — typed-in testimonials, up to 10 with `show_on_homepage` together with reviews

### Tables

- `reviews`, `testimonials`, `bookings`, `contacts`, `contact_events` (the review event)

### Acceptance tests

- `SR-39`; UF-27; the review lifecycle (requested → awaiting_approval → approved/declined; test-plan §19); only approved reviews show publicly

### Security criteria

- B1 — the token page is intentionally public; everything else per the access matrix
- AC-S2 — a customer's review is linked to their own booking

## Comments
