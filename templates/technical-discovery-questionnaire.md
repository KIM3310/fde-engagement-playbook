# Technical Discovery Questionnaire

Systematic technical fit assessment. Conduct as 2-hour session with the customer's technical lead in week 1 of discovery. Walk through section by section; don't just email this and expect written answers.

---

## Section 1: Current System Architecture

- [ ] Current system language(s) and framework(s): ___
- [ ] Deployment target: ___ (cloud vendor, self-hosted, hybrid)
- [ ] Orchestration: Kubernetes? ECS? Raw VMs? Serverless? Version: ___
- [ ] Current traffic scale (RPS, DAU, data volume): ___
- [ ] Request latency p50/p95/p99: ___
- [ ] Error budget / SLO currently in place? ___
- [ ] Monitoring stack: ___ (Datadog? Grafana? CloudWatch? Other?)
- [ ] Logging stack: ___
- [ ] Tracing in place? ___
- [ ] On-call rotation and paging tool: ___

## Section 2: Data Landscape

- [ ] Primary data sources: ___ (systems, not just "our database")
- [ ] Data formats: ___ (Parquet? JSON? CSV? Proprietary?)
- [ ] Data volume per day / month / total: ___
- [ ] Data refresh frequency: ___ (real-time, hourly, daily, weekly?)
- [ ] Data governance state: ___ (catalog? Data dictionary? Lineage?)
- [ ] PII / sensitive data present? Handling today: ___
- [ ] Data residency requirements: ___ (regions, regulatory)
- [ ] Sample data available for discovery? Size of sample: ___

## Section 3: LLM Experience and Existing Systems

- [ ] Teams currently using LLMs internally (yes/no + which teams): ___
- [ ] Providers tried: OpenAI? Anthropic? Bedrock? Self-hosted? ___
- [ ] Models in production today (if any): ___
- [ ] Existing LLM usage volume (tokens / month): ___
- [ ] Known failure modes observed: ___
- [ ] Cost budget for LLMs this year: ___
- [ ] Internal AI platform / tooling in place? ___ (e.g., a feature store, prompt registry, eval harness)

## Section 4: Security and Compliance

- [ ] Data classification framework in place? ___ (e.g., Public / Internal / Confidential / Restricted)
- [ ] Specific regulatory regimes: ___ (HIPAA? SOC2? PCI? GDPR? Industry-specific?)
- [ ] AppSec / PenTest cadence: ___
- [ ] Vulnerability management process: ___
- [ ] Secrets management: Vault? AWS Secrets Manager? Other?: ___
- [ ] Authentication: SSO? Which IdP?: ___
- [ ] Audit logging requirements: ___ (destination, retention, format)
- [ ] Data in/out of country constraints: ___

## Section 5: CI/CD and Deployment

- [ ] Source control: ___ (GitHub? GitLab? Bitbucket? Other?)
- [ ] CI/CD platform: ___ (GitHub Actions? Jenkins? CircleCI? GitLab CI? Other?)
- [ ] Typical deployment frequency: ___ (daily? weekly? monthly?)
- [ ] Rollback process: ___
- [ ] Feature flags in use? Platform: ___
- [ ] Testing strategy: ___ (unit / integration / e2e / what coverage?)
- [ ] Pre-prod environments (staging, UAT, etc.): ___
- [ ] Change approval process: ___ (CAB? automated? lead approval?)

## Section 6: Team

- [ ] Technical lead name + seniority: ___
- [ ] Team size on this project: ___
- [ ] Team's ML / LLM experience: ___ (none / some / deep)
- [ ] Team's K8s experience: ___
- [ ] Team's Python experience (critical for LLM work): ___
- [ ] Competing priorities during engagement window: ___
- [ ] Team availability commitment (FTE hours/week): ___

## Section 7: Business Context

- [ ] Exec sponsor name + title: ___
- [ ] Budget approved for this engagement: ___
- [ ] Budget approved for year-1 operational cost: ___
- [ ] Success criterion (exec sponsor's words): ___
- [ ] Timeline pressure: ___ (is there a hard date?)
- [ ] Internal skeptics (role, not names): ___
- [ ] Competing internal initiative using AI/ML: ___
- [ ] Regulatory deadlines affecting this work: ___

## Section 8: Risk Posture

- [ ] Tolerance for outage during rollout: ___ (zero / minor / acceptable)
- [ ] Risk of regression affecting users: ___
- [ ] Risk of PR issue if LLM misbehaves: ___
- [ ] Appetite for non-determinism (LLMs are not deterministic): ___
- [ ] Appetite for cost variance (LLM costs fluctuate): ___

## Section 9: Known Unknowns

Open question; record verbatim:

> "What technical concern have you not mentioned that keeps you up at night about this project?"

Common surfaced items:
- "I'm worried about the data quality; we haven't really audited it."
- "The team hasn't built anything customer-facing in Python before."
- "Our observability for AI workloads is basically nothing today."
- "I don't know if our budget is realistic."
- "[Colleague] built a prototype last year that didn't go anywhere; I'm worried we'll repeat that."

Listen for themes. They become risk register items.

## Section 10: Desired End State (18 months out)

> "Fast-forward 18 months. If this engagement and its follow-up are wildly successful, what's different for your team and for your users?"

Captures ambition beyond the pilot. Shapes pilot scope to not foreclose year-2 options.

---

## After the session

Within 24 hours:

1. **Fill in the questionnaire** with every answer captured.
2. **Flag gaps**: any unanswered section is a follow-up.
3. **Derive 5 technical risks** from the answers.
4. **Draft a "technical landscape" diagram** of the current state.
5. **Send recap email** to technical lead + project owner:

```
Subject: [Engagement Name] Technical Discovery Recap

Thanks for the session. Captured 40+ data points. Top 5 things I need to follow up on:

1. [item]
2. [item]
3. [item]
4. [item]
5. [item]

Full questionnaire attached. Flag any clarifications before we lock scope next week.

Best,
[name]
```

---

## Common failure modes of this questionnaire

1. **Rushing through it**. Takes 2 hours. Don't try to do in 45 minutes.
2. **Accepting vague answers**. "Our data is clean" is not an answer; push for specifics.
3. **Emailing it instead of running it live**. You lose 80% of value (the gaps between questions).
4. **Ticking boxes without writing a paragraph**. The paragraph for each item is where the insight is.
5. **Skipping Section 9 ("known unknowns")**. This is the highest-signal section. Don't skip it.
