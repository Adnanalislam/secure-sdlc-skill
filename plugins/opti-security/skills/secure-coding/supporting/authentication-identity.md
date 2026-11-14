# Authentication & Identity

## Checklist
- Authentication uses an approved centralized IAM (e.g., OAuth2 / OIDC)
- No custom authentication logic is implemented
- MFA is enforced for admin roles
- Sessions or tokens enforce server-side expiry and revocation
- Tokens, credentials, or secrets are never passed via URLs
- Cookies (if applicable) use approved secure settings (HttpOnly, Secure, SameSite)

---

## Guidance

Authentication must delegate to an approved centralised IAM using OAuth2 or OIDC. Custom authentication logic is never acceptable in new or updated code. For legacy systems where replacement is not immediate, compensating controls must be in place and migration planned. Only modern grant types appropriate to the application type should be used;legacy or non-interactive grant types that bypass MFA or expose credential validation directly are not acceptable.

MFA must be enforced for admin and privileged roles through the identity provider's access policy, not application code alone. Step-up authentication must apply for sensitive in-session actions such as credential changes or destructive operations. Changing authentication factors must require verification of the existing factor first.

Login and password reset endpoints must be rate limited. Rather than hard lockout;which degrades user experience and creates denial of service risk; implement progressive friction: increasing delays, CAPTCHA, or temporary soft lockouts with self-service recovery. Responses must be consistent in content and timing regardless of failure reason to prevent account enumeration. The same consistency applies to password reset;identical response whether the address exists or not. Password reset tokens must be time-limited, single-use, and invalidated immediately on use.

Passwords must be hashed using an adaptive algorithm such as bcrypt, Argon2, or PBKDF2. MD5, SHA1, and SHA256 are not acceptable for password storage. For legacy systems, re-hash transparently on next successful login.

JWT tokens must be validated on every use;signature, explicitly whitelisted algorithm, expiry, issuer, and audience. Decode functions that skip signature verification must never be used for security decisions. Signing keys must come from a secrets management solution. Access tokens must be short-lived with refresh token rotation ;rotate on every use, invalidate the previous token, and treat reuse as a compromise signal.Application-specific claims must be validated server-side on every request.

Sessions must enforce server-side expiry and revocation. Logout must invalidate server-side state and propagate to the identity provider in federated scenarios.
Where supported, sessions should be regenerated after login to prevent fixation. Tokens should be stored in memory or HttpOnly cookies;not localStorage.

Tokens and credentials must never appear in URL parameters.

Cookies carrying session state must use HttpOnly, Secure, and SameSite. For legacy applications with cross-origin auth flows use SameSite=Lax rather than Strict to avoid breaking federated login behaviour.

Service-to-service authentication must use platform-managed identity where available. Client credentials, when unavoidable, must come from a secrets management solution;never source code or version-controlled configuration.

All authentication events including failures, lockouts, token issuance, revocation, and credential changes must be logged with sufficient context for incident investigation. Logs must never contain passwords, tokens, or secrets.

---

## References
- OWASP Authentication Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html
- OWASP Session Management Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html
- OWASP Forgot Password Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Forgot_Password_Cheat_Sheet.html
- OWASP Top 10 A07: https://owasp.org/Top10/A07_2021-Identification_and_Authentication_Failures/
