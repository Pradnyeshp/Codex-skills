---
name: secrets-scan
description: Scan the codebase and git history for hardcoded secrets and credentials, then remediate and prevent leaks
---

# Secrets Scan

Find committed secrets, get them out of the code safely, and stop new ones from landing.

## Steps

1. **Scan the working tree** — search source, config, scripts, notebooks, and CI files for high-risk patterns: API keys, tokens, passwords, private keys (`BEGIN PRIVATE KEY`), connection strings, cloud credentials, and high-entropy strings
2. **Use a scanner if available** — prefer an installed tool (`gitleaks`, `trufflehog`, `detect-secrets`) for coverage; fall back to targeted grep over known patterns and common files (`.env`, `*.pem`, `config.*`, `settings.*`)
3. **Check git history** — a secret removed from HEAD but present in history is still leaked; scan past commits where feasible
4. **Triage findings** — for each hit, decide: real secret, test/sample placeholder, or false positive. Report with file, line, and secret type — never echo the full secret value
5. **Remediate** — move real secrets to environment variables or a secrets manager, reference them from code, and add a safe placeholder to `.env.example`
6. **Rotate** — flag that any genuinely exposed secret must be rotated/revoked at its provider; removing it from code is not enough
7. **Prevent** — ensure `.gitignore` covers secret files, and recommend a pre-commit secret scan hook

## Output Format

```text
## Secrets Scan

### Findings
- [HIGH] <type> in path/to/file.ext:LINE — <action: rotate + move to env>
- [LOW]  possible token in path/to/file.ext:LINE — likely test placeholder

### Remediation
1. ...

### Rotate immediately
- <provider> credential exposed since <commit/date>
```

## Rules

- **Never print full secret values** in output or logs — mask all but the last few characters
- Any secret committed to a shared/remote repo must be treated as compromised and **rotated**, not just deleted
- Removing a secret from the current files does not remove it from git history — call that out explicitly
- Distinguish real secrets from obvious test/sample placeholders to avoid noisy false positives
- Don't commit real secrets into `.env.example` or fixtures; use clearly fake placeholders
- Recommend prevention (gitignore + pre-commit scan) so the same leak can't recur
- Don't rewrite git history unless the user explicitly asks — explain the trade-offs first
