---
title: "21 — F-10 Service catalogue"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-6d8e4eee"
updated: "2026-11-17"
---

## Description

**Feature:** F-10 Service catalogue (architecture reference §12 F-10; SR-55; FR-10). Trade feature.
**Depends on:** 16 (auth), 03 (foundation).

### Forms

- `services.ServicesForm` — the admin service list
- `services.ServiceEditorForm` — pricing model (`duration`/`unit`), price, duration, hourly rate, display currency, providers, meeting types, travel buffer, group flag, required resources, payment option, deposit, payment terms, intake schema, images, video, active flag
- `services.ServiceViewerForm`
- `services.ServiceCategoriesForm`
- `services.ResourceListForm` — rooms and equipment (Services → Resources)
- `services.GroupSessionEditorForm` — sessions of a group service (UF-07 set-up)

### Server functions

- `server_services/catalogue_service` (renamed from `server_services/service.py` at this step, per the divergence inventory)
- `server_services/resource_service`
- `server_services/group_session_service`

### Tables

- `services`, `resources`, `booking_metadata_schemas`, `group_sessions`

### Acceptance tests

- `SR-55`; `IN-02`, `ST-02` — access and decorators
- Public prices are the active services' public subset (F-10 public listing is ticket 33's `ServicesPage`)

### Security criteria

- B1, B2 — Owner/Manager edit; every team member views (`ServicesForm` scope all for team roles)
- AC-S1 — every function decorated; money fields are numbers of whole cents of the system currency (schema Rules)

## Comments
