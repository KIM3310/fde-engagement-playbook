# Cost Model Template

How to build a credible TCO model for a customer's LLM deployment. The framework, example numbers, and how to present it.

---

## Why this matters

Cost at production scale is the most common exec-level surprise in LLM engagements. Pilot costs are forgivable (small). Production costs can be 100-1000x pilot costs. If the customer is surprised, you've failed.

Build the cost model in week 2 of the pilot. Share weekly. Iterate as real data comes in.

---

## Cost components

### Infrastructure costs

| Line item | Typical monthly cost range | Cost driver |
|-----------|-----------------------------|-------------|
| Compute (inference) | $500 - $50,000+ | Traffic volume, self-host vs API |
| Compute (supporting services) | $300 - $5,000 | API tier, queue capacity, etc. |
| Storage (vector DB) | $100 - $3,000 | Corpus size |
| Storage (metadata DB) | $100 - $1,000 | Request volume |
| Storage (audit logs) | $50 - $500 | Retention, volume |
| Storage (object store) | $50 - $500 | Archive volume |
| Network / data transfer | $50 - $2,000 | Cross-region, egress |
| Observability | $100 - $1,000 | Log volume, metric cardinality |
| Secrets / Vault | $0 - $200 | Managed or self-hosted |

### LLM API costs (if not self-hosting)

Primary driver: tokens consumed.

Formulas:
```
monthly_input_tokens = requests_per_month × avg_input_tokens_per_request
monthly_output_tokens = requests_per_month × avg_output_tokens_per_request
monthly_llm_cost = (monthly_input_tokens × input_price_per_token) 
                 + (monthly_output_tokens × output_price_per_token)
```

Example (Claude Sonnet 4, April 2026 pricing):
- Input: $3 per million tokens
- Output: $15 per million tokens
- Cached input: $0.30 per million tokens (10% of regular)

For 50,000 requests/day × 30 days = 1.5M requests/month.
Average 3,500 input + 800 output tokens per request:
- Input: 5.25B tokens → 5250 × $3 = $15,750/month (without caching)
- Output: 1.2B tokens → 1200 × $15 = $18,000/month
- **Total**: $33,750/month

With 70% cache hit rate on system prompt (let's say 2,000 of 3,500 tokens is cached):
- Cacheable input (cached): 70% × 1.5M × 2000 = 2.1B tokens × $0.30 = $630
- Cacheable input (first write): 30% × 1.5M × 2000 = 900M tokens × $3 = $2,700
- Non-cacheable input: 1.5M × 1500 = 2.25B tokens × $3 = $6,750
- Output: 1.2B tokens × $15 = $18,000
- **Total**: $28,080/month (17% savings via caching)

### Self-hosted model costs

If using vLLM on your own GPUs:

| GPU | Hourly cost (AWS on-demand) | Monthly cost (24/7) | Typical throughput (tokens/sec) |
|-----|------------------------------|-----------------------|----------------------------------|
| T4 | $0.53 | $390 | ~50-150 (small model) |
| A10G | $1.21 | $880 | ~150-400 (7-13B model) |
| A100 40GB | $3.06 | $2,230 | ~500-1500 (34B model) |
| H100 80GB | $5.00 | $3,650 | ~2000-5000 (70B model) |

Reserved instances: typically 30-50% discount.

Rule of thumb: self-hosted becomes cost-effective vs API around 100-200M tokens/month for similar-quality models.

### Team costs

Often overlooked in LLM TCO:

| Role | Monthly FTE cost (Korea) |
|------|--------------------------|
| Senior engineer (operating the system) | $8,000-15,000 |
| DevOps (infrastructure) | $6,000-12,000 |
| On-call rotation premium | $500-1,500 |
| Product manager (part-time) | $4,000-8,000 |

A production LLM service typically requires 0.3-0.5 FTE ongoing. Call this out in the model.

---

## Building the model — step-by-step

### Step 1: Estimate request volume (week 1-2 of pilot)

- Current baseline: how many of the target workflow happen today?
- Adoption ramp: what % of current workflow do you expect to shift to the new system? Over what period?
- Growth: what's the growth trajectory?

Example:
- Baseline: 1,000 contract reviews per month.
- Adoption: 30% by month 3, 60% by month 6, 90% by month 12.
- Growth: 20% YoY in contract volume.

### Step 2: Measure per-request cost (week 2-3 of pilot)

Run 100 representative requests through the system. Measure:
- Input tokens per request (distribution: p50, p95, max).
- Output tokens per request.
- Latency (for SLO; doesn't directly affect cost).
- Cache hit rate (for Anthropic cache).

### Step 3: Extrapolate

Apply Step 1 volume × Step 2 per-request cost. This gives your steady-state LLM cost.

Add infrastructure costs at steady-state traffic (not pilot traffic). Remember:
- Kubernetes cluster has minimum cost even at low traffic.
- Storage scales with data volume.
- Observability scales with log volume.

### Step 4: Model month-by-month

Don't present a single "steady-state cost"; present a ramp:

| Month | Adoption | LLM cost | Infra cost | Total |
|-------|----------|----------|-----------|-------|
| 1 (pilot) | 5% | $800 | $600 | $1,400 |
| 2 | 10% | $1,600 | $700 | $2,300 |
| 3 | 30% | $4,800 | $900 | $5,700 |
| 6 | 60% | $9,600 | $1,200 | $10,800 |
| 12 | 90% + growth | $17,300 | $1,500 | $18,800 |

Exec sponsors can read this; a single steady-state number often causes sticker shock.

### Step 5: Present optimization levers

For every cost line, name the lever:

- **LLM cost too high?**
  - Enable prompt caching (20-40% reduction on repeat-system-prompt traffic).
  - Batch API for async/overnight work (50% reduction).
  - Smaller model for 80% of traffic; larger model for hard cases.
  - Retrieval filtering to reduce context size.

- **Infra cost too high?**
  - Auto-scale aggressively in quiet periods.
  - Reserved instances.
  - Cross-cloud arbitrage (usually not worth the complexity).

- **Storage too high?**
  - Move cold audit logs to cold storage after 90 days.
  - Partition and archive old vector-DB segments.

### Step 6: Include team cost

Often the largest line item. Don't hide it.

---

## Presenting the cost model to the customer

### Slide 1: One-number summary

"Steady-state monthly cost: $XX,000 at full deployment (month 12)."

### Slide 2: Ramp

Month-by-month chart. Shows cost growing with adoption.

### Slide 3: Breakdown

Pie chart or bar chart of cost components at steady state.

### Slide 4: Optimization options

"If we enable prompt caching: -$Y. If we use Batch for overnight work: -$Z. If we add these, model becomes $X,000."

### Slide 5: Comparison to current state

If you can: what does the customer spend today on the same workflow (team hours × hourly cost)? Show LLM cost as % of that.

### Slide 6: Assumptions

Explicitly list: request volume assumption, input/output token assumptions, pricing assumptions. These are what you'll update.

---

## Cost monitoring in production

Once live, don't stop at the model — instrument it.

### Required dashboards:

1. **Daily cost burn**: $ spent today vs daily budget.
2. **Cost per call**: 7-day rolling avg.
3. **Cache hit rate**: higher is better.
4. **Token distribution**: histogram of input token counts (spots outliers).
5. **Per-tenant attribution** (if multi-tenant): which tenant is driving cost?

### Required alerts:

- Daily cost > 1.5x daily budget (SEV-3).
- Weekly cost > 1.3x weekly forecast (SEV-3).
- Per-tenant cost anomaly (for abuse detection).

### Required review cadence:

- Daily: engineering lead scans cost dashboard.
- Weekly: engineering manager reviews trend.
- Monthly: joint review with finance/product.

---

## Cost antipatterns

1. **Under-forecasting**: pilot costs are always low. Extrapolating pilot cost to production without volume scaling produces comfortingly-wrong numbers.

2. **Over-forecasting without optimization**: telling the customer $100K/month when with caching + Batch + smaller-model routing it's $30K/month makes the project look infeasible.

3. **Hiding team cost**: LLM services need operators. The customer will discover this within 90 days.

4. **Ignoring data egress**: pulling vector embeddings out of a managed service + cross-cloud egress can surprise.

5. **Assuming API pricing is stable**: providers change pricing. Forecast should tolerate 30% provider price changes.

---

## Template cost model (spreadsheet structure)

```
Sheet 1: Assumptions
- Request volume (month-by-month for 12 months)
- Input tokens (p50, p95)
- Output tokens (p50, p95)
- Cache hit rate (%)
- Pricing per model

Sheet 2: LLM cost
= Requests × tokens × pricing, month-by-month

Sheet 3: Infrastructure cost
- Per-component fixed + variable, month-by-month

Sheet 4: Team cost
- Fixed by FTE

Sheet 5: Total and ramp
- Sum of sheets 2-4, month-by-month
- Compare to baseline cost

Sheet 6: Sensitivity analysis
- What if pricing changes ±30%?
- What if volume is 50% higher?
- What if cache hit rate is 20% lower?
```

---

## The conversation with exec sponsor

Exec sponsors don't want a spreadsheet handed to them. They want:

1. **One number**: "$X,000/month at full deployment."
2. **One comparison**: "That's [Y]% of what you spend today on the same workflow."
3. **One option**: "With these optimizations, we can bring it to $Z,000."
4. **One commitment**: "We'll monitor and alert if it goes over budget."

If exec sponsor asks "what if volume doubles?", you have the sensitivity analysis ready (not a new conversation).

---

## Final reminders

- Build the cost model in week 2 of pilot. Not month 3 of production.
- Update it weekly during pilot; monthly during production.
- Share it with exec sponsor proactively, not reactively.
- Include optimization levers always.
- Team cost is real cost.
- Budget breaches are people problems; they require executive involvement.
