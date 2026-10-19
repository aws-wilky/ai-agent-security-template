# Universal AI Agent Security & Privacy Reference

**This is the master reference for all AI agents, coding tools, and automated workflows.**

Use this document as the single source of truth for security and privacy across your entire development ecosystem. Every AI agent, IDE plugin, automation script, and CI/CD workflow should follow these rules.

---

## Core Security Rules (Non-negotiable)

### Secrets & Credentials
- ❌ Never expose, log, or commit:
  - API keys, tokens, or authentication credentials
  - Private SSH keys or signing certificates
  - Database passwords or connection strings
  - OAuth tokens or personal access tokens (PATs)
  - Private environment variables
  - AWS/Azure/GCP secret keys
  - Private keystore or certificate data
  - Private app identifiers, provisioning profiles, or signing configs

- ✅ Instead:
  - Use GitHub Secrets for Actions workflows
  - Use environment-scoped secrets with restricted access
  - Use OIDC/OpenID Connect for service authentication
  - Store secrets in secure vaults (1Password, HashiCorp Vault, etc.)
  - Never hardcode or embed secrets anywhere

### Private Information
- ❌ Never include:
  - Personal email addresses or contact info
  - Internal hostnames or private URLs
  - Staging, testing, or internal environment URLs
  - Private infrastructure details (IPs, VPCs, subnets, DNS)
  - Internal usernames or real employee names
  - Customer names, identifiable data, or PII
  - Internal project codenames or sensitive business info
  - Machine-specific paths or local developer setup details
  - Private app identifiers or bundle IDs not meant for public release

- ✅ Instead:
  - Use generic examples and placeholder values
  - Document using "YOUR_VALUE_HERE" or similar placeholders
  - Reference official docs instead of internal processes
  - Use redacted/anonymized examples in error logs or screenshots

### Configuration Files
- ❌ Never commit:
  - .env, .env.local, .env.*.local files
  - .env.production or any environment-specific secrets
  - config/ files with embedded credentials
  - local Docker Compose files with exposed secrets
  - Private Firebase configs or API credentials
  - Apple provisioning profiles or signing configs
  - Android keystore files or signing keys
  - Private build configs or certificate files

- ✅ Instead:
  - Add to .gitignore
  - Document in README how to set up locally without committing secrets
  - Use config templates (config.example.env)
  - Use secure secret injection during build/deploy

---

## Platform-Specific Rules

### Apple / macOS / iOS / iPadOS
- **Xcode & Apple Development:**
  - Do not expose Apple Developer Team IDs, app bundle IDs for unreleased apps, or private provisioning profile UUIDs
  - Do not commit .mobileprovision files, signing certificates, or private keys
  - Do not include private entitlements or signing configurations in public repos
  - Keep Apple ID email addresses private; use generic Apple Developer account references only
  - Do not expose App Store Connect API keys or credentials
  - Keep app identifiers private unless they are intentionally public
  - Use on-device/private Apple Intelligence models when handling sensitive data
  - Avoid committing Xcode workspace settings with private paths or local machine details
  
- **Handoff for Xcode Agents (Claude, Codex, Apple Intelligence):**
  - Follow .github/copilot-instructions.md if present
  - Verify no private app identifiers or unreleased app details leak into documentation
  - Do not expose provisioning profile or signing certificate details
  - Keep build logs clean of secrets before sharing

### Android
- **Android Studio & Gradle:**
  - Do not expose app signing keys, keystores, or signing configs
  - Do not commit keystore files, private key files, or signing credentials
  - Do not include Google Play API keys, Firebase keys, or Google Cloud credentials
  - Do not expose app package names for unreleased apps or internal-only apps
  - Do not include private build variants with hardcoded credentials
  - Keep ProGuard/R8 configs free of sensitive mappings
  - Do not expose private NDK or native library signing details
  
- **Handoff for Android Agents (Cursor, Claude, Codex):**
  - Keep Gradle scripts free of embedded API keys or private configs
  - Verify no signing key paths or credential files are referenced
  - Do not expose build secrets in build logs or error output
  - Keep dependency versions and private Maven repos appropriately scoped

### Windows / PC / Linux
- **VS Code, Visual Studio, Terminal-based dev:**
  - Do not expose Windows authentication details, domain credentials, machine names, or local usernames
  - Do not include private UNC paths, local network shares, or internal domain names
  - Do not commit .vs/ folder settings with machine-specific paths
  - Do not expose PowerShell execution policies or admin credentials
  - Do not include private environment paths or local tool installations
  
- **Handoff for Windows/PC Agents (Cursor, Copilot, Windsurf, Cline):**
  - Avoid referencing C:\Users\[YourName] or machine-specific paths
  - Keep CI/CD scripts free of Windows-specific credentials or registry keys
  - Do not expose private Chocolatey or package manager credentials

### Cross-Platform / Cloud
- **GitHub Actions, CI/CD, Automation:**
  - Use minimal, scoped permissions for Actions jobs
  - Default: `permissions: { contents: read }`
  - Only add write permissions when absolutely required
  - Never store secrets in repository variables; use GitHub Secrets
  - Do not log or print secrets in workflow output
  - Do not expose cloud provider credentials (AWS, Azure, GCP)
  - Use OIDC for service-to-service authentication instead of static tokens
  
- **Handoff for CI/CD & Automation Agents (OpenHands, Aider, AutoGen, CrewAI):**
  - Follow least-privilege access rules for any automation
  - Verify no secrets are written to logs or artifacts
  - Keep deployment scripts free of hardcoded credentials
  - Confirm automation only modifies files in scope of the task

---

## Rules by Tool & Framework

### All IDE/Editor Integrations
**GitHub Copilot, Cursor, Windsurf, VS Code, JetBrains, Xcode, Android Studio, Continue, Cline, Roo Code**

- Read and follow .github/copilot-instructions.md if present
- Read and follow AGENTS.md if present
- Read platform-specific files (CLAUDE.md, .cursorrules, .windsurf/rules, etc.)
- Never expose secrets in code suggestions or completions
- Do not include credentials in generated code or examples
- Verify final diffs before accepting suggestions
- If uncertain whether a value is sensitive, assume it is and ask

### Terminal & CLI Agents
**Claude Code, Codex CLI, Aider, OpenHands, AutoGen, CrewAI, n8n**

- Do not hardcode secrets in scripts or commands
- Do not write credentials to stdout/stderr or log files
- Keep git operations (push, commit) free of sensitive data
- Verify file edits do not expose secrets before committing
- Use environment variables for sensitive values, not inline values
- Do not run commands that expose system credentials or machine details

### Language Models & Prompts
**Claude, GPT-4, Gemini, Grok, Codex, any LLM-based agent**

- Assume the model's responses may be visible to third parties
- Never paste entire .env files or config files into prompts
- Use prompts that reference the security rules, not inline secrets
- Sanitize error messages and logs before pasting into prompts
- Do not use model outputs containing secrets without redacting first
- Keep context windows free of PII or sensitive business data

### GitHub Actions & Workflows
- Use `permissions: { contents: read }` by default
- Add write permissions only when necessary
- Use GitHub Secrets for all sensitive values
- Do not output secrets in logs or artifacts
- Use OIDC for cloud provider authentication
- Review workflows for accidental secret exposure

### Automation & Scripting Frameworks
**n8n, Zapier, Zapier AI, Make (formerly Integromat), BAND, Orbi**

- Do not hardcode API keys or credentials in automation flows
- Use tool-native secret storage (n8n credentials, Zapier auth, etc.)
- Do not expose secrets in flow logs or error messages
- Keep automation workflows free of PII or customer data
- Verify automation output does not leak sensitive info

---

## The Mandatory Double-Check Process

Before finalizing ANY task, the agent MUST verify:

1. **Secrets Check**
   - [ ] No API keys, tokens, passwords, or credentials in code/docs
   - [ ] No .env files, config files with secrets, or private keys committed
   - [ ] No secrets in logs, error messages, or debug output

2. **Privacy Check**
   - [ ] No personal emails, phone numbers, or contact info
   - [ ] No internal hostnames, private URLs, or infrastructure details
   - [ ] No customer names, PII, or identifiable data
   - [ ] No real employee names, team member emails, or internal contacts
   - [ ] No unreleased app identifiers or internal-only project names
   - [ ] No machine-specific paths or local developer setup details

3. **Configuration Check**
   - [ ] No credentials hardcoded in examples or documentation
   - [ ] No internal staging/testing environment URLs or configs
   - [ ] No private provisioning profiles, signing keys, or certificates
   - [ ] No private keystore or signing data

4. **Scope Check**
   - [ ] Changes are limited to the task at hand
   - [ ] No unrelated files or configurations were modified
   - [ ] No debug files or temporary code remains

5. **Visibility Check**
   - [ ] If repo is public: all content is public-safe
   - [ ] If repo is private: content is private by default
   - [ ] Sensitive content matches the repo's intended access level

6. **Diff Review**
   - [ ] Review the final git diff carefully
   - [ ] Verify no secrets or private info appears in the diff
   - [ ] Confirm all changes are necessary and intentional

**Do not mark the task complete until all checks pass.**

---

## Quick Reference Checklist for AI Agents

Use this when starting a new task:

```
🔒 SECURITY & PRIVACY CHECKLIST

Before I start:
- [ ] I have read UNIVERSAL_AI_AGENT_SECURITY.md
- [ ] I understand the platform-specific rules (Apple/Android/Windows/Cloud)
- [ ] I know what NOT to expose (secrets, private URLs, PII, credentials)
- [ ] I will verify the final diff before finishing

While working:
- [ ] I will not hardcode or include any credentials
- [ ] I will not expose personal or internal information
- [ ] I will use generic examples, not real values
- [ ] I will keep changes scoped and minimal
- [ ] I will avoid .env files, keystores, and private configs

Before finishing:
- [ ] Secrets check: no API keys, tokens, or passwords
- [ ] Privacy check: no personal emails, internal URLs, or PII
- [ ] Configuration check: no hardcoded credentials or private configs
- [ ] Scope check: changes are limited to the task
- [ ] Visibility check: content is safe for repo visibility level
- [ ] Diff review: final changes contain no sensitive data

Only mark complete when all checks pass ✓
```

---

## Usage Instructions

### For Repository Owners
1. Copy or reference this file in your project root or .github folder
2. Name it `UNIVERSAL_AI_AGENT_SECURITY.md` or similar
3. Link to it in:
   - .github/copilot-instructions.md
   - AGENTS.md
   - README.md
   - Any agent config files
4. Update platform and tool sections as needed

### For AI Agents & Automation
When you receive a task:
1. Read UNIVERSAL_AI_AGENT_SECURITY.md first
2. Identify the platform (Apple/Android/Windows/Cloud)
3. Identify the tool (Copilot/Cursor/Codex/etc.)
4. Follow the core rules and platform-specific rules
5. Perform the mandatory double-check before finishing
6. Reference this document in your task completion summary

### For Team Handoff
When handing off to another agent:
```
Follow UNIVERSAL_AI_AGENT_SECURITY.md:
- Platform: [Apple/Android/Windows/Cloud]
- Tool: [Copilot/Cursor/Claude/etc.]
- Task: [brief description]
- Do not expose: [secrets, private URLs, PII, credentials]
- Verify before finishing: [mandatory checks]
```

---

## Final Rule

**When in doubt, assume it's sensitive and do not include it.**

If you are uncertain whether something should be exposed:
- Do not include it
- Ask for clarification instead
- Reference this document

---

## Document Version
- **Last Updated:** 2026-10-19
- **Coverage:** All AI agents, IDEs, platforms, and automation frameworks
- **Audience:** AI agents, developers, automation workflows, CI/CD systems
- **Scope:** Universal security and privacy baseline for code repositories

---

## Platform Coverage Summary

| Platform | Tools | Key Rules |
|----------|-------|-----------|
| **Apple/macOS/iOS** | Xcode, Xcode AI, Claude, Codex | No app bundle IDs, provisioning profiles, signing keys, or Apple ID emails |
| **Android** | Android Studio, Cursor, Claude, Codex | No keystores, signing keys, Google Play keys, or Firebase credentials |
| **Windows/PC/Linux** | VS Code, Visual Studio, Cursor, Terminal | No Windows auth, domain creds, machine paths, or admin credentials |
| **Cloud/CI/CD** | GitHub Actions, n8n, Zapier, Automation | Minimal permissions, GitHub Secrets only, OIDC preferred |
| **All IDEs/Editors** | Copilot, Cursor, Windsurf, Continue, Cline | Follow platform rules, verify diffs, assume sensitive if uncertain |
| **Terminal/CLI** | Claude Code, Aider, OpenHands, AutoGen | No hardcoded secrets, clean logs, verified commits |
| **LLM/AI Models** | All Claude, GPT, Gemini, Grok instances | Sanitize inputs, redact outputs, assume visibility to third parties |

---

**This is the master reference. Use it everywhere.**
