# Runbook 05 — Operational Handoff (Weeks 17-20)

## Objective

Transfer full ownership of the production system to the customer's team. By end of week 20, the customer team must be able to operate, evolve, and diagnose the system without your involvement — with you as escalation-only.

## The handoff is NOT a meeting

Handoff is a 4-week process that ends with a meeting. Treating it as just a meeting ("we had the handoff call!") is the single biggest failure mode in this phase.

Proper handoff has three layers:

1. **Artifacts**: documented, discoverable, up-to-date.
2. **Capability**: customer team has performed the operations you're handing off.
3. **Confidence**: customer team, when asked, says "we're ready." Not just nods.

## The 4-week structure

### Week 17: artifacts inventory + gap analysis

**Goal**: you and the customer technical lead produce a complete artifacts list; customer flags what's missing or unclear.

**Your pre-work**:
- Inventory every artifact: code, runbooks, dashboards, alerts, architecture docs, ADRs, post-mortems, change-management docs.
- Test your own artifacts: can a new person read them and know what to do?

**Joint session** (half-day):
- Walk through every artifact.
- Customer flags "this is unclear" / "this is missing" items.
- Agreed remediation list for week 18.

### Week 18: artifacts hardening

**Goal**: every item on the remediation list is closed. Artifacts are stand-alone usable.

Common gaps found in week 17:
- Runbooks assume knowledge the customer team doesn't have.
- Dashboards exist but customer doesn't know where / how to access.
- Alert routing to customer on-call but customer never tested receiving one.
- Deployment process documented but never executed by customer.
- Dependencies not listed (e.g., "oh, the eval suite also requires this bucket").

Each gap: write a fix; deliver within 48 hours of being flagged.

### Week 19: capability validation (the important week)

**Goal**: customer team performs every operation you're handing off, with you watching only. Not assisting. Not driving.

Required exercises:
1. Customer on-call resolves a simulated P1 incident.
2. Customer DevOps does a deployment (a real one, even if trivial).
3. Customer engineering lead updates a prompt and re-runs eval harness.
4. Customer data engineer re-runs the weekly eval on new data.
5. Customer product manager pulls cost attribution from the dashboard.

If any exercise fails, re-train; repeat until passing. No graduation without all 5 passing.

### Week 20: formal handoff meeting + week 21-24 scheduled advisory

**Handoff meeting** (2 hours):
- Review artifacts (the customer team presents, not you).
- Review capability validations (which passed, which needed re-do).
- Sign-off from customer engineering manager and exec sponsor.
- Agree on post-engagement cadence (typically weekly call for 4 weeks, then monthly).

**Post-meeting immediately**:
- Update your RACI: you are now escalation-only, not primary owner.
- Update on-call rotation: customer owns.
- Update alert routing: customer primary, you secondary escalation.

## Handoff artifacts master list

Checklist — every item exists, current as of handoff date, and tested:

**Code & Config**:
- [ ] All source code in customer-controlled repo.
- [ ] Build/deploy scripts (Makefile, CI workflows).
- [ ] Helm chart / Terraform modules / Dockerfile — versioned.
- [ ] Environment config files (.env.example, values-prod.yaml).
- [ ] Secret management: ownership of vault paths transferred.

**Documentation**:
- [ ] Architecture decision record (ADR) set.
- [ ] System-level architecture doc.
- [ ] Deployment runbook.
- [ ] Operations runbook (start/stop/deploy/rollback).
- [ ] Incident response runbook (top 5 incident types).
- [ ] Troubleshooting guide.
- [ ] Known limitations doc.

**Observability**:
- [ ] Dashboards in customer's observability tool, owned by customer.
- [ ] Alert definitions, routed to customer on-call.
- [ ] SLO definitions documented.
- [ ] Runbook entry for each alert.

**Compliance**:
- [ ] Audit log destination verified.
- [ ] Retention policies implemented.
- [ ] Compliance mappings (SOC2 / ISO / HIPAA as applicable).
- [ ] Data residency configuration.

**Change management**:
- [ ] Change log / release notes format.
- [ ] Code review gates.
- [ ] Rollout strategy for future changes (canary, feature flags).

**Quality**:
- [ ] Eval harness runnable by customer.
- [ ] Gold set maintained in customer-controlled store.
- [ ] Regression test process.

**Operations**:
- [ ] On-call rotation established.
- [ ] Escalation path to vendor (you) documented.
- [ ] Monthly review cadence agreed.

## The 5 capability validations (detailed)

### 1. Incident response (simulated)

**Simulation**: you trigger a synthetic P1 (e.g., inject a pod failure, or simulate an alert). Customer on-call must:
- Acknowledge within 5 minutes.
- Follow the runbook.
- Resolve or escalate within 30 minutes.
- Write a post-mortem within 24 hours.

**Pass criterion**: all 4 of the above.

**Common failure mode**: on-call hasn't memorized runbook location. Make it findable from the alert.

### 2. Deployment

**Exercise**: customer DevOps deploys a real (small) change to production. You watch, don't help.

**Pass criterion**: deployment succeeds; rollback tested (even if not needed); metrics return to baseline.

### 3. Prompt update + eval run

**Exercise**: customer engineering lead updates a prompt, runs the eval harness, interprets the result.

**Pass criterion**: update is committed, eval runs cleanly, the engineer can say "the score went from X to Y and here's what that means."

### 4. Weekly gold-set eval

**Exercise**: customer data engineer pulls a fresh batch of production-ish data, runs it through the eval harness against the current gold set.

**Pass criterion**: the cadence is sustainable — the engineer can do this in under 2 hours per week.

### 5. Cost attribution

**Exercise**: customer PM produces a cost breakdown for the past 30 days, attributed per feature / per tenant (if applicable).

**Pass criterion**: the PM produces the report without asking you for help; the numbers match finance's.

## What "handoff went well" looks like

End of week 20:

- All 5 capability validations passed on first or second attempt.
- Customer engineering manager signs off.
- On-call rotation has been running for 2+ weeks with customer as primary.
- At least one "caught by customer, not you" incident already happened.
- Artifacts are discoverable (not just in existence).

## Common handoff failures

1. **"You'll still be around if anything happens, right?"** — This is a yellow flag. Some advisory is expected, but if the customer expects you as hidden on-call, handoff didn't happen. Push back: "I'm on as escalation; primary is you."

2. **Dashboards are in your monitoring account, not customer's**. Customer literally can't see their own system without your login. Critical issue; fix before handoff.

3. **Customer on-call has never been paged**. They don't know the experience of being woken up by this system. Simulate it in week 19.

4. **Runbooks reference your personal Slack channels / your personal credentials**. Unusable by customer. Fix.

5. **One "hero" on customer side has absorbed everything; nobody else knows the system**. Bus factor of 1. Push training to a 2nd customer engineer.

6. **Customer can run the system but can't change it**. Evolution will stop the moment you leave. Push for deeper understanding.

## Post-handoff support schedule

Typical:

**Weeks 21-24**: weekly 1-hour advisory calls. Customer drives agenda. You're reactive.

**Weeks 25-32**: bi-weekly 30-min calls. Usually focused on "we want to add X; is this a good idea?"

**Month 4 onwards**: monthly 30-min; or event-driven (scheduled only when customer has a specific question).

**Month 6**: formal engagement wrap. Success review (see runbook 06).

## Handoff exit criteria (signed by customer engineering manager)

A formal document signed at end of week 20:

```
We, [customer team name], confirm that:

1. We have received and reviewed all handoff artifacts.
2. We have performed all 5 capability validations successfully.
3. We are operationally responsible for this system effective [date].
4. We understand the vendor's role is advisory / escalation from this date.
5. We have scheduled our first independent monthly review for [date].

Signed: [customer engineering manager name, date]
Signed: [FDE name, date]
```

Without this signature, the engagement is not handed off.

## What the FDE does differently during handoff

- **Less doing, more watching**. Week 19 in particular.
- **Ask "can you do this?" instead of "let me help you do this"**.
- **Document every question you receive**. If a customer team member asks the same question twice, the artifact is insufficient.
- **Actively discourage dependence on you**. "I know I said I'd help; but you actually need to do this yourself."

## Related

- Previous runbook: [04-production-rollout.md](04-production-rollout.md)
- Next runbook: [06-post-engagement.md](06-post-engagement.md)
- Templates: `templates/handover-package.md`
- Anti-patterns: [docs/engagement-anti-patterns.md](../docs/engagement-anti-patterns.md)
