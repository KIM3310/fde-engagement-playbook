# Executive Success Review — Template

**When**: Week 26 (month 6). Final formal meeting of the engagement.

**Duration**: 90 minutes.

**Attendees**: Exec sponsor, project owner, technical lead, customer engineering manager, FDE (you), AE (re-joins).

**Pre-read**: Sent 24 hours before the meeting. Should stand alone — attendees can read and skip the meeting if they want.

---

## Executive Success Review — [Engagement Name]

**Date**: [meeting date]
**Engagement start**: [start date]
**Engagement end**: [end date]
**Duration**: 26 weeks
**Primary deliverable**: [pilot deliverable one-sentence, restated]

---

## 1. Summary

[1 paragraph. The 30-second answer.]

Example: "The contract-risk analysis service was delivered to production in week 16, met all success criteria with sensitivity of 0.84 (target: 0.80), and has operated with customer ownership for 10 weeks. Key business outcomes: legal team's contract review time reduced from 4.2 to 2.1 hours per contract (50%); team capacity for high-value work increased by an estimated 45 hours/month."

---

## 2. Success Criteria — Pilot → Production

| Criterion | Pilot target | Pilot result | Production target | Production result |
|-----------|--------------|--------------|-------------------|-------------------|
| Sensitivity (risk clauses) | 0.80 | 0.82 | Maintain ≥0.80 | 0.84 (30-day rolling avg) |
| Specificity | 0.90 | 0.92 | Maintain ≥0.90 | 0.91 |
| P95 latency | <5 sec | 4.2 sec | <5 sec | 3.8 sec |
| Cost per contract | <$0.10 | $0.067 | <$0.08 | $0.054 |

All criteria met or exceeded. [One sentence on what this means in business terms.]

---

## 3. Business Outcomes

Quantified impact, not just metrics.

| Outcome | Baseline | Post-deployment | Change |
|---------|----------|-----------------|--------|
| Contract review time per document | 4.2 hours | 2.1 hours | -50% |
| Contracts processable per week | 32 | 64 | +100% |
| Risk clauses missed in QA review | 4.8% | 1.2% | -75% |
| Legal team's high-value work hours | ~40/month | ~85/month | +112% |

[Narrative paragraph describing what these numbers mean for the organization.]

---

## 4. What the Team Can Do Now That They Couldn't Before

[3-5 specific capabilities. This is often more compelling than metrics for exec audience.]

1. [Capability: e.g., "Process inbound contracts in same-day turnaround rather than 48-hour SLA"]
2. [Capability: e.g., "Identify risk clause patterns across quarters, enabling proactive playbook updates"]
3. [Capability: e.g., "Accept 40% more volume without headcount growth"]
4. [Capability: e.g., "Independently update risk detection rules as new contract types emerge"]

---

## 5. Operational Status

- **System uptime (30-day)**: 99.82%
- **SLO status**: all SLOs in green
- **Incidents (past 30 days)**: 2 SEV-3 incidents, 0 SEV-1 or SEV-2.
- **Customer on-call**: resolved both incidents without vendor involvement
- **Customer team size operating the system**: 4 engineers able to deploy, monitor, and respond

---

## 6. Cost

| Period | Actual | Forecast at project start | Variance |
|--------|--------|--------------------------|----------|
| Pilot (6 weeks) | $4,200 | $5,000 | -16% |
| Production rollout (6 weeks) | $9,800 | $12,000 | -18% |
| First 3 months operational | $18,400 | $22,000 | -16% |
| **Total engagement cost** | **$32,400** | **$39,000** | **-17%** |

[Narrative on what drove under-budget or over-budget.]

Projected steady-state operational cost: ~$6,200/month.

---

## 7. What Worked Well

[3-5 items. Often the things customer did well, not just what vendor did well.]

1. [Item: e.g., "Legal SME availability for gold-set labeling was the difference between a successful and a struggling pilot. Mark's 4 hours/week was invested."]
2. [Item: e.g., "Customer's existing deployment automation allowed us to ship to staging in 48 hours from decision to execute."]
3. [Item: e.g., "Exec sponsor's early scope intervention in week 2 kept the pilot bounded."]

---

## 8. What Didn't Work (And Was Fixed)

Be honest. Exec sponsors respect this.

1. [Item: e.g., "Initial data pipeline assumed contract PDFs with selectable text; 18% of documents were scanned images. Added OCR pre-processing in week 3."]
2. [Item: e.g., "First prompt version had a drift issue in legal entity extraction; caught in week 2 midpoint eval, fixed in week 3."]

---

## 9. Known Limitations

[Explicit about what the system does NOT do. No surprises later.]

1. [Limitation: e.g., "Non-English contracts are out of scope and should be manually reviewed."]
2. [Limitation: e.g., "Risk detection targets the 14 clause types in the training set; newer contract types may have missed clauses."]
3. [Limitation: e.g., "Certain dense-format corporate contracts (>100 pages) exceed context window; chunked processing is used but boundary-crossing clauses have reduced accuracy."]

See `known-limitations.md` in handover package for full list.

---

## 10. Deferred Work

[Items that came up in the engagement but weren't in scope. Useful for follow-on planning.]

| # | Item | Rationale for deferral | Suggested timing |
|---|------|------------------------|-------------------|
| 1 | Non-English contract support | Out of pilot scope; requires different dataset | Year 2 |
| 2 | Redline suggestion generation | Distinct capability; separate engagement | Year 1 Q4 |
| 3 | Integration with the DMS | Customer-side architecture decision needed | Year 1 Q3 |

---

## 11. What We'd Do Differently

[Reflective. 2-3 items maximum. Builds trust.]

1. [Item: e.g., "Run OCR feasibility test in week 0 — would have shifted the scope sooner."]
2. [Item: e.g., "Involve the QA team in week 1 instead of week 8 — their feedback loop was the most valuable signal."]

---

## 12. Advisory Relationship Going Forward

- **Weeks 27-30**: weekly 1-hour advisory calls. Customer drives agenda.
- **Month 9+**: monthly 30-minute check-ins.
- **Event-driven**: as-needed escalation for major changes or incidents.

Formal engagement closes on [date]. Advisory support continues through contract terms.

---

## 13. Discussion Topics for This Meeting

Pre-read addresses facts. The meeting is for discussion.

Proposed topics:

1. **Expansion**: [if applicable] — what's the customer's interest in a follow-on engagement? AE takes forward.
2. **Reference & case study**: would the customer be willing to serve as a reference / co-author a case study?
3. **Feedback loop**: anything vendor could have done better?
4. **Unanswered questions**: anything not addressed in this document?

---

## Appendix A: Timeline Summary

| Phase | Dates | Weeks |
|-------|-------|-------|
| Discovery | [dates] | 1-2 |
| Scoping | [dates] | 3 |
| Pilot build | [dates] | 4-9 |
| Production rollout | [dates] | 10-16 |
| Operational handoff | [dates] | 17-20 |
| Post-engagement | [dates] | 21-26 |

## Appendix B: Artifacts Delivered

- [ ] Discovery summary
- [ ] Pilot scope document
- [ ] Architecture Decision Records (12 total)
- [ ] System architecture doc
- [ ] 18 runbooks across operations and incident response
- [ ] Monitoring dashboards (7)
- [ ] Compliance mappings (SOC2, internal audit)
- [ ] Training materials (5 sessions recorded)
- [ ] Handover package (signed week 20)
- [ ] Post-mortem documents (3 from production)

## Appendix C: Metrics — Full Table

[Full metrics table for reference, not meeting discussion.]

---

## Meeting template

- 0:00-0:10: Welcome, context, agenda confirm.
- 0:10-0:30: Customer presents operational experience — 10 weeks running, what's worked, what's been hard. (Customer leads.)
- 0:30-0:50: Joint review of the success criteria and business outcomes. (Exec sponsor anchors.)
- 0:50-1:05: What's next discussion. (Exec sponsor + project owner.)
- 1:05-1:20: Feedback round — what could have been done better. (All.)
- 1:20-1:30: Close — thank-you, reference / case study ask, advisory cadence confirmation.

---

## Post-meeting actions (same day)

- [ ] Send recap email to attendees.
- [ ] Record any decisions (e.g., follow-on engagement interest).
- [ ] Update the engagement's internal learnings doc.
- [ ] Flag reference / case study opt-ins to AE.
- [ ] Schedule first post-engagement advisory call.
