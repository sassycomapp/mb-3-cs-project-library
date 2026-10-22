---
title: "CONTEXT"
doc-id: "mb-fcd66dfb"
updated: "2026-10-22"
---
# CONTEXT.md — mb-3-cs (Mybizz Consulting Services)

Domain glossary and key concepts for agent skills that need project context ([[[pdlf:app-factory/phase-b-discover-and-define/b16-reference|Step 16 Domain Modeling]]](C:/pdlf/app-factory/phase-b-discover-and-define/b16-reference.md), `grill-with-docs`, `tdd`, `diagnosing-bugs`, [[[pdlf:app-factory/phase-e-build-and-verify/e40-reference|Step 40 Improve Codebase Architecture]]](C:/pdlf/app-factory/phase-e-build-and-verify/e40-reference.md), `codebase-design`).

## Repository architecture

One project folder, `C:\dev\dev-mb-3-cs\`, holds five repositories:

- **`mb-3-cs-project-library`** — documentation, ADRs, wireframes, design artefacts. The planning authority. Does not go to Anvil.
- **`mb-3-cs-app1`** — App 1, the development workspace: live application code (Python/Anvil). The build agent writes here under explicit approval.
- **`mb-3-cs-app2`** — App 2, `master_template`: receives App 1's tagged releases (Step 42). Every client instance depends on it.
- **`mb-3-cs-app3`** — App 3, `blank_client_template`: the released schema, cloned for each new client.
- **`mb-3-cs-app5`** — App 5, Mybizz management.

Client instances (App 4) live on each client's own Anvil account.

## Anvil.works context

Built on Anvil.works with Material 3 theme, M3 dependency, and MUI overlay.
- **Protected files** (never change without explicit permission): `anvil.yaml`, `standard-page.html`, `theme.css`, `parameters.yaml`, `templates.yaml`, `**/form_template.yaml`
- **Plan/Build boundary**: Plan reads, Build writes. Plan does not write. Ever.
- **Color system**: CSS custom properties from MUI palette. No hardcoded hex/rgb/hsl.

## Application domain

### Multi-vertical consulting platform

The mb-3-cs application serves Mybizz Consulting Services, a consulting practice with multiple service verticals. The application manages clients, contacts, bookings, services, invoices, email campaigns, blog posts, landing pages, segments, tasks, and time entries.

### Core entities

- **Contact** — a person record (lead, client, or other relationship)
- **Client** — an organisation or individual with an active business relationship
- **Booking** — a scheduled service engagement between a client and the consulting practice
- **Service** — a consulting service offering that can be booked
- **Invoice** — billing record for services rendered
- **Segment** — a marketing/list segmentation of contacts for email campaigns
- **Business Profile** — the consulting practice's own configuration and settings
- **Task** — a work item in the task management / project workflow system
- **Time Entry** — a time-tracking record associated with a task or booking
- **Email Campaign** — a marketing email sent to a segment of contacts
- **Landing Page** — a public-facing marketing page
- **Blog Post** — a content article on the public blog

### Key ADR topics

Architecture Decision Records live in `adr-local/` and, for every Mybizz application, in `project-library-global\adr-global\`. Major architectural decisions include:
- Material 3 theme component scope
- Single contacts table design
- Consolidated build sequence
- Brevo email integration (replaces Zoho)
- Client instance architecture
- Dependency-based architecture (not multi-tenant)
- UI customization approach

## Tracking

- **Step outputs**: `pdlf-outputs\{step}\` — every PDLF step's receipt and artefacts for this project
- **Active todo**: [[[pdlf:pdlf-library/docs-local/pdlf-todo|Active PDLF To-Do List]]](C:/pdlf/pdlf-library/docs-local/pdlf-todo.md)

## Multi-project context

This project is one of the Mybizz businesses PDLF builds, each in its own project folder under `C:\dev\`:
- **`mb-3-cs`** — this project (`C:\dev\dev-mb-3-cs\`)
- **`mb-4-ecom`** — e-commerce (`C:\dev\dev-mb-4-ecom\`)
- **`mb-5-pdlf`** — PDLF's test project, the Daily Intentions Tracker (`C:\dev\dev-mb-5-pdlf\`)

Shared rules for every Mybizz application are in `C:\dev\project-library-global\`; PDLF itself is in `C:\pdlf\`.
