# Runbook 03 — Pilot Build (Weeks 4-9)

## Objective

Ship the pilot deliverable signed off in scoping, on time, on quality, with the customer's technical lead able to co-maintain it. Maintain weekly momentum, surface issues fast, and end the pilot with an explicit decision.

## Input state

- Signed pilot scope document.
- Customer technical lead allocated at 50%.
- Access to sample data (representative, not full prod).
- Working dev environment (yours + the customer's staging).

## Output state at end of week 9

- Deployed pilot system in customer staging.
- Completed evaluation against success criteria.
- Documentation package (runbook, architecture doc, known limitations).
- Handover prep for the production rollout decision.

## The pilot rhythm

**Daily**: 30-minute sync with the customer technical lead. Same time every day. This is the single most important ritual of the pilot.

**Weekly (Monday)**: sprint planning. Agree on this week's commitments. 45 minutes.

**Weekly (Friday)**: written status + demo of what shipped. Send to exec sponsor, project owner, technical lead. Never miss a Friday update.

**Bi-weekly**: 1-hour review with exec sponsor. Business-level, not technical. Focus: are we on track to success criteria?

## Week-by-week structure (6-week pilot)

### Week 1 (pilot week 1): data + harness

**Goal**: end the week able to run the eval harness on at least 10 representative inputs, end-to-end, even if quality is poor.

**Daily tasks**:
- Day 1: data pipeline working locally on sample. Loads, parses, hands off to processor.
- Day 2: minimal processor — call LLM with a basic prompt, return structured output (Pydantic / Zod validated).
- Day 3: eval harness — accepts (input, expected_output) pairs, scores each, produces summary metrics.
- Day 4: run harness against 10-20 dev samples. Document failure modes.
- Day 5: Friday status. Demo: show the pipeline running on customer data.

**Red flags this week**:
- Data pipeline keeps breaking on format variation. Scope risk; surface it.
- Customer technical lead is unavailable. Escalate to project owner.
- Sample data significantly differs from what was shown in discovery. Scope risk.

### Week 2: iteration + midpoint dataset

**Goal**: ship a 20-sample midpoint evaluation. Don't wait until the end to see the number.

**Key moves**:
- Iterate on prompt / model choice based on week 1 failure analysis.
- Have customer SME label 20 new samples as gold set (NOT from the same pool you've been using).
- Run the eval on the gold set. Write down the number.

**Friday demo**: show the midpoint eval result. If below failure threshold, pause and escalate (back to scoping).

**If midpoint eval is good (above success threshold)**: celebrate, but don't get complacent. Scope may have been too narrow; raise with customer technical lead whether to add a stretch goal.

**If midpoint is in the middle (50-70% of success threshold)**: normal pilot state. Next 2 weeks are the work.

### Week 3: architecture + integration

**Goal**: move from "prompt + LLM + eval harness" to "actual system with infrastructure around it."

Add as scope allows:
- Input validation and rate limiting.
- Output format contract (downstream consumers depend on this).
- Observability (structured logging, basic metrics).
- Error handling for known LLM failure modes (refusals, truncations, malformed JSON).
- Deployment to customer's staging cluster (if scope includes this).

Every add-on is justified against the exit criteria. If it's not needed to pass success criteria, push it to post-pilot.

### Week 4: staging deployment + soak test

**Goal**: system running in customer's staging environment, processing 100+ inputs without human intervention.

Tasks:
- Deploy via customer's standard deployment path.
- Sit with their DevOps/platform team as they do it. Learning moment for you; sanity check for them.
- Run 100-input soak. Measure: latency p50/p95, error rate, cost per call.
- Document what you learn (you'll need it for production-rollout runbook).

**Friday demo**: exec sponsor sees the system running in their environment for the first time. This is a significant milestone — set the agenda accordingly.

### Week 5: final evaluation + polish

**Goal**: hit success criteria on the full evaluation set. Polish UX. Write docs.

Key moves:
- Run final evaluation (e.g., 50-sample gold set from scope). Write down the number.
- If below target: do NOT start over. Specific remediation, documented.
- Document known limitations. What the system doesn't do; when it fails; failure modes to watch.
- Write the handover architecture doc.

### Week 6: handover prep + exit decision

**Goal**: customer team can operate the system without you; exit decision meeting held.

Tasks:
- Handover runbook: how to start, stop, monitor, investigate incidents.
- Training session with customer technical lead (2 hours).
- Exit review with exec sponsor: present results against success criteria, recommend next step.

## Running the daily sync

30 minutes. Same time. Video off, screen shared.

Structure:
1. What I did yesterday (2 min)
2. What you did yesterday (2 min)
3. Blockers (5 min)
4. Plan for today (5 min)
5. Any stakeholder questions (5 min)
6. Pair debug / review (10 min)

**Why daily, not weekly**: LLM engagements have fast failure modes. A prompt that was working Monday can stop working Wednesday because the model silently changed, or the data shifted, or the eval harness had a bug. Daily catches this.

## Running the weekly status

Written, not a meeting. Fridays, before 5pm customer time.

Template: `templates/weekly-status-email.md`.

Structure:
1. This week (what shipped, what didn't, brief explanation if didn't).
2. Demo link or artifact (screenshot, GIF, live URL).
3. Metrics this week (eval score, latency, cost).
4. Next week plan.
5. Risks updated.
6. Asks from the customer.

Cadence rule: **never miss Friday**. A missed status is "the engagement is flailing" — even if it isn't.

## The Tuesday problem

Pilots that ship successfully look like:

```
Week 1: building blocks
Week 2: eval gets worse before it gets better
Week 3: breakthrough
Week 4-5: polishing
Week 6: handover
```

Pilots that stumble look like:

```
Week 1: building blocks
Week 2: some numbers come out but they're not great
Week 3: Tuesday — customer technical lead is in another meeting all day, FDE blocks
Week 4: slipping, multiple issues surfaced at once
```

The signature is "Tuesday problem": customer availability drops, multiple unresolved issues compound, momentum breaks. Catch it by week 2 day 3. Escalate before it compounds.

## Common pilot failure modes

1. **Customer data looked clean in sample, messy in production pipe**. Mitigation: test against the prod pipe at day 3, not day 15.

2. **Stated accuracy target was too loose; stakeholders disagree**. Mitigation: validate target with exec sponsor in week 1.

3. **Model choice is wrong for the domain**. Mitigation: run 2 models in parallel for week 1; pick the one producing better failure modes.

4. **Customer technical lead leaves / goes on vacation / is overloaded**. Mitigation: have a backup technical contact agreed in scoping.

5. **Scope creep from exec sponsor mid-pilot**. Mitigation: every "can we also..." gets a written trade-off response from you within 24 hours. Document as change request.

6. **Eval harness bug artificially inflates numbers**. Mitigation: have customer SME spot-check 5 eval results by hand in week 2.

7. **Security blocks deployment in week 4**. Mitigation: resolve security questions in scoping; re-confirm in week 1.

8. **Cost model exceeds budget at production scale**. Mitigation: track cost per call from day 1; extrapolate to production scale in week 2.

## What "pilot went well" looks like

End of pilot:
- Success criteria hit or exceeded.
- Failure modes documented explicitly.
- Customer technical lead independently demoed the system to their peers.
- Exec sponsor signed off on production rollout.
- You have the production-rollout runbook ready to start.

## What "pilot failed" looks like (and that's OK)

End of pilot:
- Success criteria not hit.
- Explicit documentation of why (model limitations, data issues, scope issues).
- Customer understands it wasn't a team failure, it was a learning.
- Decision: either pivot scope and extend, or conclude.

Failed pilots that are handled well produce strong references. Failed pilots that are hidden produce churn.

## Artifact checklist for end of pilot

- [ ] Deployed system in staging
- [ ] Evaluation report against success criteria
- [ ] Architecture decision record
- [ ] Runbook (operation, monitoring, incident response)
- [ ] Known limitations doc
- [ ] Handover training session held
- [ ] Exit review meeting held with exec sponsor
- [ ] Production rollout decision documented
- [ ] If proceeding: production rollout scope draft
- [ ] If not proceeding: post-mortem document

## Related

- Previous runbook: [02-scoping.md](02-scoping.md)
- Next runbook: [04-production-rollout.md](04-production-rollout.md)
- Template: `templates/weekly-status-email.md`
- Anti-patterns: [docs/engagement-anti-patterns.md](../docs/engagement-anti-patterns.md)
