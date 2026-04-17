# Example: What a Failed Engagement Taught Us

*A composite narrative drawn from engagement debriefs. Specifics are fabricated.*

---

## Context

**Customer**: Mid-size financial services firm. 2,400 employees. Stated priority: "we need AI capabilities."

**Engagement scope (agreed at signing)**: 16-week pilot to build an LLM-based compliance review assistant for a pre-trade checking workflow. Process historically relies on 20+ compliance officers reviewing trades before execution.

**Outcome**: Pilot delivered something that technically worked, but was never adopted in production. Engagement ended 8 months later without a production deployment. A reference wasn't requested.

**Why we're writing this up**: the failure modes were avoidable. Each of them has become a specific rule in the runbooks upstream in this playbook.

---

## What went wrong, in order

### 1. We didn't actually validate that the stated problem was the real problem

During discovery, the VP Compliance described the pain as "our officers spend too much time on routine checks." Discovery meetings were held, the problem statement felt clear, scoping proceeded.

By week 6, as we delivered an MVP that could flag routine trade checks with good accuracy, the Compliance team's response was muted. The Senior Compliance Officer eventually said: "the routine checks aren't actually our bottleneck. Our bottleneck is the ambiguous cases where the rules conflict, and those are exactly the cases your system can't handle."

**What we missed**: the VP's framing was what a VP would say. The actual work-shape from an IC perspective was different.

**Rule that emerged**: in discovery, run 1-on-1s with ICs, not just stakeholder roundtables. Stakeholder roundtables miss uncomfortable truths.

### 2. The executive sponsor's actual involvement was marketing theater

The exec sponsor attended the kickoff call. He delegated day-to-day to the VP Compliance.

The VP was enthusiastic but didn't have budget authority. When internal reorganization hit in month 3, the VP's department was flagged for restructuring. Our project sponsor effectively disappeared.

By month 5, the sponsor's successor had other priorities.

**What we missed**: we validated exec sponsor presence once, at discovery. We didn't re-validate when organizational signals shifted.

**Rule that emerged**: exec sponsor check-ins every 4-6 weeks. Missed check-ins are a leading indicator of engagement risk.

### 3. Scope crept through a hundred small accommodations

Each week the VP had "one small request." Build the UI to show trace history. Add a downloadable audit PDF. Extend to the secondary market product line. Each was small. Each was accepted.

By week 10, the team was building 4 separate interface requirements for a pilot that was meant to validate one workflow. Technical debt piled up. The original success criterion (90%+ accuracy on a specific rule subset) was hit, but the artifact felt crowded and slow.

**What we missed**: we didn't track the cumulative cost of small accommodations. Each individually was <2 days; the total was ~3 weeks.

**Rule that emerged**: every "can you also add X?" is a change request. Change requests go to exec sponsor for approval and are tracked cumulatively.

### 4. We built for the pilot, not for production

The MVP worked against synthetic compliance rules and a hand-curated test set. When production data came in, the test set's coverage turned out to be 40% of actual rule variety. Accuracy dropped from the reported 92% to ~71%.

We spent 3 weeks retrofitting. The customer's trust dropped in that window.

**What we missed**: we didn't insist on production-shape data during pilot. The customer's compliance team didn't want us to see live trades (reasonably). We accepted synthetic data.

**Rule that emerged**: pilot data must include at least 100 real production samples, or the pilot is a demo not a pilot. If the customer won't provide, that's a scoping blocker.

### 5. Security review happened in week 13, not week 1

Security review was scoped as "will coordinate with InfoSec as needed." In week 13, after the MVP was in staging, InfoSec raised 12 blocking issues. Some were structural (the LLM ran in a VPC that didn't match the compliance data classification requirements). Fixing these pushed timeline out by 6 weeks.

**What we missed**: we treated security as a downstream gate, not an upstream constraint.

**Rule that emerged**: security contact joins the first or second scoping meeting. Architecture doc gets security review in week 1 of pilot.

### 6. The handoff never happened

By month 6, we were supposed to transition operational ownership to the customer's engineering team. Their team hadn't been included in most of the pilot work; the product owner drove everything through us. When we tried to transition, they said "we don't have capacity to take this on in Q4."

By month 7, we were doing 100% of the operational work for a system the customer wasn't using yet.

**What we missed**: the handover plan was drafted at week 10 but never validated with the receiving team. We assumed capacity that didn't exist.

**Rule that emerged**: handover criteria include named individuals from the customer team who have performed each operational task at least once. Not "the team is available"; named people.

### 7. We never named the failure

By month 7, the writing was on the wall. The pilot wasn't going to ship. But the customer was still paying, and nobody wanted to have "the conversation." We kept delivering increments.

Month 8, the new exec sponsor called a meeting. "This isn't going to ship. Let's close this out."

We should have called the meeting in month 6.

**Rule that emerged**: if 3 of 5 engagement health signals are red for 2 consecutive status cycles, call the conversation. Delaying it costs everyone.

---

## Aggregated lessons

These 7 failures are the reason the runbooks in this playbook have specific gates:

- **Discovery runbook** requires IC-level interviews (rule 1)
- **Exec sponsor check-in cadence** is explicit (rule 2)
- **Change request discipline** is documented in runbooks/02-scoping (rule 3)
- **Production-shape data requirement** is a scoping blocker (rule 4)
- **Security review scheduled at week 1** (rule 5)
- **Named-person handover criteria** (rule 6)
- **Health signals + escalation thresholds** (rule 7)

Every engagement I've done since has hit at least one of these patterns; the runbooks let us catch them earlier.

---

## What the customer got anyway

Not nothing. The MVP was eventually open-sourced internally; two other compliance teams cannibalized parts of it for their own prototypes. The failed engagement wasn't a total loss.

But the customer didn't get what they paid for. That's the thing to avoid next time.

---

*Names, specifics, and timelines above are fabricated to illustrate the failure modes. The failure modes themselves are drawn from debriefs of real engagements (not any single one).*
