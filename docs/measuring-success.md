# Measuring Success

Leading indicators, lagging indicators, and the business metric that actually matters. Written from the perspective of an FDE who needs to know mid-engagement whether things are on track — not just after.

---

## Why this document exists

LLM engagements have unusually long feedback loops. You build something in weeks 1-6. You ship to staging in weeks 7-10. You see real production data in weeks 11-16. You see real business outcomes in months 6-12.

Waiting until month 6 to learn you chose the wrong metric is career-ending. This document is about earlier signals.

---

## Three layers of measurement

### Layer 1: Leading indicators (you see within days)

Things that, if good, predict the engagement will go well:

- **Data sample is representative of production**. Week 1 observation.
- **Customer technical lead actually available 50% FTE**. Week 1 observation.
- **Eval harness running by day 5**. Week 1 observation.
- **Midpoint eval at week 3 is within 60-70% of target**. Week 3 observation.
- **Staging deployment completes in week 4**. Week 4 observation.
- **Customer SME produces gold-set labels on schedule**. Weekly observation.
- **Weekly status emails get read** (trackable via tool link clicks). Weekly.

If 5+ of these are green, pilot is likely to succeed.

### Layer 2: Lagging indicators (you see within weeks)

Things that tell you the current state:

- **Primary metric at pilot exit**: sensitivity, accuracy, F1, etc. from the scope.
- **System uptime in staging**: over last 7 days.
- **P95 latency**: meeting target or not.
- **Cost per call**: tracking to forecast or not.
- **Customer team independent operation**: how many times has the customer team executed a task without your help?

### Layer 3: Business outcome (you see within months)

The thing that actually matters:

- **Workflow time changed**: before vs after.
- **Volume handled changed**: before vs after.
- **Quality of outcome changed**: customer measures this in their own terms.
- **Headcount redistributed**: if quality is maintained at less FTE, or more can be done at same FTE.
- **Revenue / cost / risk moved**: the ultimate metric.

This is the one the exec sponsor remembers.

---

## The gap between metric and outcome

The most common engagement failure is **technical metric success + business outcome failure**. 

Example:

Technical: "Our contract risk model has sensitivity of 0.89."
Business: "But the legal team still processes contracts at the same rate, because the model's output isn't integrated into their review workflow."

The model's technical success didn't translate to business impact because the workflow wasn't changed.

**How to catch this**: in discovery, ask "if we deliver a system with the target metric, what changes in the business?" If the answer is vague, the metric is probably wrong.

---

## The 80% rule

LLM pilots often achieve 80% of target metric. A well-scoped pilot either:
- Achieves target (great).
- Achieves 80% of target (normal; the last 20% is where iteration lives).
- Achieves <50% of target (re-scope; something fundamental is wrong).

The dangerous pattern is 80% of target + declaring victory. This is because:
- The remaining 20% often contains the hard edge cases.
- Hard edge cases disproportionately matter in production (they're the incidents).
- User experience is anchored on the worst 10%, not the average.

**Fix**: Report progress as "80% of target achieved; gap is [specific failure modes]." Force the conversation about what the 80%-vs-100% difference means for production.

---

## Metrics per phase

### Discovery phase (weeks 1-2)

Measure:
- Number of stakeholders interviewed (target: 5+)
- Data samples held and explored (target: 20+)
- Prototype touching real data (target: yes)
- Red flags identified (target: you know what they are)

### Scoping phase (week 3)

Measure:
- Signed scope document (binary)
- Named resource commitments from customer (5+)
- Named risks with mitigations (5-8)
- Sign-off from all required approvers

### Pilot phase (weeks 4-9)

Per week:
- Daily sync attendance rate (customer technical lead)
- Friday status sent by 5pm (yes/no)
- Weekly commitments hit (count out of committed)
- Primary metric trajectory (plotted week-over-week)
- Cost burn vs forecast

At end:
- Primary metric vs target
- All scope exclusions respected (no creep)
- Exit review held

### Production rollout phase (weeks 10-16)

Per week:
- Workstream progress (6 workstreams, 1-5 scale)
- SLO burn rate
- Incident count and severity
- Cost variance from forecast

At end:
- 30-day production data
- All checklist items green
- Canary rollout complete

### Operational handoff phase (weeks 17-20)

Per week:
- Artifacts completed (target: 95%+)
- Capability validations passed (target: 5/5 by week 20)
- Customer on-call independent events

At end:
- Signed handover document
- Zero FDE-driven operations in last 7 days

### Post-engagement phase (weeks 21-26)

Per week:
- Customer-driven advisory meeting (vs you driving)
- Independent incident resolutions by customer
- Expansion signals (if any)

At end:
- Success review held
- References / case study opt-in (if applicable)
- Measurable business outcomes captured

---

## The three "go/no-go" decisions

Engagements have three points where you explicitly re-commit or change direction:

### Week 3 decision: proceed to pilot?

Criteria:
- Scope signed.
- Customer resource commitments confirmed.
- Data access confirmed.
- At least 1 workable data sample explored.

If any red, delay scoping until green. Better to extend discovery than launch a doomed pilot.

### Week 6 decision: proceed to production rollout?

Criteria:
- Primary success metric achieved or near-achieved.
- Customer team on-board with production.
- No unresolved security / compliance blockers.

If any red, choose: extend pilot, pivot scope, or conclude.

### Week 20 decision: operational handoff complete?

Criteria:
- 5 capability validations passed.
- All artifacts delivered and verified.
- Customer engineering manager signs off.

If any red, extend handoff. Don't create a phantom handoff.

---

## Lagging metrics that predict churn

Even successful-appearing engagements can churn. Signals:

- **Customer team's operational questions shift from "how to operate" to "how to replace"**: existential concern.
- **Exec sponsor stops being quoted internally about the project**: political cover fading.
- **Cost is becoming a conversation**: if 3 months in, cost is the main topic, something's off.
- **Team attrition on customer side**: the champions leave.
- **New vendor in the space is being evaluated**: explicit competitive pressure.

If you see 2+ of these in post-engagement, escalate to AE. Churn prevention is cheaper than win-back.

---

## Success doesn't equal expansion

A common mistake: assuming a successful engagement will produce an expansion. It doesn't always. Reasons:

- Customer's priorities shift.
- Budget cycle doesn't allow.
- Team pivots to other initiatives.
- Internal politics on customer side.

Plan for that. A successful engagement that doesn't expand is still a success — and a reference, a learning, a template.

---

## Business outcome framing

When presenting to exec sponsor at success review, avoid metric-only framings. Layer:

**Level 1 (worst)**: "Our system achieved sensitivity 0.84."

**Level 2 (better)**: "Our system caught 84% of risk clauses in the gold set, vs a target of 80%."

**Level 3 (best)**: "Our system caught 84% of risk clauses, enabling the legal team to process 64 contracts per week vs 32 baseline, reducing review time from 4.2 hours per contract to 2.1 hours. That's $[specific] in recovered capacity, or [specific number] more contracts processed per quarter."

Level 3 translates the metric into what the exec sponsor cares about. That's what gets them to champion the next engagement.

---

## Anti-pattern: metric obsession

Some engagements produce teams obsessed with squeezing 1-2 points of metric. This can:
- Ignore the business metric.
- Produce brittle systems.
- Create unsustainable operational burden.

Push back. If you're at 0.84 vs target 0.80, ask "is getting to 0.86 worth another 4 weeks?" If no clear business answer, move on to the business outcome work.

---

## Final thought

The question "was the engagement successful?" has three answers:

1. **Did we hit the metric?** (Usually yes for well-scoped pilots.)
2. **Did the customer's team operate it independently?** (Medium hit rate.)
3. **Did the business outcome materialize?** (Lowest hit rate.)

Aim for all three. Measure all three. Track the middle and the last at least as carefully as the first.
