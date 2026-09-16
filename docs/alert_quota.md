# Quota exhausted alert in Devin CLI (Windows)

Desktop notification (toast) that pops up when Devin CLI detects that the
daily/weekly quota has been exhausted and the session starts consuming
extra credits.

## How it works

1. Devin CLI runs a **`Stop` hook** at the end of each turn of a session.
2. The hook calls a Python script (`quota_alert.py`) that scans the **most
   recent CLI log** for quota-exhausted phrases
   (`quota exhausted`, `usage limit reached`, `request more usage`,
   `insufficient credits/quota`, etc.) that occurred within the **last 5 minutes**.
3. If it finds a match, it launches a **Windows notification**
   (balloon/toast) using `System.Windows.Forms.NotifyIcon` via PowerShell.
4. Each detection is recorded in `quota_alert.log` for auditing.
5. Additionally, `"notify": "always"` makes Devin CLI send its own
   terminal/desktop notifications whenever any turn finishes or
   input is needed.

## Dependencies

| Resource | Path | Purpose |
|---|---|---|
| Devin CLI user config | `%APPDATA%\devin\config.json` | Defines `"notify"` and the `"Stop"` hook |
| Detection/alert script | `%APPDATA%\devin\scripts\quota_alert.py` | Reads logs and launches the toast |
| CLI logs | `%APPDATA%\devin\cli\logs\devin_*.log` | Data source: quota events are logged here |
| Own audit log | `%APPDATA%\devin\scripts\quota_alert.log` | Created automatically, stores each detection (date + log line) |
| Python 3 (Windows) | `python3` on the PATH (on this machine: Microsoft Store alias, `...\WindowsApps\python3.exe`) | Runs `quota_alert.py` |
| PowerShell | `powershell.exe` (ships with Windows) | Shows the toast via `.NET NotifyIcon` (invoked from Python) |

No external dependencies were installed (no `pip install`, no PowerShell
modules like BurntToast): everything uses standard Python libraries and
`.NET` assemblies already present in Windows.

## Step-by-step configuration

### 1. Create the detection script

Save as `%APPDATA%\devin\scripts\quota_alert.py`:

```python
"""Devin Stop hook: warns with a Windows notification when the
daily/weekly quota is exhausted and the session starts consuming extra
credits.

Modes:
    python quota_alert.py            -> scans the most recent CLI log
                                         and, if it detects a recent quota
                                         event, shows the toast.
    python quota_alert.py --test     -> shows the toast directly
                                         (to verify it works).
"""
import os
import re
import subprocess
import sys
from datetime import datetime, timedelta, timezone
from pathlib import Path

APPDATA = Path(os.environ["APPDATA"])
LOG_DIR = APPDATA / "devin" / "cli" / "logs"
DEBUG_LOG = APPDATA / "devin" / "scripts" / "quota_alert.log"

PATTERN = re.compile(
    r"quota exhausted|usage limit reached|request more usage|"
    r"insufficient[_ ]?(quota|credits?|usage|funds)|"
    r"(quota|usage|credits?|acu)\b.{0,40}\b(exhausted?|exceeded?|depleted|limit reached|out of)",
    re.IGNORECASE,
)
TS_RE = re.compile(r"(\d{4}-\d{2}-\d{2}T[\d:.]+Z)")


def show_toast():
    """Balloon-tip notification. Shell_NotifyIcon via ctypes doesn't reliably
    surface a balloon without a real window + message loop, so we delegate
    the actual display to .NET's NotifyIcon through a short PowerShell
    one-liner (confirmed working), while detection stays in Python."""
    ps_script = (
        "Add-Type -AssemblyName System.Windows.Forms;"
        "Add-Type -AssemblyName System.Drawing;"
        "$n = New-Object System.Windows.Forms.NotifyIcon;"
        "$n.Icon = [System.Drawing.SystemIcons]::Warning;"
        "$n.Visible = $true;"
        "$n.BalloonTipTitle = 'Devin - quota exhausted';"
        "$n.BalloonTipText = 'The session is using extra credits. Run /usage to check the balance.';"
        "$n.ShowBalloonTip(8000);"
        "Start-Sleep -Seconds 9;"
        "$n.Dispose()"
    )
    subprocess.Popen(
        ["powershell.exe", "-NoProfile", "-ExecutionPolicy", "Bypass",
         "-WindowStyle", "Hidden", "-Command", ps_script],
        creationflags=subprocess.CREATE_NO_WINDOW,
    )


def log_debug(message: str) -> None:
    DEBUG_LOG.parent.mkdir(parents=True, exist_ok=True)
    with DEBUG_LOG.open("a", encoding="utf-8") as f:
        f.write(f"{datetime.now().isoformat(timespec='seconds')} {message}\n")


def check_and_alert() -> None:
    if not LOG_DIR.is_dir():
        return
    logs = sorted(LOG_DIR.glob("devin_*.log"), key=lambda p: p.stat().st_mtime, reverse=True)
    if not logs:
        return
    latest = logs[0]
    cutoff = datetime.now(timezone.utc) - timedelta(minutes=5)

    try:
        lines = latest.read_text(encoding="utf-8", errors="ignore").splitlines()[-400:]
    except OSError:
        return

    for line in lines:
        if not PATTERN.search(line):
            continue
        m = TS_RE.search(line)
        if not m:
            continue
        try:
            ts = datetime.strptime(m.group(1), "%Y-%m-%dT%H:%M:%S.%fZ").replace(tzinfo=timezone.utc)
        except ValueError:
            continue
        if ts < cutoff:
            continue
        log_debug(f"HIT ({latest.name}): {line[:220]}")
        show_toast()
        return


if __name__ == "__main__":
    if "--test" in sys.argv:
        show_toast()
    else:
        check_and_alert()
```

### 2. Register the hook in the user config

Edit `%APPDATA%\devin\config.json` and add the `"notify"` and
`"hooks"` keys (without deleting what already exists):

```json
{
  "version": 1,
  "devin": {
    "org_id": "org-7cc56b98c0e5425cbf83facbeb29b3bc"
  },
  "shell": {
    "setup_complete": true
  },
  "theme_mode": "dark",
  "notify": "always",
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "python3.exe \"C:/Users/JesusGregorioPerez/AppData/Roaming/devin/scripts/quota_alert.py\"",
            "timeout": 15
          }
        ]
      }
    ]
  }
}
```

- `"notify": "always"` — Devin CLI sends a terminal/desktop notification
  on every relevant event (end of turn, needs input, etc.), not only for
  quota.
- `"hooks.Stop"` — runs at the end of each turn of any Devin CLI session
  on this machine; calls the quota detection script.

> Replace the `command` path if your Windows username is different from
> `JesusGregorioPerez`, or if you moved the script to another folder.

### 3. Verify the JSON is valid

```powershell
python3 -c "import json; json.load(open(r'C:\Users\<user>\AppData\Roaming\devin\config.json')); print('valid JSON')"
```

### 4. Test the toast manually

```powershell
python3 "$env:APPDATA\devin\scripts\quota_alert.py" --test
```

A balloon/notification **"Devin - quota exhausted"** should appear in the
system tray (next to the clock) for a few seconds.

### 5. Verify the hook from Devin CLI

Inside a Devin CLI session:

```
/hooks
```

It should list the `Stop` hook pointing to `quota_alert.py`.

## Behavior and limitations

- It only triggers **at the end of a turn** (`Stop` event), not in real
  time during execution.
- It depends on the CLI log containing one of the phrases in the
  `PATTERN` regex in `quota_alert.py`. If Cognition changes the exact
  wording of those messages, the regex must be updated.
- The detection window is 5 minutes (`cutoff` in the script) — a quota
  event older than that will not trigger the toast on a later hook run.
- Each detection is logged to `%APPDATA%\devin\scripts\quota_alert.log`
  with the date, log file, and a fragment of the matching line.

## Uninstall / disable

1. In `%APPDATA%\devin\config.json`, remove the `"hooks"` key (or only
   the `"Stop"` section if there are other hooks) and optionally set
   `"notify"` back to `"smart"` or `"never"`.
2. Delete `%APPDATA%\devin\scripts\quota_alert.py` and
   `%APPDATA%\devin\scripts\quota_alert.log` if no longer needed.
