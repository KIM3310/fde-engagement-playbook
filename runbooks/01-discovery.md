# Runbook 01 — Discovery (Weeks 1-2)

## Objective

Understand the customer's actual problem, their constraints, their team, and their success criteria — deeply enough that the pilot you propose has a strong probability of measurable success within 4-8 weeks.

## Input state

- Account Executive has closed the engagement. Contract is signed or about to be.
- You have: customer contact (usually the VP of Data/Engineering or a Director), the Account Executive's notes from sales cycle, a 1-page problem statement.
- You have NOT: actually talked to the customer's engineers, seen their data, read their architecture diagrams, or validated that the stated problem is the real problem.

## Output state

By end of week 2, you should have:

1. A written problem statement that the customer's VP and their senior engineer both agree on.
2. A technical landscape diagram showing data sources, current systems, and the proposed LLM component.
3. A list of the customer's top 3 success criteria, ranked.
4. A list of red flags or concerns, with mitigation or "walk away" recommendation.
5. A proposed pilot scope (scoping runbook is the next step).

## Week 1 — Relationship + context

### Day 1: Kickoff meeting (with AE)

Agenda: **90 minutes**. AE introduces you; you introduce the engagement plan; customer shares their perspective. Use `templates/discovery-call-agenda.md`.

Critical moves:

- **Pause when the customer uses the word "just"**. "We just need a chatbot" or "we just need to classify these documents" always hides complexity. Ask: "when you say 'just', what are the three things you'd expect to not have to think about?"

- **Ask who will use this and when it was last reviewed**. A stated success metric that was defined a year ago and hasn't been revisited is usually wrong.

- **Explicitly ask "what have you already tried?"** If they've tried GPT-4 via the OpenAI API directly and it didn't work, that's critical context — you need to understand why.

- **Don't commit to a pilot scope in this meeting**. Your job is to listen. Propose scoping in week 2.

### Day 2-3: Stakeholder mapping

Schedule 30-45 minute 1:1s with every named stakeholder:
- Executive sponsor (the person who approved the budget)
- Project owner (the day-to-day decision maker)
- Technical lead (the engineer who will work alongside you)
- End user representative (the person who will actually use the output)
- Security / compliance contact (often a SecOps or GRC lead)

For each, use this 3-question frame:

1. "From your seat, what does success look like for this in 6 months?"
2. "What's the thing that could cause this to fail that you're most worried about?"
3. "Who else should I talk to?"

The third question usually surfaces a stakeholder the AE didn't flag.

### Day 4: Technical discovery session

Two-hour session with the technical lead. Use `templates/technical-discovery-questionnaire.md`. Topics:

- Current data landscape: sources, formats, sizes, refresh rates, governance state.
- Current architecture: what systems touch this workflow today, what changes.
- Infrastructure: cloud vendor, K8s maturity, observability stack, deployment process.
- Security: data residency requirements, auth model, audit expectations.
- Team: who will be on this, what their availability is, what's competing for their time.

End with: "if we don't ship this in 6 months, what happens?"

### Day 5: Synthesis + reflection

Write up:

- One-page problem statement (half business context, half technical context).
- Technical landscape diagram (mermaid or whatever tool you use — this is your working artifact).
- Stakeholder map with decision rights for each.
- Open questions list.

## Week 2 — Validation + depth

### Day 6-7: Data diving

Request access to a representative sample of the customer's data. Signs you can't shortcut this:

- "We'll send it to you next week" — this is usually 2-3 weeks. Follow up daily.
- "We have 100,000 documents but they're all similar" — no, they're not. Request a stratified sample.
- "The data is clean" — it isn't. Budget a day to clean it yourself before demo building.

Red flags while reviewing data:

- Formats drift significantly between documents. Suggests upstream systems changed without a schema lock.
- PII is present and anonymization hasn't been designed. Scope risk.
- Documents reference other documents the customer didn't send. Missing context.
- Labels (if supervised) are inconsistent. Quality gate needed.

### Day 8-9: Prototype spike

Build something in 2 days that processes 10-50 real customer documents with a real LLM. Not production-grade. Throw-away quality. Purpose: you learn the failure modes before scoping the pilot.

What you will discover:

- The "simple" task isn't simple at scale of formats you haven't seen yet.
- Latency assumptions were wrong by 5x.
- Token cost at full scale exceeds what was implied in sales.
- The stated accuracy target is impossible without fine-tuning, or it's trivially exceeded by off-the-shelf.

Report these findings to the customer's technical lead on day 9 or 10. Do NOT report them in a formal deliverable — raise them informally first. You want the technical lead on your side about what's possible before the executive sponsor sees a scope.

### Day 10: Trade-off conversation

60 minutes with project owner and technical lead. Walk through:

- What you observed in the data.
- What the prototype showed (warts and all).
- Three to five trade-offs the customer will need to decide (quality vs cost, speed vs depth, automation vs human-in-the-loop).

This conversation is where "the customer's stated problem" converges with "the customer's actual problem." Document the decisions.

### Day 11-12: Scoping preparation

Use the findings to draft the pilot proposal. This is the scoping runbook's input. Key decisions to land in the proposal:

- **What specifically will we build in 4-6 weeks?** Not a roadmap; a single deliverable.
- **What won't we build?** Explicit scope exclusions.
- **What does "pilot succeeded" look like?** Quantitative criterion that the executive sponsor signs off on.
- **What does "pilot failed" look like?** Equally important. An undefined failure state is how engagements drag on.

### Day 13-14: Recap + go/no-go

Wrap discovery with a recap meeting: exec sponsor + project owner + technical lead. Deliver:

- Problem statement, validated.
- Landscape diagram.
- Success criteria ranked.
- Risks and mitigations.
- Proposed pilot scope + timeline + team ask.

Explicitly ask: **"given what I've shared, do you want to proceed to pilot?"** A no here — or even a hesitation — is a signal. Better to re-scope in discovery than to launch a doomed pilot.

## Red flags that may indicate "walk away"

Not every engagement should proceed. FDEs who miss walk-away signals produce painful quarters. Walk away signals:

1. **No executive sponsor available to meet**. If the VP won't spend 30 minutes with you in week 1, the engagement has no top cover.
2. **Technical lead is not in the meeting and not reachable**. Either they don't exist, or they don't know about this project, or they're hostile to it.
3. **Customer can't produce sample data within 10 days**. Data flow is broken; the pilot won't have what it needs.
4. **Customer's success criterion is "we want it to be like ChatGPT"**. Unbounded. Escalate to AE for re-scoping.
5. **Budget was approved but team allocation wasn't**. You'll be asking the customer's senior engineer to moonlight. They won't.
6. **Security / compliance is a blocking question marked "TBD"**. Can burn 6 weeks before you can touch real data.
7. **Project is politically contested inside the customer's org**. Another team is building the same thing, or the project owner is 2 levels below the exec sponsor and constantly overruled.

If you see 3 or more, raise with AE on day 5. If AE wants to proceed anyway, document your concerns in writing. Your honesty protects the customer and protects your reputation.

## Discovery output: the 5-document pack

By end of week 2, you have:

1. `discovery-summary.md` — 2-page narrative of what you learned
2. `problem-statement-v2.md` — validated, signed off by project owner
3. `technical-landscape.png` — architecture diagram
4. `stakeholder-map.md` — RACI-lite for the engagement
5. `pilot-scope-proposal.md` — draft pilot scope (input to week 3)

Store these in a shared workspace with the customer (Google Drive, Confluence, SharePoint). They are the discovery artifact; they also become reference documents throughout the engagement.

## Time budget for discovery phase

- Meetings: 12-15 hours across 2 weeks
- Data exploration: 8-10 hours
- Prototype spike: 16 hours
- Writing + synthesis: 10-12 hours
- Your total: 50-65 hours across 2 weeks, ~60% customer-facing

If you finish discovery with significant time buffer, you went too fast. If you significantly overrun, you're heading into a pilot with problems you haven't fully characterized — slow down before committing to scope.

## What to NOT do in discovery

1. Do NOT start writing production code. Your prototypes are exploratory; they're disposable.
2. Do NOT commit to specific technical choices (model, vector DB, deployment target) before week 2 day 11.
3. Do NOT present a full roadmap. Pilot first; roadmap is month 3.
4. Do NOT negotiate contract terms (that's AE's job).
5. Do NOT promise anything that requires approval outside the room you're in.

## Related

- Next runbook: [02-scoping.md](02-scoping.md)
- Templates: `templates/discovery-call-agenda.md`, `templates/technical-discovery-questionnaire.md`
- Anti-patterns: [docs/engagement-anti-patterns.md](../docs/engagement-anti-patterns.md)
