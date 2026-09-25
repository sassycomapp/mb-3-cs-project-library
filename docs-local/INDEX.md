---
title: "INDEX"
doc-id: "INDEX"
---
# Mybizz CS — Local Documentation Index

**Project:** mb-3-cs (Consulting & Services)
**Last Updated:** 2026-07-22
**Total Docs:** 17

Project-specific documentation for mb-3-cs. Check this index before writing feature specs, design decisions, or build plans. Read only the specific document relevant to the current task.

Global resources referenced from these documents live in:
- `adr-global/` — Architecture Decision Records
- `specifications-global/` — Binding specifications
- `docs-standard-global/` — Standard document templates and contracts (placeholder — in development)

---

## Architecture & Platform

| File | Title | Description |
|---|---|---|
| [[architecture|Architecture]] | Architecture | Five-app system, package structure, module/navigation architecture, specifications table |
| [[platform-architecture|Platform Architecture]] | Platform Architecture | Vertical scope, feature set, technical stack, business model, platform constraints |
| [[platform-architecture-narrative|Comprehensive Application Treatise]] | Platform Narrative | Comprehensive treatise — executive summary, problem/solution, capabilities, competitive positioning, V2 roadmap |
| [[build-plan|Build Plan (Definitive)]] | Build Plan (Definitive) | Phased implementation plan — 6 phases with deliverables, completion criteria, and review task integration |

---

## Design System

| File | Title | Description |
|---|---|---|
| [[DESIGN|DESIGN]] | DESIGN (Canonical) | Authoritative design system — tokens, typography, spacing, component maps, form/page specs, invoicing/booking design |
| [[design-direction|Design Direction (Canonical)]] | Design Direction (Canonical) | Visual direction, color rule, Anvil role map, project-specific M3 deviations |
| [[DESIGN_palette_section|DESIGN Palette Section]] | Palette System Tokens | Full M3 CSS custom property token sets for all 4 palettes (light + dark mode) |
| [[palette_selector_tokens|Token Swap Blocks]] | Palette Selector Tokens | Python dict token blocks for the palette selector component |
| [[custom-component-requirements-matrix|Custom Component Requirements Matrix]] | Custom Component Requirements Matrix | Shared and local custom components inventory, justification, and V1 scope |

---

## Schema & Feature Specs

| File | Title | Description |
|---|---|---|
| [[authoritative-schema|Authoritative Database Schema]] | Authoritative Database Schema | Complete 44-table schema with field definitions, architectural patterns, and V1 vs V2 scope |
| [[calendar-feature-spec|Calendar Feature Spec]] | Calendar Feature Plan | Anvil + Material 3 calendar implementation — form structure, view architecture, responsive strategy |
| [[user-flows|User Flows]] | User Flows | All 13 user flows — onboarding, booking, admin operations, CRM, campaigns, error recovery |

---

## Business & Strategy

| File | Title | Description |
|---|---|---|
| [[positioning-strategy|Business Strategy & Positioning Document]] | Business Strategy & Positioning | Target market, competitive landscape, pricing, go-to-market, brand identity, support model |
| [[pricing-strategy-consultation|Pricing Strategy Consultation]] | Pricing Strategy Consultation | Market position, pricing comparison, messaging themes, strategic options |
| [[founder-strategic-review|Founder Strategic Review]] | Founder Strategic Review | Strategic product review and direction |

---

## Project Records

| File | Title | Description |
|---|---|---|
| [[CHANGELOG|CHANGELOG]] | Changelog | **EMPTY** — to be populated with versioned change history |
| [[CONTEXT|mb-3-cs (Mybizz Consulting Services)]] | Project Context | **EMPTY** — to be populated with session context, current state, and active decisions |

---

## Notes

- **CONTEXT.md** and **CHANGELOG.md** are currently empty. These gaps are flagged in the documentation integration report.
- **README.md** does not exist in this directory. The project README lives at the repository root.
- All cross-references to global documents (ADRs, specifications) use descriptive slugs (e.g., [[adr-anvil-platform-constraints|Anvil Platform Constraints and Design Boundaries]] ADR, [[spec-deployment|Deployment Specification]]). Legacy numeric-prefix references (e.g., `4-lead-capture-simultaneous-creation.md`) are stale — see the integration report for full findings.
- The `docs-standard-global/` directory contains placeholder documents not yet verified for integration — their correction is a separate workstream.
