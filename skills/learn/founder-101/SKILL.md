---
name: founder-101
description: Teach 0-to-1 product building from scratch, including the agent economy. Use when the user is new to building products, asks what MVP/PMF/distribution mean, or wants to understand how builders earn in the agent economy.
---

## What this skill does

Teaches the vocabulary and mental models of taking a product from 0 to 1, calibrated to someone who can code (or has an agent that codes) but has never shipped a product to real users. Covers the launch loop (idea -> validate -> build -> ship -> grow -> earn), the terms that gate every later conversation (MVP, PMF, distribution, retention, unit economics, positioning), and how the agent economy changes the picture: agents as products, agents as customers, marketplaces where agents hire each other.

Output is understanding, not code, and not a plan. When the user is ready to act, hand off to the Idea phase.

## When to use it

- The user is technical but has never launched a product.
- The user asks what a term means (MVP, PMF, CAC, churn, positioning, moat).
- The user asks how the agent economy works: how agents get hired, paid, reviewed.
- The user wants to know "what happens after the code is written".

## When NOT to use it

- The user has a specific idea and wants to evaluate it. Route to `validate-idea`.
- The user wants to start building. Route to `clarify-intent`.
- The user asks a coding question. Answer it; this is not a coding tutorial.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- The user's background (what they have built, what they have shipped, what confused them).
- The specific question, if any.

## Teaching approach

1. **Anchor to what they know.** A developer understands systems; map product concepts onto system concepts. Distribution is an API the market exposes; PMF is when retention curves flatten instead of decaying to zero; pricing is load-bearing architecture, not paint.

2. **One concept per exchange.** Do not dump the whole glossary. Answer the asked question, then name the one adjacent concept that unlocks the next conversation.

3. **Concrete over abstract.** Every concept comes with a worked example at indie scale ($0-10k MRR), not a Silicon Valley anecdote. The user is one person with an agent, not a funded team.

4. **The launch loop, stated once, early:**
 - **Idea**: pick a problem someone already pays to solve, where you have an edge.
 - **Validate**: get evidence of demand before building — cheapest test first.
 - **Build**: the MVP is the smallest thing that delivers the core value end to end, not a broken demo.
 - **Ship**: launching is a gate-check, not an event.
 - **Grow**: the first 100 users come from manual, unscalable work in channels you can actually reach.
 - **Earn**: revenue is the only validation that compounds. Price from day one where possible.

5. **The agent economy layer** (teach when asked, or when the user targets an agent marketplace):
 - Agents are becoming both products (you sell an agent's work) and customers (other agents hire yours).
 - Marketplaces (such as OKX.AI) give agents identity, discovery, escrowed payments, and reputation — the boring infrastructure a solo builder used to have to earn or build.
 - What still differentiates: the quality bar of the work, the niche selection, and distribution. The marketplace solves payments, not demand.
 - The failure mode to warn about: listing a generic agent nobody searched for. Same slop, new venue.

6. **Anti-slop framing throughout.** The recurring question of every phase is "would this survive contact with a paying stranger?" Introduce it here so the later skills (roast-my-product, validate-business-model) feel like continuations, not ambushes.

## Glossary (teach on demand, one at a time)

- **MVP**: smallest product that delivers the core value end to end to a real user. Not a prototype, not a waitlist page.
- **PMF (product-market fit)**: enough users retain and pay that growth stops feeling like pushing. Observable in flat retention curves and organic pull, not in feelings.
- **Distribution**: the repeatable channel through which target users discover the product. If you cannot name the channel, you do not have one.
- **Retention**: whether users come back on day 1 / 7 / 30 without being pushed. The single best predictor of whether anything else matters.
- **Unit economics**: what one user costs you (inference, hosting, support) vs what they pay you. Margin per user, before scale fantasies.
- **Positioning**: the one sentence that names the user, the pain, and the alternative you beat. If it fits ten products, it is not positioning.
- **Moat**: what a competent copycat cannot take: audience, data, integrations, trust, speed. "First" and "better UX" are not moats.
- **CAC / LTV**: what a customer costs to acquire vs what they are worth over their lifetime. Indie rule: CAC should be near zero (distribution you own) until revenue funds paid acquisition.
- **Churn**: the rate at which paying users leave. Revenue growth with high churn is a bucket with a hole.

## Quality gate (anti-slop)

Before ending the session:

- Did I answer the actual question at the user's level, not deliver a lecture?
- Did every concept get a concrete indie-scale example?
- Did I avoid inventing statistics or attributing fake quotes?
- Did I point to the next phase skill (usually `find-next-idea`) exactly once, without pushing?

## Use in your agent

- Claude Code: `claude "/launchkit:founder-101 <your question>"`
- Codex: `codex "/founder-101 <your question>"`
- Grok Build: run `grok`, then `/founder-101 <your question>` in the session
- Cursor: paste a chat message like "explain PMF like I'm a backend dev", or reference `~/.cursor/rules/founder-101.mdc`.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
