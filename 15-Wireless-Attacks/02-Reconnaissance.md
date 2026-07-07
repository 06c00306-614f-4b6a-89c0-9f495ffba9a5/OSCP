# Wireless Reconnaissance

The first phase of any wireless assessment: discover networks, clients, and channels.

## Enable Monitor Mode
```bash
# List wireless interfaces
iwconfig

# Kill interfering processes
sudo airmon-ng check kill

# Enable monitor mode on wlan0
sudo airmon-ng start wlan0

# Verify mode changed (should show "Mode:Monitor")
iwconfig wlan0mon

# Alternative: manual setup
sudo ip link set wlan0 down
sudo iw dev wlan0 set type monitor
sudo ip link set wlan0 up
```

## Discover Networks and Clients
```bash
# Scan all channels, basic discovery
sudo airodump-ng wlan0mon

# Scan specific channel with BSSID filter
sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF wlan0mon

# Save output to file
sudo airodump-ng -w recon-output wlan0mon

# Output files:
# recon-output-01.cap   - Packet capture
# recon-output-01.csv   - CSV summary of APs
# recon-output-01.kismet.csv - Kismet format
# recon-output-01.log.csv - Extended log
```

## Kismet (Advanced Recon)
```bash
# Start Kismet server
sudo kismet

# Access web interface at http://localhost:2501
# Shows: SSIDs, channels, encryption, client devices,
#         WPS capabilities, manufacturer info
```

## What to Gather
```
- SSID and BSSID (MAC) of each AP
- Channel number
- Encryption type (WEP, WPA, WPA2, WPA3)
- Signal strength
- Connected clients (MAC addresses)
- WPS enabled? (Y/N)
- Beacon interval
- Vendor/manufacturer of AP
```
