# Runbook 06 — Post-Engagement (Weeks 21-26)

## Objective

Support the customer through initial post-handoff operation, capture success signals, identify expansion opportunities, and formally close the engagement with measurable outcomes documented.

## The two goals of post-engagement

1. **Customer success**: they succeed operationally after you're gone.
2. **Our success**: capture references, identify expansion, document learnings for the next engagement.

These are compatible. Post-engagement is not a sales motion; it's a verification phase for both sides.

## Week-by-week

### Weeks 21-24: advisory cadence

Weekly 1-hour calls. Customer drives agenda.

Topical patterns that emerge:
- "We're seeing X in production — normal or concerning?" (diagnostic)
- "We want to add Y feature — how would you approach it?" (advisory)
- "A user reported Z — is this a bug in the LLM or in our integration?" (debugging)
- "Finance is questioning the cost — help us explain it" (stakeholder support)

Rules for these calls:
- No new work. You're advising, not building.
- Problems → customer owns the fix, you suggest approaches.
- Every call: customer writes notes; you don't.

### Week 25-26: success review preparation

**Week 25**:
- Pull 30+ days of production metrics.
- Compare against success criteria from scope.
- Draft success review narrative.
- Gather 2-3 short testimonials from customer team (end user, technical lead, exec sponsor if possible).

**Week 26**:
- Final success review meeting.
- Document any open items.
- Hand off remaining artifacts.
- Formal engagement close.

## The success review meeting

Final meeting of the engagement. Attendees: exec sponsor, project owner, technical lead, customer engineering manager, you, AE (re-joins for continuity).

Duration: 90 minutes.

Agenda:

1. **The numbers** (30 min): side-by-side of pilot success criteria vs production performance. Honest, including where goals shifted.
2. **The narrative** (20 min): what got built; what got learned; what the team can do now that they couldn't before.
3. **Open issues** (10 min): known bugs, deferred work, technical debt.
4. **What's next** (20 min): what the customer plans to do (independent of you); what expansion opportunities exist (future engagements).
5. **Thank-you round** (10 min): each party names what worked well.

## Metrics that matter for success review

Not every engagement produces the same metrics. Common:

**Business metrics**:
- Workflow time reduction: "from X hours per day to Y hours per day."
- Coverage expansion: "able to handle N% more cases than the old system."
- Error reduction: "downgrade rate from X% to Y%."
- Revenue impact: "N% lift in [metric customer cares about]."

**Operational metrics**:
- SLO attainment over 30+ days.
- Incident count and severity distribution.
- Cost per transaction vs forecast.
- Time-to-deploy a prompt change.

**Team metrics**:
- Customer engineer confidence (self-reported survey).
- Number of team members who can operate the system.
- Time from new-joiner to productive (if applicable).

Pick 3-5 that tell the story; don't drown in metrics.

## Capturing references

With exec sponsor's permission:

- **Written testimonial** (1 paragraph, for AE's use in sales cycle).
- **Case study opt-in** (2-page write-up; customer co-approves).
- **Reference calls** (customer speaks to 1-2 future prospects per quarter for 6-12 months).

Rule: never ask in the success review meeting. Ask in a 1-on-1 with exec sponsor 1 week later. Gives them space to say yes or no.

## Expansion identification

What are signals that this customer wants more?

- Exec sponsor mentions a second use case by name in the success review.
- Technical lead asks "what would you do if we wanted to..."
- End users are requesting features that imply expansion (wider deployment, adjacent workflows).
- Customer procurement team asks about pricing for larger deployments.

What are signals that this customer does NOT want more?

- Exec sponsor is silent on future plans.
- Technical lead is asking about off-boarding / self-support resources.
- Customer team is re-orging or downsizing.
- Budget cycle makes expansion infeasible.

Both signals are valid. Don't force expansion; it damages the relationship.

If expansion is real: AE takes the hand-off. You're now internal expert; they run the motion.

## Documenting learnings for future engagements

Even the best engagements have lessons. Maintain a private `learnings.md` that feeds back into this playbook. Patterns:

- **Things the scope underestimated**: what took longer than expected; why?
- **Things the scope over-estimated**: what turned out to be easy?
- **Anti-patterns caught late**: what should discovery have uncovered?
- **Customer-side patterns**: what kind of team dynamics work best? Which ones don't?
- **Technical patterns**: what architectural choice worked; what you'd redo?

Every 3-5 engagements, update the playbook based on these learnings. This is how the playbook gets better.

## Red flags during post-engagement

1. **Customer calls you at 3am on day 25 with a production issue**. They thought they were ready; they weren't. Work the incident; then immediately debrief: what in handoff (week 17-20) missed this?

2. **Exec sponsor goes quiet in week 23**. Something changed on their side — org change, priority shift, or dissatisfaction. Check in directly.

3. **System performance degrades over 30 days**. Drift is real. Did week 19's monitoring setup actually catch it? If not, there's a gap to fix.

4. **Customer team asks "can we extend the engagement?"**. Nuance matters:
   - If it's because they aren't ready to operate: handoff was premature. Fix now.
   - If it's because they have new scope: transition to a new engagement with new scope document. Don't let the old engagement drift.

5. **Cost spikes unexpectedly**. Usually usage change on customer side, not system bug. Diagnose jointly.

## The close-out packet

At end of week 26, deliver:

- [ ] Success review recording or minutes.
- [ ] Success metrics report.
- [ ] Open issues list (handed to customer team lead).
- [ ] Case study draft (if opt-in).
- [ ] Reference contact list (if opt-in).
- [ ] Expansion opportunities (if applicable) → handed to AE.
- [ ] Engagement learnings doc (internal, to vendor's FDE team).

## After the engagement ends

Normal cadence:
- Month 7-9: quiet. Monitor customer health via AE.
- Month 10: informal check-in (email or 30-min call). Is the system still running well?
- Month 12: formal annual check-in. Useful for case study refresh and reference renewal.

If the customer has an incident 6 months in: you respond (bounded, 2-hour cap, via AE not directly). Reputation matters more than engagement boundaries.

## What "engagement closed well" looks like

- Success criteria met.
- Customer operates independently for 30+ days.
- Exec sponsor gives unsolicited positive feedback.
- Customer engineering lead proactively shares a system improvement they made.
- You leave the engagement feeling the team will be fine without you.

## What "engagement closed poorly" looks like

- Success criteria partially met; ambiguous.
- Customer team still dependent on you 30 days in.
- Exec sponsor hedging in final review.
- Open issues list is long and not clearly prioritized.
- You leave feeling anxious about the team's ability to maintain.

Closed-poorly engagements produce churn. Post-engagement is when you fix them — not after churn has happened.

## Related

- Previous runbook: [05-operational-handoff.md](05-operational-handoff.md)
- Templates: `templates/executive-success-review.md`, `templates/engagement-close-checklist.md` (implicit)
- Anti-patterns: [docs/engagement-anti-patterns.md](../docs/engagement-anti-patterns.md)
