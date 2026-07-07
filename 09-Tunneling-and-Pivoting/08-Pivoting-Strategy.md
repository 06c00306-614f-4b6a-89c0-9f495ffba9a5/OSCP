# General Pivoting Strategy

1. **Get initial foothold** on first machine
2. **Identify internal networks**:
   - Linux: `ip route`, `ip a`, `cat /etc/hosts`
   - Windows: `route print`, `ipconfig /all`, `arp -a`, `netstat -ano`
3. **Choose pivoting method**:
   - SSH available → SSHuttle
   - SSH blocked, HTTP allowed → Chisel
   - Need UDP/ICMP → Ligolo-ng
   - Windows only → Chisel or Plink
   - Web root only → Neo-reGeorg
4. **Enumerate internal hosts**: `proxychains nmap`
5. **Exploit internal targets**: `proxychains <exploit>`
6. **Repeat** until all machines compromised
