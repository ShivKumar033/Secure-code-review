## Source code review - Regex

### Goal
Use regex during **manual source code review** to quickly identify:

- Hardcoded secrets
- Authentication material
- Cloud credentials
- OAuth tokens
- Debug / insecure configurations
- Cryptographic weaknesses
- CI/CD leaks    
- Backdoors & logic flaws

Regex **does not confirm a vulnerability** — it **points you to dangerous code paths** for manual review.

## 1. Generic Secrets (High Coverage)

### Regex
```php
(?i)(api[_-]?key|secret|token|password|passwd|pwd|auth|access[_-]?key)\s*[:=]\s*['"][^'"]{8,}['"]
```
**Notes**
- `(?i)` → case-insensitive
- Catches most **hardcoded secrets**
- `{8,}` avoids obvious false positives
- Works well for JS, Python, Java, PHP, YAML

## 2. Password Assignments

### Regex
```php
(?i)(password|passwd|pwd)\s*[:=]\s*['"][^'"]+['"]
```
**Notes**
- Flags **explicit password storage**
- High severity if in:
    - Auth logic
    - Database config
    - Admin users

## 3. Cloud Credentials

### AWS Access Key
```php
AKIA[0-9A-Z]{16}
```

### AWS Secret Key
```php
(?i)aws(.{0,20})?(secret|private).{0,5}[A-Za-z0-9/+=]{40}
```
**Notes**
- Almost **zero false positives**
- Immediate key rotation if confirmed
- Often found in:
    - CI configs
    - Test scripts
    - Old commits

## 4. OAuth / Auth Tokens

### OAuth Client Secrets
```php
(?i)(client_secret|client_id|oauth_token)\s*[:=]\s*['"][^'"]+['"]
```

### Bearer Token
```php
(?i)authorization\s*[:=]\s*['"]?bearer\s+[A-Za-z0-9\-_.=]+
```

### JWT Tokens
```php
eyJ[A-Za-z0-9_-]+\.eyJ[A-Za-z0-9_-]+\.[A-Za-z0-9_-]+
```
**Notes**
- JWT regex is **extremely reliable**
- Check:
    - Hardcoded tokens
    - Tokens logged in code
    - Tokens in comments

---

## 5. Database Secrets & Connection Strings

### DB URLs
```php
(?i)(mysql|postgres|mongodb|redis|jdbc):\/\/[^'"\s]+
```

### DB Credentials
```php
(?i)(db_user|db_username|db_pass|db_password)\s*[:=]\s*['"][^'"]+['"]
```
**Notes**
- Often leaks **credentials + host**
- Leads to:
    - Direct DB access
    - Lateral movement

---

## 6. Privilege Escalation Logic Bugs 
### Hardcoded Admin Flags
```php
(?i)(is_admin|admin|superuser|root)\s*=\s*(true|1|yes)
```

### Role Checks Bypass
```php
(?i)(role|permission|access_level)\s*==\s*['"]?(admin|root)['"]?
```

### Command Execution (RCE Sinks)
```php
(?i)\b(exec|system|popen|shell_exec|passthru|Runtime\.getRuntime)\b
```

### Unsafe Deserialization
```php
(?i)\b(unserialize|pickle\.loads|ObjectInputStream|JSON\.parse)\b
```


## 7. ADVANCED Regex (High-Value Findings)

### Private Keys (Critical)
```php
-----BEGIN (RSA|EC|DSA|OPENSSH) PRIVATE KEY-----
```

### Base64-Encoded Secrets (Hidden)
```php
(?i)(secret|token|key).{0,20}[A-Za-z0-9+/]{20,}={0,2}
```
### Hardcoded Admin Logic
```php
(?i)(is_admin|admin|superuser|root)\s*=\s*(true|1|yes)
```

### Debug / Dev Mode Enabled
```php
(?i)(debug|dev_mode|development)\s*=\s*(true|1)
```
✔ Often leads to:
- Stack traces
- Auth bypass
- Info disclosure

### Dangerous Crypto (Weak Hashing)
```php
(?i)\b(md5|sha1|des|rc4)\b
```

### Hardcoded IPs / Internal Networks

```php
\b(10\.|192\.168\.|172\.(1[6-9]|2[0-9]|3[0-1]))\.\d+\.\d+\b
```

### Command Execution (RCE Hunting)
```php
(?i)(exec|system|popen|shell_exec|Runtime\.getRuntime)
```

### File Inclusion / Path Traversal
```php
(?i)(include|require|open|readFile)\s*\(.*\$
```

## 8. CI/CD & DevOps Secrets (Goldmine)

### GitHub Actions
```php
(?i)secrets\.[A-Z0-9_]+
```

### Docker ENV Secrets
```php
(?i)ENV\s+[A-Z0-9_]+\s*=\s*.+
```

### Kubernetes Secrets
```php
(?i)kind:\s*Secret
```

---

## 9. One-Shot Mega Regex (Recon Pass)
```php
(?i)(api[_-]?key|token|secret|password|passwd|pwd|authorization|client_secret|access[_-]?key)

(?i)(api[_-]?key|token|secret|password|authorization|client_secret|access[_-]?key|jwt)
```