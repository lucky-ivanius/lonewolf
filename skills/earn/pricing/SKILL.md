---
name: pricing
description: Set the actual price and packaging — tiers, metering, anchors. Use when the user needs to decide what to charge, structure tiers, or fix underpricing.
---

## What this skill does

Turns willingness-to-pay evidence into a shipped price: the number, the packaging (tiers, metering, one-time vs subscription), the anchor it is framed against, and the pricing page logic. Where `will-real-users-pay` runs the experiments, this skill makes the call — and defends it against the founder's instinct to charge too little.

## When to use it

- Willingness-to-pay evidence exists and the price must be decided.
- The product is free and needs to start charging.
- Usage is growing but revenue is flat (packaging problem, usually).
- An agent service needs per-task pricing for a marketplace listing.

## When NOT to use it

- Zero evidence anyone would pay. Route to `will-real-users-pay` first — pricing a product nobody wants is arranging furniture in a house with no foundation.
- The unit economics are unknown. Route to `validate-business-model` Q4; price must clear cost.
- The user needs payment infrastructure. Route to `get-paid`.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- `will-real-users-pay` experiment results, if they exist (in `.lonewolf/`).
- Variable cost per user/run from `validate-business-model` Q4.
- Competitor and status-quo prices from the competitive map.
- What the product saves or replaces, in the buyer's money or hours.

## Outputs

A pricing decision at `.lonewolf/pricing.md`:

```markdown
# Pricing, <timestamp>

## The price
<the number(s), per tier or per unit, in the currency users will see>

## The model
<subscription | usage-metered | per-task | one-time | hybrid> — and the one-sentence reason this model fits how value is delivered

## Tiers (if any, max 3)
- <tier>: <price> — <the ONE capability that defines it> — for <who>

## The anchor
<what the buyer compares this to: the tool it replaces, the hours it saves, the fee it avoids. Stated as the pricing page will state it.>

## Floor check
- variable cost per <user/run>: <X> -> gross margin at this price: <Y>%
- heavy-user scenario: <still profitable? at what usage does margin break?>

## The walk-away
<the price below which we do not sell, and what we say instead of discounting>

## Review trigger
<the signal that forces a repricing: conversion > N%, zero price objections in 20 sales, a competitor move>
```

## Workflow

1. **Gather the evidence.** Experiment results, cost floor, anchors. If all three are missing, stop and route back — this skill decides, it does not guess.

2. **Pick the model from the value shape.**
 - Continuous value (a tool used weekly) -> subscription.
 - Spiky value (a task done occasionally) -> per-task or usage. This is the default for agent-marketplace services.
 - One-time transformation (a migration, an audit) -> one-time with a floor.
 - Value scales with usage -> metered, with a cap so heavy users stay profitable (check the Q4 heavy-user scenario).

3. **Set the number against the anchor, not the cost.** Cost sets the floor; the anchor sets the number. A report that saves a $500 consultant hour is not $9. Rule of thumb: price at 10-25% of the measurable value delivered, then check it clears 70%+ gross margin.

4. **Fight the underpricing reflex.** Indie founders default to cheap out of fear, and cheap reads as low-quality to serious buyers — the anti-slop bar applies to the price too. If the evidence supports a range, ship the top of it; discounting down is easy, raising is hard.

5. **Package in at most 3 tiers.** Each tier is defined by one capability difference the buyer understands, not a feature matrix. Solo products often need exactly one price — that is allowed and often right.

6. **Write the walk-away.** Decide now what happens when someone says "too expensive": the answer is a smaller scope or a no — not a discount that reprices every past and future customer.

7. **Writeback** to `.lonewolf/pricing.md`; hand off to `get-paid` to wire the rails, `landing-copy` to update the pricing framing.

## Quality gate (anti-slop)

- Is the price justified against an anchor AND cleared against the cost floor?
- Does the heavy-user scenario stay profitable?
- Is every tier defined by one comprehensible difference?
- Is there a walk-away line, in writing?
- Is the review trigger a measurable signal, not "later"?
- Did the writeback happen?

If any answer is no, the skill keeps working.

## Use in your agent

- Claude Code: `claude "/launchkit:pricing <your message>"`
- Codex: `codex "/pricing <your message>"`
- Grok Build: run `grok`, then `/pricing <your message>` in the session
- Cursor: paste a chat message like "help me price this", or reference `~/.cursor/rules/pricing.mdc`.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
