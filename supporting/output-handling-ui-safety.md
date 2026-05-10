# Output Handling & UI Safety

## Checklist
- User-controlled data is encoded before rendering
- Framework-provided output escaping is used correctly
- Unsafe rendering methods (raw HTML / dynamic script injection) are avoided
- Error messages are generic and expose no internal details

---

## Guidance

All data from user input, external systems, or any source outside direct application control must be encoded before rendering. The correct encoding depends on rendering context HTML body, attribute, JavaScript, URL, and CSS each require different encoding. Applying HTML encoding to JavaScript context or vice versa provides no protection. Context must be identified correctly before encoding is applied.

Cross-site scripting manifests in three forms requiring specific controls. Stored XSS occurs when malicious input is saved and later rendered to other users;prevented by encoding output at rendering regardless of how data was stored. Reflected XSS occurs when input from the current request is included in the response prevented by encoding all request-derived data before rendering. DOM-based XSS occurs entirely client-side when data from browser-accessible sources such as the URL, referrer, or storage is passed into dangerous DOM sinks not prevented by server-side encoding and requires safe client-side DOM manipulation. Dangerous sinks including innerHTML,document.write, eval, and setTimeout with string arguments must not receive unvalidated data. For legacy frontend codebases, introduce sanitisation at the sink as an incremental control while refactoring toward safer patterns.

Framework-provided output escaping must not be bypassed. Raw HTML rendering methods exist in virtually every framework and must be avoided unless content has been explicitly sanitised through a trusted sanitisation library. Any use of raw HTML rendering requires justification and review.

Responses containing user-controlled data must set the correct content type explicitly. JSON responses without application/json content type may be interpreted as HTML by some browsers, creating XSS vectors.

Content Security Policy must be defined and enforced for all web-facing applications. A correctly configured policy restricts script, style, and resource sources and can eliminate XSS exploitation even when encoding fails. For legacy applications, deploy in report-only mode first to identify violations before enforcement.

User-controlled values must never be used as redirect destinations without validation against an allowlist of permitted URLs or origins. 
Frame control must be enforced through response headers restricting which origins may embed the application to prevent clickjacking. For legacy applications with legitimate cross-origin framing, restrict to explicitly permitted origins rather than disabling frame control entirely.

Error handling must follow strict separation: generic responses to users, full detail server-side only. Stack traces, internal paths, database errors, and framework version information must never appear in responses. Error verbosity must be controlled through environment configuration. Production must always return generic errors and the deployment pipeline must enforce this distinction.

---

## References
- OWASP XSS Prevention Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html
- OWASP DOM XSS Prevention: https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html
- OWASP Content Security Policy: https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html
- OWASP Clickjacking Defence: https://cheatsheetseries.owasp.org/cheatsheets/Clickjacking_Defense_Cheat_Sheet.html
- OWASP Unvalidated Redirects: https://cheatsheetseries.owasp.org/cheatsheets/Unvalidated_Redirects_and_Forwards_Cheat_Sheet.html
