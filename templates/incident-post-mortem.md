# Incident Post-Mortem Template

**Purpose**: Blameless post-mortem capturing what happened, why it happened, how it was resolved, and what changes prevent recurrence. Written within 48 hours of incident resolution.

**Audience**: customer engineering team + vendor FDE. Shared with exec sponsor for SEV-1 incidents.

**Philosophy**: Blameless. The goal is system improvement, not individual blame. If the system allowed this incident, the system is what we fix.

---

## Incident Summary

**Incident ID**: INC-YYYY-MMDD-NNN
**Severity**: [SEV-1 / SEV-2 / SEV-3]
**Start time (detected)**: YYYY-MM-DD HH:MM UTC
**End time (resolved)**: YYYY-MM-DD HH:MM UTC
**Total duration**: [N hours M minutes]
**Impact**: [users / requests / data affected — quantified]
**Author**: [name + role]
**Reviewers**: [names + roles]

### Severity definitions used

- **SEV-1**: system unavailable or major data integrity issue; user-facing.
- **SEV-2**: degraded service; user-facing impact but system partially functional.
- **SEV-3**: internal impact only; no user-facing effect.

---

## TL;DR

[2-3 sentence summary of what happened, impact, and root cause. Written last, read first.]

Example: "LLM inference latency p95 exceeded 30 seconds between 14:15-15:03 UTC, causing 1,247 user requests to timeout. Root cause: Qdrant vector DB replica failover triggered by OOM on node eks-worker-03; primary replica then overloaded with sync traffic. Mitigation: increased Qdrant memory limits and added replica-healthcheck-aware circuit breaker."

---

## Timeline (UTC)

| Time (UTC) | Event |
|------------|-------|
| HH:MM | First indicator observed (what, where) |
| HH:MM | First alert fired (which alert, to whom) |
| HH:MM | Human acknowledgement (who responded) |
| HH:MM | First mitigation attempted |
| HH:MM | Diagnostic finding: [specific observation] |
| HH:MM | Root cause identified |
| HH:MM | Remediation applied |
| HH:MM | Service restored (how measured) |
| HH:MM | Monitoring confirms stable |

Aim for 1-2 minute granularity during active incident.

---

## Impact

### User impact
- Affected users: [count, if known; or user segment]
- Failed requests: [count]
- Delayed requests: [count]
- Degraded experience: [description]

### Business impact
- Revenue impact: [$ estimate or N/A]
- SLA impact: [error budget consumed %]
- Customer escalations received: [count]

### Data impact
- Data loss: [None / Partial / Full — with details]
- Data integrity: [confirmed intact or issues]
- Data exposure: [confirmed no / uncertain / yes]

---

## What happened

[3-5 paragraphs describing the incident narrative. Include contributing factors, not just the immediate trigger.]

### Contributing factors

1. [Factor 1: e.g., "Qdrant node memory limit was set at default; our indexed corpus grew to approach the limit without alerting."]
2. [Factor 2: e.g., "Circuit breaker around Qdrant wasn't aware of replica state; it passed queries to unhealthy primary."]
3. [Factor 3: e.g., "Alert on Qdrant memory was configured at 90% threshold but triggered at 95%; auto-scaling policy had a 5-minute delay."]

---

## Root cause

[1-2 paragraphs naming the specific root cause. Use the "5 whys" discipline — dig past surface-level triggers.]

**Primary root cause**: [e.g., "Qdrant memory exhaustion on node eks-worker-03 due to 40% growth in indexed vectors over past 60 days without corresponding memory scale-up. Memory alerting at 95% threshold provided insufficient warning for reactive response."]

**Secondary root cause**: [e.g., "Circuit breaker to Qdrant did not include replica-health awareness, causing the degraded primary to receive full traffic instead of failing fast to the healthy replica."]

---

## What went well

Be explicit. Name the individuals and systems that worked. Blameless post-mortems include positive observations.

- [Thing that worked: "Alerting fired within 30 seconds of first threshold breach."]
- [Thing that worked: "On-call engineer was paged and acknowledged within 2 minutes."]
- [Thing that worked: "Runbook entry for 'Qdrant memory alert' existed and was followed correctly."]

---

## What went poorly

- [Item: "Memory alert threshold was too tight (95%); earlier warning at 80% would have enabled scheduled scale-up during business hours."]
- [Item: "Circuit breaker allowed traffic to unhealthy replica, amplifying the outage rather than fast-failing."]
- [Item: "Our capacity model hadn't been refreshed since 60-day corpus growth."]

---

## Lessons learned

Each lesson is specific and actionable.

1. **Monitor growth trends, not just thresholds.** A 60-day corpus doubling should have been visible on a trend dashboard, not only on a threshold alert.
2. **Circuit breakers must be state-aware.** A vector DB failover is a known operational event; our circuit breaker should route around it, not amplify it.
3. **Quarterly capacity reviews are not optional.** The capacity model should be reviewed at least quarterly against actual data growth.

---

## Action items

| # | Action | Owner | Due | Severity | Status |
|---|--------|-------|-----|----------|--------|
| 1 | Add Qdrant memory-growth-rate alert (80% threshold, 30-day rolling) | Customer DevOps | [date] | High | Open |
| 2 | Update circuit breaker to include Qdrant replica-health awareness | FDE + Customer Eng | [date] | High | Open |
| 3 | Quarterly capacity review added to engineering ops cadence | Customer Eng Manager | [date] | Medium | Open |
| 4 | Update runbook with new alert + circuit breaker behavior | FDE | [date] | Medium | Open |
| 5 | Add soak test at 1.5x current capacity to CI | Customer DevOps | [date] | Low | Open |

Every item has: owner, due date, severity, status. Review in post-mortem follow-up meeting 1 week later.

---

## Related documents

- Runbook: [link to operations runbook]
- Architecture decision record: [link]
- Previous related incidents: [INC-YYYY-... if pattern]
- Chat transcript: [link to Slack / Teams channel logs during incident]
- Graphs / dashboards at time of incident: [screenshots or links]

---

## Post-mortem meeting

- **Date**: [date of blameless post-mortem review meeting]
- **Attendees**: [list]
- **Duration**: 60 minutes
- **Outcome**: action items approved, owners assigned, due dates confirmed

---

## Appendix A: Graphs and Data

[Attach or link relevant graphs, logs, screenshots taken during incident]

## Appendix B: Chat / Slack Transcript

[Optional — redact any PII before sharing externally]

---

## Template usage notes

1. **Write this within 48 hours.** Memory fades; details disappear.
2. **Be blameless in wording.** "Engineer X failed to do Y" is wrong. "Our process didn't require Y at time Z" is right.
3. **Include what went well.** Every post-mortem names at least 3 things that worked.
4. **Make action items specific.** "Improve monitoring" is not actionable; "Add N alert with threshold X by [date]" is.
5. **Review in person.** A post-mortem meeting 1 week later beats a post-mortem doc that dies in a folder.
