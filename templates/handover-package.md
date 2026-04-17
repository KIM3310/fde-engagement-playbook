# Handover Package — Template

**Purpose**: Documented, discoverable, self-sufficient package the customer team receives at formal handover (week 20). This template is the table of contents and a checklist.

---

## Handover Package Contents

### 1. Index (this document)

The master TOC. Every other document is linked from here.

### 2. Executive Summary (2 pages)

Narrative for exec sponsor and future leadership. Covers:

- What was built (in business terms).
- Success criteria achieved.
- Who operates it going forward.
- Ongoing advisory relationship.
- Known limitations and deferred work.

Written for the person who won't read any other document in this package.

### 3. Architecture Decision Records (ADRs)

Folder: `adr/`

Every significant architecture decision captured as a numbered ADR:

- `adr/001-inference-engine-vllm.md`
- `adr/002-vector-db-qdrant.md`
- `adr/003-rag-chunking-strategy.md`
- `adr/004-observability-stack.md`
- `adr/005-auth-integration-oidc.md`
- ... (typically 8-15 ADRs by end of engagement)

Each ADR: Status / Context / Decision / Consequences / Alternatives.

### 4. System Architecture Document

File: `architecture/system-overview.md`

- Component diagram (mermaid).
- Data flow diagrams.
- Deployment topology.
- External dependencies.
- Scaling characteristics.

Length: 8-15 pages with diagrams.

### 5. Operations Runbooks

Folder: `runbooks/`

- `runbooks/daily-operations.md` — what "normal day" looks like.
- `runbooks/deploy.md` — how to deploy.
- `runbooks/rollback.md` — how to roll back.
- `runbooks/scale-up.md` — how to handle traffic spikes.
- `runbooks/scale-down.md` — how to reduce cost in quiet periods.
- `runbooks/secret-rotation.md` — rotating API keys, DB credentials.
- `runbooks/disaster-recovery.md` — full recovery from a bad state.

### 6. Incident Response Runbooks

Folder: `incident-runbooks/`

One runbook per named alert:

- `incident-runbooks/alert-llm-latency-high.md`
- `incident-runbooks/alert-error-rate-high.md`
- `incident-runbooks/alert-cost-anomaly.md`
- `incident-runbooks/alert-vector-db-memory.md`
- `incident-runbooks/alert-circuit-breaker-open.md`
- `incident-runbooks/alert-security-failed-auth.md`

Each runbook: symptom / likely causes / diagnostic commands / mitigation steps / escalation.

### 7. Monitoring & Alerting

Folder: `monitoring/`

- `monitoring/dashboards.md` — list of dashboards with URLs and what each shows.
- `monitoring/slo-definition.md` — the SLOs you're operating against.
- `monitoring/alert-catalog.md` — every alert, threshold, routing.
- `monitoring/metric-glossary.md` — definition of every custom metric emitted.

### 8. Security & Compliance

Folder: `security/`

- `security/threat-model.md` — threat model for this system.
- `security/compliance-mapping.md` — which compliance controls this system supports (SOC2, HIPAA, etc.).
- `security/pen-test-report.md` — most recent pen-test summary.
- `security/data-handling.md` — how PII, PHI, or other sensitive data flows.
- `security/incident-response-contacts.md` — who to contact for security incidents.

### 9. Change Management

Folder: `change-management/`

- `change-management/release-process.md` — how code goes from dev to production.
- `change-management/change-review.md` — what changes require review, who reviews.
- `change-management/feature-flags.md` — how features are enabled / disabled.
- `change-management/rollback-decision.md` — when to roll back vs fix forward.

### 10. Evaluation & Quality

Folder: `eval/`

- `eval/eval-harness.md` — how to run the eval harness.
- `eval/gold-set-management.md` — how gold sets are created, stored, maintained.
- `eval/weekly-eval-cadence.md` — the weekly evaluation process and who owns it.
- `eval/regression-alerts.md` — what regression levels trigger alerts.

### 11. Prompt / Model Registry

Folder: `prompts/`

- `prompts/registry.md` — every production prompt with version history.
- `prompts/changelog.md` — recent prompt changes and their impact.
- `prompts/experiment-protocol.md` — how new prompts are tested before production.
- `prompts/rollback-prompts.md` — how to revert to a previous prompt version.

### 12. Cost Management

Folder: `cost/`

- `cost/cost-model.md` — how cost is computed and budgeted.
- `cost/attribution.md` — how cost attributes to users / tenants / features.
- `cost/monthly-review-process.md` — who reviews cost monthly.
- `cost/optimization-playbook.md` — common cost reduction patterns.

### 13. Team & Ownership

Folder: `ownership/`

- `ownership/raci.md` — who's responsible for what.
- `ownership/on-call-schedule.md` — on-call rotation.
- `ownership/escalation-matrix.md` — who to escalate to for each type of issue.
- `ownership/vendor-contacts.md` — vendor (you) contact information and advisory cadence.

### 14. Known Limitations

File: `known-limitations.md`

A catalog of things the system does NOT do, with rationale:

- Out-of-scope cases.
- Known edge cases that fail.
- Populations / use cases where the system should not be used.
- Deferred features (why deferred, when to revisit).

### 15. Evolution & Next Steps

File: `evolution-roadmap.md`

Recommended next work. Not a binding roadmap — advisory.

- Near-term (next 30 days): reliability improvements, documentation gaps.
- Medium-term (90 days): feature expansions, performance work.
- Long-term (6-12 months): architectural evolutions, re-scoping opportunities.

### 16. Training Materials

Folder: `training/`

- `training/new-engineer-onboarding.md` — for a new engineer joining customer team.
- `training/technical-deep-dives.md` — recorded sessions or transcripts from walkthroughs.
- `training/troubleshooting-scenarios.md` — example scenarios with expected responses.

### 17. Vendor Relationship

File: `vendor-advisory.md`

- Post-engagement cadence (weekly → bi-weekly → monthly).
- What the vendor (you) will help with.
- What the vendor will not help with (work you're handing off).
- Escalation path for emergencies.

### 18. Sign-off Document

File: `handover-signoff.md`

```
Customer engineering manager signature: ______________________________
Date: ___________________

Exec sponsor sign-off: ______________________________
Date: ___________________

FDE signature: ______________________________
Date: ___________________

By signing, the customer team confirms:
1. Handover artifacts reviewed and complete.
2. All 5 capability validations passed.
3. Customer team is operationally responsible effective today.
4. Vendor relationship shifts to advisory / escalation-only.
```

---

## Checklist for handover package completeness

Before week 20 handover meeting:

- [ ] Every folder has at least the documents listed above.
- [ ] No document is stale (last updated within 30 days).
- [ ] Every document is readable by a new joiner (tested with a junior engineer on customer side).
- [ ] All dashboards owned by customer, not vendor.
- [ ] All alerts routed to customer on-call, not vendor.
- [ ] All secret access is on customer-owned paths.
- [ ] Vendor contacts are explicitly labeled as advisory, not primary.
- [ ] Sign-off document ready.
- [ ] Physical / electronic copies accessible to customer team without dependence on vendor systems.

---

## Anti-patterns (what not to hand over)

- **Personal notes / Slack channels**: no "just DM me if you have questions"; replace with team channels and documented runbooks.
- **Your test credentials**: customer team creates their own; your credentials revoked at handover.
- **Documentation in your private drive**: must be in customer's documentation system.
- **Tribal knowledge that only you know**: if it matters, it's documented.
- **Tools that only you have access to**: customer has equivalent or alternative.

---

## Delivery method

At end of week 19:

- Customer forks a repo (customer-controlled) containing this handover package.
- Vendor contributes final version as PR.
- Customer engineering manager merges and holds authoritative copy.
- Any future updates go through customer's normal change control, not via vendor.
