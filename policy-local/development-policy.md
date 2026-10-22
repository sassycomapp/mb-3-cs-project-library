---
title: "development-policy"
doc-id: "mb-509458a0"
updated: "2026-10-22"
---
# Mybizz CS — Development Policy

**Category:** Policy (not Specification — see note below)
**Authority:** Mandatory — all development work must conform to this policy

**Scope:** How `mb-3-cs` development is run. Technical standards are in [[[default:specifications-global/spec-anvil-platform-standards]]](C:/dev/project-library-global/specifications-global/spec-anvil-platform-standards.md), [[[default:specifications-global/spec-m3-design-standards]]](C:/dev/project-library-global/specifications-global/spec-m3-design-standards.md) and [[[default:specifications-global/spec-security-architecture]]](C:/dev/project-library-global/specifications-global/spec-security-architecture.md).

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
store (see [[[default:specifications-global/spec-vault-system]]](C:/dev/project-library-global/specifications-global/spec-vault-system.md) for the technical pattern).

---

## 2. Version Control and Backup

All development happens in App 1 (`mb-3-cs-app1`). Agents never commit or push; the developer commits every repository with `commit-all`. A release is App 1's code tagged `vMAJOR.MINOR.PATCH` and pushed by the developer into App 2 (Step 42), as [[[default:specifications-global/spec-deployment]]](C:/dev/project-library-global/specifications-global/spec-deployment.md) sets out.

Backup protocol: see [[[default:specifications-global/spec-testing-methodology-standards]]](C:/dev/project-library-global/specifications-global/spec-testing-methodology-standards.md) section 4 — App 1 is backed up before and after every test run on hosted App 1; every client instance is backed up to its own R2 bucket before a release. Never back up a red or failing state.

---

## 3. Development Tooling

Development environment: OpenCode, used through its web UI, with the PDLF step skills, working in WSL. Each Anvil app is connected to GitHub as the shared source of truth. Changes made in the Anvil Designer are pulled into the local copy with Step 18's pull procedure before any agent edits code.

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
