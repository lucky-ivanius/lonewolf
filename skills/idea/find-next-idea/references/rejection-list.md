# Rejection list

Idea shapes to reject regardless of how well they would be executed. These fail on structure, not effort. If the user insists, record the override and the reason in `idea-context.md` — but make them say it out loud first.

## Shapes that fail on demand

- **A thin wrapper on one LLM API call.** If the product is "paste text, get better text", the model vendor's next release is your funeral. Reject unless there is proprietary data, workflow lock-in, or a distribution channel the vendor cannot reach.
- **A dashboard for data nobody acts on.** Analytics products only work when the viewer changes a decision because of them. "It would be interesting to see" is not a use case.
- **Social network cold-starts with no imported graph.** A new social product with zero seeded supply or an existing community attached dies in the empty-room phase.
- **"Uber for X" in a market with no urgency.** Marketplace mechanics need demand-side pain measured in hours, not vibes.

## Shapes that fail on economics

- **Token launchpads and meme-coin tooling.** Crowded, mercenary users, zero retention, reputational tail risk. The worst combination of all four axes.
- **Products whose unit economics depend on subsidized inference.** If margin only exists while you eat the LLM bill, the product is a fundraising deck, not a business.
- **Consumer products that need millions of users before anyone pays.** Ad-scale economics are not reachable for a solo founder's v1.

## Shapes that fail on distribution

- **Products for an audience the founder has never talked to and cannot reach.** No channel, no wedge, no product.
- **B2B tools that require a 6-month enterprise sales cycle.** A solo founder cannot outlast procurement.
- **Anything whose plan is "go viral".** Virality is an outcome, not a strategy.

## Shapes that fail on defensibility

- **A feature of an incumbent's roadmap.** If the platform you build on has this on their public roadmap, you are doing their QA for free.
- **Pure aggregation of freely available data with no transformation.** One `curl` away from being cloned; the data source can also cut you off.

## The override rule

Any rejection can be overridden if the founder has a specific, named unfair advantage that neutralizes the failure mode (e.g. "cold-start social" + the founder already runs a 50k-member community). The override must name the advantage. "I believe in it" is not an advantage.
