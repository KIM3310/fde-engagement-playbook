# Runbook 02 — Scoping (Week 3)

## Objective

Convert the discovery output into a written pilot scope that (a) the customer's exec sponsor agrees to, (b) the customer's technical lead believes is deliverable, and (c) you can actually ship in 4-6 weeks with the team and data available.

## Input state

- Discovery output pack (from runbook 01).
- Prototype findings.
- Trade-off conversation notes.
- Stakeholder alignment on the problem.

## Output state

A written pilot scope document:

- Problem statement (1 paragraph).
- Pilot deliverable (1 paragraph, specific).
- Success criteria (measurable).
- Failure criteria (measurable).
- Scope exclusions (explicit list).
- Timeline (week-by-week).
- Resource asks (customer side + vendor side).
- Risks and mitigations.
- Exit criteria (pilot → production decision).

Signed off by: exec sponsor, project owner, technical lead.

## The scope negotiation framework

Three axes to negotiate:

```
Scope  ←——————————————→  Time
           \\
            \\
             Quality
```

You can move two, never all three. In scoping, you make explicit which two.

**Default for LLM pilots**: fix quality (success criteria), fix scope (specific deliverable), let time flex between 4-8 weeks.

Common mistake: fixing time (customer demands 4-week pilot), fixing scope (customer wants the world), and assuming quality will work out. It won't.

## Writing the pilot scope

### Step 1: The one-sentence deliverable

Can you describe what you'll hand over at end of pilot in one sentence? If not, the scope is too fuzzy. Examples:

**Too fuzzy**: "An AI assistant for the legal team."

**Scoped well**: "A web service that accepts a contract PDF and returns a structured summary with confidence-scored risk flags, deployed to the customer's staging environment and tested against 50 gold-set contracts."

Notice what the second version has:
- Specific input and output format
- Specific environment (staging, not production)
- Specific evaluation (50 gold-set contracts, not "all of our data")
- Implied quality gate (confidence-scored means there's a threshold)

### Step 2: Success and failure criteria

**Success criterion** must be:
- Measurable
- Agreed by both sides before work starts
- Achievable by most-likely engineering path (not only the best-case)

Example: "For a held-out set of 50 contracts labeled by the customer's legal team, the system identifies at least 80% of flagged risk clauses with precision ≥ 0.85."

**Failure criterion** must be:
- Equally measurable
- Defines when the engagement explicitly stops, not just "disappointing"

Example: "If on a 20-contract midpoint evaluation, recall is below 0.50, the pilot pauses for re-scoping rather than proceeding to full 50-contract evaluation."

Failure criteria protect both sides. They prevent the "death march" pattern where a pilot limps on past the point it's already failed.

### Step 3: Scope exclusions

List what you're NOT doing. This prevents scope creep mid-pilot. Typical exclusions:

- Production deployment (staging only)
- Integration with the customer's auth/identity provider
- Multi-language support (English only)
- Mobile UI
- Historical data backfill
- Migration from the existing system

Every exclusion you list upfront is a conversation you don't have to have in week 5 when the customer expects something you didn't build.

### Step 4: Resource asks

A pilot fails silently when the customer side can't allocate time. Be explicit:

**Customer commits**:
- Technical lead: 50% FTE for 6 weeks
- Data engineer: 20% FTE for first 2 weeks (data pipeline)
- Subject matter expert (SME): 4 hours/week for gold-set labeling
- Product owner: 2 hours/week for weekly status
- Security contact: available within 48 hours for blocking questions

**Vendor commits**:
- FDE (you): 100% FTE for 6 weeks
- Support from DevRel / product team: async, response within 24 hours
- Access to engineering escalation path for product bugs

If the customer can't commit the technical lead's 50%, raise it as a pilot risk and either: re-scope smaller, push timeline out, or escalate to exec sponsor.

### Step 5: Weekly timeline

Week-by-week breakdown. Typical 6-week pilot:

- **Week 1**: data pipeline + initial prompt iteration
- **Week 2**: core component built + eval harness in place
- **Week 3**: iteration to hit midpoint eval target
- **Week 4**: full system integrated + staging deployment
- **Week 5**: final evaluation + UX polish + documentation
- **Week 6**: handover prep + exit review + decision meeting

Each week has a written Friday deliverable. Don't commit to daily milestones — too brittle.

### Step 6: Risk register

5-8 risks. Format:

```markdown
### Risk: Data format drift in production documents

**Impact**: High. Pilot accuracy measured on sample may not reflect prod distribution.
**Likelihood**: Medium.
**Mitigation**: Before locking scope, request 3 batches of 20 documents each from different months. If format variance is visible, add "format normalization" as scope item.
**Owner**: FDE (you) with customer data engineer.
**Escalation trigger**: Week 2 if >20% of documents fail parse.
```

Customers respect scoping that names risks. They lose faith in scoping that pretends risks don't exist.

### Step 7: Exit criteria for pilot → production

What decision do you want at end of pilot? Options:

- **Proceed to production rollout** (default success path)
- **Extend pilot by 2 weeks** (if we're close but not there)
- **Pivot scope** (if we learned something requiring re-scoping)
- **Conclude without production** (if the pilot proved the approach isn't right)

Each exit path has named criteria. The decision is made by exec sponsor in a 60-minute exit review meeting on the Monday after pilot week 6.

## The scoping negotiation meeting

One 2-hour meeting. Attendees: exec sponsor, project owner, technical lead, security lead (if data residency is involved), you, AE (for context but stays quiet).

Agenda:

1. Walk through scope document section by section (45 min).
2. Negotiate. Common moments:
   - "Can we add [feature]?" — negotiate: what drops out, or does time extend?
   - "6 weeks is too long / too short." — negotiate: what changes?
   - "We need production deployment by end of pilot." — negotiate: redefine what staging vs production means, or extend scope post-pilot.
3. Risk walkthrough (15 min).
4. Resource commitment confirmation (15 min).
5. Sign-off (5 min).

If sign-off doesn't happen in this meeting, follow up within 48 hours with revised scope. If sign-off still doesn't happen after revision, escalate to AE: the engagement has a fundamental misalignment.

## Anti-patterns

### Scope creep during scoping

Customer asks for "just one more thing" during scoping. Common "one more things":
- Multi-language support
- A second user persona
- An integration that sounds simple but isn't

Resist. Every "one more thing" either extends timeline or reduces quality. If you must accept, explicitly trade something else out.

### Under-specified success criteria

"We want it to be accurate" is not a criterion. Push back: "Accurate how? What's the dataset? What's the metric? What number?"

If the customer can't answer, it means they don't yet know what they want. That's a discovery-phase issue; go back to runbook 01.

### Skipping the security conversation

Security often gets a paragraph in scope: "will comply with customer's security policies." This is a landmine. Week 4, security will come back with 20 questions that block deployment.

Pull security into the scoping meeting. Get written sign-off on: data handling, authentication, audit logging, deployment environment.

### Vendor team over-committing

FDEs, especially new ones, want to say yes. Over-committing during scoping is how engagements become death marches. Allow yourself to say: "I can't do that in 6 weeks. Here's what I can do in 6 weeks that solves 80% of the value."

## Scope document template

See [templates/pilot-proposal.md](../templates/pilot-proposal.md) for the canonical template.

Length target: 4-6 pages. Long enough to be unambiguous; short enough the exec will read it.

## What "scoped correctly" looks like by end of week 3

- Scope document signed by exec sponsor.
- Technical lead has ack'd their 50% FTE and has a calendar hold for pilot weeks.
- Data pipeline is agreed and week 1 has a clear first sprint task.
- You have internal vendor approval that this scope matches your team's commit.
- You have mentally simulated pilot week-by-week and believe the plan is feasible.

If any of these five is missing, do not start pilot. The cost of an extra week of scoping is far less than the cost of a pilot launched on an unstable scope.

## Related

- Previous runbook: [01-discovery.md](01-discovery.md)
- Next runbook: [03-pilot-build.md](03-pilot-build.md)
- Template: `templates/pilot-proposal.md`
- Anti-patterns: [docs/engagement-anti-patterns.md](../docs/engagement-anti-patterns.md)
