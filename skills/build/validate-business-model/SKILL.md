---
name: validate-business-model
description: Validate a product business model (who pays, how much, why). Use when the user wants to validate monetization, pricing, or business model.
---


## What this skill does

Walks the user through five concrete questions about how the project makes money. Refuses to write a "validated business model" output if any answer is hand-wavy or speculative. The point is to force the user to either commit to a real answer or accept that they have not yet validated this dimension. Both outcomes are useful; pretending to validate when nothing is validated is the slop.

`launch-checklist` reads this skill's output and refuses to deploy if no business-model output exists.

## When to use it

- Pre-mainnet, when the project is about to be exposed to real users.
- During pitch prep when the user is about to claim a business model in front of investors or judges.
- Before applying for grants or hackathon prizes.

## When NOT to use it

- Pre-MVP, when the product is not built. Defer until there is a working v1.
- For research-only projects that the user has explicitly framed as non-commercial.
- For pre-validated existing products being ported to a new platform (the model already exists; document it instead).

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- `.lonewolf/idea-context.md` for context.
- The current product state (early build, MVP, near-launch).
- Whether the user has talked to any prospective payers.

## Outputs

A `.lonewolf/business-model.md` with explicit answers to all five questions, or an explicit `unvalidated` marker on each unanswered question. Format:

```markdown
## Business model, <timestamp>

### Q1, who pays?
- answer: <user | developer | partner | treasury | advertiser | unvalidated>
- evidence: <one sentence with citation>
- confidence: <high | medium | low>

### Q2, how much?
- pricing model: <per-tx | per-month | per-feature | per-volume | one-time | unvalidated>
- price point: <number with currency>
- evidence: <one sentence>
- confidence: <high | medium | low>

### Q3, why would they pay you instead of an alternative?
- alternative: <named competitor or status quo>
- our advantage: <one sentence>
- evidence: <one sentence>
- confidence: <high | medium | low>

### Q4, what is the unit economic at one paying user?
- revenue per user per month: <number>
- variable cost per user per month: <number including inference / storage / third-party APIs / hosting>
- gross margin: <percent>
- confidence: <high | medium | low>

### Q5, what test could falsify this in the next two weeks?
- test: <one sentence>
- success threshold: <number, e.g. "5 paying users at $X">
- planned date: <yyyy-mm-dd>

### Verdict
- validated: <yes | partial | no>
- if partial or no: which questions are open
```

## Workflow

1. **Read idea-context.md**
 - Pull the chosen idea, target user, sponsor track if any.

2. **Ask Q1**
 - "Who specifically pays you? Pick one of: user, developer, partner, treasury, advertiser. If multiple, pick the largest."
 - Reject "everyone" and "depends".

3. **Ask Q2**
 - "Concretely, how much? Per transaction in basis points? Per month subscription? One-time?"
 - Reject ranges that span more than 2x.

4. **Ask Q3**
 - "Why would the payer pay you instead of <named alternative>?"
 - Reject "we are better" and "we are cheaper" without specifics.

5. **Ask Q4**
 - "If you had ONE paying user at the price in Q2, what is your monthly margin?"
 - Force the math. Variable costs include any sponsor protocol fees the product passes through.

6. **Ask Q5**
 - "What experiment, runnable in two weeks, would tell you if Q1-Q3 are right?"
 - Reject "build the product and see". The test must be cheaper than building.

7. **Score and write**
 - For each question, mark high / medium / low confidence.
 - For each question with no concrete answer, mark `unvalidated`.
 - Write the verdict honestly. Two or more `unvalidated` = `no`. One = `partial`. Zero = `yes`.

8. **Hand off**
 - If `validated: yes`, the user is cleared for `launch-checklist` (combined with the other gates).
 - If `partial` or `no`, recommend `will-real-users-pay` to run the cheap experiment.

## Quality gate (anti-slop)

Before reporting done:

- Did every question get a concrete answer or an explicit `unvalidated`? (Hand-wavy answers count as `unvalidated`.)
- Is Q4 backed by actual math, not "should be profitable"?
- Is Q5 a test that costs less than building the product, with a date and a threshold?
- Did the writeback happen?
- If the verdict is `yes`, can the skill point at the evidence in each question?

If any answer is no, the skill keeps working.

## References

On-demand references (load when relevant to the user's question):

- `references/payer-types.md`: Definitions and examples of each payer type.
- `references/cost-checklist.md`: Variable costs to include in Q4 (sponsor protocol fees, RPC, hosting, support).

## Use in your agent

- Claude Code: `claude "/launchkit:validate-business-model <your message>"`
- Codex: `codex "/validate-business-model <your message>"`
- Grok Build: run `grok`, then `/validate-business-model <your message>` in the session
- Cursor: paste a chat message that includes a phrase like "validate my business model", or load `~/.cursor/rules/validate-business-model.mdc` and reference it.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
