# Seven roast dimensions

Walk these in order. For each, write a diagnosis only if there is a real weakness. Do not pad.

## 1. Generic positioning

Diagnostic questions:

- Is the positioning sentence interchangeable with five other projects in the catalog?
- Does it name a specific user, a specific pain, and a specific alternative?
- If you remove the chain name, is the sentence still distinct?

Example diagnosis:

> The positioning "AI-powered social app" applies to 12 other products launched this month. Name the user (indie game studios, on-chain analysts, newsletter operators) and the alternative (Discord, Substack, X) explicitly.

## 2. Unclear value

Diagnostic questions:

- Within 10 seconds of seeing the product, can a stranger say what it does and why they would care?
- Is the value claim concrete (saves N hours, makes N basis points) or abstract (better experience, faster)?
- Does the demo show value in the first 30 seconds, or does it show navigation?

Example diagnosis:

> The first paint shows a sidebar and an empty dashboard. Lead with the outcome a user gets in their first session. Move "settings" off the homepage.

## 3. Demo theater

Diagnostic questions:

- Is the demo a recorded happy path? Does it show error handling, empty states, real network latency?
- Is the data hand-curated to make the product look more lively than it is?
- Are the testnet transactions actually finalizing, or are they hand-crafted screenshots?

Example diagnosis:

> The demo cuts to a "payment confirmed" toast within 200 ms, but the real round-trip is closer to 2 seconds. Either show the loading state honestly or record against a local stub so the demo timing is real.

## 4. Missing competitive moat

Diagnostic questions:

- Why won't the next builder copy this in two weekends?
- Is the moat data, distribution, network effects, regulatory, or speed of execution?
- If "speed of execution", can the user back this with a credible track record?

Example diagnosis:

> The moat is "we shipped first". A copy is one weekend of work for a competent dev with a coding agent. Either find a real moat (data, integrations, brand) or accept that the moat is execution and lean into shipping more.

## 5. Pricing that does not make sense

Diagnostic questions:

- Does the pricing math support a sustainable margin? Does it cover inference / storage / third-party APIs / hosting at scale?
- Does the price match the payer's willingness from `business-model.md` or `will-pay.md`?
- Is the cadence (per tx, per month, per volume) right for the value cadence?

Example diagnosis:

> $5 per month does not cover inference costs at the usage the demo implies. Either raise the price, meter by usage, or cap usage per tier.

## 6. Tech that is not load-bearing

Diagnostic questions:

- Would the product still work if you swapped the flagship tech (the chain, the LLM, the protocol) for a commodity alternative?
- Would it work with no chain at all?
- For each headline integration used, is the product genuinely better with it, or is the integration a sticker?

Example diagnosis:

> The product claims to be on-chain but the only on-chain part is sign-in. The same UX runs on OAuth + a postgres database. Either make the chain load-bearing for the user's data and ship the advantages that come with that, or drop the chain claim.

## 7. Brand that is forgettable

Diagnostic questions:

- Does the name help or hurt recall? Is it pronounceable, spellable, distinctive?
- Is the visual identity distinguishable from the next 10 projects?
- Does the homepage render the same as a generic Vercel or Next.js template?

Example diagnosis:

> The name shares a stem with three other projects, the homepage uses the default Vercel font and gradient, and there is no consistent typography or color logic. Pick a single typeface, a single accent color, and one structural element that recurs.

## How to write the diagnosis

- One sentence diagnosis. Specific, not vague.
- One sentence fix. Actionable in 24 hours.
- No softening qualifiers ("might", "could", "perhaps"). Direct.
- No insults. Critique the work, not the person.
