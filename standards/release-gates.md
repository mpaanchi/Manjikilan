# KODEE-Secure — Production Release Gates

Use these gates before exposing a new application, significant feature, infrastructure change, or security-sensitive update to production. Tailor the gates to the system's risk and document any accepted exceptions.

## 1. Release decision

Choose one:

- **READY** — mandatory gates pass and evidence is available.
- **READY WITH ACCEPTED RISK** — no unresolved critical issue; a responsible owner has formally accepted specific, time-bound residual risk.
- **NOT READY** — one or more mandatory gates fail.
- **NOT ASSESSED** — evidence is insufficient to make a release decision.

The person or team accountable for production must own the final release decision. An AI agent may prepare evidence and a recommendation; it must not silently accept business risk.

## 2. Hard blockers

Do not recommend production release while any of the following remain true:

- an unresolved CRITICAL finding exists;
- a verified HIGH finding has no remediation or approved, time-bound exception;
- authentication or server-side authorisation is bypassable;
- cross-user or cross-tenant access has not been adequately tested in a multi-tenant system;
- production secrets are exposed in source, client bundles, logs, or build artifacts;
- known unauthorised public access exists to sensitive data or administrative interfaces;
- critical input paths permit a demonstrated injection or code execution path;
- backups or recovery arrangements required for the release are unavailable;
- a high-impact operation can execute without required authorisation or approval;
- deployment credentials or CI permissions are materially over-privileged and unmitigated;
- an active incident or unresolved compromise affects the release environment;
- the team cannot identify a safe rollback or recovery path for a high-risk change.

A risk exception cannot override law, contract, or a mandatory organisational policy.

## 3. Application security gates

### Authentication

- [ ] Login, logout, recovery, and session lifecycle tested.
- [ ] Privileged accounts protected appropriately, including MFA where supported.
- [ ] Tokens validated for signature, issuer, audience, expiry, and scope as relevant.
- [ ] Session revocation and account suspension work.

### Authorisation

- [ ] Server-side checks protect every sensitive action.
- [ ] Roles and permissions are least-privilege.
- [ ] Object-level and function-level access tests pass.
- [ ] Cross-user and cross-tenant negative tests pass.
- [ ] Admin and service-role capabilities are restricted and audited.

### Input, output, and APIs

- [ ] Input schemas and size limits are enforced.
- [ ] Database calls are parameterised.
- [ ] XSS, CSRF, SSRF, path traversal, and injection risks are addressed where relevant.
- [ ] Error responses do not leak secrets or internal stack details.
- [ ] Webhooks authenticate, validate, and handle replay/idempotency.
- [ ] Sensitive endpoints have rate and resource limits.

### Data and files

- [ ] Sensitive data is minimised and access-controlled.
- [ ] Database and object-storage policies are tested.
- [ ] Private data cannot leak through CDN or shared caches.
- [ ] Upload validation, storage, processing, and download controls are tested.
- [ ] Retention and deletion behaviour is understood.

## 4. Infrastructure and supply-chain gates

- [ ] Production and non-production credentials are separated.
- [ ] No secrets are committed or exposed in build artifacts.
- [ ] Dependencies and base images are reviewed for known vulnerabilities.
- [ ] Build and deployment identities have appropriate scope.
- [ ] Branch and release permissions are controlled.
- [ ] TLS and certificate configuration are valid.
- [ ] Public network exposure is intentional and reviewed.
- [ ] WAF, CDN, DNS, and origin settings are consistent with the architecture.
- [ ] Admin and database interfaces are restricted.
- [ ] Infrastructure-as-code and deployment configuration changes are reviewed.

## 5. AI and agent gates

For AI-enabled features or agents:

- [ ] External content and tool output are treated as untrusted.
- [ ] Prompt injection cannot change tool permissions.
- [ ] Tools enforce server-side user and tenant authorisation.
- [ ] High-impact actions have an independent approval boundary.
- [ ] Model output is validated before execution or persistence.
- [ ] Retrieval and memory enforce access and tenant separation.
- [ ] Secrets and unnecessary personal data are excluded from prompts and traces.
- [ ] Tool, token, time, concurrency, and spend limits exist.
- [ ] There is a way to disable autonomous actions and revoke credentials.

## 6. Operational readiness gates

- [ ] Application and infrastructure health checks are available.
- [ ] Security-relevant events are logged with appropriate redaction.
- [ ] Alerts exist for material failures and suspicious activity.
- [ ] Logs and monitoring access are restricted.
- [ ] Backup jobs are verified and restore evidence is available where required.
- [ ] Incident owner and escalation path are known.
- [ ] Support and user communication routes are ready.
- [ ] Capacity, timeout, retry, queue, and quota settings are appropriate.
- [ ] External provider dependencies and failure behaviour are understood.

## 7. Testing evidence

Attach or reference:

- unit, integration, end-to-end, and regression test results;
- static analysis and security lint results;
- dependency and secret scan results;
- authorisation and tenant-isolation tests;
- relevant dynamic tests performed within authorised scope;
- configuration review and infrastructure plan;
- backup/restore or rollback test results;
- staging verification and known limitations.

Record tool names, versions, dates, scope, and exclusions. A green scanner does not establish that the application is secure.

## 8. Change management

Before deployment, document:

- release identifier and source revision;
- summary of changes;
- risk and affected components;
- deployment owner and approver;
- rollout strategy and expected downtime, if any;
- monitoring plan;
- rollback trigger and steps;
- data migration and backup plan;
- dependencies and external communications.

Obtain explicit approval before production DNS, firewall, WAF, identity, database, or security-policy changes that may disrupt service or access.

## 9. Risk exception

A risk exception must include:

- finding ID and severity;
- affected component and exposure;
- why the issue cannot be fixed before release;
- compensating controls;
- business impact if exploited;
- owner accepting the risk;
- approval date and expiry/review date;
- remediation plan and target date.

Exceptions must be time-bound and revisited. Do not use a blanket exception for an entire class of findings.

## 10. Release record

~~~markdown
# Production Release Security Record

- Application:
- Release / commit:
- Environment:
- Date:
- Release owner:
- Security reviewer:
- Mode: PRE-DEPLOYMENT

## Decision
READY / READY WITH ACCEPTED RISK / NOT READY / NOT ASSESSED

## Mandatory gates
| Gate | Status | Evidence / notes |
|---|---|---|

## Findings
| ID | Severity | Status | Owner | Exception |
|---|---|---|---|---|

## Tests
List commands, tools, versions, scope, and results.

## Backup and rollback
State the backup status, restore evidence, rollback trigger, and owner.

## Monitoring
State key signals, alert destinations, and incident contact.

## Approved exceptions
List approver, reason, compensating control, expiry, and remediation date.

## Residual risks and exclusions
State what remains untested or unresolved.

## Approval
- Technical owner:
- Risk owner:
- Approval reference:
- Approval date:
~~~

## 11. Post-deployment verification

After release:

- [ ] Confirm health checks and key user journeys.
- [ ] Verify authentication and authorisation on critical routes.
- [ ] Confirm background jobs, webhooks, and integrations behave correctly.
- [ ] Review error rates and security events.
- [ ] Confirm no unexpected cache, WAF, or rate-limit behaviour.
- [ ] Monitor for suspicious access and abnormal resource use.
- [ ] Confirm rollback remains available until the release is stable.
- [ ] Record outcome and unresolved issues.

A release is complete only when the deployed system has been checked, not merely when the deployment command succeeds.

