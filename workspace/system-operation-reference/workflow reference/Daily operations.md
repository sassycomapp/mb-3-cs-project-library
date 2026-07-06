# Getting To Work
python3 -c "import sqlite3;c=sqlite3.connect('/home/dev-p/.local/share/opencode/opencode.db');c.execute(\"UPDATE project SET worktree='/mnt/c/projects/mb-3-cs/project-library' WHERE id='c99de7a1276539abb02da8ac80ac3878025bd1cd'\");c.commit();print('done')"
## Apps list

You only need these for normal work:

- Ubuntu 22.04.5 LTS (WSL)  
- Bash shell in Ubuntu  
- OpenCode Web UI  
- GStack (as skills used by the OpenCode agents)  
- GBrain (pglite + OpenRouter embeddings, used through GStack)  
- Git (from Bash in Ubuntu)  
- Matt-skills 

***

## UBUNTU Start sequence

1. Open **Windows Terminal**.  
2. In the dropdown (the little ▾ arrow), click **“Ubuntu 22.04.5 LTS”**.  
   - That profile is your WSL Ubuntu instance.  
3. You should get a prompt like:

   ```bash
   myubuntu@dev-p:~$
   ```

   This shell is **home base** for:

   - running the OpenCode server,  
   - running Git,  
   - inspecting the repos under `/mnt/c/projects/mb-3-cs/...`.  
 
 ## OPENCODE START Sequence
**In WSL:**
A) start sequence to start opencode server and open the opencode web UI:
	```bash
	cd /mnt/c/projects/mb-3-cs/project-library
	opencode web --hostname 0.0.0.0
	```
B) Alternative start sequence to start opencode server and open the opencode web UI:
	```bash
	cd /mnt/c/mybizz/gstack
	opencode web --hostname 0.0.0.0
	```
You will receive the following message in WSL :
	```text
     Warning: OPENCODE_SERVER_PASSWORD is not set; server is unsecured.
     opencode server listening on http://0.0.0.0:4096
	```

This starts the server and opens the WEB UI
This ensures the project root is gstack, so it will see gstack/.opencode/skills and the new gstack/.opencode/commands

## OPENCODE STOP Sequence 

**In WSL:**
```bash
pkill -f "opencode web"
```
Then run Start sequence above.

Important: PGLite single-writer pattern — see "Updating GBrain after new work" below for the definitive workflow. `gbrain serve` holds an exclusive database lock; CLI commands (sync, embed, extract) cannot run while serve is active. Always stop serve before running CLI commands, then restart it after.

---
***

## Active repos  (Windows side)

1. `C:\projects\mb-3-cs` — **Workspace folder**  
   - Not a Git repo.  
   - Container for project repos.

2. `C:\projects\mb-3-cs\mb-3-cs` — **Git code repo**  
   `/mnt/c/projects/mb-3-cs/mb-3-cs`  
   - Anvil‑linked production‑adjacent application code.  

3. `C:\projects\mb-3-cs\project-library` — **Git docs repo**  
   `/mnt/c/projects/mb-3-cs/project-library`
   - Design/planning/architecture documents.  
   - Not Anvil‑linked.

4. `C:\mybizz\gstack` — **GStack repo**  
   `/mnt/c/mybizz/gstack` 
   - Workflow/skillpack used by the agents inside OpenCode.
   
5. `C:\mybizz\gbrain` — **GBrain repo**  
   - Workflow/skillpack used by the agents inside OpenCode.

***

## Switching Projects
you switch projects from within OpenCode by sending the appropriate `/mnt/...` path in chat after the server is running.
You **do not** switch projects by restarting the server; 
***

## Committing to Github after new work 
**In WSL:**
	```bash
	cd /mnt/c/projects/mb-3-cs/project-library
	git add -A
	git commit -m "/Example: Plan-eng-review-remediation" (Create a commit message )
	git push origin main
	```

## Updating GBrain after new work

**Background — PGLite single-writer constraint:**
GBrain uses PGLite, which is a single-writer database. Only one process can hold the database connection at a time. `gbrain serve` holds this exclusive lock while running. CLI commands (`sync`, `embed`, `extract`) also need the lock. If `gbrain serve` is running, CLI commands will time out waiting for the PGLite lock. This is the root cause of all GBrain sync timeouts.

**Definitive workflow (verified 2026-07-06):**

**In WSL:**
```bash
# 1. Stop OpenCode (it may hold gbrain serve via MCP)
pkill -f "opencode web"

# 2. Stop gbrain serve (releases the PGLite lock)
pkill -f "gbrain serve"

# 3. Wait for the lock to release
sleep 2

# 4. Sync — use --no-hard-deadline for large syncs to prevent watchdog timeout
gbrain sync --repo /mnt/c/projects/mb-3-cs/project-library --no-hard-deadline

# 5. Embed new chunks
gbrain embed --stale --source-id project-library

# 6. Extract link/timeline edges
gbrain extract --stale --source-id project-library

# 7. Restart gbrain serve in background
nohup gbrain serve > /dev/null 2>&1 &

# 8. Restart OpenCode
cd /mnt/c/projects/mb-3-cs/project-library
opencode web --hostname 0.0.0.0
```

**Key points:**
- Always stop `gbrain serve` BEFORE running any CLI command — this is the #1 cause of sync timeouts
- Use `--no-hard-deadline` on sync for large changes (50+ files) — the default watchdog can time out
- Run `embed --stale` after sync to generate embeddings for new chunks
- Run `extract --stale` after embed to extract link/timeline edges
- Always restart `gbrain serve` after sync — OpenCode's MCP config expects it to be running
- The `~/.gbrain/.locks/` directory is a coordination mechanism, not a stale-lock problem — it's normally empty when no process is running
- Verify after sync: `gbrain stats` should show updated page/chunk counts

**Troubleshooting:**
- If `gbrain sync` times out: check if `gbrain serve` is still running (`pgrep -f "gbrain serve"`). Kill it, wait 2 seconds, retry.
- If `gbrain stats` times out: same cause — `gbrain serve` is holding the lock. Kill it first.
- If the sync watchdog fires: add `--no-hard-deadline` flag. The default watchdog (3600s + 30s grace) can still time out in practice on large syncs.
---

## WHERE SKILL OUTPUTS ARE SAVED
	[Bare commands follow Gstack default save protocol: [\\wsl.localhost\Ubuntu\home\dev-p\.gstack\projects]
	Named  skills save to gstack-outputs by custom instruction in AGENTS.MD ["C:\projects\mb-3-cs\project-library\gstack-outputs"]

---
### Opening the GStack projects folder

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
# Agents.MD

**Global**
\\wsl$\Ubuntu\home\dev-p\.config\opencode\AGENTS.md
**Local**
"C:\projects\mb-3-cs\project-library\AGENTS.md"

