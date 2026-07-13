# Scoring rubric

Score each candidate on four axes, 1 to 5 each, sum to a 4-20 score. Use this to rank, not to dictate. A 16 with a clear reason to lose to a 13 is fine if the user surfaces a real reason.

## Market timing (1-5)

Is the demand visible right now?

- **5**: real revenue exists somewhere (another platform, off-chain, off-platform), users are searching, funders are funding.
- **4**: clear demand signal (community discussions, recent fundraises in the category, search trend up).
- **3**: theoretical demand. People nod when you describe it but no one is paying.
- **2**: maybe a future market, hard to validate today.
- **1**: speculative. Built on assumptions about future user behavior.

A score of 1 or 2 here means the demand story is speculative, fine for a research bet, weaker if the user wants real users at launch.

## Distribution fit (1-5)

Can THIS founder reach the target users?

- **5**: the founder already owns a channel the target user reads (community, audience, marketplace presence, existing product).
- **4**: a named, reachable channel exists and the founder has a credible plan to enter it (a marketplace listing, a partner, a niche forum they participate in).
- **3**: channels exist but the founder starts from zero in all of them.
- **2**: the audience is diffuse and expensive to reach (broad consumer, paid acquisition only).
- **1**: the plan is "go viral" or "the platform will feature us".

Reject any candidate that scores 1 unless the user names an unfair advantage that changes the math.

## Builder fit (1-5)

Can THIS user ship a v1 with AI-assisted pace? Default optimistic. AI pair-coding compresses former multi-week work into days, so do not score down just because the idea sounds big or the user is solo.

- **5**: matches the user's prior experience or stack, agent can carry most of the unfamiliar parts.
- **4**: stretch but the unknowns are scoped and named.
- **3**: doable, requires the user to learn one new primitive (e.g. first time integrating payments or an LLM pipeline).
- **2**: requires multiple genuinely new primitives at once with no canonical reference, agent will need heavy author guidance.
- **1**: requires novel research the user has to drive personally (new cryptographic protocol, training a new model, untested infrastructure), not just "lots of code".

Score 1 only on genuine research gaps, not on apparent scope. "Build a perp dex" is not a 1, "build a new ZK proof system from scratch" is. If the user has a hard external deadline and the candidate clearly needs research-grade work to even start, note it as a Risk in the writeup rather than killing the candidate outright.

## Differentiation (1-5)

What does the candidate do that the existing market does not?

- **5**: clear gap. No one is doing it. Or one team is doing it badly with public friction signals.
- **4**: one or two competitors but unclear winner; positioning is open.
- **3**: established competition but a defensible angle (audience, mechanism, surface area).
- **2**: crowded field, candidate would be the third or fourth entrant with no clear edge.
- **1**: dominant incumbent. The candidate would be a "me too".

If the differentiation is below 3, the candidate needs a specific angle the user can articulate in one sentence. "We will be cheaper" does not count.

## Score interpretation

- **17-20**: strong candidate, present.
- **13-16**: viable candidate. Often the right pick if the user has a specific reason to prefer it over a higher-scored one.
- **9-12**: ok-to-discuss. Probably not the lead.
- **4-8**: drop unless the user names a hard reason to keep.

## Tie-breaks

When two candidates score equally:

1. Prefer the one with a clearer riskiest assumption (specific, falsifiable beats vague).
2. Prefer the one with a closer match to the user's prior experience.
3. Prefer the one aligned with the target platform or hackathon, if there is one.
4. Prefer the one with a smaller v1 scope.

## Anti-pattern: scoring as cover

If the user wants to pick a candidate that the score does not favor, the right answer is to ask why and update the scores if the user surfaces a real reason. Do not let the rubric override the user's judgment when they have one. The rubric is for filtering noise, not for ruling.
