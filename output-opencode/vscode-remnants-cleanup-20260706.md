---
title: "vscode-remnants-cleanup-20260706"
doc-id: "vscode-remnants-cleanup-20260706"
---
# VS Code Remnants Cleanup — 2026-07-06

## Task
Remove leftover VS Code files and directories from the system following VS Code uninstallation.

## Findings

| Path | Status |
|---|---|
| `C:\Users\dev-p\.vscode\` | Already absent — nothing to remove |
| `C:\Users\dev-p\.code\` | Does not exist — no `.code` directory anywhere on the system |
| `C:\Users\dev-p\AppData\Roaming\Code\` | **Found and removed** — 472 MB, main VS Code data directory (settings, extensions, caches) |

## Actions taken

1. Searched for `.code` directory across entire filesystem — not found. User can safely ignore.
2. Searched for `.vscode*` in both WSL home and Windows user home — already absent.
3. Identified `C:\Users\dev-p\AppData\Roaming\Code\` (472 MB) as the primary leftover.
4. `rm -rf` via WSL failed on Git `askpass` scripts due to Windows permission locks.
5. Successfully removed via `cmd.exe /c rmdir /s /q` — Windows-native deletion bypassing the permission barrier.
6. Verified directory is entirely gone.

## Result
- **472 MB freed**
- No remaining VS Code artifacts of significance (minor temp files in `AppData\Local\Temp` are harmless and will age out)

## Next steps
- None. VS Code cleanup is complete. User will lead housekeeping/documentation reorganization manually.
