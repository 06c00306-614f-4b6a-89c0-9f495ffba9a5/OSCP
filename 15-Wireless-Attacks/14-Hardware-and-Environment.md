# Wireless Adapter & Environment Setup

## Recommended Hardware
Not all wireless adapters support monitor mode and packet injection.

| Adapter | Chipset | Monitor Mode | Injection | Bands |
|---|---|---|---|---|
| Alfa AWUS036ACH | RTL8812AU | ✅ | ✅ | 2.4/5 GHz |
| Alfa AWUS036ACM | MT7610U | ✅ | ✅ | 2.4/5 GHz |
| Panda PAU09 | RTL8187L | ✅ | ✅ | 2.4 GHz only |
| TP-Link TL-WN722N (v1) | AR9271 | ✅ | ✅ | 2.4 GHz only |
| TP-Link TL-WN722N (v2/v3) | RTL8188EU | ❌ | ❌ | Avoid these |

## Check Adapter Capabilities
```bash
# Check if driver supports monitor mode
iw list | grep -A 10 "Supported interface modes"
# Look for: "* monitor"

# Check if driver supports injection
iw list | grep "Supported TX frame types"
```

## Enable Monitor Mode
```bash
# Method 1: airmon-ng (recommended)
sudo airmon-ng start wlan0

# Method 2: Manual
sudo ifconfig wlan0 down
sudo iw dev wlan0 set type monitor
sudo ifconfig wlan0 up

# Verify
iwconfig
```

## Kill Interfering Processes
```bash
# Processes that interfere with wireless monitoring
sudo airmon-ng check kill
# Usually kills: NetworkManager, wpa_supplicant, dhclient
```

## Troubleshooting Common Issues
```bash
# "No such device" - Adapter not connected or wrong driver
lsusb | grep -i wireless
dmesg | grep -i firmware

# "Operation not supported" - Adapter doesn't support monitor mode
# Get a supported adapter

# "Fixed channel wlan0mon" - Channel hopping stopped
sudo iw dev wlan0mon set channel 6

# "Injection not working" - Check driver capabilities
aireplay-ng -9 wlan0mon    # Injection test
```

## Restore Normal Mode
```bash
# After finishing wireless testing
sudo airmon-ng stop wlan0mon
sudo systemctl restart NetworkManager
```
