# Manjikilan

**A security engineering toolkit by The KØDE.**

Manjikilan is a home for reusable security standards and workflows that help developers and AI coding agents build, review, deploy, and maintain safer software.

Its core project is **KODEE-Secure**: a reusable security master skill for AI coding agents. It guides security work from architecture discovery and threat modelling through implementation, verification, deployment readiness, and incident response.

## Repository description

A reusable security engineering toolkit for AI coding agents. KODEE-Secure provides security audits, threat modelling, vulnerability prevention, infrastructure hardening, incident response, and production release standards.

## What is included

| Path | Purpose |
|---|---|
| `skills/kodee-secure/SKILL.md` | The master security skill and operating workflow |
| `standards/security-checklist.md` | A cross-stack security review checklist |
| `standards/cloudflare-hardening.md` | Cloudflare edge, WAF, origin, DNS, and email hardening |
| `standards/ai-agent-security.md` | Security requirements for AI agents, tools, and retrieval systems |
| `standards/incident-response.md` | A practical incident handling playbook |
| `standards/release-gates.md` | Evidence-based production release criteria |

## Install KODEE-Secure

KODEE-Secure follows the Agent Skills format: a skill directory containing a `SKILL.md` file with YAML frontmatter and instructions.

### 1. Get the repository

~~~bash
git clone https://github.com/mpaanchi/Manjikilan.git
~~~

### 2. Copy the skill into the directory your agent discovers

The repository stores the canonical copy at `skills/kodee-secure/`. Copy that whole folder into the relevant project-level skills directory.

| Agent | Project-level location |
|---|---|
| OpenAI Codex | `.agents/skills/kodee-secure/` |
| Claude Code | `.claude/skills/kodee-secure/` |
| Cursor | `.agents/skills/kodee-secure/` or `.cursor/skills/kodee-secure/` |
| Gemini CLI | `.agents/skills/kodee-secure/` or `.gemini/skills/kodee-secure/` |
| GitHub Copilot | `.github/skills/kodee-secure/` or `.agents/skills/kodee-secure/` |

**macOS / Linux example (Codex, Cursor, or Gemini using `.agents/skills`):**

~~~bash
mkdir -p .agents/skills
cp -R Manjikilan/skills/kodee-secure .agents/skills/
~~~

**Windows PowerShell example:**

~~~powershell
New-Item -ItemType Directory -Force .agents/skills
Copy-Item -Recurse Manjikilan/skills/kodee-secure .agents/skills/
~~~

Change the destination folder to the path used by your agent. The table reflects documented project-level skill locations at the time of writing; support and discovery behavior can vary by product version and execution surface. Check the current host documentation before installing globally or into a managed/cloud agent.

### 3. Confirm that the agent discovered the skill

Start or reload the agent session, then ask it to list its available skills. When supported, explicitly invoke:

~~~text
Use the kodee-secure skill to audit this project.
~~~

Do not assume that a skill is active simply because its folder exists. Verify that the host has discovered it.

## How to use it

KODEE-Secure supports these modes:

- **NEW BUILD** — security requirements and controls during architecture and implementation.
- **EXISTING PROJECT** — discover the stack, map trust boundaries, and assess current controls.
- **AUDIT ONLY** — inspect and report; do not modify files or settings.
- **REMEDIATION** — fix approved findings with focused changes and regression tests.
- **PRE-DEPLOYMENT** — assess release risk and production readiness.
- **INCIDENT RESPONSE** — help triage, contain, investigate, recover, and document an incident.

Example requests:

- "Use KODEE-Secure in NEW BUILD mode. Threat-model this application before implementation."
- "Run an AUDIT ONLY review. Do not change any files."
- "Use PRE-DEPLOYMENT mode and identify release blockers."
- "Review our Cloudflare configuration against the hardening standard."

## Core principles

1. Security is part of design, implementation, testing, deployment, and maintenance—not a final cosmetic checklist.
2. Inspect the actual stack and configuration. Do not assume a framework, provider, security feature, or plan tier.
3. Prefer least privilege, secure defaults, explicit trust boundaries, and layered controls.
4. Verify access controls with tests. A setting being enabled is not proof that its policy is correct.
5. Treat AI prompts, retrieved content, files, and external tool output as untrusted input.
6. Do not run intrusive testing or make destructive or production changes without explicit authorisation.
7. Report evidence, uncertainty, residual risk, and untested areas honestly. Never promise absolute security or claim legal compliance from an automated review.

## Reference frameworks

KODEE-Secure is an implementation aid, not a replacement for specialist assessment. It points agents toward primary and maintained references, including:

- [OWASP Top 10](https://owasp.org/projects/top-ten/)
- [OWASP API Security Top 10](https://owasp.org/www-project-api-security/)
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
- [OWASP Application Security Verification Standard](https://owasp.org/www-project-application-security-verification-standard/)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [NIST Secure Software Development Framework (SP 800-218)](https://csrc.nist.gov/pubs/sp/800/218/final)
- [MITRE ATT&CK](https://attack.mitre.org/)
- [Cloudflare Developer Documentation](https://developers.cloudflare.com/)

Use the current version of each reference. Where a control is provider-, plan-, jurisdiction-, or technology-dependent, verify it against the actual environment.

## Important scope and legal note

KODEE-Secure provides engineering guidance and structured review workflows. It is not a penetration-testing service, legal opinion, compliance certification, or guarantee that a system is secure. POPIA, GDPR, PCI DSS, and other obligations require context-specific assessment by appropriately qualified people.

The standards in this repository are original summaries and implementation checklists that link to public primary references. They do not reproduce third-party guide material. The Faction Group LLC's free resources are not represented as an endorsement or as a source of copied content; check the publisher's licence and obtain permission before redistributing any of its materials.

## Contributing

Keep guidance evidence-based and actionable. Link to primary sources, identify version-sensitive controls, distinguish mandatory requirements from recommendations, and include a verification method for each security claim. Never add credentials, private keys, access tokens, customer data, or real incident evidence to commits.

## Maintainer

Created and maintained by **The KØDE**.

## Licence

No open-source licence has been added yet. Until the repository owner selects and adds a licence, do not assume that public visibility grants permission to reuse, modify, or redistribute the contents.
