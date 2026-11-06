# KODEE-Secure — Cloudflare Hardening Standard

This guide is a practical baseline for domains and applications using Cloudflare. It is not a universal configuration recipe. Confirm the product, plan, origin architecture, traffic profile, and current dashboard/API capabilities before changing anything.

## 1. Objectives

- Reduce direct exposure of origin infrastructure.
- Put appropriate edge protections in front of public application traffic.
- Protect authentication, APIs, and expensive application paths from abuse.
- Preserve legitimate users, integrations, and search crawlers.
- Avoid creating a false sense of security by relying on Cloudflare while leaving the origin publicly reachable.

## 2. Discovery before changes

Record:

- Cloudflare account, zone, domain, and owner;
- active plan and enabled products;
- DNS records, proxy status, and origin targets;
- origin provider, IPs, ports, and web server;
- application routes, API paths, webhooks, and admin surfaces;
- current WAF, rate-limiting, bot, cache, and firewall rules;
- TLS mode and origin certificate setup;
- existing monitoring and rollback method.

Export or document current rules before changing them. Never assume a feature is included in the account's plan.

## 3. DNS and origin protection

### Required review

- Proxy public web records through Cloudflare where compatible.
- Identify DNS-only records that disclose origin addresses or expose services unintentionally.
- Remove stale and dangling records only after confirming ownership and dependencies.
- Keep mail-related DNS records correct; do not proxy mail records as if they were HTTP traffic.
- Restrict origin firewall ingress to Cloudflare IP ranges where the architecture supports it.
- Keep management access (SSH, RDP, database admin) private or separately restricted.
- Consider Cloudflare Tunnel for suitable origins that do not need public inbound reachability.

### Important caveats

Proxying DNS hides the origin from ordinary DNS answers; it does not make a publicly reachable origin safe. Attackers may learn an origin IP from historical DNS, other records, application leaks, or misconfiguration.

If origin firewall restrictions are deployed, verify Cloudflare IP ranges from Cloudflare's official published list and maintain them as they change. Do not paste an unverified range into a production firewall.

### Authenticated Origin Pulls

Where supported by the origin architecture, consider Authenticated Origin Pulls (AOP) as an additional control so the origin can verify that inbound TLS connections came through Cloudflare.

- AOP requires a compatible TLS configuration.
- It is not compatible with Cloudflare Tunnel in the documented architecture.
- The globally shared Cloudflare client certificate proves a request came from Cloudflare, not uniquely from one zone. For stronger origin identity, assess custom/zone-level certificates where supported.
- Test certificate and origin configuration in staging first.

References:
- https://developers.cloudflare.com/fundamentals/security/protect-your-origin-server/
- https://developers.cloudflare.com/ssl/origin-configuration/authenticated-origin-pull/
- https://developers.cloudflare.com/fundamentals/concepts/cloudflare-ip-addresses/

## 4. TLS and certificates

- Prefer **Full (strict)** when the origin can present a valid certificate and hostname.
- Do not use Flexible mode as a substitute for securing origin TLS.
- Confirm certificates are valid, correctly scoped, and renewable.
- Redirect HTTP to HTTPS and avoid mixed-content dependencies.
- Consider HSTS only after validating all required hostnames and HTTPS readiness. The preload directive is a significant commitment and should not be enabled casually.
- Keep TLS settings compatible with application clients, APIs, webhooks, and integrations.

Do not change TLS mode on a production zone without validating origin readiness and having a rollback plan.

Reference: https://developers.cloudflare.com/ssl/origin-configuration/ssl-modes/

## 5. WAF and managed rules

- Review Cloudflare Managed Rulesets and the rules relevant to the application's technology and risk.
- Start in a non-blocking or logged mode where available and safe.
- Review events and tune exceptions narrowly.
- Use specific path, method, header, or managed rule identifiers for exceptions.
- Avoid broad allow rules that bypass an entire security layer.
- Monitor false positives and false negatives after rollout.
- Protect login, account recovery, admin, upload, and exposed API paths appropriately.

Do not assume the WAF detects every business-logic flaw, authenticated abuse case, or zero-day. The application still needs secure code and authorisation.

Reference: https://developers.cloudflare.com/waf/

## 6. Rate limiting and abuse controls

Create route-specific controls for sensitive or expensive operations:

- login and account recovery;
- OTP and email/SMS verification;
- registration, invitations, and password reset;
- search, exports, report generation, and uploads;
- payment initiation and other sensitive workflows;
- AI model inference, agent execution, and paid third-party API calls.

Design:

- choose the best available characteristics for counting requests;
- distinguish users, tokens, IPs, and global resource constraints;
- use challenge, block, or delay actions according to business impact;
- set thresholds from observed normal traffic, not guesses;
- consider shared NATs, mobile carrier IPs, VPNs, and distributed attackers;
- test legitimate automation and integrations;
- enforce critical quotas and concurrency limits in the application as well.

Cloudflare rate limiting counters can have propagation delay and may not be a precise hard cap on requests reaching the origin. Use application-side controls for strict resource and business limits.

Feature availability and matching fields differ by plan. Verify the current account's actual capabilities.

Reference: https://developers.cloudflare.com/waf/rate-limiting-rules/

## 7. Bot management and challenges

- Identify critical verified bots and legitimate crawlers.
- Protect high-abuse routes without challenging every visitor.
- Avoid blanket challenges that block APIs, webhooks, accessibility tools, or customers.
- Tune by route, method, verified bot status, and available bot signals.
- Review challenge outcomes, false positives, and support reports after changes.

Bot controls and signals vary by plan. Do not claim Bot Management is active unless it is present and configured in the account.

## 8. Cache security

- Do not publicly cache personalised or authenticated content.
- Review cache keys and variation by cookies, headers, query strings, and host.
- Set appropriate origin Cache-Control behaviour.
- Bypass cache for login, session, account, admin, and private API routes unless a carefully reviewed design says otherwise.
- Test cache behaviour with at least two distinct users and sessions.
- Review purge permissions and credential scope.

A cache rule that ignores a session cookie or tenant boundary can become a data disclosure vulnerability.

## 9. DNS and email domain security

- Enable DNSSEC where registrar and DNS configuration support it, and validate the chain after activation.
- Maintain a single valid SPF record that includes legitimate senders.
- Configure DKIM for each authorised sending service.
- Publish DMARC with reporting; move toward quarantine/reject only after inventorying legitimate senders and monitoring alignment.
- Review subdomains, lookalike domains, and stale records.
- Protect registrar accounts with MFA and restricted roles.

SPF, DKIM, and DMARC reduce sender-domain spoofing. They do not prevent all phishing, lookalike domains, compromised accounts, malicious attachments, or social engineering.

References:
- https://developers.cloudflare.com/dns/dnssec/
- https://developers.cloudflare.com/dns/manage-dns-records/how-to/email-records/

## 10. Security headers and application controls

Cloudflare may help apply or transform some response controls, but the origin application remains responsible for correct behaviour.

Review:

- Content-Security-Policy;
- Strict-Transport-Security;
- X-Content-Type-Options;
- Referrer-Policy;
- frame restrictions through CSP or X-Frame-Options;
- Permissions-Policy where relevant.

Test headers against the actual application. Do not blindly deploy a strict CSP or HSTS policy that breaks required functionality.

## 11. Logging and monitoring

Monitor:

- WAF events and rule matches;
- rate-limit actions;
- challenge outcomes;
- spikes in 4xx/5xx responses;
- unusual countries, ASNs, user agents, and request patterns where relevant;
- origin traffic that bypasses expected edge paths;
- DNS and certificate changes;
- changes to rules and privileged account access.

Protect logs from unauthorised access and redact tokens, cookies, and personal information. Confirm retention and export capabilities for the current plan.

## 12. Controlled rollout

1. Capture the current configuration and baseline traffic.
2. Identify the precise threat and intended control.
3. Build a narrow rule for one route or risk class.
4. Test in staging or non-blocking mode where possible.
5. Review event data and false positives.
6. Obtain production approval.
7. Deploy in a controlled window.
8. Verify legitimate flows and blocked test cases.
9. Monitor after deployment.
10. Record the final state and rollback method.

## 13. Validation scenarios

- A legitimate anonymous visitor can load public pages.
- An authenticated user can access only their own authorised data.
- A second user cannot access the first user's private response through a shared cache.
- An unauthorised direct request to a protected API is denied.
- A controlled abusive request pattern triggers the intended limit in an authorised test environment.
- Legitimate webhook and integration traffic still works.
- Direct-to-origin access is blocked or otherwise protected as designed.
- HTTP redirects correctly to HTTPS.
- Email authentication records align with legitimate senders.
- WAF and rate-limit events are visible to the responsible operator.

## 14. Change approval checklist

- [ ] Current configuration captured.
- [ ] Cloudflare plan and feature availability confirmed.
- [ ] Origin architecture and firewall dependencies understood.
- [ ] Rule scope and expected effect documented.
- [ ] Legitimate bots and integrations considered.
- [ ] Staging/non-blocking test completed where possible.
- [ ] Production approval received.
- [ ] Rollback plan documented.
- [ ] Post-change monitoring assigned.

Never present Cloudflare as a replacement for secure application code, identity controls, database authorisation, backups, or incident response.
