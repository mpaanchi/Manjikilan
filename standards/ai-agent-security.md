# KODEE-Secure — AI Agent Security Standard

This standard applies to LLM-powered features, autonomous agents, coding agents, retrieval-augmented generation (RAG), MCP integrations, browser agents, and systems that can call tools or change external state.

## 1. Security objective

An AI agent must not gain authority merely because a prompt, retrieved document, web page, tool output, or memory record tells it to do something.

The model may interpret and propose actions. Enforce identity, permissions, data boundaries, approval requirements, and execution policy in trusted application code and infrastructure.

## 2. Threat model

Identify:

- users, tenants, administrators, and external operators;
- system prompts, developer instructions, and policy configuration;
- user input and uploaded files;
- retrieval stores, embeddings, memory, and knowledge sources;
- tools, APIs, MCP servers, browser access, shell access, and integrations;
- model provider, telemetry, traces, and retention;
- data and state the agent can read, create, modify, export, or delete;
- actions with financial, legal, privacy, or production impact.

Consider direct and indirect prompt injection, tool misuse, data exfiltration, memory poisoning, retrieval poisoning, cross-tenant leakage, malicious files, supply-chain compromise, excessive autonomy, and denial of wallet/resource exhaustion.

## 3. Identity and permission architecture

- Give each agent or workload a distinct identity where feasible.
- Scope credentials to the minimum actions, resources, and duration required.
- Prefer short-lived tokens and server-side credential storage.
- Do not expose privileged secrets in prompts, browser code, model output, or retrieval content.
- Enforce authorisation at the tool/API boundary on every call.
- Re-check the current user's identity and permission before sensitive operations.
- Do not trust a user ID, tenant ID, role, or permission supplied only in free-form model text.
- Separate read-only and write-capable tools.
- Do not reuse broad administrator credentials for ordinary agent activity.

A tool name or JSON schema is not an access-control mechanism.

## 4. Prompt injection and untrusted content

Treat the following as untrusted data:

- user messages and attachments;
- retrieved documents and embeddings;
- websites, emails, tickets, comments, and code repositories;
- tool responses and logs;
- third-party model responses;
- content stored in agent memory.

Controls:

- Separate trusted policy/instructions from untrusted content in the application design.
- Clearly mark untrusted content as data to analyse, not instructions to obey.
- Never let retrieved content change the agent's allowed tools, identity, or approval rules.
- Do not execute commands or follow links solely because external content requests it.
- Use allowlists and schema validation for tool inputs.
- Validate output before passing it to shells, SQL, URLs, HTML, email, or downstream APIs.
- Require independent policy checks for high-impact decisions.
- Test both direct and indirect prompt-injection attempts.

Prompt wording is a defence-in-depth aid, not the primary security control.

## 5. Tool and action security

For each tool define:

- owner and purpose;
- input schema and validation;
- authenticated identity;
- allowed resources and operations;
- data classification;
- read/write/delete side effects;
- timeouts, limits, and retry behaviour;
- audit event and error handling;
- approval requirement.

Use:

- allowlisted tool registries;
- strict argument validation;
- server-side permission checks;
- narrowly scoped API tokens;
- per-user and per-tenant quotas;
- idempotency and replay protection for state-changing calls;
- output filtering and redaction;
- sandboxing for shell, browser, code, and file operations.

Do not expose unrestricted shell, arbitrary URL fetch, filesystem-wide access, or general database administration to an agent unless the environment and risk explicitly justify it and appropriate isolation is present.

## 6. Human approval and high-impact actions

Require an explicit approval checkpoint outside the model for actions such as:

- money movement, purchases, refunds, or financial commitments;
- production deployments and infrastructure changes;
- deleting or modifying business-critical records;
- sending bulk communications or publishing public content;
- changing user roles, security policies, or access permissions;
- exporting sensitive or regulated data;
- irreversible external actions.

The approval interface must show the exact action, target, scope, and material consequences. Approval for one operation must not silently authorise a broader batch or future action.

## 7. Retrieval, memory, and tenant isolation

- Enforce access control before retrieval, not only after generation.
- Partition or filter retrieval by tenant, user, and document permissions.
- Preserve provenance: record source, owner, timestamp, and access level.
- Re-authorise source access when data is retrieved or used for sensitive action.
- Prevent one tenant's memory or conversation state from entering another tenant's context.
- Treat memory writes as untrusted and potentially adversarial.
- Define expiry, deletion, correction, and retention behaviour.
- Avoid storing secrets or unnecessary sensitive personal data in vector stores.
- Test cross-tenant retrieval and memory leakage explicitly.

A retrieval filter based solely on a model-generated tenant identifier is insufficient.

## 8. Files, parsers, and multimodal input

- Scan or quarantine untrusted files where appropriate.
- Enforce type, size, decompression, and processing limits.
- Isolate document, image, archive, and code parsers where feasible.
- Treat OCR text, image text, transcripts, metadata, and embedded instructions as untrusted.
- Do not execute embedded macros, scripts, or commands.
- Validate generated files before exposing or executing them.
- Strip or protect sensitive metadata when required.

## 9. Output handling

Model output is untrusted until validated.

- Parse structured output against strict schemas.
- Encode or sanitise output before HTML, SQL, shell, email, or browser use.
- Do not interpolate model-generated strings into privileged commands.
- Use parameterised database calls.
- Validate URLs and restrict outbound destinations to reduce SSRF and data exfiltration risk.
- Apply content and policy filters appropriate to the product.
- Keep audit trails of consequential decisions and actions.

## 10. MCP and integration supply chain

For each MCP server or third-party tool integration:

- verify publisher, source, version, and provenance;
- review requested scopes and available tools;
- inspect dependencies and update history;
- isolate the server where possible;
- limit network egress and credentials;
- validate tool descriptions and schemas;
- monitor tool calls and failures;
- pin versions or releases where practical;
- review changes before upgrades;
- revoke access when no longer needed.

Do not treat a tool description as trusted policy. Tool outputs can be compromised or manipulated.

## 11. Data privacy and model providers

Determine:

- what user data enters prompts;
- which provider receives the data;
- retention, training, and telemetry settings;
- data residency and cross-border flows;
- whether the provider is a processor or subprocessor;
- how deletion requests propagate;
- what is captured in traces, evals, and logs.

Minimise data before sending it to a model. Redact identifiers and secrets where feasible. Do not claim privacy or regulatory compliance without verifying the provider contract, settings, and actual data flows.

## 12. Abuse and cost controls

- Set per-user, per-tenant, and global rate limits.
- Bound tokens, context size, tool calls, retries, and wall-clock execution.
- Cap concurrent agent runs and expensive external operations.
- Detect loops, repeated tool failures, and runaway delegation.
- Use budgets and alerts for model/API spend.
- Apply abuse controls to public-facing endpoints.
- Fail safely when quotas or providers are unavailable.

## 13. Observability and audit

Record, subject to privacy minimisation:

- initiating user or workload identity;
- agent/session/run ID;
- model and relevant version/configuration;
- tools called and authorised action scope;
- approval decisions;
- result status, errors, and duration;
- policy denials and rate-limit events.

Do not log full prompts, secrets, private files, or personal data by default. Protect logs with access controls and retention limits.

## 14. Security test matrix

Test at minimum:

| Test | Expected result |
|---|---|
| Direct prompt injection asks for secrets | Agent refuses disclosure; secrets are not in accessible context or tool output. |
| Retrieved document instructs agent to ignore policy | Treated as untrusted content; tool permissions unchanged. |
| User supplies another tenant's record ID | API denies access regardless of model response. |
| Agent attempts an unapproved delete | External policy layer blocks the operation. |
| Tool receives malformed or unexpected arguments | Validation rejects the request safely. |
| Model returns shell/SQL/HTML payload | Downstream boundary validates, parameterises, or encodes it. |
| Malicious file contains instructions or exploit payload | File is isolated and content cannot gain execution authority. |
| Agent loops or retries excessively | Budget, timeout, and concurrency limits stop it. |
| MCP tool is unavailable or returns hostile output | Agent fails safely and treats output as untrusted. |
| User requests export of data they cannot access | Server-side authorisation denies the export. |
| Memory from tenant A is queried by tenant B | No cross-tenant content is returned. |
| Provider or tool call times out | No duplicate irreversible action is triggered on retry. |

Run these tests in an isolated environment with synthetic data.

## 15. Release gates

Do not release an agent with:

- unrestricted credentials or tools without a documented justification;
- untested tenant or user access boundaries;
- high-impact actions lacking independent authorisation;
- no limits on execution, spend, or concurrency;
- raw model output passed into privileged execution without validation;
- unreviewed third-party tool access;
- uncontrolled retention of sensitive prompts, files, or traces;
- no incident disablement or credential revocation path.

Record residual risks and secure approval for any time-bound exception.

## 16. References

- OWASP Top 10 for LLM Applications: https://genai.owasp.org/
- OWASP Cheat Sheet Series: https://cheatsheetseries.owasp.org/
- OpenAI Agents and tools documentation: https://developers.openai.com/
- Anthropic Agent Skills and tool documentation: https://platform.claude.com/docs/
- Model Context Protocol: https://modelcontextprotocol.io/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework

This standard is a starting point. Map controls to the actual model, agent framework, hosting environment, integrations, data, and business impact.
