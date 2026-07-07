# AD Enumeration (Without Credentials)

## SMB Null Session
```bash
smbclient -L //$TARGET_IP -N
crackmapexec smb $TARGET_IP -u '' -p ''
enum4linux-ng -A $TARGET_IP
```

## Anonymous LDAP
```bash
ldapsearch -x -H ldap://$TARGET_IP -b "dc=$DOMAIN,dc=local"
ldapsearch -x -H ldap://$TARGET_IP -b "dc=$DOMAIN,dc=local" | grep -i description
```

## Check SMB Signing
```bash
crackmapexec smb $TARGET_IP --gen-relay-list targets.txt
nmap --script smb-security-mode -p 445 $TARGET_IP
```

## Kerberos User Enumeration
```bash
kerbrute userenum -d $DOMAIN /usr/share/seclists/Usernames/Names/names.txt --dc $DC_IP
```

# AD Enumeration (With Credentials)

## BloodHound
```bash
bloodhound-python -u user -p password -d $DOMAIN -ns $DC_IP -c all
sudo neo4j start
bloodhound --no-sandbox
```
On Windows: `SharpHound.exe -c All -d $DOMAIN`

## CrackMapExec
```bash
crackmapexec smb $TARGET_IP -u user -p pass --users --groups --shares
crackmapexec smb $TARGET_IP -u user -p pass --local-groups
crackmapexec smb $TARGET_IP -u user -p pass -M spider_plus
crackmapexec smb $TARGET_IP -u user -p pass --admin-count
```

## Impacket Enumeration
```bash
impacket-lookupsid $DOMAIN/user:$pass@$TARGET_IP
impacket-samrdump $DOMAIN/user:$pass@$TARGET_IP
```
