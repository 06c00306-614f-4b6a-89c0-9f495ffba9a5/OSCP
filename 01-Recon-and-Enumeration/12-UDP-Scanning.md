# UDP Port Scanning

Many OSCP boxes run services on UDP that are missed by default scans. Missing UDP can cost hours.

## Nmap UDP Scan
```bash
# Top 20 UDP ports (fast)
nmap -sU --top-ports=20 $TARGET_IP -oA nmap/udp-top20

# Top 100 UDP ports (more thorough)
nmap -sU --top-ports=100 $TARGET_IP -oA nmap/udp-top100

# Specific common UDP ports
nmap -sU -p 53,67,68,69,111,123,135,137,138,139,161,162,500,514,520,631,1434,1812,4500 $TARGET_IP
```

## Common UDP Ports to Check
| Port | Service | Purpose |
|---|---|---|
| 53 | DNS | Zone transfer, recon |
| 67/68 | DHCP | Internal network info |
| 69 | TFTP | File transfer (no auth) |
| 111 | RPC | RPC info leak |
| 123 | NTP | NTP amplification, recon |
| 137-139 | NetBIOS | Windows enumeration |
| 161 | SNMP | System information leak |
| 162 | SNMP Trap | SNMP data |
| 500 | ISAKMP | VPN detection |
| 514 | Syslog | Log access |
| 520 | RIP | Routing info |
| 631 | IPP | Printer info |
| 1434 | MSSQL | MSSQL browser service |
| 1812 | RADIUS | Authentication |
| 4500 | IPSec NAT-T | VPN detection |

## Quick UDP Script for Bash
```bash
# Quick check without nmap
for port in 53 69 111 123 137 161 500 1434; do
    (echo >/dev/udp/$TARGET_IP/$port) 2>/dev/null && echo "UDP $port open" || echo "UDP $port closed/timeout"
done
```

> **Tip**: If you're stuck on a machine, run a UDP scan in the background. It might find a service that's your entry point.
