# Technical Decision Framework

Heuristics for common LLM engagement decisions. Not prescriptive — context matters — but useful starting points that save weeks of deliberation.

---

## Which LLM provider?

Consider in this order:

1. **Is data residency or sovereignty a constraint?**
   - Yes (on-prem required): self-hosted inference (vLLM + open-weights model).
   - Yes (must stay in-region): check provider region availability.
   - No: go to next question.

2. **Is cost the dominant concern?**
   - Yes, high volume: OpenAI gpt-4o-mini or Claude Haiku for most cases; escalate to Sonnet / GPT-4o for hard tasks.
   - No: Claude Sonnet or GPT-4o primary, with fallback.

3. **Is task reasoning-heavy?**
   - Yes: Claude Sonnet with extended thinking, or OpenAI o3.
   - No: Claude Sonnet or GPT-4o baseline is fine.

4. **Is multilingual performance critical?**
   - Yes for European languages: GPT-4o, Claude Sonnet both strong.
   - Yes for CJK: test both; Claude has historically been strong in Japanese/Korean specifically.
   - Yes for low-resource languages: may require specialized model or fine-tuning.

5. **Is vision needed?**
   - Yes: GPT-4o or Claude Sonnet both good.
   - No: stay with text-optimized choices.

6. **Is long-context needed (>100K tokens)?**
   - Yes: Claude Sonnet (200K context, better long-context adherence).
   - No: any.

**Default recommendation for enterprise pilots (2026)**: Claude Sonnet as primary, OpenAI GPT-4o as fallback. Tested more thoroughly in enterprise settings; both provide enterprise contracts with SLA; both have mature security review readiness.

---

## Fine-tune or RAG?

### Use RAG when:

- Knowledge base changes frequently (hours to days).
- Answers must cite source material.
- Knowledge base is large (millions of documents).
- You need confidence the answer is grounded in retrieved context.
- Cost per query acceptable with larger context windows.

### Use fine-tuning when:

- You need specific style, tone, or output format adherence beyond what prompting achieves.
- Domain language is highly specialized (legal contracts, medical terminology).
- Task-specific behavior is needed at scale (e.g., structured extraction with customer-specific schema).
- Latency matters (fine-tuned smaller model faster than large model with RAG).

### Use both when:

- Grounded generation AND specialized output format (e.g., customer service agent with company-specific tone + RAG over current help docs).

### Default recommendation for 90% of pilots: RAG first, fine-tune second

Fine-tuning is often seen as the "more powerful" option but it's:
- Slower to iterate on (hours per fine-tune vs seconds per prompt update).
- Expensive to maintain (re-tune when foundation model updates).
- Less transparent (can't easily explain why a specific answer was produced).

Start with RAG. Fine-tune only when RAG plateaus and the gap is measurable.

---

## Which vector database?

Consider in this order:

1. **Data volume**:
   - <1M vectors: pgvector (Postgres extension) is often sufficient.
   - 1M-100M: Qdrant, Weaviate, Milvus all viable.
   - >100M: Pinecone (managed), Weaviate (self-hosted), or custom.

2. **Do you need hybrid search (vector + keyword)?**
   - Yes: Weaviate (best built-in hybrid), Qdrant, or Elasticsearch with vectors.
   - No: any vector DB.

3. **Operational preference**:
   - Fully managed: Pinecone, Vectara, managed Qdrant Cloud.
   - Self-hosted: Qdrant, Weaviate, Milvus, pgvector.
   - Hybrid (managed + self-hosted option): Qdrant has both.

4. **Compliance constraints**:
   - Must be on-prem: Qdrant, Weaviate, pgvector (all self-hostable).
   - Managed acceptable: any.

**Default recommendation**: Qdrant for self-hosted (simpler than Weaviate, better Helm chart than Milvus); Pinecone for managed; pgvector if you're already deeply in Postgres.

---

## Inference engine for self-hosted models?

| Need | Choice |
|------|--------|
| Max throughput on GPU | vLLM |
| Max flexibility (custom models) | TGI |
| Simplest ops | Ollama (dev) or Together AI / Fireworks (prod) |
| Most compatible with HuggingFace | TGI |
| Best cost/performance on A100s | vLLM |
| Runs on CPU (small model) | llama.cpp |

**Default recommendation**: vLLM for production self-hosting. Better throughput, mature, active community.

---

## Agent framework?

| Need | Choice |
|------|--------|
| Simple sequential agent | Vanilla LangChain / raw OpenAI SDK |
| Stateful multi-step workflow | LangGraph |
| Role-based multi-agent | CrewAI |
| Conversational multi-agent | AutoGen |
| Best tool-calling reliability | stage-pilot + your framework of choice |

**Default recommendation**: Start raw (no framework) for simple cases. LangGraph when you have real state machines. Layer stage-pilot on top for tool-call reliability regardless of framework.

Avoid: adopting 3 frameworks at once. Pick one; fully internalize it.

---

## Cloud vendor for deployment?

Usually dictated by customer; rarely your choice. When it is:

| Need | Choice |
|------|--------|
| Best K8s experience | GKE > EKS > AKS (opinionated; others disagree) |
| Best LLM provider co-location | AWS Bedrock + EKS, or Azure OpenAI + AKS |
| Cost-sensitive | Any, but optimize heavily |
| EU sovereignty | Azure or self-host |
| Asia sovereignty | NCP (Korea), AWS Seoul, Azure Korea Central |

**Default recommendation**: customer's existing cloud. Switching clouds during an LLM engagement is a non-starter.

---

## Prompt caching — when to use?

Use prompt caching (Anthropic / OpenAI) when:

- System prompt is >2000 tokens.
- Same system prompt used across many requests.
- Cost reduction matters.

Expected benefits:
- Anthropic: 90% discount on cached input tokens. Cache lives ~5 minutes.
- OpenAI: automatic (their management); less user control.

**Default recommendation**: always on for Anthropic if your system prompt is stable.

---

## Evaluation framework?

Build vs. buy:

**Build** (common for pilots): simple rubric-based eval harness in Python, stored alongside source. 200-500 LOC typically.

**Buy** (for larger deployments):
- OpenAI Evals: open-source, mature.
- LangSmith: paid, integrates with LangChain.
- PromptFoo: open-source, good UI.
- Braintrust: paid, production-grade.

**Default recommendation**: build your own for the pilot. Evaluate buying options post-pilot when you know what you need.

---

## Prompt versioning?

Options:

1. **Git + config files**: simple, version-controlled. Recommended default.
2. **Prompt registry** (LangSmith, Humanloop, PromptLayer): paid, gives UI and non-engineer editing.
3. **Feature flags for prompts**: LaunchDarkly / Statsig patterns applied to prompt strings.

**Default recommendation**: Git-based in YAML or JSON for pilot. Move to a dedicated prompt registry only if non-engineers need to edit prompts.

---

## Observability stack?

Reuse customer's existing stack. Do not introduce new tooling into a pilot.

If customer has nothing:

- Logs: Loki or OpenSearch or CloudWatch.
- Metrics: Prometheus.
- Dashboards: Grafana.
- Tracing: OpenTelemetry with Jaeger or Tempo.
- Paging: PagerDuty, Opsgenie, or Datadog's native.

**Default recommendation**: OpenTelemetry end-to-end. Portable across backends.

---

## How to document decisions?

Use Architecture Decision Records (ADRs). 5-section template:

- **Status**: proposed / accepted / superseded
- **Context**: what situation demands this decision
- **Decision**: what you decided
- **Consequences**: positive and negative
- **Alternatives**: what else you considered and why rejected

Numbered in the `docs/adr/` folder. One ADR per decision that's meaningful to anyone who joins the project later.

Minimum set for an LLM deployment:
- 001: model and provider choice
- 002: framework choice
- 003: vector DB choice (if RAG)
- 004: deployment target
- 005: observability stack
- 006: auth integration
- 007: cost management approach
- 008: evaluation framework

---

## What NOT to do

1. **Don't adopt 5 new tools for a pilot.** Every new tool is a learning curve and a dependency risk. Adopt 1-2; minimize the rest.
2. **Don't optimize prematurely.** Your first prompt doesn't need to be the final prompt.
3. **Don't skip the eval harness to move faster.** It feels fast in week 1; it costs weeks in week 4.
4. **Don't solo-engineer.** Pair with customer's engineer constantly; they're the ones operating this post-handoff.
5. **Don't pick technology based on what's trendy.** Pick on fit.

---

## When in doubt

1. Pick the simpler option.
2. Pick the option the customer's team already knows.
3. Pick the option that can be changed later without rewriting.
4. Pick the option you can explain to the exec sponsor in 2 minutes.
