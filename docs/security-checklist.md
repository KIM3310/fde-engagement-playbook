# Security Checklist — Pilot Exit Criterion

30-item security posture checklist. Used as pilot → production go/no-go. Every item must be green (or have an accepted risk waiver) before production rollout.

Designed for enterprise LLM deployments. Adapt specifics to your customer's compliance context (HIPAA, SOC2, PCI, GDPR, etc.).

---

## Authentication

- [ ] **A1**: Client-side authentication uses OIDC or SAML with customer's IdP. No local user accounts.
- [ ] **A2**: Service-to-service auth uses mTLS or signed JWTs (not shared API keys in env vars).
- [ ] **A3**: Session tokens expire within 8 hours idle or 12 hours absolute.
- [ ] **A4**: MFA required for all write operations. Method: FIDO2 or TOTP (not SMS).
- [ ] **A5**: Failed auth attempts logged with source IP; lockout after N consecutive failures.

## Authorization

- [ ] **Z1**: RBAC model defined and documented. Per-role permission matrix in code.
- [ ] **Z2**: Least-privilege verified via review of every role's permissions.
- [ ] **Z3**: Tenant isolation (if multi-tenant) enforced by row-level or query-level filtering.
- [ ] **Z4**: Break-glass access requires second-factor + logged reason + post-hoc review within 72h.

## Secrets and credentials

- [ ] **S1**: No secrets in source code, Dockerfile, ConfigMap, or container image.
- [ ] **S2**: Secrets sourced from customer Vault / ESO / Secrets Manager. One source of truth.
- [ ] **S3**: Secrets rotated on known cadence (quarterly minimum; immediately on personnel change).
- [ ] **S4**: No long-lived API keys with admin scope; use scoped tokens with least privilege.

## Data protection

- [ ] **D1**: Data encrypted at rest (AES-256 minimum) in all stores.
- [ ] **D2**: Data encrypted in transit via TLS 1.2+ (cluster-internal and external).
- [ ] **D3**: PII identified, classified, and handled per customer policy.
- [ ] **D4**: Debug logs contain no PII (tested with synthetic PII in dev; verified none leaks).
- [ ] **D5**: Data residency requirements honored. No data crosses regional boundaries without explicit authorization.

## Network

- [ ] **N1**: Internal services private (not publicly reachable).
- [ ] **N2**: External ingress terminates TLS at ALB / WAF / Gateway with modern ciphers.
- [ ] **N3**: Egress controlled via NetworkPolicy; only allowed destinations reachable.
- [ ] **N4**: Vector DB, metadata DB, cache all on private subnets with no public endpoint.
- [ ] **N5**: DNS resolution via private DNS where applicable (avoid public DNS for internal names).

## Observability and audit

- [ ] **O1**: Structured logging with request_id correlation across services.
- [ ] **O2**: Audit log captures: actor, action, subject, outcome, timestamp, source IP.
- [ ] **O3**: Audit log ship to customer's SIEM or equivalent retention store.
- [ ] **O4**: Log retention meets compliance requirement (7 years for HIPAA, as required for others).
- [ ] **O5**: Audit log integrity (hash chain or append-only) verified periodically.

## Supply chain

- [ ] **C1**: Dependency scan (Snyk / Dependabot / GitHub Advisory) on every build.
- [ ] **C2**: Container image scan (Trivy / Grype) on every build.
- [ ] **C3**: SBOM generated for production artifacts.
- [ ] **C4**: No known CVEs with critical or high severity un-remediated.

## Incident response

- [ ] **R1**: On-call rotation established (customer team).
- [ ] **R2**: Incident response runbook published and rehearsed.
- [ ] **R3**: Security incident contact documented (who to notify).
- [ ] **R4**: Blameless post-mortem process agreed.

## LLM-specific

- [ ] **L1**: Prompt injection testing performed on production prompts.
- [ ] **L2**: Output validation / sanitization (nothing executes LLM output as code without review).
- [ ] **L3**: PII leak check: known-PII inputs don't result in PII in logs or outputs to unauthorized roles.
- [ ] **L4**: Rate limiting (global + per-user) prevents cost runaway or abuse.

---

## How to use this checklist

### During pilot (week 1-2)

Run through the checklist. Identify which items are "N/A" (e.g., if system doesn't have multi-tenancy, Z3 is N/A), which are "in progress," which are "to do."

Store as Google Sheet or Confluence page. Update weekly.

### Security review (week 3-4 of pilot)

Schedule a 90-minute review with customer security lead. Walk through each item. Customer marks:

- **Pass**: item satisfied.
- **Risk accepted**: item not fully satisfied, but customer security has accepted the risk with documented rationale.
- **In progress**: remediation planned.

Any items in "in progress" by week 5 become a blocker for production rollout.

### Pilot → production gate

Before going to production, all items are either Pass or Risk Accepted. No "in progress." Security lead signs off.

---

## Common gotchas

1. **D4 (no PII in debug logs)**: structured logging often pulls entire request objects into debug logs. Test with synthetic PII (e.g., fake SSN patterns).

2. **S1 (no secrets in Dockerfile)**: build-args can leak secrets into image layers. Audit with `docker history`.

3. **N3 (controlled egress)**: LLM API calls go to providers. Ensure `api.anthropic.com`, `api.openai.com` are explicitly allowed, not implicitly open.

4. **L2 (output validation)**: if LLM output is used in a templating system or shell command, this is code injection waiting to happen. Always treat LLM output as untrusted.

5. **L3 (PII leak check)**: Claude / GPT sometimes quote back parts of the input. If input has PII and output goes to a lower-trust role, PII leaks. Test specifically for this.

---

## Risk waivers

When a risk is accepted, document:

- What the risk is (what item from checklist).
- Why it's being accepted (business reason).
- What compensating controls exist.
- When it will be reviewed (typically annually or at next engagement).
- Who approved it (name + role + date).

Store in the security review artifact alongside the checklist.

---

## Templates references

- Model card template (for Responsible AI posture): see governance/model-card.md
- Data processing impact assessment: see governance/gdpr-dpia.md if GDPR applies
- Threat model: template in security/ folder
