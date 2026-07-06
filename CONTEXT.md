# CONTEXT.md — mb-3-cs (Mybizz Consulting Services)

Domain glossary and key concepts for agent skills that need project context (`domain-modeling`, `grill-with-docs`, `tdd`, `diagnosing-bugs`, `improve-codebase-architecture`, `codebase-design`).

## Repository architecture

Two repos, one application:

- **`project-library`** (this repo) — documentation, ADRs, wireframes, design artefacts. The planning authority. Does not go to Anvil.
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

47 Architecture Decision Records in `adr/`. Indexed at `docs/adr-index.md`. Major architectural decisions include:
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
- **Active todo**: `wip/todo.md`

## Multi-project context

This project is part of a multi-project workspace under `C:\projects\`:
- **mb-3-cs** — this project (GStack-managed, transitioning to PDLF)
- **mb4ecom** — E-Commerce (PDLF-managed)
- **mb5pdlf** — PDLF test project (Daily Intentions Tracker)
- **scratch** — test/experiment workspace

Shared infrastructure at `C:\mybizz\`: GStack tools, GBrain database, skills directory, Matt Pocock skills teaching workspace.
