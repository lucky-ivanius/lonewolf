# Where to look for competitors

Sources to scan when building the direct, adjacent, and status-quo rings of the competitive map. Visit at least one source per ring before concluding the map is complete.

## Direct ring

- **Product Hunt**: search the category, sort by recent; check both launches and upcoming pages.
- **GitHub search**: `topic:<category>` and plain keyword search, sort by recently updated. Often surfaces stealth or pre-launch projects.
- **Hacker News (Algolia search)**: "Show HN" posts in the category; the comment threads name competitors for free.
- **App stores / Chrome Web Store**: if the product shape is an app or extension, the store search IS the competitive map.
- **X search**: "<category>" sorted by latest; launch announcements and build-in-public threads.
- **For crypto products — DefiLlama**: TVL, fees, and revenue by category, with every live protocol named.
- **For crypto products — DexScreener, chain explorers**: what is actually getting used, not just announced.
- **For agent products — agent marketplaces** (OKX.AI and similar): what agents are listed in the category, what they charge, what reviews say.

## Adjacent ring

- **Category aggregators**: G2, Capterra for SaaS; DefiLlama category pages for crypto; Token Terminal for revenue data.
- **Crunchbase**: fundraises in the category, signals which teams have runway to extend sideways.
- **Dune**: query-driven analytics dashboards, often community-built per crypto category.
- **The incumbent's changelog**: read the last 6 months of release notes of the biggest adjacent player. What they ship tells you where they are heading.

For each adjacent player, ask: have they announced anything in the candidate's exact spot? If yes, when? If they have not, that is either an opportunity (gap) or a signal (the spot is not worth their time — figure out which).

## Status-quo ring

- **The user's own daily tools**: what does the target user use today to solve the problem? A spreadsheet, a Discord server, a notebook, a human assistant.
- **Web2 incumbents in the same problem space**: every category has them. Trading -> Robinhood, Coinbase. Storage -> Dropbox, AWS. Identity -> Auth0, Clerk. Payments -> Stripe, Wise.
- **Forums / Reddit**: where do users complain about the current solution? The complaints are the feature list.

## How to capture findings

Per source visited, write a one-liner:

```markdown
- source: <name + URL>, scope: <direct | adjacent | status-quo>, projects found: <list>, time spent: <minutes>
```

Sources matter for two reasons: defensibility (you can show the work) and revisit cadence (categories evolve fast; the user can re-scan in 2-4 months).

## Avoid these sources

- Generic "top 10 projects" SEO listicles. Often outdated, listicle content optimizes for clicks not accuracy.
- Aggregator dashboards that have not been updated in over 90 days. Stale data is worse than no data.
- LLM-generated competitor lists without citations. Always confirm each name in a primary source.

## Time budget

A useful competitive map takes 60 to 120 minutes:

- 30 min direct ring (visit 3 sources, capture 3-7 projects).
- 30 min adjacent ring (visit 2 sources, capture 3-5 projects).
- 15 min status-quo ring (visit 1-2 sources, capture 1-3 alternatives).
- 15-30 min synthesis and write-up.

Spending more is rarely useful at this stage; the candidate has not been tested in the market yet, so deeper analysis is over-fitting.
