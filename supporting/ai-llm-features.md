# AI, LLM, MCP & Gateway Security

## Checklist

### AI / LLM Features
- System prompts are isolated from user input
- LLM-accessible tools use least-privilege permissions
- Sensitive data is redacted before model interaction or logging
- Model outputs are validated or constrained with guardrails
- Rate limits and usage quotas are enforced

### MCP Servers:Building
- Every tool has a minimal clearly scoped permission set
- Tool descriptions are accurate and cannot mislead the model
- All tool inputs are validated before execution
- Tool responses return only data the invoking context requires
- MCP server authenticates the host before accepting connections
- All tool invocations and responses are logged

### MCP Servers:Consuming
- Only trusted reviewed MCP servers are connected to production agents
- Each connected server is granted only the tools the agent requires
- Tool definitions are reviewed before connection
- Cross-server tool invocation is explicitly controlled
- Third-party MCP servers are treated as untrusted until verified

### LLM Gateway
- All model requests are authenticated and attributed to an identity
- Request and response logging redacts sensitive data and PII
- Per-identity token consumption and cost are tracked and bounded
- Model routing is explicit and cannot be manipulated by request content
- Gateway is hardened against prompt injection at infrastructure layer
- Access to the gateway is restricted to authorised services and identities
- Gateway configuration and routing rules are version controlled and auditable

---

## Guidance

### AI / LLM Applications

AI and LLM features introduce a non-deterministic security surface that cannot be formally verified or exhaustively tested. Controls must constrain what the
model can access, what it can do, and what reaches users;not model behaviour directly. For Optimizely's Opal platform and agent workflows, these controls are foundational to security and customer trust.

Prompt injection is the primary attack vector. Direct injection comes from user input; indirect injection comes from external content the model processes; documents, web pages, data records. Both must be treated as threats. System prompts must be structurally separated from user input. External content must be treated as untrusted data, not trusted instruction. For agent workflows processing customer-supplied or third-party content, assume that content may attempt to redirect agent actions.

LLM-accessible tools must follow least privilege;each tool exposes only what the intended feature requires. Tool parameters supplied by the model must be validated in application code before execution, never delegated to the model's judgement. For agentic workflows taking real-world actions, autonomous scope must match what was explicitly configured. Confirmation applies only to actions outside configured scope or anomalous to the workflow's purpose.

Multi-agent architectures must not propagate trust implicitly. Each agent boundary must validate that the requested action falls within the original authorisation scope;a compromised orchestrating agent must not instruct downstream agents beyond what the user request authorised.

Context window content can be poisoned by external data designed to hijack agent behaviour particularly relevant for Opal workflows processing customer documents. Untrusted content must be clearly demarcated from instructions within the context structure.

Sensitive data like PII, credentials, customer data, internal system details must be redacted before reaching the model and before interactions are logged. For globally distributed products, data residency requirements may constrain which model or infrastructure can process specific customer data;evaluate at design phase.

Model outputs are probabilistic. Outputs used in security decisions or presented as verified fact must be validated in application code. For content generation features, guardrails should focus on safety and appropriateness rather than factual constraint. Delegating validation back to the model is insufficient;the model can be manipulated into approving its own outputs.

Rate limits and usage quotas must be enforced per identity before requests reach the model. Token consumption, context window size, and request frequency must all be bounded and calibrated against real usage patterns.

---

### MCP Servers

MCP servers are the primary mechanism through which agents interact with external systems. Because the model decides which tools to call based on natural language context, the security surface differs fundamentally from traditional APIs;the caller is non-deterministic and manipulable via prompt injection.

**Building:** Tool descriptions directly influence model tool selection and parameter construction;review them as security artifacts, not documentation.
All tool inputs must be validated as untrusted in the tool implementation. Tool responses must return only data necessary for the invoking context. Servers must authenticate the connecting host. All invocations must be logged;this is the primary mechanism for detecting manipulation.

**Consuming:** Only reviewed and approved servers must be connected to production agents;tool poisoning via misleading tool descriptions is a real documented threat. Connect only the specific tools the agent's workflows require. Cross-server invocation must be explicitly designed and controlled; implicit trust between connected servers allows a compromised server to pivot into other servers' capabilities. Third-party servers must be pinned to reviewed versions and monitored for updates with the same rigour as dependencies.

---

### LLM Gateway

An LLM gateway centralises all model traffic;it is both a critical control point and a high-value target. A compromised gateway affects every AI feature simultaneously.

Every request must be authenticated and attributed to a specific identity. Model routing must be controlled by gateway configuration, not request content; content-influenced routing enables traffic redirection to bypass safety controls. If the gateway enriches or transforms request content, that processing must treat arriving content as untrusted data at every stage.

All requests and responses must be logged with sensitive data and PII redacted. Token consumption and cost must be tracked per identity with limits enforced
at the gateway before requests reach the model. Anomalous consumption must generate alerts.

The gateway management plane;configuration, routing rules, model credentials, policy settings must be protected separately from the data plane with least privilege access and all changes logged. Gateway configuration must be version controlled. Model credentials must never be stored in configuration files; reference a secrets management solution. For legacy systems calling models directly, migration to gateway-based access should be planned as incremental consolidation, not a forced breaking change.

---

## References
- Optimizely Design-Phase Security Checklist: https://optimizely.atlassian.net/wiki/spaces/SEC/pages/4193452344
- OWASP LLM Top 10: https://owasp.org/www-project-top-10-for-large-language-model-applications/
- OWASP LLM01 Prompt Injection: https://genai.owasp.org/llmrisk/llm01-prompt-injection/
- OWASP LLM02 Sensitive Information Disclosure: https://genai.owasp.org/llmrisk/llm02-sensitive-information-disclosure/
- OWASP LLM06 Excessive Agency: https://genai.owasp.org/llmrisk/llm06-excessive-agency/
- OWASP Agentic AI Threats: https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/
- MCP Security Best Practices: https://modelcontextprotocol.io/docs/concepts/security
