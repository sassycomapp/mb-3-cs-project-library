# GStack Configuration Reference

## 1. Installation

| Property | Value |
|---|---|
| **Version** | 1.58.1.0 |
| **Install location** | `/mnt/c/mybizz/gstack` (Windows: `C:\mybizz\gstack`) |
| **Repository** | Git repo with full skill ecosystem |
| **Skills source** | `/mnt/c/mybizz/skills/` (70 entries, 61 symlinked to OpenCode) |
| **Binary directory** | `/mnt/c/mybizz/gstack/bin/` (87 executables) |
| **OpenCode skills** | `~/.config/opencode/skills/` — 61 symlinks → `/mnt/c/mybizz/skills/` |
| **Claude skills** | `~/.claude/skills/gstack` — symlink → `/mnt/c/mybizz/gstack` |
| **Updates** | `gstack-update-check` — UP_TO_DATE |

## 2. Runtime Config — `~/.gstack/config.yaml`

```yaml
# Custom values:
artifacts_sync_mode: full
telemetry: community
question_tuning: true
user_slug_at_local: dev-p

# Defaults (not overridden):
proactive: true
routing_declined: false
auto_upgrade: false
update_check: true
skill_prefix: false
checkpoint_mode: explicit
checkpoint_push: false
explain_level: default
codex_reviews: enabled
gstack_contributor: false
skip_eng_review: false
workspace_root: /home/dev-p/conductor/workspaces
plan_tune_hooks: prompt
```

### Key settings explained

| Setting | Value | Notes |
|---|---|---|
| `artifacts_sync_mode` | `full` | All artifacts synced to brain |
| `telemetry` | `community` | Sends usage data + stable device ID |
| `question_tuning` | `true` | Enables `/plan-tune` preference learning |
| `proactive` | `true` | Skills suggest themselves (e.g., QA for "does this work?") |
| `checkpoint_mode` | `explicit` | Manual commits only, no auto-WIP commits |

## 3. Project State — `~/.gstack/projects/`

| Slug | Path | Status |
|---|---|---|
| `sassycomapp-project-library` | `/mnt/c/projects/mb-3-cs/project-library` | Active |
| `mb5pdlf` | `/mnt/c/projects/mb5pdlf` | PDLF test, Stage 13 |
| *(mb4ecom, scratch)* | *(not yet registered)* | — |

## 4. Session and Learning Data

| File | Status |
|---|---|
| `~/.gstack/sessions/` | 1 empty session file — unused |
| `~/.gstack/projects/*/learnings.jsonl` | Zero learnings across all projects |
| `~/.gstack/analytics/skill-usage.jsonl` | 7 entries (sparse usage, May-June 2026) |
| `~/.gstack/developer-profile.json` | Declared only (scope: 0.85, risk: 0.25, architecture: 0.85) |

## 5. Skills Inventory

**61 skills available to OpenCode**, symlinked from `/mnt/c/mybizz/skills/`:

| Category | Skills |
|---|---|
| **Planning** | `plan-ceo-review`, `plan-eng-review`, `plan-design-review`, `plan-devex-review`, `plan-tune`, `office-hours`, `autoplan` |
| **Implementation** | `build` (custom), `investigate`, `prototype` |
| **Design** | `design-consultation`, `design-html`, `design-review`, `design-shotgun`, `wireframe` |
| **QA/Review** | `qa`, `qa-only`, `browse`, `review`, `design-review`, `benchmark`, `health` |
| **Release** | `ship`, `land-and-deploy`, `landing-report`, `canary` |
| **Memory** | `setup-gbrain`, `sync-gbrain`, `context-save`, `context-restore`, `learn` |
| **Security** | `cso`, `careful`, `guard` |
| **Utilities** | `diagram`, `make-pdf`, `scrape`, `spec`, `gstack`, `gstack-upgrade`, `freeze`, `unfreeze`, `claude`, `codex` |
| **PDLF** | `pdlf` |

## 6. Project-Local Commands — `project-library/.opencode/commands/`

49 `.md` files provide slash-command routing to GStack skills. Standard pattern:

```md
# /command-name

Run the gstack-command-name workflow.

When you run this command, you should:
- Load and use the `gstack-command-name` skill.
- Follow the skill instructions exactly.
- Respect the current workspace rules, AGENTS.md, and OpenCode permissions.
- Keep changes minimal and fit for purpose.
```

## 7. Binary Path Override

Per `AGENTS.md` Hard Rule: always use `/mnt/c/mybizz/gstack/bin/<binary-name>` for any GStack binary. Do not attempt host-detection logic — the literal path is correct and verified.

Key binaries:
- `gstack-config` — config get/set
- `gstack-slug` — resolve project slug from CWD
- `gstack-gbrain-detect` — probe GBrain status
- `gstack-gbrain-sync.ts` — full brain sync orchestrator
- `gstack-brain-sync` — brain push to git
- `gstack-brain-cache` — brain-aware planning cache
- `gstack-next-version` — version bump for /ship
- `gstack-learnings-log` / `gstack-learnings-search` — learning management

## 8. GStack + GBrain Integration

- **Detection:** `gstack-gbrain-detect` — returns `gbrain_local_status: ok` (after 2026-07-06 fix)
- **Sync:** `gstack-gbrain-sync.ts` — orchestrates code → memory → brain-sync stages
- **Status:** `gstack-config gbrain-refresh` — checks gbrain and refreshes SKILL.md brain-aware blocks
- **Note:** PGLite is single-writer. Stop `gbrain serve` before running sync.

## 9. Operational Notes

- **PGLite single-writer:** Before running GStack brain sync commands, stop `gbrain serve`: `kill <PID>`. Restart after: `nohup /home/dev-p/.bun/bin/gbrain serve > /dev/null 2>&1 &`
- **No GStack-specific `.opencode` config** — integration is via skill symlinks + project-local command files
- **No artifacts remote** — `.gstack-artifacts-remote.txt` is empty; cross-machine sync not configured
- **All docs populated** — workspace/gstack/ all 10 files have real content
