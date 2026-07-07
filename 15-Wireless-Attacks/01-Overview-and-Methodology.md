# Wireless Penetration Testing Overview

Wireless network attacks are a critical skill for any penetration tester. While not explicitly listed as a separate exam domain in the current PEN-200 syllabus (the focus is on AD/Web/AWS), wireless knowledge is assumed for real-world assessments and covered by Kali Linux's extensive wireless toolset.

## Wireless Pentest Phases

```
1. Reconnaissance      → Discover SSIDs, channels, clients, APs
2. Monitor Mode Setup  → Enable wireless card for packet capture
3. Packet Capture      → Capture handshakes, probe requests, beacon frames
4. Cracking            → Crack WPA/WPA2 PSK, PMKID, WEP
5. Client Attacks      → Deauth, evil twin, KRACK, WPS Pixie-Dust
6. Enterprise Attacks  → WPA2-Enterprise (PEAP/EAP-TTLS credential capture)
7. Post-Exploitation   → Network access, lateral movement via wireless entry
8. Reporting           → Document findings, recommendations
```

## Kali Wireless Tools Overview

| Tool | Purpose |
|---|---|
| `aircrack-ng` | WEP/WPA PSK cracking |
| `airmon-ng` | Enable/disable monitor mode |
| `airodump-ng` | Packet capture, client/AP discovery |
| `aireplay-ng` | Packet injection (deauth, replay) |
| `airbase-ng` | Fake AP / rogue AP creation |
| `airdecap-ng` | Decrypt captured WEP/WPA traffic |
| `hcxdumptool` | Capture PMKID and handshakes |
| `hcxpcapngtool` | Convert PCAP to hashcat format |
| `hashcat` | GPU-accelerated PMKID/WPA cracking |
| `reaver` | WPS PIN brute force |
| `bully` | WPS PIN brute force (alternative) |
| `pixiewps` | WPS offline Pixie-Dust attack |
| `wifite` | Automated wireless auditing |
| `bettercap` | MITM, credential capture, rogue AP |
| `kismet` | Wireless sniffer/IDS |
| `cowpatty` | WPA-PSK cracking (brute force) |
| `asleap` | LEAP/PEAP credential cracking |

## Legal Disclaimer
> Always have **written authorization** before testing wireless networks. Wireless pentesting without permission is illegal.
