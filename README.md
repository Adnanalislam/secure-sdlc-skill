# Optimizely Security Skills for Claude

## What This Is

This is the Optimizely Security Skills marketplace for Claude Code. It provides security-focused skills built and maintained by the Optimizely Product Security team — ensuring that every piece of code written or reviewed through Claude meets Optimizely's internal security standards,OWASP standards whether security is explicitly asked about or not.

---

## Skills Available

| Skill | Description |
|---|---|
| `secure-coding` | Enforces Optimizely security standards silently across all code generation, review, refactoring, and debugging workflows |

More skills will be added here as the security programme expands.

---

## Repository Structure

```
secure-sdlc-skill/
├── .claude-plugin/
│   └── marketplace.json          ← marketplace entry point
├── plugins/
│   └── opti-security/
│       ├── .claude-plugin/
│       │   └── plugin.json       ← plugin manifest
│       ├── skills/
│       │   └── secure-coding/    ← secure coding skill
│       │       ├── SKILL.md
│       │       └── supporting/
│       │           ├── authentication-identity.md
│       │           ├── authorization-access-control.md
│       │           ├── data-protection-privacy.md
│       │           ├── input-validation-injection.md
│       │           ├── output-handling-ui-safety.md
│       │           ├── abuse-misuse-protection.md
│       │           ├── dependencies-platform-security.md
│       │           ├── logging-observability.md
│       │           └── ai-llm-features.md
│       └── README.md
└── README.md
```
---

## Quick Install (Claude Code)

Two commands. That is it.

```bash
/plugin marketplace add https://github.com/optimizely/secure-sdlc-skill
/plugin install opti-security@opti-security
/reload-plugins
```

Verify the skill loaded:

```
/plugin
```

Go to **Installed** — you should see `opti-security` enabled with `secure-coding`
listed as an installed component.

---

## How It Works

Once installed, the `secure-coding` skill activates automatically whenever you
write, review, refactor, or discuss code in Claude Code. No slash command needed.
Security is enforced silently — you receive secure code without needing to ask for it.

To verify it is active, run any code request:

```
write a login function that accepts email and password
```

You should see `Skill(opti-security:secure-coding) — Successfully loaded skill`
at the top of the response.

<img width="2884" height="2038" alt="image" src="https://github.com/user-attachments/assets/3aec556f-18cf-490f-b2af-879b66a45216" />


<img width="3386" height="1866" alt="image" src="https://github.com/user-attachments/assets/0a0a8278-5d0a-4adc-92d1-7b8bd880c100" />


---
### If Auto-Trigger Is Not Activating

In rare cases where the skill does not activate automatically, add this line to your project's `CLAUDE.md` file at the repo root:

```markdown
Use the opti-security:secure-coding skill for all code interactions.
```

This explicitly instructs Claude Code to always apply the skill in that specific project.


---

## Workflow

**For individual developers:**
Install once via the marketplace — the skill then applies automatically across every repository you work in, regardless of which repo you clone.
**No per-repo setup needed**.

**For automatic enforcement for all collaborators:**

Commit the skill directly into your product repository. Every contributor who clones the repo gets the skill automatically without any manual installation steps.

**Step 1 — Download the skill files**
Go to `https://github.com/optimizely/secure-sdlc-skill` → **Code → Download ZIP**.
Extract the ZIP on your machine.

**Step 2 — Copy the skill into your product repo**

```bash
# Navigate to your product repo root
cd /path/to/your/product-repo

# Create the skill directory
mkdir -p .claude/skills/secure-coding

# Copy SKILL.md
cp ~/Downloads/secure-sdlc-skill-main/plugins/opti-security/skills/secure-coding/SKILL.md .claude/skills/secure-coding/SKILL.md

# Copy all supporting files
cp -r ~/Downloads/secure-sdlc-skill-main/plugins/opti-security/skills/secure-coding/supporting .claude/skills/secure-coding/supporting
```

**Step 3 — Commit the `.claude/` directory to version control**

```bash
git add .claude/
git commit -m "chore: add Optimizely secure-coding skill for Claude Code"
git push origin main
```
**Windows users:** Replace `~/Downloads/secure-sdlc-skill-main/` with `C:\Users\YourName\Downloads\secure-sdlc-skill-main\` and use PowerShell.
---

## Security Coverage

| # | Supporting File | Domain |
|---|---|---|
| 1 | authentication-identity.md | OAuth2/OIDC, MFA, JWT, sessions, password hashing |
| 2 | authorization-access-control.md | IDOR, least privilege, server-side enforcement |
| 3 | data-protection-privacy.md | Encryption, TLS, secrets management, PII, caching |
| 4 | input-validation-injection.md | SQL injection, XSS, file uploads, SSRF, deserialization |
| 5 | output-handling-ui-safety.md | All XSS variants, CSP, clickjacking, error handling |
| 6 | abuse-misuse-protection.md | Rate limiting, business logic abuse, resource exhaustion |
| 7 | dependencies-platform-security.md | SCA, SAST, secrets detection, HSTS, CORS, CSP |
| 8 | logging-observability.md | Security event logging, Datadog, log integrity, retention |
| 9 | ai-llm-features.md | Prompt injection, agent workflows, MCP security, output guardrails |

---

## Other Installation Methods

### Claude.ai UI (Personal Skill)

For individuals who prefer the Claude.ai web interface over Claude Code:

**Step 1 — Download**
Go to `https://github.com/optimizely/secure-sdlc-skill` → **Code → Download ZIP**.
Extract the ZIP. Navigate into
`secure-sdlc-skill-main/plugins/opti-security/skills/secure-coding/`
and compress just that folder into a new ZIP — this is what you upload to Claude.

**Step 2 — Upload**
In Claude.ai → profile name bottom left → **Settings → Capabilities → Skills**
→ **+** → **Upload skill** → **Upload ZIP file** → select your ZIP → confirm.

**Step 3 — Enable Auto trigger**
Set the skill trigger to **Auto** so it activates silently on every relevant request.

---

### Claude Code — Manual Installation (Alternative to Marketplace)

If you prefer to install without the marketplace, use this method instead:

**macOS — Global installation:**

```bash
# Download and extract the ZIP from https://github.com/optimizely/secure-sdlc-skill
# then run:

mkdir -p ~/.claude/skills/secure-coding
cp ~/Downloads/secure-sdlc-skill-main/plugins/opti-security/skills/secure-coding/SKILL.md ~/.claude/skills/secure-coding/SKILL.md
cp -r ~/Downloads/secure-sdlc-skill-main/plugins/opti-security/skills/secure-coding/supporting ~/.claude/skills/secure-coding/supporting
```

**macOS — Project-level installation:**

```bash
cd /path/to/your/project
mkdir -p .claude/skills/secure-coding
cp ~/Downloads/secure-sdlc-skill-main/plugins/opti-security/skills/secure-coding/SKILL.md .claude/skills/secure-coding/SKILL.md
cp -r ~/Downloads/secure-sdlc-skill-main/plugins/opti-security/skills/secure-coding/supporting .claude/skills/secure-coding/supporting
```

Commit the `.claude/` directory so every developer cloning the repo gets
the skill automatically.

**Windows users:** Replace `~/Downloads/secure-sdlc-skill-main/` with
`C:\Users\YourName\Downloads\secure-sdlc-skill-main\` and use PowerShell.

---

### VS Code

**Claude extension:** Skills deployed via the Claude.ai org admin panel apply
automatically in VS Code sessions. No extra steps needed.

**Claude Code in VS Code terminal:** Follow the Global installation above.
The `~/.claude/skills/` path applies regardless of which terminal you use.

---

## Skill Trigger Configuration (Claude.ai Admin Panel)

For org-wide deployment via the Claude admin panel, set the trigger to **Auto** or **Slash command + Auto**. This ensures the skill activates silently on every relevant developer request without manual invocation.

---

## For the Product Security Team — Adding New Skills

To add a new skill to this marketplace:

1. Create a new folder under `plugins/opti-security/skills/` — e.g. `plugins/opti-security/skills/threat-model/`
2. Add `SKILL.md` and a `supporting/` subfolder with the relevant domain files
3. Commit and push — Claude Code auto-discovers the new skill automatically

No changes to `plugin.json` or `marketplace.json` are needed. Auto-discovery
handles everything.

---

## Maintained By

Optimizely Product Security Team — `product.security@optimizely.com`
