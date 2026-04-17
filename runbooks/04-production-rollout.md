# Runbook 04 — Production Rollout (Weeks 10-16)

## Objective

Take the pilot from staging-grade to production-grade: 99.9% SLA, governed access, observable, auditable, supportable by the customer's on-call rotation. End with a production system running on the customer's infrastructure serving real users.

## Input state

- Passing pilot in staging.
- Signed-off decision to go to production.
- New resource commitments (often a larger team on customer side — ops, security, QA).
- Revised scope for production readiness.

## Output state at end of week 16

- Production deployment running in customer's prod environment.
- Customer on-call rotation owns first-line response.
- Dashboards visible to customer leadership.
- 30+ days of production data collected.
- Handoff criteria met (see runbook 05).

## Production rollout is not "bigger pilot"

A common failure mode: treating production rollout as "pilot with more users." It's not. Pilot was a controlled experiment. Production is:

- A system that can't fail catastrophically.
- A system that has to be maintainable by someone who isn't you.
- A system that interacts with the customer's real users, real auth, real data flows, real compliance obligations.

Reset expectations. Production rollout is typically **longer than the pilot**, not shorter.

## The 6-workstream model

Production rollout is 6 parallel workstreams. Each has a named owner (you or customer) and a definition of done:

| Workstream | Owner | Done definition |
|-----------|-------|-----------------|
| 1. Infrastructure | Customer DevOps, you advising | Prod cluster live, DR tested, HA validated |
| 2. Observability | You, customer advising | Dashboards + alerts wired, SLO defined |
| 3. Security & Compliance | Customer Security, you advising | Penetration test passed, compliance doc complete |
| 4. Integration | Customer + you | Real auth, real data flows, real downstream consumers |
| 5. Reliability engineering | You, customer advising | SLO met in soak test, runbook tested in tabletop |
| 6. Change management | Customer PM + you | User training, rollout comms, feedback loop |

Run these in parallel. Weekly sync across workstream leads.

## Week-by-week (6-week rollout)

### Week 10 — Plan + parallelize

**Goal**: all 6 workstreams have a week 10-15 plan with named owners and weekly milestones.

**Key artifacts**:
- Updated architecture doc (prod-specific).
- Production scope document (what's in, what's out).
- SLO definition.
- Rollout comms plan.

**Meetings**:
- Day 1: all-hands kickoff (all 6 workstream leads, 2 hours).
- Day 2-4: workstream-specific deep dives (1 hour each).
- Day 5: consolidated plan published.

### Week 11 — Infrastructure + observability

**Infra workstream**:
- Customer DevOps deploys the chart/Terraform to a prod namespace (still no prod traffic).
- HA validated: node failure, pod restart, warehouse failover.
- Backups and DR validated.

**Observability workstream**:
- Grafana dashboards for: latency, error rate, cost, model version, input distribution.
- Alerts for: SLO burn rate, circuit breaker open, cost anomaly, error spike.
- Paging integration with customer's on-call tool.

### Week 12 — Security + compliance

**Security workstream**:
- Penetration test by customer's AppSec team or external.
- Threat model review.
- Dependency vulnerability scan.
- Secrets audit: no hardcoded credentials, all via Vault/ESO.
- Audit log destination validated (to SIEM).

**Compliance workstream**:
- Data residency confirmed.
- Audit retention configured.
- SOC2 / ISO / HIPAA (whichever applies) mapping validated.
- Privacy review complete (DPIA if EU).

### Week 13 — Integration

**Integration workstream**:
- Customer's real auth (OIDC/SAML) integrated.
- Downstream consumers identified; contracts with them written.
- Data flows from real source systems (not sample data).
- Rate limits per-tenant / per-user implemented.

### Week 14 — Reliability engineering

**Reliability workstream**:
- Soak test: 10x expected peak load for 24 hours.
- Chaos test: inject 3-5 failure modes (pod kill, network partition, warehouse slow).
- Runbook tested in a tabletop exercise with customer on-call.
- Incident response drill: can the customer on-call resolve a simulated P1 without calling you?

### Week 15 — Canary rollout

**Goal**: real users see the system for the first time.

Canary strategy options:
- **User-based canary**: 5% of users → 25% → 50% → 100%, over 2 weeks.
- **Feature-flag canary**: dark launch behind flag, shadow-evaluate, then enable.
- **Tenant-based canary**: one customer team / department first, then broader.

Metrics to watch during canary:
- Error rate vs baseline.
- Latency vs baseline.
- Cost (per call, per day) vs forecast.
- User satisfaction (direct feedback + analytics).

### Week 16 — Full production + handoff

**Goal**: 100% production traffic, customer on-call rotation owns first-line response, you're on-call as escalation only.

Handoff meeting (2 hours):
- Review the 30 days of production data.
- Review the open issues list.
- Sign-off from customer engineering manager that they own it.
- Schedule weekly advisory calls for the next 4 weeks (post-engagement).

## Production-grade requirements checklist

Below is the "pilot-done is not prod-done" checklist. Every item must be green before go-live.

**Infrastructure**:
- [ ] HA: no single-point-of-failure component.
- [ ] Auto-scaling tested against 3x peak load.
- [ ] DR runbook tested (not just written).
- [ ] Backup / restore tested for metadata and model artifacts.
- [ ] Multi-AZ or multi-region as per customer risk tolerance.

**Security**:
- [ ] All secrets via Vault / ESO; none in repo, configmap, or Dockerfile.
- [ ] TLS everywhere (cluster-internal + external).
- [ ] RBAC least-privilege verified.
- [ ] Penetration test passed.
- [ ] Audit logging to SIEM verified.

**Observability**:
- [ ] SLO defined and agreed with customer.
- [ ] Dashboards owned by customer ops.
- [ ] Alerts routed to customer on-call, not your team.
- [ ] Cost dashboard with daily budget burn-rate.

**Reliability**:
- [ ] Circuit breakers for all external dependencies.
- [ ] Rate limiting (global + per-tenant).
- [ ] Graceful degradation (what happens when LLM provider is down?).
- [ ] Input validation and size limits.
- [ ] Timeout budgets documented.

**Compliance**:
- [ ] Data retention policy implemented.
- [ ] PII handling reviewed and documented.
- [ ] Consent / purpose-of-use captured where relevant.
- [ ] Regional data residency honored.

**Operations**:
- [ ] Runbook for 5 most-likely incidents, tested in tabletop.
- [ ] On-call rotation established and owning primary response.
- [ ] Escalation path to vendor (your team) documented.
- [ ] Change-management process: how to deploy, how to rollback.

**Change management**:
- [ ] User training delivered.
- [ ] User-facing docs published.
- [ ] Feedback capture channel live.
- [ ] Rollout comms delivered to affected users.

## SLO definition (important, often skipped)

Define 3-5 SLOs with the customer during week 10. Typical LLM service SLOs:

- **Availability**: 99.5% (pragmatic for pilot-to-prod; aim for 99.9% after 90 days).
- **Latency**: P99 response time under N ms for endpoint X.
- **Accuracy**: eval suite passes at threshold T on weekly gold-set run.
- **Cost**: daily cost stays within $X budget; burn-rate alerts.

SLOs drive alerts, which drive on-call behavior. Without them, the system is "best-effort" forever.

## Cost monitoring during rollout

Production rollout is when the customer sees the real cost. Often a surprise.

- Publish a cost dashboard from day 1 of week 15 (canary).
- Daily budget burn rate.
- Per-user / per-tenant cost attribution if possible.
- Alert when daily cost > 1.5x forecasted average.

If cost exceeds budget, options:
1. Prompt caching (Claude/GPT cache hit rates can cut input cost 90%).
2. Batch API for async workflows.
3. Model downsize for 80% of traffic (use small model except when big is needed).
4. Rate limiting with quotas.

Raise cost concerns to project owner before they become exec-level issues.

## Common production rollout failures

1. **Skipped security review; discovered in week 15**. Security blocks go-live. Pilot-plus delays 4 weeks.

2. **Customer on-call not actually ready**. You ghost-page for first 30 days because customer team needs more training.

3. **Cost at production scale exceeds budget**. Exec sponsor gets surprised. Re-scoping conversation needed.

4. **Downstream consumer depends on a field not in the contract**. Breaks when you update the response format. Mitigation: formal API contract with version.

5. **Model provider rate limits hit at production traffic**. Hadn't been tested at real scale. Emergency: multi-provider fallback or provider-level rate limit increase.

6. **Prompt regression**: team updates the prompt to improve one case; silently degrades 5 others. Mitigation: regression eval in CI.

7. **Drift**: production data distribution differs from pilot eval set. Accuracy slowly degrades. Mitigation: production eval gold-set from real data.

## What "production rollout went well" looks like

- 30 days of production data with SLOs met.
- Customer on-call has resolved at least one incident without FDE involvement.
- Cost is within forecast +/- 15%.
- At least one exec-level success story documented ("we reduced X by Y%").
- Customer team can demo the system to their own peers without you in the room.

## Related

- Previous runbook: [03-pilot-build.md](03-pilot-build.md)
- Next runbook: [05-operational-handoff.md](05-operational-handoff.md)
- Templates: `templates/deployment-architecture-doc.md`, `templates/incident-post-mortem.md`
- Anti-patterns: [docs/engagement-anti-patterns.md](../docs/engagement-anti-patterns.md)
