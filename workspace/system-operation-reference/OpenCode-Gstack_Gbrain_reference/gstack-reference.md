# GStack Reference

## What is GStack?

GStack is the workflow layer. It provides slash commands (skills) for planning, building, reviewing, and shipping software.

**GBrain** = memory (stores and searches)  
**GStack** = workflow (plans, builds, ships)

**For the full skill catalog (59 skills with usage, prerequisites, output, presentation):** See `gstack-skills-reference.md`

---

## Opening the GStack projects folder

**From PowerShell:**
```powershell
explorer.exe "\\wsl.localhost\Ubuntu\home\dev-p\.gstack\projects\"
```

**From WSL:**
```bash
explorer.exe "$(wslpath -w /home/dev-p/.gstack/projects/)"
```

**From Windows Explorer:**
```
\\wsl.localhost\Ubuntu\home\dev-p\.gstack\projects\
```

---

## Skills Installation

**Location:** `~/.config/opencode/skills/` (symlinked to `/mnt/c/_mb2-cs-app/skills/`)

**Total skills:** 59 (54 top-level + 4 openclaw + 1 browser-skills)

**How it works:** All GStack skills are symlinked to the global OpenCode skills directory. This makes them available in every project automatically. OpenCode discovers skills from `~/.config/opencode/skills/<name>/SKILL.md`.

**To verify:**
```bash
ls ~/.config/opencode/skills/ | wc -l
```

**To update skills:**
```bash
# Skills are symlinked — updates to the source files are automatic
# Just edit the SKILL.md in /mnt/c/_mb2-cs-app/skills/
```

**To undo:**
```bash
rm -rf ~/.config/opencode/skills/
```

---

## GStack Brain Sync

GStack brain sync pushes working state (learnings, plans, retros) to a private git repo for cross-machine memory.

**Remote:** `https://github.com/sassycomapp/gbrain-repo.git`  
**Sync mode:** `full`  
**Worktree:** `~/.gstack-brain-worktree/`

### Sync Now

**Apply in:** WSL terminal  
**Path:** any directory

```bash
bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-brain-sync --once
```

### Check Sync Status

**Apply in:** WSL terminal  
**Path:** any directory

```bash
bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-brain-sync --status
```

### Check Both GBrain and GStack Status

**Apply in:** WSL terminal  
**Path:** any directory

```bash
bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-gbrain-detect
```

---

## Artifact Locations

### Where GStack Skills Save by Default

GStack skills save to `~/.gstack/projects/$SLUG/` internally. This is the GStack working directory.

| Skill | Saves to |
|---|---|
| `/office-hours` | `~/.gstack/projects/$SLUG/` |
| `/plan-ceo-review` | `~/.gstack/projects/$SLUG/ceo-plans/` |
| `/plan-eng-review` | `~/.gstack/projects/$SLUG/` |
| `/design-consultation` | `~/.gstack/projects/$SLUG/designs/` + `DESIGN.md` in project root |
| `/context-save` | `~/.gstack/projects/$SLUG/.context/` |
| `/retro` | `~/.gstack/projects/$SLUG/retros/` |

### Where to Save in project-library

Per `AGENTS.md`, GStack outputs should be saved to `gstack-outputs/`. **Skills don't do this automatically — you copy artifacts manually.**

| Artifact | Save to |
|---|---|
| Office-hours reports | `gstack-outputs/` |
| CEO review reports | `gstack-outputs/` |
| Engineering review reports | `gstack-outputs/` |
| Design consultation plans | `gstack-outputs/` |
| Design review reports | `gstack-outputs/` |
| Dev plans | `gstack-outputs/` |
| Handoff notes | `gstack-outputs/` |
| DESIGN.md | `docs/` |
| Wireframes, mockups | `wireframes/` |
| ADRs | `adr/` |

### Open GStack Projects Folder in Windows Explorer

**Apply in:** WSL terminal  
**Path:** any directory

```bash
explorer.exe "$(wslpath -w /home/dev-p/.gstack/projects/)"
```

**Important:** Always use `wslpath -w` from WSL. Using a Linux path directly opens the wrong location (OneDrive Documents).

---

## File Locations

| Item | Location |
|---|---|
| GStack repo | `/mnt/c/_mb2-cs-app/gstack/` |
| GStack bin scripts | `/mnt/c/_mb2-cs-app/gstack/bin/` |
| GStack skills source | `/mnt/c/_mb2-cs-app/skills/` |
| GStack skills (symlinked) | `~/.config/opencode/skills/` |
| GStack home | `~/.gstack/` |
| GStack brain remote | `~/.gstack-brain-remote.txt` |
| GStack brain worktree | `~/.gstack-brain-worktree/` |
| OpenCode commands | `/mnt/c/_mb2-cs-app/project-library/.opencode/commands/` |
| GStack outputs | `/mnt/c/_mb2-cs-app/project-library/gstack-outputs/` |

### Open GStack Home in Windows Explorer

**Apply in:** WSL terminal  
**Path:** any directory

```bash
explorer.exe "$(wslpath -w /home/dev-p/.gstack/)"
```

---

## Related Documentation

| Document | Purpose |
|---|---|
| `gstack-skills-reference.md` | Full skill catalog (59 skills with usage, prerequisites, output, presentation) |
| `gbrain-reference.md` | GBrain commands, health score, troubleshooting |
| `opencode-reference.md` | OpenCode configuration, keyboard shortcuts, MCP |
