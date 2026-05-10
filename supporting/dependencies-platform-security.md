# Dependencies & Platform Security

## Checklist
- Third-party libraries are actively maintained and supported
- Deprecated or end-of-life components are not used
- Platform security controls are enabled as per standards (e.g., HSTS, CSP, CORS)

---

## Guidance

The goal is preventing vulnerabilities from reaching pull requests. Optimizely operates SAST, SCA, secret detection, and CSPM tools across the SDLC; findings contribute directly to vulnerability management scores visible to leadership. Addressing root causes here reduces PR-stage findings.

Every third-party library must be actively maintained with a responsive security disclosure process and must not be at or approaching end-of-life. Unmaintained libraries accumulate unfixable CVEs;the only remediation is replacement, which is more disruptive the longer it is deferred. For legacy systems carrying EOL dependencies, maintain a tracked inventory and prioritise replacement by severity and exposure.

Dependencies must be pinned to specific versions. Defaulting to latest  whether manually or by an AI agent generating dependency declarations is not safe. The latest version may introduce security behaviour changes or new vulnerabilities silently. Unpinned dependencies produce inconsistent SCA results between builds. Lock files must be committed and kept current.

Security-sensitive dependency updates require scrutiny beyond compatibility review. A change to an authentication library, cryptographic primitive, or token validation component may look identical to any other diff but can silently alter security behaviour. Major version bumps in security-sensitive libraries must trigger a deliberate security-focused changelog review.

Transitive dependencies carry the same risk as direct dependencies. Direct dependency choices must account for the quality and security posture of their full dependency trees. License compliance must also be verified;copyleft licenses introduced through transitive dependencies carry legal risk alongside security risk.

AI agents and code generation tools including Claude make dependency decisions as part of code generation. Dependencies introduced by AI-generated code must be reviewed with the same rigour as manually chosen ones. AI agents tend to reference library versions from training data which may be outdated or superseded. All AI-generated dependency declarations must be verified against current maintained versions and pinned explicitly before being committed.

Secret detection findings are best prevented by never committing secrets; all secrets must come from an approved secrets management solution. Encoding, splitting, or obfuscating secrets to avoid detection is deliberate circumvention and must never be used.

Container base images must be pinned, updated on a defined cadence, and rebuilt regularly even when application code has not changed. CSPM findings frequently originate from outdated base images unrelated to application code.

Platform security controls like HSTS, CSP, and CORS must be correctly configured. Misconfiguration is as dangerous as absence. HSTS must include appropriate max-age and subdomain coverage. CORS must define an explicit allowlist;wildcard origins on authenticated endpoints defeat same-origin protections. CSP must restrict sources to known trusted origins;unsafe-inline or wildcard sources provide no XSS protection. For legacy applications, deploy CSP in report-only mode first.

---

## References
- Optimizely Design-Phase Security Checklist: https://optimizely.atlassian.net/wiki/spaces/SEC/pages/4193452344
- OWASP Vulnerable and Outdated Components: https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/
- OWASP Software and Data Integrity Failures: https://owasp.org/Top10/A08_2021-Software_and_Data_Integrity_Failures/
- OWASP HSTS Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html
- OWASP CORS Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Cross-Origin_Resource_Sharing_Cheat_Sheet.html
- OWASP CSP Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html
