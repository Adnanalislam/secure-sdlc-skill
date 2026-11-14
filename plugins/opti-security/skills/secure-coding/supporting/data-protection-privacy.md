# Data Protection & Privacy

## Checklist
- All traffic uses approved secure TLS configuration
- Sensitive data (PII, credentials, tokens, secrets) is encrypted at rest
- Secrets are stored in an approved secrets management solution
- Only the minimum required data is collected and stored
- Authenticated or sensitive responses are non-cacheable

---

## Guidance

Before applying protection controls, classify data. Identify whether it contains PII, credentials, secrets, financial, or health information before deciding how it must be handled, stored, transmitted, and retained.

TLS with a currently approved configuration must apply to all traffic;external-facing endpoints, internal service-to-service, database connections, and message queues. Internal network position is not a substitute for encryption in transit. For legacy systems on deprecated TLS versions, upgrading is rarely a breaking change and must be treated as priority work.

Sensitive data must be encrypted at rest. Encryption strength depends entirely on key management;keys stored alongside the data they protect provide no meaningful security. Keys must be managed through an approved secrets management solution, rotated on a defined schedule, and access restricted to only the identities that require them. For legacy systems, field-level encryption on the most sensitive data is a viable incremental approach.

Cryptographic implementations must use approved well-maintained libraries. Custom cryptography must never be implemented. Approved algorithms: AES-256 for symmetric, RSA-2048 or higher and elliptic curve equivalents for asymmetric. Algorithm choice must be made at design phase.

Secrets must be stored exclusively in an approved secrets management solution; never in source code, configuration files, or version-controlled environment variables. For legacy systems with embedded secrets, restrict access to relevant repositories and define a migration timeline.

Only the minimum data necessary must be collected and stored. Retention limits must be defined and enforced. Backups and exports must apply the same encryption and access controls as production data.

Sensitive data must never appear in logs, error messages, or diagnostic output regardless of log level. Error responses to users must be generic;full detail captured server-side only. Error verbosity must be controlled through environment configuration, not hardcoded. Production must always return generic responses. Deployment pipelines must enforce this distinction.

Authenticated responses and any response containing sensitive or personalised data must be marked non-cacheable. Appropriate cache control headers must be set explicitly;absence of directives does not guarantee content will not be cached.

---

## References
- OWASP Cryptographic Failures: https://owasp.org/Top10/A02_2021-Cryptographic_Failures/
- OWASP Cryptographic Storage Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html
- OWASP TLS Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html
- OWASP Secrets Management Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html
