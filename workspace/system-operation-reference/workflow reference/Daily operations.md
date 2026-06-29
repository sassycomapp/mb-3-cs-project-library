# Getting To Work

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
   - inspecting the repos under `/mnt/c/_mb2-cs-app/...`.  
 
 ## OPENCODE START Sequence
**In WSL:**
A) start sequence to start opencode server and open the opencode web UI:
	```bash
	cd /mnt/c/_mb2-cs-app/project-library
	opencode web --hostname 0.0.0.0
	```
B) Alternative start sequence to start opencode server and open the opencode web UI:
	```bash
	cd /mnt/c/_mb2-cs-app/gstack
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

---
***

## Active repos  (Windows side)

1. `C:\_mb2-cs-app` — **Workspace folder**  
   - Not a Git repo.  
   - Container for everything (repos, GStack, workspace docs).

2. `C:\_mb2-cs-app\mb-3-cs` — **Git code repo**  
	`/mnt/c/_mb2-cs-app/mb-3-cs`  
   - Anvil‑linked production‑adjacent application code.  

3. `C:\_mb2-cs-app\project-library` — **Git docs repo**  
   `/mnt/c/_mb2-cs-app/project-library`
   - Design/planning/architecture documents.  
   - Not Anvil‑linked.

4. `C:\_mb2-cs-app\gstack` — **GStack repo**  
   `/mnt/c/_mb2-cs-app/gstack` 
   - Workflow/skillpack used by the agents inside OpenCode.
   
5. `C:\_mb2-cs-app\gbrain` — **GBrain repo**  
   - Workflow/skillpack used by the agents inside OpenCode.

***

## Switching Projects
you switch projects from within OpenCode by sending the appropriate `/mnt/...` path in chat after the server is running.
You **do not** switch projects by restarting the server; 
***

## Committing to Github after new work 
**In WSL:**
	```bash
	cd /mnt/c/_mb2-cs-app/project-library
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
	Named  skills save to gstack-outputs by custom instruction in AGENTS.MD ["C:\_mb2-cs-app\project-library\gstack-outputs"]]

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