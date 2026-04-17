# Weekly Status Email — Template

**When**: Every Friday, before 5pm customer time. No exceptions. Missed Friday = engagement flailing signal.

**To**: Project owner (primary), Exec sponsor, Technical lead.

**Length target**: 400-600 words. Scannable in 90 seconds.

---

## Template

```
Subject: [Engagement Name] — Week N Status ([pilot / rollout / handoff phase])

Hi team,

Week N recap below. Full artifacts in [shared workspace].

## This week (shipped)

- [Concrete deliverable 1 with link or artifact]
- [Concrete deliverable 2]
- [Concrete deliverable 3]

## Not shipped (from last week's plan)

- [Item that was planned but didn't land] — [1-line explanation and new ETA]

## Metrics this week

- Eval score (primary metric): [value] vs target [target]
- Cost per call: $[value] (week average)
- Latency p95: [value] ms
- [other relevant metric]

## Demo / artifact

[Screenshot, GIF, or live URL showing what got built this week. 1 image is worth 1000 words.]

## Risks updated

- [Risk from register]: [status change, if any]
- [New risk if any]

## Next week plan

- [Commitment 1]
- [Commitment 2]
- [Commitment 3]

## Asks from you

- [Specific thing you need from customer side]
- [Specific thing you need from customer side]

Happy to hop on a call anytime. Next scheduled touchpoint: [Monday sprint planning, date].

[Your name]
```

---

## Example filled-in

```
Subject: Acme Contract Pilot — Week 3 Status

Hi team,

Week 3 recap below. Full artifacts in the shared Drive (Engagement Folder).

## This week (shipped)

- Midpoint evaluation on 20-contract gold set: sensitivity 0.78 at specificity 0.92 (screenshot attached)
- Added prompt-caching; token cost dropped 42% on repeat-doc workflows
- Staging cluster deployment runbook published to Engagement Folder

## Not shipped (from last week's plan)

- User-facing error messages polish — deferred to week 4. Lower priority than midpoint eval; no impact on success criteria.

## Metrics this week

- Sensitivity (primary): 0.78 (target 0.80; on track)
- Specificity: 0.92 (target ≥0.90; ✓)
- Cost per contract: $0.067 (target <$0.10; ✓)
- Latency p95: 4.2s (target <5s; ✓)

## Demo / artifact

[Screenshot of 3-contract side-by-side: extracted clauses + risk flags + confidence scores]

## Risks updated

- Format drift risk: partially mitigated. We saw format variance in batch 2 (60 new contracts) — added a normalization step; no regression in eval.
- New risk: customer legal team has been stretched this week; gold-set labeling for week 5 at risk of delay. Proposed mitigation: ship week 4's eval on current gold set; extend labeling into week 5 in parallel.

## Next week plan

- Close remaining 2 points on sensitivity via prompt iteration
- Integrate customer auth (OIDC flow to Okta tenant)
- Run 100-contract soak in staging

## Asks from you

- Sandra: can you unblock access to the Okta tenant by Tuesday? Asked in Slack Friday; need engineering escalation if no movement by Monday.
- Mark: can legal team commit 3 hours on Wednesday for week 5 labeling session? I'll block the time on your calendar pending confirmation.

Happy to hop on a call anytime. Next scheduled touchpoint: Monday sprint planning 9am PT.

Doeon
```

---

## Writing rules

1. **Lead with shipped, not plans**. Customers want evidence, not intentions.
2. **Include a visual.** Every status email has at least one screenshot, GIF, or live URL.
3. **Name the metric explicitly.** "Good progress on accuracy" is useless; "sensitivity 0.78 vs target 0.80" is scannable.
4. **Be honest about misses.** Hiding misses destroys trust. Reporting them openly builds it.
5. **Always include asks.** If you have zero asks, you're doing all the work yourself; the customer won't own it.
6. **Short is better than comprehensive.** 400 words read; 1400 words skipped.
7. **Same format every week.** Customers learn to scan it; muscle memory helps.

## Adjustments by phase

### During pilot

Focus metrics: eval score, cost per call, latency. Demo: feature shipped this week.

### During production rollout

Focus metrics: SLO burn, traffic shifted to prod, incident count. Demo: dashboard screenshot, canary rollout state.

### During handoff

Focus metrics: capability validations passed, artifacts ready. Demo: customer team performing a handover exercise.

### During post-engagement advisory

Cadence shifts to bi-weekly or monthly. Focus: customer team's own metrics; your role is advisory.

## When the report looks bad

Weeks happen where metrics regressed, a deadline slipped, a stakeholder got upset. Report them. Don't hide.

Bad-news status email structure:

1. **Lead with the bad news.** Don't bury it.
2. **State the impact clearly.**
3. **State what changed.**
4. **State what you're doing about it.**
5. **Ask for what you need.**

Example:

```
## This week (shipped, and one significant miss)

- Week 4 soak test failed at 60% completion — 7% of inputs timed out at LLM provider rate limit.
- Impact: can't validate production readiness until resolved.
- Root cause: provider's per-minute rate limit is 3x tighter than our projected peak; not caught in pilot because pilot traffic didn't hit the cap.
- Remediation: moving to a multi-provider fallback pattern (Anthropic primary, OpenAI fallback). Adds 2-3 days of work to week 5.
- Impact on schedule: week 6 exit review is at risk of sliding to early week 7. Flagging now so we can confirm at Monday's sync.

Ask: can you approve the spend for a provider account upgrade to get higher rate limits as a short-term bridge? Estimated $800 for the 2-week pilot window.
```

Customers respect this. They don't respect surprise bad news delivered at the exit review.
