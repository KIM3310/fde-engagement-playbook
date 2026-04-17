# Example: Acme Manufacturing — 6-Month FDE Engagement

*A composite narrative. Numbers, names, and specifics are fabricated but representative of real enterprise LLM engagements.*

---

## Context

**Customer**: Acme Manufacturing, 12,000 employees, industrial components supplier operating 4 plants in Korea and 2 in Vietnam.

**Problem**: Quality engineering team processed ~800 field-failure reports per month, each written in free-text Korean, English, or a mix. Classification, root-cause tagging, and escalation routing consumed 4 engineers full-time. Turnaround: 5-7 days per report. Backlog: 300+ reports at any moment.

**Executive sponsor**: VP Quality Engineering.

**Engagement scope**: 26-week FDE engagement. Deliver an LLM-based triage system that classifies each report into failure class (12 categories), extracts structured entities (part ID, plant, batch, failure mode), and routes to the right sub-team. Customer's Quality team operates it post-handoff; FDE remains advisory for 6 months.

**Contract value**: not material to this narrative.

---

## The six phases in practice

### Phase 1 — Discovery (weeks 1-2)

Discovery call included VP Quality (exec sponsor), Director of Engineering Operations (project owner), Senior Quality Engineer (technical lead), and Plant Manager from Plant B (stakeholder). Stakeholder mapping also surfaced:

- A reluctant subsystem owner at Plant C who had been burned by a previous ML initiative.
- An IT director who was firmly opposed to cloud LLM APIs due to proprietary component specifications in the reports.
- A data engineer who was pulled into the project at 20% FTE but was actively ramping down another initiative.

The two-day data spike (~50 sample reports) revealed:

- ~18% of reports mixed Korean and English in single paragraphs.
- ~8% included scanned images from handwritten field notes (OCR needed).
- Entity extraction would need to handle non-standard part ID formats across 3 legacy numbering systems.
- The "root cause" free-text field was often empty or contained the reporter's opinion rather than a diagnosed cause.

Risks raised before scoping:
- Data quality: scanned image subset would blow out scope.
- IT resistance: on-prem only was likely a requirement.
- Low-quality ground truth: the customer's existing category labels were inconsistent.

### Phase 2 — Scoping (week 3)

The pilot scope narrowed to:

- Text-only reports (defer scanned images to phase 2).
- 10 of 12 categories (skipping two rare ones that had <1% of reports).
- First 500-report gold set labeled by a 2-engineer panel in weeks 1-3 of pilot.
- Deployment target: customer's existing Kubernetes cluster at Plant HQ, with an LLM running on-prem via [llm-onprem-deployment-kit](https://github.com/KIM3310/llm-onprem-deployment-kit) patterns.
- Primary success criterion: top-1 classification accuracy ≥ 0.85 on the gold set; entity extraction F1 ≥ 0.75.
- Secondary: average triage time per report to reduce from ~5 hours to <15 minutes post-deployment.

Failure criterion: midpoint eval < 0.65 accuracy triggers re-scope.

Explicit scope exclusions documented:
- Scanned-image OCR.
- The 2 rare categories.
- Vietnamese-language reports (deferred to phase 2).
- Integration with Acme's existing issue-tracking system (the MVP emits JSON that Quality operators manually paste in).

### Phase 3 — Pilot build (weeks 4-9)

Week 1: Data pipeline stood up by end of week. Customer DE's 20% FTE turned into 40% during weeks 4-5 because the data director saw it as high-priority and let the DE lean in.

Week 2: Midpoint eval came in at 0.71 accuracy — below success target of 0.85 but above failure criterion of 0.65. The two-hour post-midpoint conversation with the customer covered:

- Failure analysis: 6 of 10 categories were >0.85 accuracy; 4 were <0.70.
- Two of the 4 low-performing categories had <15 examples in the gold set (insufficient data).
- Two low-performers shared phrasings with adjacent categories and required more specific prompting.

Decision: extend gold set for those 4 categories, iterate on prompt design, re-evaluate end of week 4.

Week 3-4: Iteration. By week 4 eval, accuracy hit 0.83 on the full test set. Entity extraction F1 at 0.78. Both within shooting distance of success.

Week 5: Staging deployment on Plant HQ cluster. llm-onprem-deployment-kit's Helm chart was the substrate; a Cohere Command R self-hosted via Cohere Toolkit was the inference engine. Quality engineer spent 10 hours over the week validating outputs.

Week 6: Final evaluation hit 0.87 accuracy and 0.82 F1. Both exceed success criteria. Production rollout approved in the exit review.

### Phase 4 — Production rollout (weeks 10-16)

Parallel workstreams that ran:

1. **Infrastructure**: Plant HQ cluster scaled from 3 nodes to 5; dedicated GPU nodes added for the inference workload. IT director, previously resistant, became an ally because the deployment was purely on-prem.

2. **Observability**: Grafana dashboards installed. SLO: 95% reports triaged within 30 seconds of submission. Alert routing to Quality team's PagerDuty rotation.

3. **Security**: Acme's internal pen-test uncovered 2 issues — one was a logging configuration that included raw report text (fixed by redaction patterns); the other was a ConfigMap with a non-secret value that ended up in audit logs. Both closed in week 13.

4. **Integration**: Report submission system hooked to the inference service via a message queue. Operators reviewed each classification; initial confidence threshold was set high so only clear cases auto-classified, the rest flagged for human review.

5. **Reliability**: 48-hour soak test at week 14 with 6× expected peak load uncovered a memory leak in the embedding service (patched in 24 hours).

6. **Change management**: VP Quality held four town-halls with the Quality team to set expectations. "This system will catch 85% of reports clearly; your judgment is still essential for the 15%."

Canary rollout in week 15: 10% of reports → 50% → 100% over 5 days. No regressions.

By end of week 16: production serving ~35 reports/hour during business hours. Average triage time for auto-classified cases: 45 seconds (including queue + human review of the classification).

### Phase 5 — Operational handoff (weeks 17-20)

The five capability validations in week 19:

1. **Incident response**: Simulated a replica failure in Qdrant. Quality Engineer on-call responded in 4 minutes and recovered in 28 minutes.
2. **Deployment**: DE team upgraded the inference service to a new Helm chart version. Clean.
3. **Prompt update**: Senior Quality Engineer modified the category 3 prompt, re-ran eval, confirmed no regression. Committed the change to git.
4. **Weekly gold set**: Data engineer set up a weekly cron that sampled 30 new reports, had them labeled by the panel, and ran the eval harness. The flow took 4 hours of the DE's time per week.
5. **Cost attribution**: PM produced the first monthly cost breakdown, identified one tenant (Plant D) over-indexed on API calls, worked with that plant's lead to tune down unnecessary calls.

All 5 passed on first attempt. Sign-off in week 20.

### Phase 6 — Post-engagement (weeks 21-26)

Advisory cadence: weekly calls in weeks 21-24, bi-weekly in weeks 25-26.

Post-handoff incidents: 2 SEV-3 (one related to a Qdrant HNSW parameter change, one related to a downstream consumer format mismatch). Both handled by customer team without FDE intervention.

Week 26 success review numbers:

| Metric | Baseline | After | Change |
|---|---|---|---|
| Average triage time per report | 5.2 hours | 0.5 hours | -90% |
| Reports in backlog (steady-state) | 310 | 38 | -88% |
| Quality engineers on triage | 4 FTE | 1.5 FTE (reallocated to root-cause analysis) | -62% |
| Monthly infra cost (including LLM on-prem) | n/a | $8,400 | - |
| Time from field incident to routing | 7 days median | 1 day median | -86% |

Exec sponsor's summary: "This shifted the bottleneck from triage to root-cause analysis, which is where our engineers add the most value."

Reference letter signed. Case study opt-in. Expansion conversation begun for phase 2 (scanned-image OCR + Vietnamese reports).

---

## What worked

1. **The on-prem constraint, forced by the IT director, became a selling point with other regulated customers**. The kit built here was reusable.
2. **Letting the customer Senior Quality Engineer co-own prompt design** made the handoff trivial. He'd written half the final prompts.
3. **Midpoint eval triggered a scope recovery conversation** instead of hiding concerns until the exit review.
4. **The canary rollout with explicit stage gates caught the memory leak** before it hit full production.
5. **Weekly gold-set refresh** (even at a small sample size) caught three model drift events over months 4-6 that would have silently degraded quality.

## What didn't

1. **Initial gold set was under-balanced across categories.** Redistributing cost 2 weeks of schedule.
2. **The reluctant Plant C subsystem owner** was never converted. Their plant's reports were classified the same way, but adoption uptake there lagged by 2 months compared to other plants.
3. **The scanned-image scope exclusion** was the right call, but the Quality team kept asking "when is it coming?" from month 2 on. Better expectation-setting at scoping would have helped.
4. **Cost model under-forecast the LLM on-prem infrastructure** by ~20%. Operating a GPU cluster 24/7 costs more than the spiky usage pattern I'd modeled.

## Reusable artifacts from this engagement

Everything upstream in this playbook — runbooks, templates, security checklist — was shaped by this engagement. The specifics are generalized but the structure comes from here.

---

*Names, numbers, and organizational details above are composite / fabricated. The engagement shape is representative.*
