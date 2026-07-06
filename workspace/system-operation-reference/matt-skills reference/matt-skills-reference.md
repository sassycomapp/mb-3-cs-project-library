# Matt Pocock Skills — Reference

**Source repo:** `github.com/mattpocock/skills` (MIT licensed)
**Installed via:** `npx skills@latest add mattpocock/skills` → selected OpenCode as
the target agent
**Confirmed install location (WSL):** `~/.agents/skills/<skill-name>/SKILL.md`
**Confirmed discoverable in OpenCode:** yes — appear as `/skill-name` slash
commands tagged "skill" (verified directly in the OpenCode `/commands` list)
**Date configured:** 2026-06-24 (initial install), 2026-07-06 (multi-project setup, documentation)
**Projects configured:** mb-3-cs, mb4ecom, mb5pdlf

**Verification status:** `setup-matt-pocock-skills` has been run and confirmed on
mb-3-cs. Per-project setup files replicated to mb4ecom and mb5pdlf project-libraries
on 2026-07-06. CONTEXT.md bootstrapped for mb-3-cs. All required directories created
for all three projects.

---

## Installed Skills (18)

```
ask-matt
codebase-design
diagnosing-bugs
domain-modeling
find-skills
grill-me
grill-with-docs
grilling
handoff
improve-codebase-architecture
prototype
setup-matt-pocock-skills   ← run once per repo, then disregard
tdd
teach
to-issues
to-prd
triage
writing-great-skills
```

---

## Invocation Type

Per the source repo's own categorization. **User-invoked** skills are only
reachable when typed explicitly (e.g. `/grill-me`); they orchestrate.
**Model-invoked** skills can also be reached automatically by the agent when
a task fits; they hold reusable discipline. A user-invoked skill may call a
model-invoked one, never another user-invoked one.

| Skill | Type | Category |
|---|---|---|
| `ask-matt` | User-invoked | Engineering |
| `grill-with-docs` | User-invoked | Engineering |
| `triage` | User-invoked | Engineering |
| `improve-codebase-architecture` | User-invoked | Engineering |
| `setup-matt-pocock-skills` | User-invoked | Engineering |
| `to-issues` | User-invoked | Engineering |
| `to-prd` | User-invoked | Engineering |
| `prototype` | User-invoked | Engineering |
| `diagnosing-bugs` | Model-invoked | Engineering |
| `tdd` | Model-invoked | Engineering |
| `domain-modeling` | Model-invoked | Engineering |
| `codebase-design` | Model-invoked | Engineering |
| `grill-me` | User-invoked | Productivity |
| `handoff` | User-invoked | Productivity |
| `teach` | User-invoked | Productivity |
| `writing-great-skills` | User-invoked | Productivity |
| `grilling` | Model-invoked | Productivity |
| `find-skills` | *Unconfirmed* | *Not listed in source repo's main README table — likely a utility/meta skill for discovering other skills* |

---

## Output Locations

### Skills that write nothing to the workspace

| # | Skill | Produces | Goes to | Verified? |
|---|---|---|---|---|
| 1 | `ask-matt` | Routing advice | Nowhere — conversational | No |
| 2 | `codebase-design` | Vocabulary/definitions | Nowhere — pure reference skill | No |
| 3 | `grilling` | Interview questions | Nowhere — conversational, no side effects | No |
| 4 | `grill-me` | Interview questions | Nowhere — calls `/grilling`, stateless | No |
| 5 | `find-skills` | Skill recommendations | Nowhere — conversational | No |
| 6 | `writing-great-skills` | Writing guidance | Nowhere — pure reference skill | No |

### Skills that write to the repo (default behavior, before governance overrides)

| # | Skill | Artifact | Default Windows path | Verified? |
|---|---|---|---|---|
| 7 | `domain-modeling` | Glossary terms | `C:\projects\mb-3-cs\project-library\CONTEXT.md` (created lazily) | No |
| 8 | `domain-modeling` | Architecture decisions | `C:\projects\mb-3-cs\project-library\docs\adr\` (numbered ADRs) | No |
| 9 | `grill-with-docs` | Same as `domain-modeling` (calls it internally) | `CONTEXT.md` + `docs\adr\` | No |
| 10 | `to-prd` | PRD document | `C:\_mb2-cs-OK app\project-library\.scratch\<feature>\<prd-slug>.md` | No |
| 11 | `to-issues` | Individual issue files | `C:\projects\mb-3-cs\project-library\.scratch\<feature>\<issue-slug>.md` | No |
| 12 | `triage` | Issue updates (comments, labels) | In-place on the issue file in `.scratch\` | No |
| 13 | `triage` | Rejection records | `C:\projects\mb-3-cs\project-library\.out-of-scope\<topic>.md` | No |
| 14 | `setup-matt-pocock-skills` | Issue tracker config | `C:\projects\mb-3-cs\project-library\docs\agents\issue-tracker.md` | **Yes** |
| 15 | `setup-matt-pocock-skills` | Triage labels | `C:\projects\mb-3-cs\project-library\docs\agents\triage-labels.md` | **Yes** |
| 16 | `setup-matt-pocock-skills` | Domain doc rules | `C:\projects\mb-3-cs\project-library\docs\agents\domain.md` | **Yes** |
| 17 | `setup-matt-pocock-skills` | Agent skills section | `C:\projects\mb-3-cs\project-library\CLAUDE.md` (edits existing) | **Yes** |
| 18 | `tdd` | Tests + implementation | In-place in the codebase | No |

### Skills that write to TEMP, outside the repo

| # | Skill | Artifact | Default Windows path | Verified? |
|---|---|---|---|---|
| 19 | `handoff` | Handoff document | `%TEMP%\handoff-<random>.md` (e.g. `C:\Users\dev-p\AppData\Local\Temp\`) | No |
| 20 | `improve-codebase-architecture` | HTML architecture review | `%TEMP%\architecture-review-<timestamp>.html` | No |

> Note: `handoff`'s actual source (`mktemp -t handoff-XXXXXX.md`) generates a
> **random** suffix, not a timestamp — corrected here from the original draft
> of this table.

### Skills that write to a teaching workspace (user-chosen directory)

| # | Skill | Artifacts | Default Windows path | Verified? |
|---|---|---|---|---|
| 21 | `teach` | `MISSION.md`, `RESOURCES.md`, `NOTES.md` | `<current-directory>\` (wherever invoked) | No |
| 22 | `teach` | Lessons | `<current-directory>\lessons\0001-<name>.html` | No |
| 23 | `teach` | Reference docs | `<current-directory>\reference\*.html` | No |
| 24 | `teach` | Learning records | `<current-directory>\learning-records\0001-<name>.md` | No |
| 25 | `teach` | Reusable components | `<current-directory>\assets\*` | No |

### Skills that modify code in place

| # | Skill | What changes | Where | Verified? |
|---|---|---|---|---|
| 26 | `diagnosing-bugs` | Fix + regression test | In-place in the codebase; temp debug files cleaned up in its own Phase 6 | No |
| 27 | `prototype` | Throwaway code | Next to the module/page being prototyped; deleted or absorbed when done | No |

---

## Governance Overrides — As Decided and Applied to `AGENTS.md`

The defaults above are the skills' own behavior. The following overrides were
deliberately chosen and are now live in this project's `AGENTS.md`, under
`## Matt Pocock skills artifact governance`:

| Skill | Default | Override applied |
|---|---|---|
| `domain-modeling`, `grill-with-docs` (ADRs) | `docs\adr\` | → `adr\` instead |
| `domain-modeling`, `grill-with-docs` (context) | `CONTEXT.md` | No change — confirmed as-is |
| `to-prd`, `to-issues`, `triage` | `.scratch\` | No change — confirmed as-is |
| `triage` rejections | `.out-of-scope\` | No change — confirmed as-is |
| `setup-matt-pocock-skills` | `docs\agents\`, `CLAUDE.md` | No change — confirmed as-is |
| `handoff`, `improve-codebase-architecture` | `%TEMP%` only | %TEMP% kept **+** copy added to `matt-skills-output\` |
| `teach` | Invoking directory | Redirected entirely to `C:\_mb2-cs-app\matt-skills-teach` (outside `project-library`) |
| Any skill not listed above | — | No rule yet — do not guess; ask before saving |

---

## Summary by Destination

| Destination | Skills that write there |
|---|---|
| `C:\projects\mb-3-cs\project-library\.scratch\` | `to-prd`, `to-issues`, `triage` |
| `C:\projects\mb-3-cs\project-library\CONTEXT.md` | `domain-modeling`, `grill-with-docs` |
| `C:\projects\mb-3-cs\project-library\adr\` | `domain-modeling`, `grill-with-docs` *(post-override)* |
| `C:\projects\mb-3-cs\project-library\docs\agents\` | `setup-matt-pocock-skills` |
| `C:\projects\mb-3-cs\project-library\CLAUDE.md` | `setup-matt-pocock-skills` |
| `C:\projects\mb-3-cs\project-library\.out-of-scope\` | `triage` (rejected items only) |
| `%TEMP%` (outside repo) | `handoff`, `improve-codebase-architecture` |
| `C:\projects\mb-3-cs\project-library\matt-skills-output\` | `handoff`, `improve-codebase-architecture` *(added copy, post-override)* |
| `C:\_mb2-cs-app\matt-skills-teach` (outside `project-library`) | `teach` *(post-override)* |
| Teaching workspace, invoking directory | `teach` *(pre-override default — superseded above)* |
| In-place codebase changes | `tdd`, `diagnosing-bugs`, `prototype` |
| Nothing on disk | `ask-matt`, `codebase-design`, `grilling`, `grill-me`, `find-skills`, `writing-great-skills` |

---

## Open Items (updated 2026-07-06)

1. **CONTEXT.md** — bootstrapped for mb-3-cs (2026-07-06). Not yet created for mb4ecom or mb5pdlf — skills will create lazily when `/domain-modeling` or `/grill-with-docs` is first run there.
2. **Multi-project setup** — mb4ecom and mb5pdlf project-libraries configured with `docs/agents/`, `.scratch/`, `matt-skills-output/`, `.out-of-scope/`, and Agent skills block in AGENTS.md (2026-07-06).
3. **Folders verified on disk** — all required directories exist for all three projects: `.scratch/`, `matt-skills-output/`, `.out-of-scope/`, `docs/agents/`, `C:\_mb2-cs-app\matt-skills-teach`.
4. **find-skills** — confirmed as model-invoked skill from `vercel-labs/skills`. Listed in installed skills but not part of the main Matt Pocock workflow.
5. Every row marked "No" under Verified is a documentation-sourced claim, not an observed result. Confirm each the first time its skill is used, and update this file accordingly.
