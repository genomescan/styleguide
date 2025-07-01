# ✅ The Security Checklist

## 🔐 AUTHENTICATION SYSTEMS (Signup / Signin / 2FA / Password Reset)

- [ ] Use HTTPS everywhere.
- [ ] Store password hashes using Bcrypt (no salt necessary – Bcrypt does it for you).
- [ ] Destroy the session identifier after logout.
- [ ] Rotate session ID after login to prevent session fixation.
- [ ] Destroy all active sessions on password reset (or at least offer to).
- [ ] No open redirects after successful login or in intermediate redirects.
- [ ] Sanitize Signup/Login input for `javascript://`, `data://`, and CRLF characters.
- [ ] Limit login/verify/resend/generate API attempts per user. Use exponential backoff or CAPTCHA.
- [ ] Ensure reset password token is random.
- [ ] Set an expiration on the reset password token.
- [ ] Expire reset token after successful use.
- [ ] Ensure session cookies are marked Secure, HttpOnly, and SameSite=Strict (or Lax).
- [ ] Implement 2FA (e.g., OAuth2 or WebAuthn) for high-privilege actions like login, password change, or account deletion.

---

## 👤 USER DATA & AUTHORIZATION

- [ ] Verify logged-in user's ownership of resources (e.g., cart, history) using session ID.
- [ ] Avoid serially iterable resource IDs. Use `/me/orders` instead of `/user/37153/orders`.
- [ ] Rate-limit by user IP and account ID to prevent user enumeration.
- [ ] Require verification email to change account email.
- [ ] Sanitize uploaded filenames.
- [ ] Restrict file types on both frontend and backend.
- [ ] Enforce maximum file size limits.
- [ ] Validate file contents — don’t rely only on extension or MIME type.
- [ ] Sanitize EXIF tags in uploaded profile photos.
- [ ] Use RFC-compliant UUIDs for IDs instead of integers.
- [ ] Avoid exposing user existence via error messages (e.g., during login/signup/forgot password).

---

## 🛡️ SECURITY HEADERS & CONFIGURATIONS

- [ ] Add Content Security Policy (CSP) header to prevent XSS/data injection.
- [ ] Add CSRF header and set SameSite cookie attributes.
- [ ] Add HSTS header to prevent SSL stripping.
- [ ] Add your domain to the [HSTS Preload List](https://hstspreload.org).
- [ ] Add X-Frame-Options header to prevent clickjacking.
- [ ] Add SPF DNS record to reduce spam and phishing.
- [ ] Add subresource integrity (SRI) checks when loading JavaScript from CDNs.
- [ ] Use `require-sri-for` CSP directive to enforce SRI usage.
- [ ] Use random CSRF tokens; expose business APIs only via POST.
- [ ] Avoid exposing CSRF tokens over HTTP.
- [ ] Do not use sensitive data or tokens in GET parameters.

---

## 🧼 SANITIZATION OF INPUT

- [ ] Sanitize all user inputs to prevent XSS.
- [ ] Use parameterized queries to prevent SQL injection.
- [ ] Sanitize user input used for CSV import.
- [ ] Sanitize URL-like user inputs (e.g., usernames in custom URLs).
- [ ] Never hand-code JSON using string concatenation. Use language-native libraries.
- [ ] Sanitize URL inputs to avoid SSRF.
- [ ] Sanitize outputs before rendering to the user.

---

## ⚙️ OPERATIONS

- [ ] Use a managed service (e.g., AWS Elastic Beanstalk) if inexperienced.
- [ ] Use proper provisioning scripts for VM setup.
- [ ] Scan for machines with unnecessary open ports.
- [ ] Remove default passwords, especially for MongoDB & Redis.
- [ ] Use SSH keys — disable password-based SSH login.
- [ ] Apply software updates promptly to fix zero-days.
- [ ] Use only TLS 1.2 or higher for HTTPS.
- [ ] Disable DEBUG mode in production.
- [ ] Prepare for bad actors & DDoS — use services with mitigation support.
- [ ] Set up system monitoring and logging.
- [ ] Use a secrets manager — never hardcode credentials.
- [ ] Audit dependencies for known vulnerabilities (e.g., `pip-audit`, etc.).
- [ ] Set alerts for anomalous behavior (e.g., large uploads, unusual logins).

---

## 🧑 PEOPLE

- [ ] Set up `security@yourdomain.com` and a page for vulnerability reports.
- [ ] Limit access to production user databases.
- [ ] Be polite and responsive to security researchers.
- [ ] Conduct code reviews with security in mind.
- [ ] After a breach, review logs, notify users, reset credentials, and audit.
- [ ] Provide basic security awareness training for devs and ops.

---
