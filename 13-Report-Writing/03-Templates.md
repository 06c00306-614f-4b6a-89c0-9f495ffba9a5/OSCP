# Report Templates

## Executive Summary Template
```
On [DATE], a penetration test was conducted against [NUMBER] target systems.
The test successfully compromised [NUMBER] systems, including [LIST].

Key Findings:
- [Critical finding 1]
- [Critical finding 2]

Root Causes:
- Outdated software with known vulnerabilities
- Weak password policies
- Service misconfigurations

Recommendations:
- Apply security patches
- Implement strong password policies
- Review service configurations
```

## Tools for Report Writing
```bash
# Markdown → PDF
pandoc report.md -o report.pdf --from markdown --template=eisvogel --listings
```

**CherryTree**: Hierarchical notes with embedded screenshots
**Obsidian**: Markdown knowledge base with linking
