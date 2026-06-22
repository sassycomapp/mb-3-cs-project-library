# Daily Operations — Quick Reference

---

## START OPENCODE

**In WSL:**
```bash
cd /mnt/c/_mb2-cs-app/project-library
opencode web --hostname 0.0.0.0
```
Then open browser: `http://localhost:3000`

---

## STOP / RESTART OPENCODE

**In WSL:**
```bash
pkill -f "opencode web"
```
Then run Start sequence above.

---

## AFTER DOING WORK — run every time in WSL

```bash
cd /mnt/c/_mb2-cs-app/project-library
git add -A
git commit -m "/Plan-eng-review-remediation"
git push origin main

pkill -f "opencode web"
pkill -f "gbrain serve"
sleep 3
gbrain sync --source project-library
gbrain embed --stale

opencode web --hostname 0.0.0.0

---

## WHERE SKILL OUTPUTS ARE SAVED

Skills save their output to `~/.gstack/projects/` automatically.  
You need to **manually copy** the output into the project-library after running a skill.

| Skill output | Copy it to |
|---|---|
| CEO review, Eng review, Design review reports | `gstack-outputs/` |
| DESIGN.md (from design-consultation) | `docs/` |
| Wireframes and mockups | `wireframes/` |
| ADRs | `adr/` |

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

## GBRAIN HEALTH CHECK

**In WSL:**
```bash
gbrain doctor
```
A score of **65–70/100 is normal and healthy** for this type of project.  
Warnings about `pgvector`, `graph_coverage`, `resolver_health`, and `brain_score` are expected — ignore them.

---

## GBRAIN TEST — confirm search is working

**In WSL:**
```bash
gbrain query "client instance architecture"
```
Should return a list of results. If you see a toast error — see Fix below.

---

## FIX: EMBEDDING FAILS — API KEY ERROR

If you see `OPENAI_API_KEY environment variable is missing`:

**In WSL:**
```bash
source ~/.bashrc
```
Then retry whatever you were doing.

---

## FIX: GBRAIN WASM ERROR / WON'T START

Close the WSL terminal completely. Open a fresh one. Retry.

If still broken:
```bash
rm -rf ~/.gbrain/brain.pglite
gbrain init
gbrain import ~/gbrain-backup-20260615/
gbrain embed --stale
```

---

## FIX: GBRAIN SYNC SAYS "NO ALLOWLISTED CHANGES"

This is normal — it means nothing new to sync from GStack's internal working state.  
Your `git push` already backed up the project-library. Nothing to worry about.

---

## CHECK WHAT'S NOT COMMITTED YET

**In WSL:**
```bash
cd /mnt/c/_mb2-cs-app/project-library
git status
```

---

## OPENCODE — INFRASTRUCTURE (type in the chat box)

### GBrain
| Command | What it does |
|---|---|
| `/setup-gbrain doctor` | GBrain health check |
| `/sync-gbrain` | Sync GBrain state |

### Save and Restore Your Place
| Command | What it does |
|---|---|
| `/context-save` | Save where you are before closing OpenCode |
| `/context-restore` | Pick up where you left off in a new session |

### Tell the Agent to Search GBrain
Add this to the start of any prompt:
```
Before starting, search GBrain for: [your topic here]
```

### GBrain / MCP Not Responding in OpenCode
Restart OpenCode from WSL:
```bash
pkill -f "opencode web"
cd /mnt/c/_mb2-cs-app/project-library
opencode web --hostname 0.0.0.0
```

---

## OPENCODE — USEFUL KEYS

| Key | What it does |
|---|---|
| Tab | Switch Plan / Build mode |
| Ctrl+G | Stop the agent mid-response |
| Ctrl+X then N | New session |

---

*That's it. Everything else lives in the reference docs — or just ask.*
