# Engagement Anti-Patterns

The 12 mistakes I've seen kill LLM engagements. Composite observations from multiple engagements and published FDE accounts. Each anti-pattern has a catch-it-early indicator and a fix-it recipe.

---

## 1. "We'll figure out the data later"

**Pattern**: Discovery ends without the team having touched the customer's real data. Scoping proceeds based on sales-cycle descriptions. Week 3 of pilot: data is messier than described, scope breaks.

**Catch early**: In week 1, demand a 10-50 document sample. If customer says "next week," escalate. If you haven't held real data in your hands by day 10, the engagement is already drifting.

**Fix**: Don't sign off scope until you've built a 2-day prototype against real data. Non-negotiable.

---

## 2. The invisible technical lead

**Pattern**: Customer has a project owner and an exec sponsor. The "technical lead" is nominal — either doesn't exist, is unavailable, or is hostile to the project. The FDE ends up doing 100% of the engineering without a customer counterpart.

**Catch early**: Day 1 discovery call should include the technical lead. If they're not present and can't join by day 3, raise with project owner.

**Fix**: Either get a real technical lead allocated (minimum 25% FTE; 50% for pilot), or re-scope to something the FDE can carry alone (usually smaller).

---

## 3. Success criteria drift

**Pattern**: Success criteria defined in scoping ("sensitivity ≥0.80 on 50 gold contracts"). By week 4, stakeholders are discussing "well, sensitivity isn't really the right metric" or "that 0.80 number was aspirational, we really want 0.85." The pilot has no stable target.

**Catch early**: In weekly status emails, explicitly state the target and measure. If anyone proposes a new interpretation, document and re-confirm.

**Fix**: Written scope document sign-off captures criteria. Changes require written change request. No verbal amendments accepted.

---

## 4. The single-threaded customer engineer

**Pattern**: All customer-side work flows through one senior engineer. They're brilliant but stretched across 5 projects. Bus factor of 1. They take vacation in week 5; pilot stalls.

**Catch early**: In discovery, ask "if [engineer] is on vacation, who's my backup?" If no backup, flag it. Propose a second engineer (even at 10% FTE) to maintain continuity.

**Fix**: Require bus-factor-of-2 on the customer side as a scoping condition.

---

## 5. The exec sponsor who doesn't show up

**Pattern**: Exec sponsor approved the budget but never attends meetings. Scope creep from mid-level stakeholders goes unchallenged. When misalignment surfaces, there's no top cover.

**Catch early**: If exec sponsor doesn't join the 90-minute discovery kickoff, or hasn't responded to your request for a 30-minute 1:1 by day 5, it's a red flag.

**Fix**: Raise with AE early. Escalate to "this engagement has no executive cover" conversation before pilot starts.

---

## 6. "Just one more thing" mid-pilot

**Pattern**: Week 3 of pilot, project owner asks "can we also add [feature]?" FDE, wanting to say yes, does it. Now the original scope is at risk; pilot slips.

**Catch early**: Every "can we also..." request gets a written trade-off response within 24 hours. What drops out, or how much later, or what's the quality impact.

**Fix**: Discipline. Document the trade-off. Require exec sponsor sign-off for scope additions. Most scope creep evaporates when a trade-off has to be explicitly accepted.

---

## 7. Demo-driven development (the anti-pattern form)

**Pattern**: Team optimizes for the weekly demo. Shipped features look great in demo, but they're brittle, lack tests, can't scale. Week 5 staging deploy exposes the reality.

**Catch early**: In week 2, ask "what's our test coverage? What's the p95 latency?" If the team can't answer, they're in demo mode.

**Fix**: Define engineering quality gates in scope. Test coverage targets. Latency budgets. These aren't "phase 2" — they're entry gates to the demo being considered done.

---

## 8. Security as a week-4 surprise

**Pattern**: Scope says "will comply with customer's security policies." Week 4, security review identifies 15 items blocking deployment. Pilot slips 3+ weeks.

**Catch early**: Scoping meeting has a security contact present. Architecture doc reviewed by security in week 1. Pen-test scheduled in advance.

**Fix**: Make security review an explicit milestone, not an assumed check-in. Schedule it. Communicate it.

---

## 9. Not measuring cost early

**Pattern**: Pilot runs fine at 50 requests/day. Production rollout goes to 50,000. Cost suddenly 1000x. Exec sponsor surprised, budget blown.

**Catch early**: Week 1 of pilot, start measuring cost per call. Extrapolate to production scale in week 2. Share weekly.

**Fix**: Cost forecasting dashboard by week 2. Budget check-in by week 4. Cost caps configured pre-production rollout.

---

## 10. The handoff that wasn't

**Pattern**: Week 20 "handoff" is a meeting. Customer team gets artifacts. Weeks 21-25, FDE is still doing 60% of the operational work, just with "advisory" in the subject line.

**Catch early**: Week 19 capability validation exercises. If customer team can't perform the operations without you, handoff hasn't happened.

**Fix**: Don't sign the handover document until the 5 capability validations have passed. Delay handover if needed; the alternative is drift.

---

## 11. Silent regressions

**Pattern**: A prompt update in week 12 improves one metric but silently degrades another. No regression test suite caught it. Weeks 14-16 spent chasing a quality degradation nobody can pinpoint.

**Catch early**: Eval suite in CI by week 2 of pilot. Runs on every PR. Regression alerts on threshold.

**Fix**: Quality is a product feature, not a retrospective check. Build the eval harness as a first-class artifact, not a side deliverable.

---

## 12. The success criterion that doesn't matter

**Pattern**: Pilot nails "sensitivity 0.84" — exceeds target. Production rollout: users hate it. The metric optimized was not the business metric. Exec sponsor disappointed.

**Catch early**: In scoping, ask "if we hit sensitivity 0.84, does that mean the legal team actually uses this in their daily workflow?" Probe the connection between metric and reality.

**Fix**: Always pair technical metric with business outcome metric. "Sensitivity" alone isn't a business metric; "contracts processed per week with acceptable quality" is.

---

## Meta anti-pattern: the hero FDE

The most dangerous pattern: FDE who, facing any of the above, works 80-hour weeks and covers for customer-side gaps. Short-term: looks like success. Long-term:
- Customer team doesn't build their own capability.
- Engagement ends with hand-off gaps.
- FDE burns out.
- Next engagement inherits unsustainable expectations.

The job is to make the customer succeed, not to personally carry the engagement. Push back. Raise. Escalate.

---

## How to use this list

- **Before every engagement**: read through. Which 3 are most likely to bite you this time? Pre-empt them.
- **Weekly**: during Friday reflection, did any anti-pattern emerge? Act on it this week, not next.
- **Engagement retro**: which anti-patterns showed up? Update this list with specifics from your engagement.

The list grows. Every engagement teaches a variant.
