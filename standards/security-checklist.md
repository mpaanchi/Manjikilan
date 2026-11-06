# KODEE-Secure — Security Checklist

Use this checklist alongside `skills/kodee-secure/SKILL.md`. It is a review aid, not a certification. Record evidence and mark untested controls honestly.

## Project context

- [ ] Application, repository, stack, environment, owner, and deployment target recorded.
- [ ] Assessment mode and authorised scope recorded.
- [ ] Production, staging, and local environments distinguished.
- [ ] Sensitive data and critical business operations identified.
- [ ] Trust boundaries, third-party integrations, and external entry points mapped.
- [ ] Testing restrictions and approval requirements recorded.

## Frontend and browser

- [ ] Untrusted output is encoded for its rendering context.
- [ ] HTML/Markdown rendering uses appropriate sanitisation.
- [ ] No secrets, privileged keys, or service credentials are shipped to the browser.
- [ ] Browser storage does not retain sensitive tokens unnecessarily.
- [ ] Cookie flags are appropriate where cookies are used.
- [ ] CSRF protections are applied where the authentication design requires them.
- [ ] CSP and other security headers are considered and tested.
- [ ] postMessage validates sender origin and message shape.
- [ ] Open redirects and unsafe URL handling are controlled.
- [ ] Third-party scripts and dependencies are inventoried.

## API and backend

- [ ] Every sensitive route enforces authentication and authorisation server-side.
- [ ] Input is validated against explicit schemas and size limits.
- [ ] Database access uses parameterised queries or safe ORM patterns.
- [ ] Output and error handling do not disclose secrets or internal details.
- [ ] Object-, property-, and function-level authorisation are tested.
- [ ] SSRF, command injection, path traversal, and unsafe deserialisation are considered.
- [ ] Webhooks authenticate signatures and handle replay/idempotency.
- [ ] API inventory and deprecated routes are reviewed.
- [ ] Rate limits and resource quotas protect expensive operations.

## Authentication and authorisation

- [ ] MFA is enabled for privileged users where supported.
- [ ] Password storage uses a recognised password-hashing approach if passwords are managed locally.
- [ ] Registration, verification, login, logout, and recovery flows are tested.
- [ ] Session/token expiry, rotation, revocation, and audience/scope validation are correct.
- [ ] Brute-force and credential-stuffing controls exist.
- [ ] Role changes and invitations cannot be abused for privilege escalation.
- [ ] Admin and support access is limited and auditable.
- [ ] Direct API access cannot bypass UI restrictions.

## Multi-tenancy and database

- [ ] Tenant identity is established from trusted server-side context.
- [ ] Cross-tenant reads and writes are denied.
- [ ] Anonymous, authenticated, admin, and service-role paths are separately tested.
- [ ] RLS/policies are reviewed, not merely checked as enabled.
- [ ] Sensitive fields and unnecessary personal data are minimised.
- [ ] Database roles follow least privilege.
- [ ] Backups, retention, deletion, and restore are documented.
- [ ] Object storage is private by default where content is not public.
- [ ] Signed URLs are scoped and expire appropriately.
- [ ] Migrations have a rollback/data recovery plan.

## Hosting, cloud, and network

- [ ] Only required ports and services are exposed.
- [ ] Admin interfaces are protected and restricted.
- [ ] Production credentials are not reused in development.
- [ ] Infrastructure identities use least privilege.
- [ ] Network ingress and egress are restricted where practical.
- [ ] Cloud storage and database endpoints are not unintentionally public.
- [ ] TLS is correctly configured and certificates are renewed.
- [ ] Debug modes and sample credentials are removed from production.
- [ ] Infrastructure changes are reviewed and traceable.

## CI/CD and supply chain

- [ ] Branch protections and code review are enabled where appropriate.
- [ ] CI credentials are scoped to the workflow's actual needs.
- [ ] Secrets are not printed or embedded in build artifacts.
- [ ] Dependencies, build actions, and base images are reviewed and pinned appropriately.
- [ ] Vulnerability and secret scanning run on relevant changes.
- [ ] Deployment credentials are separated from build/test credentials.
- [ ] Artifacts have traceable provenance.
- [ ] Production deploys require an authorised identity.
- [ ] Rollback is tested or has a verified procedure.

## Security configuration, CDN, and abuse controls

- [ ] CORS permits only intended origins and methods.
- [ ] Authenticated responses cannot leak through shared caches.
- [ ] Security headers are appropriate to the application.
- [ ] WAF rules are tuned to the application and verified against false positives.
- [ ] Origin infrastructure cannot be trivially accessed around the CDN.
- [ ] Login, reset, OTP, signup, exports, and AI endpoints have abuse controls.
- [ ] Limits exist for body size, concurrency, execution time, and expensive tasks.
- [ ] Verified crawlers and critical API clients are not accidentally blocked.

## File uploads

- [ ] File type is checked using content/magic bytes, not extension alone.
- [ ] Size, decompression, and parser resource limits exist.
- [ ] Files are stored under safe generated names.
- [ ] Uploads cannot execute as server-side code.
- [ ] Processing is isolated where risk warrants it.
- [ ] Malware scanning/quarantine is used where appropriate.
- [ ] Downloads re-check authorisation.
- [ ] Signed links are short-lived and scoped.

## AI and agent security

- [ ] User input, retrieved documents, web pages, and tool output are treated as untrusted.
- [ ] Prompt injection cannot directly grant new tool permissions.
- [ ] Tools are allowlisted and scoped to the minimum required operations.
- [ ] High-impact actions require out-of-model authorisation and human approval.
- [ ] Tool inputs and model outputs are validated before execution or persistence.
- [ ] Memory and retrieval are tenant-scoped and access-controlled.
- [ ] Secrets and personal information are excluded or redacted from prompts and logs.
- [ ] MCP servers and agent dependencies are reviewed.
- [ ] Shell, browser, network, and file access are sandboxed or constrained.
- [ ] Model usage has budget, rate, and abuse limits.
- [ ] Failure, timeout, and fallback behaviour is safe.

## Phishing, email, domain, and privacy

- [ ] Registrar and DNS accounts use MFA and limited access.
- [ ] Domain/subdomain inventory is maintained.
- [ ] Dangling DNS records are identified and removed safely.
- [ ] SPF has one valid record per sending domain.
- [ ] DKIM is configured for legitimate sending services.
- [ ] DMARC is monitored and moved toward an appropriate enforcement policy.
- [ ] Email authentication is not mistaken for complete anti-phishing protection.
- [ ] Personal data flows, purposes, retention, and access rights are documented.
- [ ] Privacy notices and processor relationships are reviewed where applicable.
- [ ] Breach escalation and notification responsibilities are known.

## Logging, monitoring, backup, and resilience

- [ ] Auth, permission failures, privileged actions, and sensitive events are logged.
- [ ] Logs exclude passwords, tokens, full payment details, and unnecessary personal data.
- [ ] Log access and retention are controlled.
- [ ] Alerts exist for material abuse and service anomalies.
- [ ] Timeouts, retries, backpressure, and queue limits are configured.
- [ ] Backups are encrypted and isolated from ordinary application credentials.
- [ ] Restore tests demonstrate that backups are usable.
- [ ] Recovery ownership and objectives are documented.
- [ ] Incident contacts and escalation channels are current.

## Final assessment

For each applicable control, record one status:

- **PASS** — tested and evidenced.
- **WARNING** — partially effective or dependent on an assumption.
- **CRITICAL** — verified material failure.
- **NOT TESTED** — not assessed or not authorised.
- **N/A** — not applicable, with a reason.

### Release decision

- [ ] No unresolved CRITICAL finding.
- [ ] HIGH findings are fixed or covered by a documented, time-bound risk exception approved by the accountable owner.
- [ ] Required regression and security tests passed.
- [ ] Production approvals and rollback arrangements are confirmed.
- [ ] Residual risks and untested areas are disclosed.

A completed checklist does not prove security or regulatory compliance. It records the scope and evidence of this assessment.
