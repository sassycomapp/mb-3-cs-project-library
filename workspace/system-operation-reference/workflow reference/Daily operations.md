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
   - inspecting the repos under `/mnt/c/mybizz/...`.  
 
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

Important: serve daemon note
The gbrain serve process was restarted as a daemon (PID 21415) after maintenance. When you start your next OpenCode session, the MCP config will try to start a SECOND serve process — this will fail because PGLite is single-writer. Before starting a new session, kill the daemon: kill 21415. OpenCode will start serve fresh via MCP.

---
***

## Active repos  (Windows side)

1. `C:\mybizz` — **Workspace folder**  
   - Not a Git repo.  
   - Container for everything (repos, GStack, workspace docs).

2. `C:\mybizz\mb-3-cs` — **Git code repo**  
	`/mnt/c/projects/mb-3-cs/mb-3-cs`  
   - Anvil‑linked production‑adjacent application code.  

3. `C:\mybizz\project-library` — **Git docs repo**  
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

## Updating G brain after new work 
**In WSL:**
	```bash
	pkill -f "opencode web"
	pkill -f "gbrain serve"
	sleep 3
	gbrain sync --source project-library
	gbrain embed --stale

	opencode web --hostname 0.0.0.0
	```
---

## WHERE SKILL OUTPUTS ARE SAVED
	[Bare commands follow Gstack default save protocol: [\\wsl.localhost\Ubuntu\home\dev-p\.gstack\projects]
	Named  skills save to gstack-outputs by custom instruction in AGENTS.MD ["C:\mybizz\project-library\gstack-outputs"]]

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
**LOcal**
"C:\mybizz\project-library\agents.md"

