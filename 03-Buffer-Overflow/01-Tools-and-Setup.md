# Buffer Overflow: Tools & Setup

## Required Tools
- **Immunity Debugger** (on Windows VM)
- **mona.py** plugin for Immunity Debugger
- **Vulnserver** or custom vulnerable app
- **Python 3** for exploit scripting
- **Metasploit** `msfvenom` for shellcode

## Setup
```bash
# Install mona.py in Immunity Debugger
# Place mona.py in: C:\Program Files\Immunity Inc\Immunity Debugger\PyCommands\

# Set mona working directory
!mona config -set workingfolder c:\mona\%p
```

## Mona Commands Reference
| Command | Purpose |
|---|---|
| `!mona config -set workingfolder c:\mona\%p` | Set working dir |
| `!mona modules` | List loaded modules & protections |
| `!mona bytearray -b "\x00"` | Create byte array |
| `!mona bytearray -b "\x00\x0a\x0d"` | With known bad chars |
| `!mona compare -f c:\mona\bytearray.bin -a <addr>` | Compare memory |
| `!mona jmp -r esp` | Find JMP ESP |
| `!mona jmp -r esp -cpb "\x00\x0a"` | JMP ESP excluding bad chars |
| `!mona find -s "\xff\xe4"` | Find JMP ESP manually |
| `!mona findmsp` | Find offset via pattern |
| `!mona pattern_create 3000` | Create cyclic pattern |
| `!mona pattern_offset <EIP>` | Get offset value |

## Process Overview
```
1. Fuzzing → 2. Offset Finding → 3. Control EIP → 
4. Bad Characters → 5. JMP ESP → 6. Shellcode → 7. Exploit
```
