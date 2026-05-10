# Abuse & Misuse Protection

## Checklist
- Rate limits are applied to authentication and sensitive API endpoints
- High-risk or expensive operations have abuse controls (enforce usage limits)

---

## Guidance

Authentication endpoint rate limiting and progressive friction controls are covered in the Authentication & Identity section. This section addresses the broader abuse surface beyond authentication.

Rate limiting must be applied at a granularity appropriate to the endpoint and risk. A single global limit is insufficient;sensitive operations, data retrieval endpoints, and expensive functions each require calibrated limits. Before enforcing any limit, observe legitimate traffic patterns to establish a baseline;limits applied without this understanding will affect real users, enterprise integrations, and internal automation. A monitoring-first approach, measuring limits against real traffic before enforcement, reduces disruption risk. Limits must be enforced server-side and must account for distributed abuse across multiple IP addresses or identities. For legacy systems, API gateway or infrastructure-level rate limiting serves as a compensating control while application-level controls are introduced.

High-risk and expensive operations like bulk data exports, report generation, file processing, email dispatch, payment initiation must have explicit usage limits per identity within a time window. Before setting limits, audit actual legitimate usage patterns. A limit blocking a valid enterprise workflow creates pressure to remove the control entirely. Limits must reflect the upper boundary of legitimate use with sufficient headroom, not an arbitrary low threshold.

Business logic abuse occurs when legitimate features are used unintentionally; stacking discounts, exploiting referral systems, manipulating voting, or automating manual actions. These attacks operate within defined functionality and are invisible to controls looking only at malformed input. Abuse controls must be designed at the feature level for any feature involving incentives, competitive mechanics, or financial or reputational consequences.

Resource exhaustion attacks use technically valid but expensive-to-process requests;large payloads, deeply nested structures, complex queries. Request size limits, payload depth limits, query complexity limits, and processing timeouts must be enforced. Before applying limits to legacy systems, audit existing legitimate request sizes.
Non-sequential resource identifiers reduce enumeration efficiency as a defence-in-depth measure. This does not substitute for authorization controls.

For features incorporating AI or LLM capabilities, abuse controls specific to model usage are covered in the AI & LLM Features section.

---

## References
- OWASP API Security Unrestricted Resource Consumption: https://owasp.org/API-Security/editions/2023/en/0xa4-unrestricted-resource-consumption/
- OWASP Denial of Service Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Denial_of_Service_Cheat_Sheet.html
- OWASP Abuse Case Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Abuse_Case_Cheat_Sheet.html
