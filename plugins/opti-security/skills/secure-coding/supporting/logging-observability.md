# Logging & Observability

## Checklist
- Security-relevant events are logged (auth failures, access changes, privileged actions)
- Logs do not contain passwords, tokens, or sensitive data
- Logs provide sufficient context for investigation and incident response

---

## Guidance

Logging serves both operational observability and security observability. Security-relevant events must be logged deliberately and consistently regardless of operational logging completeness. Absence of security event logging is typically discovered only during an incident when reconstruction is impossible.

Security-relevant events that must always be logged: authentication outcomes (successes and failures), authorisation denials, privileged and administrative
actions, permission and role changes, account modifications including credential and MFA changes, session creation and termination, and actions that modify, delete, or export sensitive data. These are the signals that distinguish attacker activity from legitimate use. For legacy systems without this logging, instrument authentication failures and privileged actions first;highest investigative value and rarely disruptive to add.

Every security log entry must contain: who (user ID, service account, or client identifier), what (action attempted or performed), where (resource or endpoint affected), when (UTC timestamp), result (success, failure, or denial), and context (source IP or request identifier for correlation). In distributed architectures, a consistent correlation identifier;request ID or trace ID must appear in every entry to enable cross-service chain reconstruction.

Logs must be structured rather than free-form strings. Structured logs are searchable, filterable, and aggregatable by log management platforms at scale. For legacy systems producing unstructured logs, a structured wrapper at the logging layer is an incremental improvement requiring no changes to individual call sites.

Logs must never contain passwords, secrets, tokens, full PII, or any data whose exposure constitutes a security incident. Sensitive data enters logs frequently through request body logging, verbose error capture, or debug output never removed from production. Log access is typically broader than application data access;many identities that cannot access production databases can access logs. User-controlled data written directly into log entries without sanitisation creates log injection risk.User-supplied values must be data within structured fields, not interpolated into messages.

Logs must be written to a destination the application cannot modify or delete. Log streams must be forwarded to a centralised platform;Optimizely uses Datadog mostly where retention, access control, and integrity are managed independently. Not everything should be forwarded at equal volume;debug and verbose operational logs serve development purposes but shipping them at full production volume generates significant ingestion cost without proportionate security value. Security-relevant events must always be forwarded. High-volume operational noise should be filtered or sampled before forwarding.

Security event logging without alerting provides only reactive value. Critical security events like repeated authentication failures, access denials on sensitive resources, privilege escalation attempts, anomalous patterns must have corresponding alerts. Alert thresholds must be calibrated against normal baseline behaviour to avoid fatigue while ensuring genuine incidents surface.

Log retention must be sufficient to support investigation of late-discovered incidents. A tiered approach aligns cost with value;high-value security events warrant longer retention, high-volume operational logs shorter retention. Retention periods must be defined explicitly, not left as default platform settings.

---

## References
- Optimizely Design-Phase Security Checklist: https://optimizely.atlassian.net/wiki/spaces/SEC/pages/4193452344
- OWASP Logging and Monitoring Failures: https://owasp.org/Top10/A09_2021-Security_Logging_and_Monitoring_Failures/
- OWASP Logging Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html
- OWASP Log Injection: https://owasp.org/www-community/attacks/Log_Injection
- Datadog Security Monitoring: https://docs.datadoghq.com/security/
