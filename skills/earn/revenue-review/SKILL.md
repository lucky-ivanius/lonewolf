---
name: revenue-review
description: Diagnose the revenue engine — where money leaks and what to fix first. Use when revenue is flat, churn is eating growth, or the user asks "why am I not making money".
---

## What this skill does

Walks the money path end to end — visitor -> signup -> activation -> paid -> retained -> expanded — with real numbers at each step, finds the biggest leak, and prescribes the one fix that moves it. The output is a diagnosis with a single priority, not a dashboard tour.

This is the Earn-phase counterpart of `product-review`: same first-principles honesty, applied to the funnel instead of the UX.

## When to use it

- Revenue exists but is flat or declining.
- Usage grows while revenue does not.
- Churn quietly cancels growth ("we add 10, we lose 9").
- A monthly money check-in, once paying users exist.

## When NOT to use it

- Zero paying users ever. Route to `will-real-users-pay` (pre-launch) or `first-100-users` (no users at all).
- The price itself is the open question. Route to `pricing`.
- Users leave because the product loses its pull. Route to `retention-loop` — this skill will detect that leak and hand off.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- Real numbers for the last 60-90 days: visitors, signups, activations, conversions, churn, MRR/ARR or per-task revenue.
- `.lonewolf/pricing.md` and `retention-loop` output if they exist.
- For agent-marketplace sellers: task volume, acceptance rate, review scores, repeat-buyer rate.

## Outputs

A diagnosis at `.lonewolf/revenue-review.md`:

```markdown
# Revenue review, <timestamp>

## The funnel (last <N> days, real numbers)
visitor -> signup: <n>% | signup -> activated: <n>% | activated -> paid: <n>% | paid -> retained M2: <n>% | expansion: <n>%

## The leak
<the ONE stage bleeding the most revenue, with the number that proves it>

## Why it leaks
<the honest mechanism: wrong traffic, weak aha, price-value gap, silent churn, ...>

## The fix
<one intervention, scoped to <2 weeks, with the metric that will prove it worked>

## Deprioritized (on purpose)
<the leaks NOT being fixed this cycle, so nobody relitigates them weekly>

## Unit economics check
- revenue per user: <X> | variable cost per user: <Y> | margin: <Z>%
- verdict: <healthy | margin problem | volume problem>
```

## Workflow

1. **Assemble the funnel with real numbers.** Refuse estimates where data exists; where data does not exist, that absence is finding #1 (wire analytics before diagnosing).

2. **Find the biggest leak by revenue impact,** not by which number looks ugliest. A 2% visitor->signup rate matters less than 40% month-2 churn in almost every real case. Benchmarks, loosely: signup->activation should clear 40-60%; activation->paid 5-15% (freemium) or 40%+ (trial); month-2 logo churn under 10% for SaaS, repeat-buyer rate over 30% for per-task services.

3. **Name the mechanism.** For the leaking stage, say why in one honest sentence. Common mechanisms:
 - Top-of-funnel leak with good downstream numbers -> wrong traffic; the fix lives in `distribution-plan`, not the product.
 - Activation leak -> the aha moment is too far from signup; fix the first 10 minutes.
 - Paid-conversion leak -> value is real but the price-value story is not; check `pricing`'s anchor.
 - Retention leak -> the product lacks a pull; hand off to `retention-loop`.
 - Margin leak -> revenue fine, costs eat it; back to `validate-business-model` Q4.

4. **Prescribe ONE fix.** Scoped to two weeks, with the single metric that will move. Everything else goes to Deprioritized — written down so the founder stops context-switching between five leaks.

5. **Run the unit economics check** while the numbers are open. Flat revenue with healthy margin is a volume problem (Grow phase); growing revenue with collapsing margin is a cost problem (fix before scaling it).

6. **Writeback, schedule the re-run.** Append to `.lonewolf/revenue-review.md`; the review repeats after the fix ships or in 30 days, whichever first.

## Quality gate (anti-slop)

- Is every funnel number real (from analytics/payment data), with missing data flagged as a finding?
- Is the leak chosen by revenue impact, with the arithmetic shown?
- Is there exactly ONE fix, scoped and measurable?
- Are the deprioritized leaks written down?
- Did the margin check run?
- Did the writeback happen?

If any answer is no, the skill keeps working.

## Use in your agent

- Claude Code: `claude "/launchkit:revenue-review <your message>"`
- Codex: `codex "/revenue-review <your message>"`
- Grok Build: run `grok`, then `/revenue-review <your message>` in the session
- Cursor: paste a chat message like "review my revenue funnel", or reference `~/.cursor/rules/revenue-review.mdc`.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
