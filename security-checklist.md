# 🔐 The Security Checklist

---

## 🧾 AUTHENTICATION SYSTEMS (Signup / Signin / 2FA / Password Reset)

- [ ] Use HTTPS everywhere.
- [ ] Store password hashes using **Bcrypt** (no salt necessary – Bcrypt handles it).
- [ ] Destroy the session identifier after logout.
- [ ] Rotate session ID after login to prevent session fixation.
- [ ] Destroy all active sessions on password reset (or at least offer to).
- [ ] No open redirects after login or in intermediate redirects.
- [ ] Sanitize Signup/Login input for `javascript://`, `data://`, CRLF characters.
- [ ] Limit login/verify/resend/generate API attempts per user. Use exponential backoff or CAPTCHA.
- [ ] Ensure reset password token is random.
- [ ] Set an expiration time on reset password tokens.
- [ ] Expire reset token after successful use.
- [ ] Mark session cookies as `Secure`, `HttpOnly`, and `SameSite=Strict` (or `Lax`).
- [ ] Implement 2FA (e.g., TOTP or WebAuthn) for sensitive actions.

---

## 👤 USER DATA & AUTHORIZATION

- [ ] Validate ownership of resources using session ID.
- [ ] Avoid serial IDs – use `/me/orders` instead of `/user/123/orders`.
- [ ] Apply rate-limiting per IP and per account ID.
- [ ] Require email verification for changing email.
- [ ] Sanitize uploaded filenames.
- [ ] Restrict allowed file types on frontend and backend.
- [ ] Enforce maximum file size limits.
- [ ] Validate file content, not just extension or MIME type.
- [ ] Sanitize EXIF tags in uploaded images.
- [ ] Use RFC-compliant UUIDs for user/resource IDs.
- [ ] Do not reveal if a user exists via error messages.

---

## 🛡️ SECURITY HEADERS & CONFIGURATIONS

- [ ] Add Content-Security-Policy (CSP) header.
- [ ] Add CSRF protection and `SameSite` cookie attributes.
- [ ] Add HTTP Strict Transport Security (HSTS) header.
- [ ] Submit your domain to the [HSTS preload list](https://hstspreload.org).
- [ ] Add `X-Frame-Options` header.
- [ ] Add `X-Content-Type-Options: nosniff` header.
- [ ] Set a strict `Referrer-Policy`.
- [ ] Add SPF DNS record to protect from spoofing.
- [ ] Use Subresource Integrity (SRI) when loading from CDNs.
- [ ] Add `require-sri-for` to CSP to enforce SRI use.
- [ ] Use random CSRF tokens and make APIs use HTTP `POST`.
- [ ] Do not expose CSRF tokens over insecure HTTP.
- [ ] Avoid including sensitive data in GET request parameters.

---

## 🧼 SANITIZATION OF INPUT

- [ ] Sanitize all user inputs to prevent XSS.
- [ ] Use parameterized queries (e.g., SQL prepared statements).
- [ ] Sanitize inputs used for CSV import.
- [ ] Sanitize inputs that may appear in URLs (e.g., usernames).
- [ ] Never build JSON via string concatenation — use native libraries.
- [ ] Sanitize URL inputs to avoid SSRF.
- [ ] Sanitize outputs before rendering to users.
- [ ] Avoid rendering raw user HTML unless using sanitizers (e.g., DOMPurify).

---

## ⚙️ OPERATIONS

- [ ] Consider using PaaS like AWS Elastic Beanstalk if inexperienced.
- [ ] Use provisioning scripts for infrastructure.
- [ ] Check for and close unnecessary open ports.
- [ ] Remove/change default DB credentials (e.g., MongoDB, Redis).
- [ ] Use SSH key-based authentication (disable passwords).
- [ ] Apply OS and library security updates promptly.
- [ ] Use only TLS 1.2+; disable weaker versions.
- [ ] Ensure DEBUG mode is disabled in production.
- [ ] Use DDoS-resistant hosting or mitigation tools.
- [ ] Set up logging and system monitoring.
- [ ] Use secrets managers — never hardcode credentials.
- [ ] Regularly audit third-party packages (e.g., `pip-audit`, `npm audit`).
- [ ] Set alerts for unusual activity (e.g., login spikes, large uploads).

---

## 🧑‍🤝‍🧑 PEOPLE

- [ ] Set up `security@yourdomain.com` for vulnerability reports.
- [ ] Create a `security.txt` or vulnerability disclosure page.
- [ ] Limit internal access to production user data.
- [ ] Be polite and responsive to bug reporters.
- [ ] Conduct secure code reviews with peers.
- [ ] After a breach: audit logs, notify users, force password resets.
- [ ] Provide security training for developers and operations staff.

---
