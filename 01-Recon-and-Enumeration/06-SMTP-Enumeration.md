# SMTP Enumeration (Port 25)

```bash
# VRFY users
smtp-user-enum -M VRFY -U /usr/share/seclists/Usernames/Names/names.txt -t $TARGET_IP

# Manual SMTP commands
telnet $TARGET_IP 25
EHLO test
VRFY root
VRFY admin
```
