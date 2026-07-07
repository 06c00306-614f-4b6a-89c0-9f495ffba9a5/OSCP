# SSHuttle (VPN-Like Tunneling)

SSHuttle creates a transparent VPN-like tunnel through SSH — no proxychains needed once it's running.

```bash
# Basic usage (forwards entire subnet through SSH)
sshuttle -vr user@pivot_ip 10.0.0.0/24

# With explicit key file
sshuttle -vr user@pivot_ip 10.0.0.0/24 -e "ssh -i id_rsa"

# Multiple subnets
sshuttle -vr user@pivot_ip 10.0.0.0/24 172.16.0.0/16

# Daemonize (run in background)
sshuttle -D -vr user@pivot_ip 10.0.0.0/24

# Auto-discover remote subnet
sshuttle -vr user@pivot_ip 0/0 -N
```

## Tool Comparison
| Tool | Best For | Limitations |
|---|---|---|
| **SSH** tunnels | Simple single-port forward | One port at a time |
| **SSHuttle** | VPN-like transparent proxy | SSH must be available; TCP only |
| **Chisel** | HTTP/HTTPS tunneling (through firewalls) | Manual proxy setup |
| **Ligolo-ng** | Full network access, UDP+ICMP, performance | TUN interface setup needed |

```bash
# Quick decision guide:
# SSH available? → SSHuttle (easiest)
# SSH blocked? → Chisel (HTTP tunnel)
# Need UDP/ICMP through pivot? → Ligolo-ng
# Windows pivot? → Chisel or Plink
```
