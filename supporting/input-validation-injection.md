# Input Validation & Injection Prevention

## Checklist
- All external input is validated using allow-lists (type, length, format)
- APIs enforce a strict request schema (e.g., OpenAPI)
- Database access uses parameterized queries only
- File uploads (if applicable) are:
  - Validated by content (not extension)
  - Stored outside the web root and non-executable

---

## Guidance

All external input must be treated as untrusted regardless of source;user-submitted data, API request bodies and parameters, headers, cookies, URL segments, query strings, and data from external services. Internal services receiving externally-originated data must apply the same validation as the original entry point.

Input validation must use allowlists defining exactly what is acceptable in type, length, format, and range. Denylist approaches are insufficient as attack patterns evolve and encoding variations bypass them. For legacy systems, prioritise validation on inputs reaching database queries, file operations, system commands, and rendering contexts;these carry the highest injection risk.

APIs must enforce a strict request schema. Schema enforcement acts as a systematic allowlist at the API boundary;non-conforming requests rejected before reaching application logic. Mass assignment;binding all request fields to a model without explicit allowlisting must be avoided. Only fields the caller is permitted to set should be accepted.

Database access must use parameterized queries or prepared statements exclusively. String concatenation to construct queries must never appear regardless of how input has been validated;validation reduces risk but parameterization eliminates the injection vector. ORM usage does not automatically guarantee safety;raw query methods and string interpolation within ORM query builders bypass parameterization and must be treated as raw SQL. For legacy codebases with string-concatenated queries, ensure those queries operate under least-privilege database credentials while migration to parameterized queries is prioritised as critical security debt.

Second-order injection must be considered;data safely stored at entry can still cause injection when later used in a different context without re-validation.

User-supplied URLs triggering server-side requests must be validated against an allowlist of permitted destinations or schemes before any request is made to prevent SSRF.

Template engines processing user-supplied input are vulnerable to template injection. User input must never be passed directly into template rendering without validation. Use the engine's safe variable interpolation mechanisms.

Deserialization of untrusted data is high-risk. Serialized data from external sources must never be deserialized without integrity verification. Use data formats that do not support executable object types and apply strict type constraints during deserialization.

File uploads must be validated by actual content not client-declared extension or MIME type. Store outside the web root in a location where files cannot be directly requested or executed. Never use client-provided filenames in file system operations;generate server-side names. The original filename may be stored separately for display only after sanitisation.

---

## References
- OWASP Injection: https://owasp.org/Top10/A03_2021-Injection/
- OWASP Input Validation Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html
- OWASP SQL Injection Prevention: https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html
- OWASP File Upload Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html
- OWASP SSRF Prevention: https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html
- OWASP Deserialization Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Deserialization_Cheat_Sheet.html
