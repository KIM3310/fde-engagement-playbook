# Discovery Call Agenda — 90 minutes

**Meeting context**: First meeting after engagement contract is signed. Attendees: customer exec sponsor (20-30 min), customer project owner (full meeting), customer technical lead (full meeting), you as FDE, AE (for continuity, minimal speaking).

**Preparation**:
- Read AE notes from sales cycle.
- Research the customer: recent news, tech blog, public talks from their team.
- Prepare 5 specific questions referencing what you learned. Shows you did the work.

---

## Agenda

### 0:00-0:10 — Introductions

Introduce yourself briefly (who, background relevant to this engagement, 90 seconds max). Invite customer to introduce their team.

> "Thanks for the time. I'm [name]. Before today I was [role]. I'll be the FDE embedded with your team for the next 6 months. For today's call my goal is to listen more than I talk; I want to understand what you're hoping we build together."

### 0:10-0:30 — Customer's problem statement (customer leads)

Invite customer to walk through:
- What's the problem they're solving?
- Why now?
- What have they tried?
- Who's affected?

Your role: ask clarifying questions, not leading ones. Do not propose solutions. If you find yourself wanting to, pause and note it; raise it in week 2 instead.

**Useful probing questions**:
- "When did this become a priority for your team?"
- "Who's feeling this pain most acutely today?"
- "If this was solved, what would change for [named stakeholder]?"
- "What have you tried that didn't work? What was the specific failure mode?"

### 0:30-0:50 — Exec sponsor segment

Exec sponsor typically joins this segment. Focused on business context.

**Questions for exec sponsor**:
1. "From your seat, what does success look like in 6 months?"
2. "What would 'this project went poorly' look like?"
3. "What other initiatives are this team juggling right now?"
4. "Who's the internal skeptic of this project? Not naming names; just the role."
5. "If we delivered what we scoped, what would you do differently with it in year 2?"

The fifth question often surfaces future ambition that shapes the pilot scope.

Exec sponsor leaves after this segment (respects their time).

### 0:50-1:10 — Technical deep dive

Now the technical lead is the primary voice.

**Questions for technical lead**:
- "Walk me through your current system end-to-end — what happens when [typical workflow] runs?"
- "Where's the data? What's the format? How often does it refresh?"
- "What's your deployment model? K8s? Serverless? Which cloud?"
- "What's your on-call rotation look like? Who'd get paged if a new system we build fails?"
- "What's your biggest technical constraint nobody's mentioned yet?"

Open-text final question often gets honest answers: "we're under-resourced on security," "our warehouse is on an end-of-life version," "our team hasn't worked with production ML before."

### 1:10-1:25 — Engagement structure + next steps

You drive this segment.

Walk through:
- The 6-phase engagement (discovery → scoping → pilot → rollout → handoff → post).
- What the next 2 weeks look like (discovery).
- What you'll need from the customer team in the next 10 days:
  - Introductions to remaining stakeholders.
  - Access to sample data (representative, ~50 items).
  - 3 x 1-hour sessions with technical lead across next 2 weeks.
  - Access to their code repos (read-only is fine).

**Commit to**: a written discovery summary at end of week 2, followed by scoping session in week 3.

### 1:25-1:30 — Open floor

"Any questions from your side I haven't addressed?"

Note anything that comes up; it usually reveals what's actually on the customer's mind.

---

## After the meeting

Within 4 hours of the call end:

1. **Write up discovery notes** — key points from each segment.
2. **Email recap to attendees** — the meeting summary + your 3-5 specific next actions.
3. **Schedule the stakeholder 1:1s** for days 2-3.
4. **Flag any red flags in your internal notes** (engagement-anti-patterns.md list).

Email template:

```
Subject: [Engagement Name] Discovery Call Recap + Next Steps

Thank you all for the time. Here's what I captured:

**Problem statement** (our working draft, to be validated):
[1-paragraph version of what they described]

**Success criteria** (what exec sponsor said):
- [item 1]
- [item 2]
- [item 3]

**Top technical factors**:
- [item]
- [item]

**Next 10 days**:
- Thu [date]: 1:1 with [project owner] — 45 min
- Fri [date]: 1:1 with [technical lead] — 45 min
- Mon [date]: technical discovery session — 2 hours
- Tue [date]: access to sample data
- End of week 2: written discovery summary

**My first asks**:
- Access to [specific system/repo]
- Introduction to [specific stakeholder mentioned]
- [data request]

Please let me know if I missed anything or if next steps need adjustment.

Best,
[Your name]
```

---

## Red flags to note during the call

If any of these occur, note in internal doc (not in recap email):

- [ ] Exec sponsor didn't join or left within 15 minutes.
- [ ] Technical lead not present.
- [ ] No clear project owner ("we're all sort of responsible").
- [ ] Budget was approved but no team was allocated.
- [ ] Sales promised something outside what you can deliver.
- [ ] Stated success criteria are vague ("we want it to work").
- [ ] Customer can't articulate what they've tried.
- [ ] Customer expects you to drive; can't commit their own team's time.
- [ ] Political tension visible in the call (interruptions, disagreement among customer team).

Any 3+ of these = elevated engagement risk. Raise with AE before week 2.

---

## What this call accomplishes

After a well-run discovery call:

1. You have the customer's own words on what the problem is (not AE's paraphrase).
2. You have the exec sponsor's success criteria (their words).
3. You have the technical lead's view of constraints (their words).
4. You have names and roles of 3-5 additional stakeholders to meet.
5. Customer has a sense of cadence and expectations for discovery.
6. Customer trusts that you know what you're doing.

If any of those is missing after the call, find a way to get it in days 2-3 (via the 1:1s).
