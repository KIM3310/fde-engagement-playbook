# fde-engagement-playbook

> A Forward Deployed Engineer's field playbook for enterprise LLM deployments. From first customer call to day-180 operational handoff — the artifacts, templates, and decision patterns that make a 6-month engagement repeatable.

[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Playbook Status](https://img.shields.io/badge/status-field--tested-blue.svg)]()

---

## Why this exists

An FDE who shows up on day 1 with a laptop and hope produces inconsistent outcomes. An FDE who shows up with a playbook — templates for every conversation, runbooks for every deployment phase, pre-written artifacts for every handoff — produces customers who renew.

This playbook is the second version. It's the artifact an FDE wishes they'd had on their first engagement, refined through the lessons of not-having-it. It's organized by the six phases of a typical enterprise LLM engagement:

1. **Discovery** (weeks 1-2) — understand the customer's actual problem, not their stated problem.
2. **Scoping** (weeks 2-3) — convert problem into bounded pilot.
3. **Pilot build** (weeks 3-8) — ship something that works on their data.
4. **Production rollout** (weeks 8-16) — go from pilot to production grade.
5. **Operational handoff** (weeks 16-20) — transfer ownership to the customer's team.
6. **Post-engagement** (weeks 20-26) — advisory support + success measurement.

## Who this is for

- FDEs at LLM vendors (Cohere, Anthropic, Databricks MosaicML, etc.) running customer engagements.
- Solutions architects transitioning into customer-facing roles.
- Teams building internal FDE functions who need a starting template.

## What's in here

### Phase-by-phase runbooks

| Phase | Runbook | Purpose |
|-------|---------|---------|
| 1. Discovery | [runbooks/01-discovery.md](runbooks/01-discovery.md) | Questions that surface the real problem; red flags that signal doomed projects |
| 2. Scoping | [runbooks/02-scoping.md](runbooks/02-scoping.md) | Converting ambiguous "we want AI" into a bounded 4-week pilot |
| 3. Pilot build | [runbooks/03-pilot-build.md](runbooks/03-pilot-build.md) | Running a multi-week sprint with the customer in the room |
| 4. Production rollout | [runbooks/04-production-rollout.md](runbooks/04-production-rollout.md) | Going from demo-grade to 99.9% SLA |
| 5. Operational handoff | [runbooks/05-operational-handoff.md](runbooks/05-operational-handoff.md) | Making sure the customer can operate without you |
| 6. Post-engagement | [runbooks/06-post-engagement.md](runbooks/06-post-engagement.md) | Measuring success, catching regressions, upsell motion |

### Templates (drop-in, edit-per-customer)

| Template | Purpose |
|----------|---------|
| [templates/discovery-call-agenda.md](templates/discovery-call-agenda.md) | First-call structure (60 min) |
| [templates/technical-discovery-questionnaire.md](templates/technical-discovery-questionnaire.md) | Systematic tech-fit assessment |
| [templates/pilot-proposal.md](templates/pilot-proposal.md) | Customer-facing pilot scope document |
| [templates/deployment-architecture-doc.md](templates/deployment-architecture-doc.md) | Architecture decision record for the pilot |
| [templates/weekly-status-email.md](templates/weekly-status-email.md) | Status cadence that prevents "black box" perception |
| [templates/incident-post-mortem.md](templates/incident-post-mortem.md) | Blameless post-mortem for production incidents |
| [templates/handover-package.md](templates/handover-package.md) | Day-180 handoff table of contents |
| [templates/executive-success-review.md](templates/executive-success-review.md) | Month-6 business review for exec sponsor |

### Supporting docs

- [docs/engagement-anti-patterns.md](docs/engagement-anti-patterns.md) — the 12 mistakes I've seen kill engagements
- [docs/technical-decision-framework.md](docs/technical-decision-framework.md) — when to pick vLLM vs TGI, Qdrant vs pgvector, RAG vs fine-tune
- [docs/cost-model-template.md](docs/cost-model-template.md) — how to build a credible TCO model for the customer
- [docs/security-checklist.md](docs/security-checklist.md) — 30-item security posture checklist used as pilot exit criterion
- [docs/measuring-success.md](docs/measuring-success.md) — leading indicators, lagging indicators, and the business metric that actually matters

### Reference case studies

- [examples/acme-manufacturing.md](examples/acme-manufacturing.md) — 6-month engagement, 4-way success (technical, business, customer-team, our-team)
- [examples/failed-engagement-lessons.md](examples/failed-engagement-lessons.md) — what a failed engagement taught us

## How to use

**You are a new FDE starting engagement #1**: read the six runbooks in order. Copy `templates/` into your engagement folder. Run the discovery call from `templates/discovery-call-agenda.md` verbatim on first call.

**You are an experienced FDE**: grep for the specific artifact you need. The playbook is designed to be reference, not read-through. Every template is self-contained.

**You are building an internal FDE function**: treat this as a starting skeleton. Every team adapts it; the phased structure is the durable part.

## Scope boundaries

This playbook is **technology-neutral** in its process but **LLM-specific** in its content. It assumes:

- You are deploying LLM-based products (inference, agents, RAG, fine-tuned models).
- Customers are mid-to-large enterprises (not startups or consumer).
- Engagement length is 3-6 months with explicit handoff.
- Cloud-based, hybrid-cloud, and on-prem deployments all covered.

It does NOT cover:
- Pre-sales motion (handled by AE + SE before FDE engagement starts).
- Hyper-specialized domains (healthcare compliance, finance regulatory) — referenced but not comprehensive.
- Non-LLM ML deployments (classical ML, CV) — patterns transfer but examples here are LLM-centric.

## Related projects

| Project | Relationship |
|---------|-------------|
| [llm-onprem-deployment-kit](https://github.com/KIM3310/llm-onprem-deployment-kit) | The technical deployment stack this playbook references for on-prem engagements |
| [claude-agent-cookbook](https://github.com/KIM3310/claude-agent-cookbook) | Agent patterns referenced in pilot-build runbook |
| [enterprise-llm-adoption-kit](https://github.com/KIM3310/enterprise-llm-adoption-kit) | Governance patterns referenced in production-rollout runbook |
| [stage-pilot](https://github.com/KIM3310/stage-pilot) | Tool-calling reliability runtime used in customer production deployments |

## License

MIT. Use it, fork it, adapt it. If you find it useful, open an issue with "what I changed and why" — the playbook evolves by accretion.
