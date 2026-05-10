# Secure Coding Skill for Claude

## What This Is

This is the Secure Coding Skill for Claude. It ensures that every piece of code written or reviewed by anyone through Claude meets security standards;whether security is explicitly asked about or not.

---

## What This Skill Does

Once deployed, this skill activates automatically whenever any persona uses Claude for code generation, code review, feature building, architecture design, code translation, optimisation, refactoring, debugging, or any other interaction where code is written, read, or modified. Security is enforced silently;we receive secure code without needing to ask for it.

---

## File Structure

```
secure-coding/
├── SKILL.md                              ← Brain of the skill. Contains the
│                                            trigger logic, operating modes,
│                                            and fundamental enforcement
│                                            principles. This is the entry
│                                            point Claude reads first.
│
└── supporting/                           ← Knowledge base. Each file covers
     ├── authentication-identity.md           one security domain. Claude loads
     ├── authorization-access-control.md      only the files relevant to the
     ├── data-protection-privacy.md           current developer request —
     ├── input-validation-injection.md        keeping context efficient.
     ├── output-handling-ui-safety.md
     ├── abuse-misuse-protection.md
     ├── dependencies-platform-security.md
     ├── logging-observability.md
     ├── ai-llm-features.md
```

### How It Works

`SKILL.md` is the trigger and brain;it tells Claude when to activate, how to behave across four operating modes, and which supporting file to load based on what anyone is working on.

The `supporting/` files are the detailed security guidance contains the specific controls Claude enforces when generating or reviewing code in that area.

The two components work together. `SKILL.md` without the supporting files gives Claude the behaviour rules but not the domain knowledge. The supporting files without `SKILL.md` have no trigger or enforcement logic. Both must be deployed together in the structure shown above.

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
| 9 | ai-llm-features.md | Prompt injection, agent workflows, output guardrails |
---

## Deployment

All files must be deployed together maintaining the exact directory structure above. `SKILL.md` must remain at the root of the skill directory. All 9 supporting files must remain inside the `supporting/` subfolder. Renaming files or changing the directory structure will break the references in `SKILL.md`.

## Skill Trigger Configuration

This skill is designed to activate automatically and silently on every relevant developing request;not as an opt-in slash command. When deploying, the trigger must be set to **Auto** or **Slash command + Auto** in the Claude admin panel.

## Personal Skill Installation (Claude.ai UI)

For individual developers who want to install the skill directly through the Claude.ai interface without using the command line:

**Step 1 — Download the skill**
Go to `https://github.com/Adnanalislam/secure-sdlc-skill/` and click
**Code → Download ZIP**. 

**Step 2 — Open Claude Settings**
In Claude.ai, click your profile name at the bottom left →
**Settings → Capabilities → Skills**

**Step 3 — Add Personal Skill**
Click the **+** button at the top right of the Skills panel →
**Upload skill** → **Upload ZIP file** →
Select the downloaded ZIP file → confirm.

**Step 4 — Verify**
The skill appears in your Personal Skills list as `secure-coding` with `SKILL.md` and the `supporting/` folder visible underneath it — exactly as shown below:
```
secure-coding/
├── SKILL.md
├── supporting/
└── README.md (optional)
```
**Step 5 — Enable Auto trigger**
Make sure the skill trigger is set to **Auto** so it activates silently on every relevant request without needing to type a slash command.

## Claude Code (the CLI) Deployment
For developers using **Claude Code** (the CLI), the skill is deployed locally rather than through the Claude admin panel. The file structure and content are identical — no changes to any files are needed.
The source for all files is:
`https://github.com/Adnanalislam/secure-sdlc-skill/`

---

**Option A — Global installation (applies to all Claude Code sessions across all projects on the developer's machine):**

**Step 1 — Download the skill files**
Go to `https://github.com/Adnanalislam/secure-sdlc-skill/` and click **Code → Download ZIP**. Extract the ZIP file on your machine. The extracted folder will be named `secure-sdlc-skill-main`. 

**Step 2 — Run these commands**
````bash
# Create the skill root directory
mkdir -p ~/.claude/skills/secure-sdlc-skill

# Place SKILL.md at the skill root — Claude Code discovers it here
cp ~/Downloads/secure-sdlc-skill-main/SKILL.md ~/.claude/skills/secure-sdlc-skill/SKILL.md

# Copy the entire supporting folder with all files
cp -r ~/Downloads/secure-sdlc-skill-main/supporting ~/.claude/skills/secure-sdlc-skill/supporting
````
**Note:** The commands above assume you extracted the ZIP to your Downloads folder on macOS — which is the default extraction location. If you extracted to a different location, replace ~/Downloads/secure-sdlc-skill-main/ with your actual extraction path.

**Windows users:** Replace ~/Downloads/secure-sdlc-skill-main/ with
C:\Users\YourName\Downloads\secure-sdlc-skill-main\ and use PowerShell.

After installation the structure will look like this:
```
~/.claude/skills/secure-sdlc-skill/
├── SKILL.md                    ← must be here at root
└── supporting/
    ├── authentication-identity.md
    ├── authorization-access-control.md
    ├── data-protection-privacy.md
    ├── input-validation-injection.md
    ├── output-handling-ui-safety.md
    ├── abuse-misuse-protection.md
    ├── dependencies-platform-security.md
    ├── logging-observability.md
    └── ai-llm-features.md
```

**Option B — Project-level installation (applies to one project only and can be committed to version control for the whole team):**

**Step 1 — Download the skill files**
Go to `https://github.com/Adnanalislam/secure-sdlc-skill/` and click **Code → Download ZIP**. Extract the ZIP file on your machine. The extracted folder will be named `secure-sdlc-skill-main`. 

**Step 2 — Run these commands**
```bash
# Navigate to your project root first
cd /path/to/your/project

# Create the skill root directory
mkdir -p .claude/skills/secure-sdlc-skill

# Place SKILL.md at the skill root — Claude Code discovers it here
cp ~/Downloads/secure-sdlc-skill-main/SKILL.md .claude/skills/secure-sdlc-skill/SKILL.md

# Copy the entire supporting folder with all files
cp -r ~/Downloads/secure-sdlc-skill-main/supporting .claude/skills/secure-sdlc-skill/supporting
```
**Note:** The commands above assume you extracted the ZIP to your Downloads folder on macOS — which is the default extraction location. If you extracted to a different location replace ~/Downloads/secure-sdlc-skill-main/ with your actual extraction path.

**Windows users:** Replace ~/Downloads/secure-sdlc-skill-main/ with
C:\Users\YourName\Downloads\secure-sdlc-skill-main\ and use PowerShell.
After installation the structure will look like this

```
your-project/
└── .claude/
    └── skills/
        └── secure-sdlc-skill/
            ├── SKILL.md                    ← must be here at root
            └── supporting/
                ├── authentication-identity.md
                ├── authorization-access-control.md
                ├── data-protection-privacy.md
                ├── input-validation-injection.md
                ├── output-handling-ui-safety.md
                ├── abuse-misuse-protection.md
                ├── dependencies-platform-security.md
                ├── logging-observability.md
                └── ai-llm-features.md
```
**Commit the `.claude/` directory to version control so every developer cloning the repository gets the skill automatically without manual installation. **


---


