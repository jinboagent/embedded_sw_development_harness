---
name: tmux-esp32-workflow
description: Route ESP32 build/flash/monitor commands through a WSL tmux session from a Windows AI agent, with sentinel polling and log-tail reads instead of dumping output into context. Use when developing ESP-IDF firmware on a Windows host with WSL and the user wants live build visibility without flooding the agent context.
---

# tmux ESP32 Workflow

Pattern for controlling ESP32 build/flash/monitor through a WSL tmux session from a Windows AI agent.

## When to Use

When the user has a tmux session set up for ESP32 development and wants the AI to route all build/flash/monitor operations through it for live visibility.

## Prerequisites

- Windows host with WSL (Ubuntu) installed
- tmux session running in WSL (default session name: `build_infor`)
- ESP-IDF installed on Windows (at `C:\Espressif\`)
- ESP32 board connected via USB (note the COM port)
- Build scripts in project root: `build.bat`, `flash.bat`, `monitor.bat`

## Build Scripts

Create these in the project root for easy tmux invocation:

**build.bat:**
```bat
@echo off
call C:\Espressif\idf_cmd_init.bat
cd /d <PROJECT_PATH>
idf.py build
```

**flash.bat:**
```bat
@echo off
call C:\Espressif\idf_cmd_init.bat
cd /d <PROJECT_PATH>
idf.py -p <COM_PORT> flash
```

**monitor.bat:**
```bat
@echo off
call C:\Espressif\idf_cmd_init.bat
cd /d <PROJECT_PATH>
idf.py -p <COM_PORT> monitor
```

## Core Commands

### Send command to tmux session
```bash
wsl -d Ubuntu -e tmux send-keys -t <session_name> "<command>" Enter
```

### Read output from tmux session
```bash
wsl -d Ubuntu -e tmux capture-pane -t <session_name> -p -S -<lines>
```

### Stop interactive program (monitor)
```bash
wsl -d Ubuntu -e tmux send-keys -t <session_name> C-]
```

## Typical Workflow

### 1. Build
```bash
wsl -d Ubuntu -e tmux send-keys -t build_infor ".\build.bat" Enter
# Wait ~60-120 seconds for build
wsl -d Ubuntu -e tmux capture-pane -t build_infor -p -S -15
```

### 2. Flash
```bash
wsl -d Ubuntu -e tmux send-keys -t build_infor ".\flash.bat" Enter
# Wait ~30 seconds for flash
wsl -d Ubuntu -e tmux capture-pane -t build_infor -p -S -10
```

### 3. Monitor
```bash
wsl -d Ubuntu -e tmux send-keys -t build_infor ".\monitor.bat" Enter
# Wait ~10-15 seconds for boot output
wsl -d Ubuntu -e tmux capture-pane -t build_infor -p -S -30
```

### 4. Stop Monitor (before building again)
```bash
wsl -d Ubuntu -e tmux send-keys -t build_infor C-]
# Wait 2-3 seconds for clean exit
```

### 5. Clean Rebuild
```bash
wsl -d Ubuntu -e tmux send-keys -t build_infor "Remove-Item -Recurse -Force .\build; .\build.bat" Enter
```

## Important Notes

- **Only one program can hold the COM port at a time.** Stop monitor before flashing.
- **The tmux session may run PowerShell or bash.** Adjust commands accordingly (e.g., `.\build.bat` for PowerShell, `./build.bat` for bash).
- **Use `timeout /t N /nobreak >nul` in Windows cmd** to wait between send-keys and capture-pane.
- **Incremental builds are fast** (~10-30s). Clean builds take ~2 minutes.
- **Flash takes ~5-10 seconds** depending on binary size.
- **If build fails with version mismatch**, delete `build/` directory and rebuild from scratch.

## Troubleshooting

| Problem | Cause | Fix |
|---------|-------|-----|
| `idf.py not recognized` | ESP-IDF env not loaded | Use `build.bat` wrapper (calls `idf_cmd_init.bat`) |
| Build cache mismatch | ESP-IDF version changed | Delete `build/` and rebuild |
| Monitor shows old output | Flash didn't complete | Wait longer after flash before starting monitor |
| COM port busy | Monitor still running | Send `Ctrl+]` to stop monitor first |
| Commands go to wrong place | Sent to monitor stdin | Stop monitor first, then send commands |
