---
title: "docs-reorg-consolidated-20260706"
doc-id: "mb-286fb93e"
---
# Docs Reorganization — Consolidated Completion Report

**Date:** 2026-07-06
**Period:** Full session covering docs reorganization, standardization, and housekeeping

---

## Tasks Completed

### ADR numeric references — slug replacement
- Replaced all `ADR-NN` prose references with slug names across 38 files
- Replaced all `adr/NN-slug.md` file paths with `adr/adr-global/slug.md` across 50 files
- Zero remaining numeric ADR references in docs/, specifications/, adr/

### GBrain sync workflow documented
- Recorded definitive GBrain sync workflow in `workspace/system-operation-reference/workflow reference/Daily operations.md`
- Root cause: PGLite single-writer — `gbrain serve` holds exclusive lock, CLI commands must run while serve is stopped
- Verified workflow: stop serve → sync --no-hard-deadline → embed --stale → extract --stale → restart serve

### Daily operations.md stale paths fixed
- Fixed all `C:\mybizz\` paths → `C:\projects\mb-3-cs\` for mb-3-cs and project-library repos
- Fixed gstack-outputs path, local AGENTS.md path
- Removed stale serve daemon note (PID 21415)

### Handover splits verified
- `test-specifications.md` → `spec-testing.md`: split correct. Original deleted. CS-specific test plan (269 lines) in specs, generic methodology in `spec-testing-methodology-standards.md` (117 lines). Fixed stale reference to methodology file.
- `platform-architecture.md` → `spec-five-app-architecture-model.md`: split correct. Five-app model extracted with placeholder names. Doc references spec. Deleted stale reference to removed ADR-9. Replaced numeric ADR references.

### Template blanks created
- `templates-checklists/`: 20 files emptied (zero-byte) and renamed with `tmp-` prefix
- Naming convention established: `tmp-` for templates, `chk-` for checklists
- 472 MB freed from old full-document copies

### HtmlTemplate ban (per `htmltemplate-use` ADR)
- Fixed `spec-m3-design-standards.md` §7: NavigationRailLayout/NavigationDrawerLayout are now standard
- Fixed `docs/architecture.md`: "This project uses native M3 Layouts"
- Fixed `docs/DESIGN.md`: AdminLayout (NavigationRailLayout), Public Layout (NavigationDrawerLayout), ErrorLayout (BlankLayout)
- Fixed `docs/build-plan.md`, `docs/positioning-strategy.md`, `docs/scaffold-spec.md`
- Fixed `spec-anvil-spec-table.md`: 6 HtmlTemplate references replaced
- `adr/adr-global/navigation-lambda-link-open-form.md`: marked HtmlTemplate decision as superseded

### Security architecture elevated to global
- `docs/security-architecture.md` → `specifications/spec-security-architecture.md`
- Multi-tenant language corrected per `dependency-based-not-multi-tenant` ADR
- RBAC role limits, token security, regulatory posture now Mybizz-wide
- Cross-references updated in `spec-vault-system.md` and `spec-regulatory-compliance-baseline.md`

### Vault system consolidated
- Created `specifications/spec-vault-system.md` — single authoritative Vault document (119 lines)
- Content merged from `spec-security.md` (generic pattern) and `docs/vault-system.md` (CS implementation)
- Removed duplicate Vault content from `spec-security.md` and `spec-anvil-platform-standards.md`
- Deleted `docs/vault-system.md`

### Files confirmed non-existent
- `spec-development-policy.md` — does not exist
- `spec-coding-standards.md` — does not exist
- `security-architecture-old.md` — does not exist
- `theme_mui_patch.css`, `theme_patched.css` — do not exist

### `(2)` suffix files in specifications resolved
- `spec-m3-design-standards (2).md`: v1.2 (has Section 18) replaced v1.1; suffix dropped
- `spec-anvil-platform-standards (2).md`: v1.2 (has Sections 15-18) replaced v1.1; suffix dropped

### Devlog/todo system status resolved
- Old `C:\mybizz\devlog\` + `C:\mybizz\todo\` does not exist on disk — superseded
- Replacement: `devlog-index.md` + `judgement-trail.md` + `wip/todo.md` at project root
- Defined in AGENTS.md §Task Completion Reporting

### Additional
- Deleted `docs/anvil-platform-standards.md` — stale duplicate of global spec
- Prefix convention settled: `spec-` = specification, `policy-` = policy, no-prefix = app docs
- PDLF todo initialized: `pdlf/todo-pdlf.md`

---

## Next Steps (deferred — in `wip/todo.md`)

1. README.md rewrite
2. Populate 17 empty spec placeholder files
3. Audit all `instance_id` references per ADR-41
4. Checklist templates

---

*Consolidated completion report — covers all docs reorganization tasks 2026-07-06.*
