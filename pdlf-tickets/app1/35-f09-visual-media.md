---
title: "35 — F-09 Visual and media presentation"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-2e1ff7f6"
updated: "2026-11-17"
---

## Description

**Feature:** F-09 Visual and media presentation (custom-component-requirements-matrix: VideoDisplayComponent, ParallaxComponent; `m3.Carousel`; FR-09).
**Depends on:** 06 (VideoDisplayComponent), 07 (ParallaxComponent).

### Forms

- None new. The components are tickets 06 and 07; the media library is ticket 33. This ticket closes the feature: the homepage hero's `m3.Carousel` advanced by a Timer with a pause/play `m3.IconButton` (DESIGN.md, Not built: TimeLapseCarouselComponent), and the media-presentation rules across every host context of the matrix.

### Server functions

- None new.

### Tables

- `business_profile` (video fields), `files` (media library)

### Acceptance tests

- `FR-09`; `ST-11` across the host contexts; the video visibility rule per host (services page, HomePage, blog post, ServiceViewerForm, help videos at `media.mybizz.live`)

### Security criteria

- Consent gating rides the components (ticket 06); `media.mybizz.live` plays with no consent (decision D6) and no cookie-setting Cloudflare feature is enabled on it — checked at release (T1 of the Step 15 tasks)

## Comments
