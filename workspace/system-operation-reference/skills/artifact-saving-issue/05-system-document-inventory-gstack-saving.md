# System Document Inventory: GStack Artifact-Saving Governance

**Date:** 2026-06-26
**Scope:** Every document and storage location on this system directly
related to where/how GStack skill outputs get saved, found or created
during this investigation. Windows-format paths used throughout
(`\\wsl.localhost\Ubuntu\...` for WSL home-directory paths,
`C:\...` for `/mnt/c/...` paths).

**Confidence key:**
- ✅ **CONFIRMED** — path and existence directly verified during this
  investigation (via `ls`, `cat`, `grep`, or direct file view)
- ⚠️ **REFERENCED, NOT VERIFIED** — mentioned in conversation or implied by
  convention, but exact path or current existence was not directly checked

---

## 1. Project-Level Governance Documents
*(These directly control or describe where outputs should save)*

| Document | Path | Status |
|---|---|---|
| Project agent rules — contains the `## GStack artifact governance` section we edited twice | `C:\_mb2-cs-app\project-library\AGENTS.md` | ✅ |
| Backup of AGENTS.md taken before edits | `C:\_mb2-cs-app\project-library\AGENTS.md.backup-20260625` | ✅ |
| Project Claude/agent config — skill routing rules, GBrain config | `C:\_mb2-cs-app\project-library\CLAUDE.md` | ✅ |

---

## 2. GStack's Own Configuration

| Document | Path | Status |
|---|---|---|
| GStack config file (`gstack-config` reads/writes this) — confirmed no save-location key exists among its ~14 settings | `\\wsl.localhost\Ubuntu\home\dev-p\.gstack\config.yaml` | ✅ |

---

## 3. Your Own GStack/OpenCode Reference Documentation

⚠️ **None of these have a confirmed absolute path** — you uploaded them
during this conversation but never stated where they live on disk. Likely
candidates based on your own folder conventions (`workspace/gstack/` or a
system-operation-reference folder), but this needs your confirmation.

| Document | Likely location pattern | Status |
|---|---|---|
| `gstack-reference.md` | `C:\_mb2-cs-app\project-library\workspace\...` | ⚠️ |
| `gstack-skills-reference.md` (full 59-skill catalog) | same area | ⚠️ |
| `gbrain-reference.md` (referenced inside gstack-reference.md, not yet read) | same area | ⚠️ |
| `opencode-reference.md` (referenced inside gstack-reference.md, not yet read) | same area | ⚠️ |
| `opencode-10-config-files.md` (uploaded, not yet read in detail) | same area | ⚠️ |

---

## 4. Live Skill Source Files Containing the Actual Save-Path/Lookup Logic

This is the **mechanism itself** — every file confirmed via direct `grep`
to contain a literal `.gstack/projects` reference, under the canonical
folder OpenCode actually reads (`C:\_mb2-cs-app\skills\`). ✅ **All entries
in this section are confirmed** (this exact list came from a real grep
run earlier in this conversation, not reconstructed from memory).

**Build/generator files:**
- `C:\_mb2-cs-app\skills\package.json`
- `C:\_mb2-cs-app\skills\scripts\resolvers\design.ts`
- `C:\_mb2-cs-app\skills\scripts\resolvers\learnings.ts`
- `C:\_mb2-cs-app\skills\scripts\resolvers\review.ts`
- `C:\_mb2-cs-app\skills\scripts\resolvers\tasks-section.ts`
- `C:\_mb2-cs-app\skills\scripts\resolvers\testing.ts`
- `C:\_mb2-cs-app\skills\scripts\resolvers\utility.ts`
- `C:\_mb2-cs-app\skills\scripts\task-emission-schema.ts`
- `C:\_mb2-cs-app\skills\TODOS.md`

**Per-skill `SKILL.md` (generated, live) and `SKILL.md.tmpl` (source template) pairs:**

| Skill | SKILL.md | SKILL.md.tmpl |
|---|---|---|
| autoplan | `C:\_mb2-cs-app\skills\autoplan\SKILL.md` | `...\autoplan\SKILL.md.tmpl` |
| canary | `C:\_mb2-cs-app\skills\canary\SKILL.md` | `...\canary\SKILL.md.tmpl` |
| context-restore | `C:\_mb2-cs-app\skills\context-restore\SKILL.md` | *(none found)* |
| context-save | `C:\_mb2-cs-app\skills\context-save\SKILL.md` | *(none found)* |
| design-consultation | `C:\_mb2-cs-app\skills\design-consultation\SKILL.md` | `...\design-consultation\SKILL.md.tmpl` |
| design-html | `C:\_mb2-cs-app\skills\design-html\SKILL.md` | `...\design-html\SKILL.md.tmpl` |
| design-review | `C:\_mb2-cs-app\skills\design-review\SKILL.md` | `...\design-review\SKILL.md.tmpl` |
| design-shotgun | `C:\_mb2-cs-app\skills\design-shotgun\SKILL.md` | `...\design-shotgun\SKILL.md.tmpl` |
| health | `C:\_mb2-cs-app\skills\health\SKILL.md` | `...\health\SKILL.md.tmpl` |
| investigate | `C:\_mb2-cs-app\skills\investigate\SKILL.md` | `...\investigate\SKILL.md.tmpl` |
| ios-design-review | `C:\_mb2-cs-app\skills\ios-design-review\SKILL.md` | `...\ios-design-review\SKILL.md.tmpl` |
| land-and-deploy | `C:\_mb2-cs-app\skills\land-and-deploy\SKILL.md` | `...\land-and-deploy\SKILL.md.tmpl` |
| office-hours | `C:\_mb2-cs-app\skills\office-hours\SKILL.md` | `...\office-hours\SKILL.md.tmpl` |
| plan-ceo-review | `C:\_mb2-cs-app\skills\plan-ceo-review\SKILL.md` | `...\plan-ceo-review\SKILL.md.tmpl` |
| plan-design-review | `C:\_mb2-cs-app\skills\plan-design-review\SKILL.md` | `...\plan-design-review\SKILL.md.tmpl` |
| plan-devex-review | `C:\_mb2-cs-app\skills\plan-devex-review\SKILL.md` | `...\plan-devex-review\SKILL.md.tmpl` |
| plan-eng-review | `C:\_mb2-cs-app\skills\plan-eng-review\SKILL.md` | `...\plan-eng-review\SKILL.md.tmpl` |
| plan-tune | `C:\_mb2-cs-app\skills\plan-tune\SKILL.md` | `...\plan-tune\SKILL.md.tmpl` |
| qa | `C:\_mb2-cs-app\skills\qa\SKILL.md` | `...\qa\SKILL.md.tmpl` |
| qa-only | `C:\_mb2-cs-app\skills\qa-only\SKILL.md` | `...\qa-only\SKILL.md.tmpl` |
| retro | `C:\_mb2-cs-app\skills\retro\SKILL.md` | `...\retro\SKILL.md.tmpl` |
| review | `C:\_mb2-cs-app\skills\review\SKILL.md` | *(none found)* |
| ship | `C:\_mb2-cs-app\skills\ship\SKILL.md` | `...\ship\SKILL.md.tmpl` |

**Associated section/sub-files (referenced by the skills above):**
- `C:\_mb2-cs-app\skills\design-consultation\sections\proposal-and-preview.md` (+ `.tmpl`)
- `C:\_mb2-cs-app\skills\office-hours\sections\design-and-handoff.md` (+ `.tmpl`)
- `C:\_mb2-cs-app\skills\plan-ceo-review\sections\review-sections.md` (+ `.tmpl`)
- `C:\_mb2-cs-app\skills\plan-design-review\sections\review-sections.md` (+ `.tmpl`)
- `C:\_mb2-cs-app\skills\plan-devex-review\sections\review-sections.md`
- `C:\_mb2-cs-app\skills\plan-eng-review\sections\review-sections.md`
- `C:\_mb2-cs-app\skills\ship\sections\plan-completion.md`
- `C:\_mb2-cs-app\skills\ship\sections\test-coverage.md`
- `C:\_mb2-cs-app\skills\review\greptile-triage.md`
- `C:\_mb2-cs-app\skills\setup-gbrain\memory.md`
- `C:\_mb2-cs-app\skills\test\fixtures\golden\claude-ship-SKILL.md`
- `C:\_mb2-cs-app\skills\test\fixtures\golden\codex-ship-SKILL.md`
- `C:\_mb2-cs-app\skills\test\fixtures\golden\factory-ship-SKILL.md`
- `C:\_mb2-cs-app\skills\test\fixtures\golden-ship-claude.md`

**Note:** ~36 other skills exist under `C:\_mb2-cs-app\skills\` that did
**not** match the grep (no direct `.gstack/projects` reference) — these
either save elsewhere, save nothing, or reference the path indirectly via
a binary call. Not listed here since they weren't confirmed relevant to
*this specific* save-path question.

---

## 5. GStack Repository Internals (Background Reference)
*(A separate clone at `C:\_mb2-cs-app\gstack\` — not the live skill source
OpenCode reads, but where the `gstack-*` binaries and project-wide docs live)*

| Document | Path | Status |
|---|---|---|
| Slug computation script | `C:\_mb2-cs-app\gstack\bin\gstack-slug` | ✅ |
| Config binary | `C:\_mb2-cs-app\gstack\bin\gstack-config` | ✅ |
| Repo-mode detector | `C:\_mb2-cs-app\gstack\bin\gstack-repo-mode` | ⚠️ (path known, content not read) |
| Artifacts-init script | `C:\_mb2-cs-app\gstack\bin\gstack-artifacts-init` | ⚠️ |
| Brain-cache script | `C:\_mb2-cs-app\gstack\bin\gstack-brain-cache` | ⚠️ |
| Developer-profile script | `C:\_mb2-cs-app\gstack\bin\gstack-developer-profile` | ⚠️ |
| Learnings-search script | `C:\_mb2-cs-app\gstack\bin\gstack-learnings-search` | ⚠️ |
| Memory-ingest script | `C:\_mb2-cs-app\gstack\bin\gstack-memory-ingest.ts` | ⚠️ |
| Question-preference script | `C:\_mb2-cs-app\gstack\bin\gstack-question-preference` | ⚠️ |
| Taste-update script | `C:\_mb2-cs-app\gstack\bin\gstack-taste-update` | ⚠️ |
| Timeline-read script | `C:\_mb2-cs-app\gstack\bin\gstack-timeline-read` | ⚠️ |
| Project changelog (artifact-sync history) | `C:\_mb2-cs-app\gstack\CHANGELOG.md` | ✅ |
| GStack's own internal CLAUDE.md (distinct from your project's) | `C:\_mb2-cs-app\gstack\CLAUDE.md` | ✅ |
| Skill deep-dive docs | `C:\_mb2-cs-app\gstack\docs\skills.md` | ✅ |
| Design-history docs (PLAN_TUNING, SELF_LEARNING, SESSION_INTELLIGENCE, etc.) | `C:\_mb2-cs-app\gstack\docs\designs\*.md` | ✅ (listing) |
| GBrain sync error index | `C:\_mb2-cs-app\gstack\docs\gbrain-sync-errors.md` | ⚠️ |
| Browse server source | `C:\_mb2-cs-app\gstack\browse\src\server.ts` | ✅ |
| Per-host duplicate skill folders (9 agent-specific copies — confirmed real and official, not build artifacts) | `C:\_mb2-cs-app\gstack\.agents\`, `.cursor\`, `.factory\`, `.gbrain\`, `.hermes\`, `.kiro\`, `.openclaw\`, `.opencode\`, `.slate\` | ✅ |

---

## 6. Investigation Documents Produced During This Session

| Document | Intended path | Status |
|---|---|---|
| Problem Statement | `C:\_mb2-cs-app\project-library\workspace\system-operation-reference\artifact-saving-problem-statement.md` | ⚠️ delivered to you; not confirmed you placed it at this exact path |
| Truth Statement | `...\workspace\system-operation-reference\truth-statement-gstack-artifact-saving-inconsistency.md` | ⚠️ same |
| Communities Statement | `...\workspace\system-operation-reference\communities-statement-gstack-artifact-saving-inconsistency.md` | ⚠️ same |
| Targeted skill spec: plan-ceo-review | `...\workspace\system-operation-reference\skills\plan-ceo-review-spec.md` | ⚠️ you generated this via OpenCode; exact saved path not independently verified |
| Blanket skill audit (all 59 skills, found to contain at least one error) | unknown — you uploaded this but its save location was never stated | ⚠️ |

---

## 7. Actual Artifact Storage Locations (Not Documents, But Directly Relevant)

| Location | Path | Status |
|---|---|---|
| Default project folder — basename-fallback slug, holds `/context-save` checkpoint | `\\wsl.localhost\Ubuntu\home\dev-p\.gstack\projects\project-library\` | ✅ |
| Default project folder — empty-`$SLUG` fallback, holds `/health` log | `\\wsl.localhost\Ubuntu\home\dev-p\.gstack\projects\unknown\` | ✅ |
| Default project folder — git-remote-derived slug, holds historical CEO/eng reports from before this investigation | `\\wsl.localhost\Ubuntu\home\dev-p\.gstack\projects\sassycomapp-project-library\` | ✅ |
| Your preferred manual output folder, governed by AGENTS.md | `C:\_mb2-cs-app\project-library\gstack-outputs\` | ✅ |
| Unexpected location found via `/retro` test — matches none of the three documented claims for that skill | `C:\_mb2-cs-app\project-library\memory\` | ✅ |
| `/retro`'s actual documented per-project save folder (per its own `SKILL.md` text) — existence on disk not yet checked | `C:\_mb2-cs-app\project-library\.context\retros\` | ⚠️ |
