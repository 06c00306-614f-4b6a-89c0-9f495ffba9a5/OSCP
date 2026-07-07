# SMB Enumeration (Port 139, 445)

```bash
# List shares (null session)
smbclient -L //$TARGET_IP -N

# Connect to share
smbclient //$TARGET_IP/share -N

# Recursively download share
smbclient //$TARGET_IP/share -N -c 'prompt OFF; recurse ON; mget *'

# SMBMap
smbmap -H $TARGET_IP
smbmap -H $TARGET_IP -u user -p pass

# CrackMapExec
crackmapexec smb $TARGET_IP
crackmapexec smb $TARGET_IP -u user -p pass --shares
crackmapexec smb $TARGET_IP -u '' -p '' --shares

# Enum4linux-ng
enum4linux-ng -A $TARGET_IP
```
