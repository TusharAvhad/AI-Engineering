---
inclusion: always
description: "Secure coding rules applied to all generated code. Covers input validation, injection prevention, authentication, authorization, cryptography, and data protection."
---

# Secure Coding Rules

## Rules

You MUST follow these security rules in all code you write or modify.

### Input validation

- Validate all external input at the boundary (API handlers, event processors, CLI parsers). Reject invalid input before processing.
- Validate type, length, range, and format. Use allowlists over denylists.
- Sanitize strings for the output context (HTML-encode for HTML, URL-encode for URLs, parameterize for SQL).
- Never trust client-side validation alone. Server-side validation is mandatory.

### Injection prevention

- **SQL**: Always use parameterized queries or prepared statements. Never concatenate input into SQL strings.
- **Command**: Never pass user input to `exec()`, `eval()`, `system()`, `child_process.exec()`, or shell interpreters. Use parameterized APIs (e.g., `subprocess.run([...], shell=False)` in Python, `execFile` in Node.js).
- **XSS**: Encode output for the rendering context. Use framework auto-escaping (React JSX, Jinja2 autoescape, Angular templates). Never insert untrusted data via `innerHTML`, `dangerouslySetInnerHTML`, or template literals into HTML.
- **Path traversal**: Validate and canonicalize file paths. Reject input containing `..`, null bytes, or absolute paths. Resolve against a known safe base directory.

### Authentication

- Hash passwords with bcrypt (cost ≥12), scrypt, or Argon2id. Never MD5, SHA-1, or unsalted SHA-256.
- Use timing-safe comparison for tokens and password hashes (`crypto.timingSafeEqual`, `hmac.compare_digest`).
- Set secure cookie flags: `HttpOnly`, `Secure`, `SameSite=Strict` (or `Lax` with justification).
- Tokens must expire. Access tokens: short-lived (≤15 minutes). Refresh tokens: rotated on use.
- Implement account lockout or exponential backoff after repeated failed attempts.

### Authorization

- Check permissions on every request. Never rely on client-side hiding of UI elements.
- Default deny. Grant access explicitly, never assume access because a check is absent.
- Prevent IDOR: validate that the authenticated user owns or has access to the requested resource.
- Use role-based or attribute-based access control. Keep authorization logic in a single layer, not scattered across handlers.

### CSRF protection

- State-changing requests (POST, PUT, DELETE) must include an anti-CSRF token or use `SameSite=Strict` cookies.
- Verify `Origin` or `Referer` header on state-changing requests when tokens are impractical.

### Cryptography

- Use established libraries (e.g., `crypto` in Node.js, `cryptography` in Python, AWS KMS). Never implement your own.
- Symmetric encryption: AES-256-GCM (authenticated encryption). Never ECB mode.
- Use `crypto.randomBytes()` / `secrets.token_bytes()` for random values. Never `Math.random()` or `random.random()` for security-sensitive purposes.
- Key management: store keys in environment variables, secrets managers, or KMS. Never in source code.

### Sensitive data protection

- Never log passwords, tokens, API keys, credit card numbers, or PII. Log only identifiers (user ID, request ID).
- Sanitize error responses in production. Return generic messages to clients, log detailed errors server-side.
- Never expose stack traces, SQL queries, or internal paths in API responses.
- Encrypt sensitive data at rest. Use TLS 1.2+ for all data in transit.

### Security headers

When generating HTTP server code, include:
- `Strict-Transport-Security: max-age=31536000; includeSubDomains`
- `Content-Security-Policy` appropriate to the application
- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: DENY` (or `SAMEORIGIN` with justification)
- `Referrer-Policy: strict-origin-when-cross-origin`

### Dependency security

- Use exact/pinned versions in lockfiles. Never open ranges for production dependencies.
- Prefer well-known, actively maintained packages. Flag unfamiliar or low-download packages.
- Never run `eval()` on package content or trust post-install scripts from unknown packages.

### Rate limiting

- Apply rate limiting to authentication endpoints, password reset, and any endpoint that triggers external actions (email, SMS, API calls).
- Return `429 Too Many Requests` with a `Retry-After` header.

### Session management

- Regenerate the session ID on login, privilege escalation, and any authentication state change.
- Set an inactivity timeout. Terminate the session server-side on logout — do not rely on client-side cookie deletion.
- Never expose session identifiers in URLs, error messages, or logs.
- Generate a new session after switching from HTTP to HTTPS.

### Secure authentication responses

- Authentication failure messages must not reveal which field was incorrect. Return a generic "invalid credentials" response, never "user not found" or "wrong password" separately.
- Use POST for credential submission. Never place passwords or tokens in URL query strings.
- Re-authenticate the user before critical operations (changing email, password, payment method, or deleting an account).

### Data in transit

- Never include sensitive data (tokens, passwords, API keys, PII) in GET request parameters. URL query strings are logged by browsers, proxies, and servers.
- Disable client-side caching on responses containing sensitive data (`Cache-Control: no-store`).

### Database security

- Application database connections must use the lowest-privilege role required. Never connect as superuser/admin in application code.
- Close database connections promptly. Use connection pooling with bounded pool sizes.
- Store connection strings in environment variables or secrets managers, never in source code.

### Fail securely

- When security logic (authentication, authorization, cryptographic validation) encounters an error, the default must be DENY — not allow, not skip, not return null.
- Never write catch blocks that swallow auth/authz errors and proceed as if successful.
- If security configuration cannot be loaded, refuse to start rather than running with no controls.

### Security event logging

- Log all authentication failures (with username but never the attempted password).
- Log all authorization/access-control denials (user, resource, action).
- Log all input validation rejections (field and reason, not the rejected value if it may contain credentials).
- Log administrative actions and changes to security configuration.
- Never log session tokens, passwords, or secrets in any log entry.

### File handling

- Validate file type by content (magic bytes), not just extension or MIME type from the client.
- Store uploaded files outside the webroot with generated filenames.
- Set a maximum file size. Reject before fully buffering.

## What these rules prevent

- SQL injection, command injection, XSS, path traversal (OWASP A03: Injection)
- Broken authentication and session management (OWASP A07: Identification and Authentication Failures)
- Broken access control and IDOR (OWASP A01: Broken Access Control)
- Sensitive data exposure (OWASP A02: Cryptographic Failures)
- Security misconfiguration (OWASP A05: Security Misconfiguration)
- Supply chain attacks from compromised dependencies (OWASP A06: Vulnerable and Outdated Components)
