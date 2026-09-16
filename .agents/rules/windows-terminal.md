---
description: "CRÍTICO EN WINDOWS: Para ejecutar comandos en PowerShell 5.1 o bash y evitar errores comunes."
trigger: always_on
---

# Entorno de ejecución: Windows — detecta la shell primero

- La shell puede ser **PowerShell** o **bash**: si `uname` funciona es bash; si no, PowerShell.
- Si es bash, distingue: existe `/mnt/c` → **WSL**; existe `/c` → **Git Bash**. En WSL la interop puede estar deshabilitada (`cmd.exe`/`powershell.exe` fallan con "Exec format error"); en Git Bash sí funciona.

## ⚠️ REGLA CRÍTICA DE CONCATENACIÓN EN POWERSHELL (PS 5.1)

- **NUNCA utilices `&&` ni `||`** para encadenar comandos en PowerShell en Windows.
- Windows PowerShell 5.1 es la versión por defecto en Windows. El uso de `&&` o `||` produce un error de sintaxis fatal e inmediato (`ParserError: El token '&&' no es un separador de instrucciones válido en esta versión`) y ningún comando se ejecuta.
- **Cómo encadenar comandos en PowerShell:**
  - **Secuencial (sin importar el resultado):** Usa punto y coma `;`
    - *Ejemplo:* `git add archivo.txt ; git commit -m "mensaje"`
  - **Éxito condicional (equivalente a `&&`):** Usa `; if ($?) { ... }`
    - *Ejemplo:* `git add archivo.txt ; if ($?) { git commit -m "mensaje" }`
  - **Fallo condicional (equivalente a `||`):** Usa `; if (-not $?) { ... }`
    - *Ejemplo:* `comando_principal ; if (-not $?) { comando_alternativo }`
  - **Mejor opción:** No encadenes comandos complejos en un solo comando de terminal; divídelos en múltiples herramientas secuenciales.

## Rutas
- PowerShell: `C:\dev\proyecto\archivo.txt`
- bash: traduce `C:\` → `/mnt/c/` (WSL) o `/c/` (Git Bash), unidad en minúscula. `ls "C:\..."` siempre falla.

## PowerShell — NO es cmd
- `dir`/`ls`/`cat`/`rm` son alias: `dir /s /b` falla con `ParameterBindingException` → `Get-ChildItem -Recurse -Name "C:\ruta" | Select-String -Pattern "patron"`
- `type` → `Get-Content` · `%VAR%` → `$env:VAR` · `which` → `Get-Command` · `rm -rf` → `Remove-Item -Recurse -Force` · `tail -f` → `Get-Content -Wait -Tail`
- PS 5.1 (comprueba `$PSVersionTable`): `curl`/`wget` son `Invoke-WebRequest` (usa `curl.exe`); `>` escribe UTF-16LE (usa `Out-File -Encoding utf8`). No hay `sudo`.
- cmd real (batch, `mklink`): `cmd /c 'comando "con dobles"'` — comillas simples externas; `\"` no escapa en PowerShell.

## PATH
- Si `node`/`npm`/`git`/`python` no se encuentran, recarga el entorno o usa la ruta completa; no asumas que faltan.
