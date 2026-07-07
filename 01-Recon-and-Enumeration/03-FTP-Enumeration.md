# FTP Enumeration (Port 21)

```bash
# Anonymous login
ftp $TARGET_IP
# username: anonymous, password: anonymous

# Recursive download with wget
wget -r --no-passive ftp://anonymous:anonymous@$TARGET_IP
```
