# Bluetooth & IoT Attacks

## Bluetooth Reconnaissance
```bash
# Install bluez tools
sudo apt install bluez bluez-tools

# Scan for Bluetooth devices
sudo hcitool scan

# Scan for BLE (Bluetooth Low Energy) devices
sudo hcitool lescan

# Detailed info on device
sudo hcitool info XX:XX:XX:XX:XX:XX
```

## Bluetooth Service Discovery
```bash
# Find services on a device
sdptool browse XX:XX:XX:XX:XX:XX

# Record all Bluetooth traffic (sniff)
# Requires specific hardware (Ubertooth, BlueSniffer)
```

## Bluetooth Attacks
```bash
# Bluesnarfing (old) - OPP data access
# Bluebugging - remote control of device
# BlueBorne (CVE-2017-0781) - RCE over BT

# Use bluetoothctl for modern device interaction
sudo bluetoothctl
scan on
devices
info XX:XX:XX:XX:XX:XX
```

## Zigbee/Z-Wave (IoT)
Requires specialized hardware (e.g., Texas Instruments CC2531, HackRF).

```bash
# KillerBee framework for Zigbee
git clone https://github.com/riverloopsec/killerbee

# Scan Zigbee channels
zbstumbler

# Sniff Zigbee traffic
zbdump -c 11 -w zigbee_capture.pcap
```

## Tools for IoT Assessment
| Tool | Purpose |
|---|---|
| `hcitool` | Basic BT/BLE scanning |
| `bluetoothctl` | BT device interaction |
| `bettercap` | BLE advertisement attacks |
| `killerbee` | Zigbee assessment |
| `ubertooth` | BT/BLE packet capture (hardware) |
| `hackrf` | SDR for RF analysis (hardware) |

> Note: Bluetooth and IoT attacks require specific hardware adapters and are an advanced topic beyond core wireless pentesting.
