# Authorization & Access Control

## Checklist
- All authorization checks are enforced server-side
- UI-based restrictions are not relied on for security
- Resource access enforces object-level authorization (IDOR prevention)
- Roles, permissions, and service accounts follow least privilege
- Privileged actions are explicitly restricted and auditable

---

## Guidance

Authorization must be enforced server-side on every request. UI-based restrictions like hidden buttons, disabled menus, conditional rendering are user experience controls only and provide no security guarantee. Every protected action must be verified at the server regardless of what the interface presents.

Authorization must be applied at every layer independently;API gateway, service layer, and data layer. Enforcing only at the entry point leaves inner layers exposed if bypassed or called directly. For legacy systems where this is inconsistent, compensating controls at the outermost layer must be maintained while inner layer checks are introduced incrementally.

Authorization logic must avoid being scattered across individual endpoints without a unifying pattern;a single missed call site is sufficient for a bypass. For new development, route all requests through a centralised authorization layer or middleware. For legacy systems, introduce a shared authorization utility that new code paths use, migrating existing checks incrementally as those areas are touched in normal development.

Every request to access a resource must verify the requesting identity is authorised for that specific resource instance;not just the resource type. This object-level check must use the authenticated identity from the server-side session or token, never a user-supplied identifier. Both horizontal escalation (accessing another user's resources at the same privilege level) and vertical escalation (accessing higher-privilege functionality) must be explicitly prevented.

Roles, permissions, and service accounts must follow least privilege. Overly broad permissions accumulate over time in legacy systems and must be reviewed periodically. Service accounts must be scoped to the specific resources and actions they need.

Privileged actions like administrative operations, bulk data access, destructive operations, and permission changes must be restricted to authorised roles and generate tamper-evident audit log entries capturing who, what, when, and the affected resource. For legacy systems without this logging, introducing it is a priority ;it is rarely a breaking change.

Service-to-service calls must enforce authorization;internal network position is not a substitute. Each service must verify the caller is authorised for the requested operation.

---

## References
- OWASP Broken Access Control: https://owasp.org/Top10/A01_2021-Broken_Access_Control/
- OWASP Authorization Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html
- OWASP IDOR: https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References
