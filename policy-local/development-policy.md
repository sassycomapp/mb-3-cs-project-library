---
title: "development-policy"
doc-id: "mb-509458a0"
updated: "2026-10-01"
---
# Mybizz CS — Development Policy

**Category:** Policy (not Specification — see note below)
**Authority:** Mandatory — all development work must conform to this policy

**Note on this revision:** This document previously mixed Policy content with Specification
content. The Specification content (M3 usage rules, navigation, code quality, error handling,
naming conventions, package structure, performance standards, security, and regulatory
compliance) was already fully covered by [[[default:specifications-global/spec-anvil-platform-standards|Anvil Platform Mechanics]]](C:/dev/project-library-global/specifications-global/spec-anvil-platform-standards.md),
[[[default:specifications-global/spec-m3-design-standards|M3 Design & Component Standards]]](C:/dev/project-library-global/specifications-global/spec-m3-design-standards.md), and `security-architecture-canonical.md` — nothing was lost, it
was simply removed from here as duplicate. What remains below is genuine Policy: rules about how
decisions get made and how the team operates, not technical specifications.

---

## 1. The Anvil-First Mandate

Mybizz is an Anvil application. Every technical decision defaults to the Anvil way. Where Anvil
provides a facility — authentication, data persistence, background tasks, email, secrets — that
facility is used. Custom alternatives are not introduced unless Anvil's facility is genuinely
inadequate.

**Before implementing any feature, check in this order:**
1. Anvil's built-in components and services
2. Anvil documentation for native solutions
3. Anvil's example apps for established patterns
4. Anvil community forum for community approaches
5. Only then consider a custom implementation — and document the justification

The approved exception is The Vault. Client API keys cannot go in Anvil Secrets because doing so
would require giving clients IDE access. The Vault is an application-level encrypted secrets
store (see [[[default:specifications-global/spec-anvil-platform-standards|Anvil Platform Mechanics]]](C:/dev/project-library-global/specifications-global/spec-anvil-platform-standards.md) Section 10 for the technical pattern).

---

## 2. Version Control and Backup

Commit format: `[TYPE] Brief description` where TYPE is one of FEATURE, FIX, REFACTOR, SECURITY.

Branching strategy: `master` (production), `dev` (development integration),
`feature/feature-name`, `fix/bug-description`.

Backup protocol: see [[[default:specifications-global/spec-testing-methodology-standards|Testing Methodology]]](C:/dev/project-library-global/specifications-global/spec-testing-methodology-standards.md) Section 4 for the two-backup-point
discipline (before integration tests, after tests pass). Never back up a red or failing state.

---

## 3. Development Tooling

Development environment: VS Code with Continue.dev as the agent. The Anvil app is connected to
GitHub as the shared source of truth. Changes made in VS Code are pushed to GitHub; Anvil syncs
from GitHub. Changes made in the Anvil Designer are pulled into VS Code before any code draft
prompt is run.

---

## 4. Enforcement

| Level | Definition | Response |
|---|---|---|
| Critical | Security issue or data loss risk | Immediate fix — do not proceed |
| High | Policy violation | Fix before moving to next task |
| Medium | Best practice deviation | Document and schedule |
| Low | Style or formatting | Fix when convenient |

Security issues are always Critical. RBAC violations, secret handling errors, and any code that
returns sensitive data to the client are treated as Critical.

---

*Development Policy v3.0 — Specification content fully removed (was duplicate of
spec-anvil-platform-standards.md, spec-m3-design-standards.md, and
security-architecture-canonical.md). Only genuine Policy content remains: the Anvil-First
mandate, version control/backup rules, tooling, and enforcement severity levels.*
