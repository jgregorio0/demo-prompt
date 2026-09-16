---
description: "Entorno Windows: la terminal del agente puede ser PowerShell o bash (WSL/Git Bash) — nunca asumas cmd"
trigger: always_on
---

# Entorno de ejecución: Windows — detecta la shell primero

- La shell puede ser **PowerShell** o **bash**: si `uname` funciona es bash; si no, PowerShell.
- Si es bash, distingue: existe `/mnt/c` → **WSL**; existe `/c` → **Git Bash**. En WSL la interop puede estar deshabilitada (`cmd.exe`/`powershell.exe` fallan con "Exec format error"); en Git Bash sí funciona.

## Rutas
- PowerShell: `C:\dev\proyecto\archivo.txt`
- bash: traduce `C:\` → `/mnt/c/` (WSL) o `/c/` (Git Bash), unidad en minúscula. `ls "C:\..."` siempre falla.

## PowerShell — NO es cmd
- `dir`/`ls`/`cat`/`rm` son alias: `dir /s /b` falla con `ParameterBindingException` → `Get-ChildItem -Recurse -Name "C:\ruta" | Select-String -Pattern "patron"`
- `type` → `Get-Content` · `%VAR%` → `$env:VAR` · `which` → `Get-Command` · `rm -rf` → `Remove-Item -Recurse -Force` · `tail -f` → `Get-Content -Wait -Tail`
- PS 5.1 (comprueba `$PSVersionTable`): `curl`/`wget` son `Invoke-WebRequest` (usa `curl.exe`); `&&`/`||` no existen (usa `;` o `if ($?) {}`); `>` escribe UTF-16LE (usa `Out-File -Encoding utf8`). No hay `sudo`.
- cmd real (batch, `mklink`): `cmd /c 'comando "con dobles"'` — comillas simples externas; `\"` no escapa en PowerShell.

## PATH
- Si `node`/`npm`/`git`/`python` no se encuentran, recarga el entorno o usa la ruta completa; no asumas que faltan.
