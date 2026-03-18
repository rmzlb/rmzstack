# Security Review Checklist

Adapted from OWASP Top 10 for code review context.

---

## Input Validation

```
[ ] All user input validated on the server (not just client)
[ ] SQL queries use parameterized statements (never string concatenation)
[ ] HTML output is escaped/sanitized (XSS prevention)
[ ] File uploads: type checked, size limited, stored outside webroot
[ ] URL redirects: whitelist allowed destinations (open redirect prevention)
[ ] JSON parsing: schema validated before use
[ ] Path traversal: user input never directly in file paths
```

## Authentication & Authorization

```
[ ] Passwords hashed with bcrypt/argon2 (not MD5/SHA)
[ ] Session tokens: sufficient entropy, httpOnly, secure flags
[ ] Rate limiting on login/signup/reset endpoints
[ ] Every API endpoint checks authorization (not just authentication)
[ ] Admin routes have separate auth middleware
[ ] JWT: algorithm verified (no "alg: none"), expiry checked, refresh token rotated
[ ] OAuth: state parameter used, redirect URI validated
```

## Data Exposure

```
[ ] No secrets in code (API keys, passwords, tokens)
[ ] No secrets in git history (check with: git log -p --all -S 'password')
[ ] Error messages don't leak stack traces or internal paths
[ ] API responses don't include fields the user shouldn't see
[ ] Logs don't contain PII or credentials
[ ] .env files in .gitignore
[ ] Database credentials not hardcoded
```

## Infrastructure

```
[ ] HTTPS enforced (no HTTP fallback)
[ ] CORS: specific origins, not wildcard (*)
[ ] CSP headers set (Content-Security-Policy)
[ ] Dependencies: no known vulnerabilities (npm audit / cargo audit)
[ ] Docker: non-root user, minimal base image, no secrets in layers
[ ] Database: connections use SSL, credentials rotated
```

## Common Patterns to Flag

| Pattern | Risk | Fix |
|---------|------|-----|
| `eval()` or `exec()` with user input | Critical — code injection | Remove or sandbox |
| `dangerouslySetInnerHTML` | High — XSS | Sanitize with DOMPurify |
| `SELECT * FROM ... WHERE id = '${id}'` | Critical — SQL injection | Use parameterized query |
| Hardcoded JWT secret | High — token forgery | Move to env variable |
| `cors({ origin: '*' })` | Medium — CSRF | Whitelist specific origins |
| `JSON.parse()` without try/catch | Medium — crash on bad input | Wrap in try/catch |
| Logging `req.body` wholesale | Medium — PII leak | Log specific fields only |
