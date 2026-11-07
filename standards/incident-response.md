# KODEE-Secure — Incident Response Playbook

> Copyright © 2026 The KØDE. This original document is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). See the repository [NOTICE](../NOTICE.md) for scope, exclusions, and attribution requirements.

This playbook provides a practical response structure for suspected or confirmed security incidents. Adapt it to the organisation's systems, contacts, contracts, insurance, and legal obligations before an incident occurs.

## 1. Objectives

1. Protect people, customers, systems, and data.
2. Contain active harm without destroying evidence.
3. Establish what happened, when, and what was affected.
4. Remove attacker access and root cause.
5. Restore trustworthy service.
6. Meet contractual and legal notification duties.
7. Learn from the incident and reduce recurrence.

## 2. What counts as an incident?

Examples include:

- suspected account takeover or privileged access misuse;
- exposed API keys, passwords, certificates, or service tokens;
- unauthorised access to personal, financial, or confidential data;
- ransomware, malware, web-shell, or unauthorised code execution;
- database deletion, tampering, or unexpected export;
- cross-tenant or cross-user data exposure;
- phishing or domain/email compromise;
- WAF/CDN/origin bypass or major infrastructure intrusion;
- compromised dependencies, build pipelines, or deployment credentials;
- AI agent action that exposes data or makes unauthorised changes;
- unexplained service disruption that may be malicious.

A vulnerability report is not always an active incident. Record the distinction and escalate if exploitation is suspected.

## 3. Roles and authority

Identify before an incident:

- incident commander / accountable decision-maker;
- technical response lead;
- infrastructure/cloud owner;
- application and database owner;
- privacy officer or data protection lead;
- legal counsel;
- customer communications owner;
- relevant vendors, hosting providers, payment providers, and insurers.

One person may hold multiple roles in a small organisation, but decision authority must be clear.

Only authorised owners should approve production shutdowns, data restoration, account-wide credential rotation, public statements, and regulator/customer notifications.

## 4. Severity and escalation

| Level | Description | Initial response |
|---|---|---|
| **SEV-1 Critical** | Active compromise of critical systems, destructive access, widespread sensitive-data exposure, or major outage with security implications. | Escalate immediately; activate incident command and containment. |
| **SEV-2 High** | Credible compromise of privileged identity, material customer data, or important service, with scope not yet fully known. | Urgent investigation and containment. |
| **SEV-3 Moderate** | Limited confirmed compromise or contained exposure with constrained impact. | Assign owner, investigate promptly, preserve evidence. |
| **SEV-4 Low** | Suspicious event or low-impact weakness without evidence of compromise. | Triage, document, and monitor. |

Severity can increase as evidence emerges. Do not down-rank an incident simply because the full scope is unknown.

## 5. First response: first 15–30 minutes

The exact timing depends on the incident and staffing. Prioritise immediate safety and containment over an arbitrary clock.

1. Open an incident record with a unique ID.
2. Identify the reporter, time detected, systems, and observed symptoms.
3. Assign an incident commander and technical lead.
4. Establish a trusted communication channel separate from potentially compromised accounts.
5. Preserve relevant logs, alerts, snapshots, and evidence.
6. Determine whether the activity is ongoing.
7. Identify the highest-risk exposed credentials, systems, data, and users.
8. Choose the least disruptive effective containment action.
9. Record every action, decision, timestamp, and actor.

Do not post secrets or detailed exploitation steps in a broad chat or public issue.

## 6. Containment

Containment must be proportionate to the threat and approved by the authorised owner when it could disrupt production.

Possible actions:

- disable or suspend a compromised account;
- revoke exposed sessions, API tokens, or credentials;
- restrict a malicious integration or service identity;
- isolate a host, container, workload, or network segment;
- disable a compromised deployment pipeline;
- block known malicious indicators at the edge or firewall;
- pause a vulnerable feature or high-risk endpoint;
- restrict public access to affected storage;
- suspend agent tools or autonomous workflows;
- switch to a known safe operational mode.

Before destructive action, preserve evidence when possible. Do not wipe, rebuild, or restore a compromised system before collecting the evidence required for investigation, unless immediate harm makes it necessary.

## 7. Evidence preservation

Preserve, where available and lawful:

- relevant application, identity, database, cloud, CDN, and WAF logs;
- timestamps and time zone;
- authentication and privileged action records;
- deployment history and source revisions;
- affected host/container snapshots or forensic images;
- alerts, email headers, malicious files, and network indicators;
- user reports and support tickets;
- configuration changes and access logs.

Record:

- evidence ID;
- collection time and collector;
- source system;
- method and tool;
- integrity hash where appropriate;
- storage location and access restrictions.

Keep originals protected. Analyse copies. Do not collect unrelated personal information.

## 8. Investigation

Establish:

- initial entry point and likely attack vector;
- accounts, tokens, hosts, routes, data stores, and integrations affected;
- attacker capabilities and observed actions;
- start time, dwell time, and last known malicious activity;
- data accessed, altered, deleted, or exported;
- persistence mechanisms and secondary access;
- systems potentially affected through shared credentials or dependencies;
- whether the issue is ongoing;
- what remains unknown.

Use logs and evidence, not assumptions. A missing log entry does not prove an action did not occur.

## 9. Eradication

After evidence preservation and approval:

- remove malicious accounts, keys, jobs, files, workloads, and persistence;
- patch the exploited vulnerability or misconfiguration;
- rotate affected credentials and revoke sessions;
- rebuild compromised systems from trusted sources where appropriate;
- verify dependencies and build pipelines;
- remove unauthorised DNS, IAM, firewall, and integration changes;
- address the original root cause and lateral movement paths.

Do not merely block the observed IP or delete the obvious malware while leaving compromised credentials or persistence in place.

## 10. Recovery

- Restore from a known-good backup or rebuild from trusted artifacts.
- Verify backup integrity and the recovery point before restoring.
- Patch and harden the system before returning it to public exposure.
- Re-enable access and integrations gradually.
- Validate authentication, authorisation, data integrity, and core workflows.
- Monitor closely for recurrence and unusual access.
- Confirm that users and dependent systems can operate normally.

Never restore a backup over live data without understanding the data-loss window and obtaining explicit authorisation.

## 11. Communications and notifications

Communications should be accurate, timely, approved, and limited to people who need the information.

Coordinate with legal counsel, the privacy officer, insurers, and relevant providers to determine:

- whether personal or regulated data was affected;
- which individuals, customers, partners, or authorities must be notified;
- applicable deadlines and notification thresholds;
- required content and approved communication channel;
- contractual and cross-border reporting duties.

For South African operations, consider POPIA and the Information Regulator's current guidance. Do not assume a fixed notification timeline without assessing the facts and applicable law. Engage qualified counsel or the responsible privacy officer.

Do not speculate publicly, overstate the scope, minimise harm, or promise that the incident is fully resolved before verification.

## 12. Post-incident review

Within an appropriate period after containment and recovery, document:

- concise timeline;
- root cause and contributing factors;
- affected systems and data;
- confirmed impact and remaining uncertainty;
- containment, eradication, and recovery actions;
- what worked and what failed;
- control, process, staffing, and vendor gaps;
- customer/regulatory communications as authorised;
- remediation actions with owners and deadlines;
- lessons to feed into threat models, tests, and release gates.

Focus on systemic improvements, not blame.

## 13. Incident record template

~~~markdown
# Security Incident Record

- Incident ID:
- Severity:
- Status:
- Incident commander:
- Technical lead:
- Date/time detected:
- Time zone:
- Reporter:
- Affected systems:
- Scope and exclusions:

## Summary
What was observed and why it is being treated as an incident.

## Timeline
| Time | Event / action | Actor | Evidence |
|---|---|---|---|

## Impact
- Systems affected:
- Data potentially affected:
- Users / tenants affected:
- Availability impact:
- Confirmed vs unknown:

## Containment
Actions taken, approvals, and remaining exposure.

## Evidence
Evidence IDs, collection methods, hashes, and storage references.

## Root cause and attack path
Verified facts, hypotheses, and unknowns clearly separated.

## Eradication and recovery
Actions taken, restoration checks, and monitoring.

## Notifications
Decision owner, recipients, rationale, and dates. Do not include unapproved public statements.

## Follow-up
| Action | Owner | Due date | Status |
|---|---|---|---|

## Closure criteria
Why the incident is considered contained and recovered, and what residual risk remains.
~~~

## 14. Closure criteria

Do not close an incident only because the service is online. Confirm:

- active attacker access is contained;
- affected credentials and persistence are addressed;
- systems and data have been checked for integrity;
- recovery validation is complete;
- required communications and notifications are handled;
- residual risk is accepted by the responsible owner;
- follow-up remediation has named owners and deadlines;
- monitoring is in place for recurrence.

## 15. Reference material

- NIST Incident Response: https://csrc.nist.gov/projects/incident-response
- CISA Incident Response resources: https://www.cisa.gov/topics/cyber-threats-and-advisories/incident-response
- MITRE ATT&CK: https://attack.mitre.org/
- South African Information Regulator: https://inforegulator.org.za/

This playbook is operational guidance, not legal advice or a substitute for a tested organisation-specific incident response plan.
