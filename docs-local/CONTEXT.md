---
title: "CONTEXT"
doc-id: "mb-fcd66dfb"
updated: "2026-10-11"
---
# CONTEXT.md — mb-3-cs (Mybizz Consulting Services)

Domain glossary and key concepts for agent skills that need project context ([[[pdlf:app-factory/phase-b-discover-and-define/16-domain-modeling|Step 16 Domain Modeling]]](C:/pdlf/app-factory/phase-b-discover-and-define/16-domain-modeling.md), `grill-with-docs`, `tdd`, `diagnosing-bugs`, [[[pdlf:app-factory/phase-e-build-and-verify/40-improve-codebase-architecture|Step 40 Improve Codebase Architecture]]](C:/pdlf/app-factory/phase-e-build-and-verify/40-improve-codebase-architecture.md), `codebase-design`).

## Repository architecture

Two repos, one application:

- **`mb-3-cs-project-library`** — documentation, ADRs, wireframes, design artefacts. The planning authority. Does not go to Anvil.
- **`mb-3-cs`** — live application code (Python/Anvil). Build agent writes here under explicit approval. Pushed to Anvil via GitHub VC.

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

Architecture Decision Records live in `adr-local/` (currently 3). Major architectural decisions include:
- Material 3 theme component scope
- Single contacts table design
- Consolidated build sequence
- Brevo email integration (replaces Zoho)
- Client instance architecture
- Dependency-based architecture (not multi-tenant)
- UI customization approach

## Tracking

- **Devlog index**: `devlog-index.md` — reverse-chronological task completion log
- **Judgement trail**: `judgement-trail.md` — flagged issues, decisions, approved deviations
- **Active todo**: [[[pdlf:pdlf-library/docs-local/pdlf-todo|Active PDLF To-Do List]]](C:/pdlf/pdlf-library/docs-local/pdlf-todo.md)

## Multi-project context

This project is part of a multi-project workspace under `C:\projects\`:
- **mb-3-cs** — this project (GStack-managed, transitioning to PDLF)
- **mb4ecom** — E-Commerce (PDLF-managed)
- **mb5pdlf** — PDLF test project (Daily Intentions Tracker)
- **scratch** — test/experiment workspace

Shared infrastructure at `C:\mybizz\`: GStack tools, GBrain database, skills directory, Matt Pocock skills teaching workspace.
