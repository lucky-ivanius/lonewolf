# Variable cost checklist for Q4

The Q4 unit economic question is "what is your margin per paying user". Most builders skip variable costs they will eventually pay. Walk this checklist; include each that applies.

## Product costs

- **AI inference**: if the product calls an LLM or other model, estimate tokens per user-month at expected activity. This is the cost most AI products underestimate by 10x.
- **Storage / CDN**: per-user file or blob storage, egress on media-heavy products.
- **Payment processing**: card fees (~3% + fixed), payout fees, chargeback exposure. On crypto rails, protocol fees and spread.
- **Third-party APIs**: per-call pricing on data providers, enrichment services, search APIs.

## Crypto costs (if the product touches a chain)

- **Gas per transaction**: every user action that hits chain costs gas — unless sponsored, in which case it is YOUR cost. Estimate avg gas per user-month.
- **Protocol fees**: basis points on volume routed through DEX pools, lending spreads, oracle update costs.
- **RPC reads**: most users hit RPC nodes hundreds of times per session. Own infra is a real ops cost; hosted RPC has pricing tiers that bite at scale.

## Infrastructure costs

- **Indexer**: if the product depends on a custom indexer, hosting and DB cost adds up at scale.
- **Hosting**: frontend (Vercel, Cloudflare), backend services, any worker queues.
- **Email / SMS / push notifications**: if the product is transactional.

## Support costs

- **Customer support time**: even at zero paid support, the founder's time is a cost. Estimate hours per active user.
- **Onboarding burden**: if the first session needs handholding (especially for non-technical users), that is recurring cost.

## One-time amortized costs

- **Audit / pentest**: a security audit is a one-time cost. Amortize over expected user count.
- **Legal**: terms of service review, jurisdiction structuring.
- **Marketing**: if there is paid acquisition, the CAC enters here.

## Costs to NOT include in Q4

- Founder salary (if pre-revenue, this is a runway question, not a unit-economic question).
- Opportunity cost of capital.
- Token treasury value (these are not variable per-user costs).

## Sample math

User pays $5/month. Variable costs:

- inference: $0.40 (30 requests per month at ~$0.013 average)
- storage/CDN: $0.20 (50MB stored plus egress)
- third-party APIs: $0.10 (300 reads per session, 4 sessions per month, hosted tier)
- support: $0.30 (5 minutes per month at $30/hr blended)
- hosting (per-user share): $0.05

Total variable cost: $1.05
Gross margin: ($5.00 - $1.05) / $5.00 = 79%

That is a healthy margin. Now check at scale: does any cost compound non-linearly (e.g. support time grows superlinearly with active users)?

## Red flags

- Total variable cost over 50% of revenue at one user. Hard to scale.
- A single cost over 30% of revenue. Makes the product fragile to that cost moving.
- Costs that grow superlinearly with users (typically support, abuse mitigation, manual operations).
- AI products only: margin that only works if the user stays a light user. Heavy users should not be unprofitable.

If any flag fires, the user should adjust pricing, scope down the product, or accept lower margins consciously.
