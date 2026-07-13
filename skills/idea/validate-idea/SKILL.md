---
name: validate-idea
description: Stress-test an idea for demand, competition, feasibility, and distribution fit. Use when the user has an idea and wants validation, go/no-go, or sanity check.
---

## What this skill does

Pressure tests a product idea on four dimensions, demand evidence, competition, feasibility (real research-grade unknowns only, not "this sounds like a lot of code"), and distribution fit, and produces a written go/no-go recommendation. The skill is designed to kill weak ideas early. A "no go" with a clear reason is more valuable than a "go" with hand-waving. With AI-assisted pacing, do not kill ideas on apparent scope alone, application-layer scope is rarely the bottleneck.

## When to use it

- After `find-next-idea` has written a chosen idea to `.lonewolf/idea-context.md` and the user wants a second pass before scaffolding.
- The user has an idea (their own, not from the corpus) and wants pressure testing.
- The user is mid-build and questioning whether to continue. Late validation is still useful.

## When NOT to use it

- The user has not picked an idea yet. Route to `find-next-idea`.
- The user wants competitive landscape only. Route to `competitive-landscape`.
- The user wants a brutal critique of the existing product. Route to `roast-my-product`.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- `.lonewolf/idea-context.md` if it exists.
- The idea in one or two sentences if no context file.
- Optional: any prior validation work the user has done (interviews, surveys, search-trend data).

## Outputs

A validation block appended to `.lonewolf/idea-context.md`:

```markdown
### Validation, <timestamp>
- demand evidence: <strong | medium | weak>, with one to three citations
- competition: <none | one | crowded>, with named competitors (direct and adjacent)
- feasibility: <yes | research-blocked> (research-blocked means the idea requires novel cryptographic, model-training, or infrastructure-level work the user must drive personally, not just "lots of code"; AI-assisted pace handles application-layer scope)
- distribution fit: <strong | medium | weak>
- recommendation: <go | go-with-pivot | no-go>
- reasoning: <one paragraph>
- next experiments: <bullet list of 1-3 cheap tests to run before deeper investment>
```

The recommendation is one of `go`, `go-with-pivot`, `no-go`. `go-with-pivot` means the underlying market is real but the proposed product needs a specific change.

## Workflow

1. **Read the context**
 - Open `.lonewolf/idea-context.md`.
 - If missing, ask the user to describe the idea in one or two sentences and write a minimal context file before proceeding.

2. **Walk demand evidence**
 - Has anyone paid for something similar (in this ecosystem, in an adjacent one, off-chain, off-platform)? Cite at least one.
 - Are there public discussions, fundraises, or platform priorities pointing at the category?
 - What is the falsifiable signal that demand is real? (Search trends, paying customers elsewhere, an RFP or grant program, repeated user request.)

3. **Walk competition**
 - List direct competitors. None? One? Crowded?
 - List adjacent players. What is their state (live, growing, struggling, dead)?
 - For each, what is the candidate's specific differentiation in one sentence?
 - Hand off to `competitive-landscape` for the full map if the picture is murky.

4. **Walk feasibility**
 - Identify any genuinely research-grade dependency (novel cryptography, training a new model, untested infrastructure the user must build). Application-layer scope, SDK integrations, and frontends are not research-grade, AI-assisted pace handles them.
 - If a hard external deadline exists (hackathon cutoff, grant milestone), flag any research-grade dependency as a hard feasibility risk against that deadline. Otherwise, mark feasibility yes.
 - Do not invent "cuts to scope" unless the user asks for the minimum surface. The agent can usually build the full thing.

5. **Walk distribution fit**
 - Where do the target users already gather, and does the founder have any access to that channel (an audience, a community, a marketplace, a platform listing)?
 - What does this founder or this product get that a random competitor would not (audience, data, integrations, credibility)?
 - "We'll post on X" alone is not distribution. Neither is "the platform will feature us".

6. **Compose the recommendation**
 - `go` if at least three of four dimensions are strong or medium and no dimension is weak.
 - `go-with-pivot` if one dimension is weak but the underlying category is healthy.
 - `no-go` if two or more dimensions are weak, or if the idea matches the rejection patterns in `references/no-go-criteria.md` with no override.

7. **Suggest cheap next experiments**
 - 1 to 3 tests, each runnable in under a day, that would move at least one weak dimension to medium.
 - Examples: post the idea in the target community and count engagement; run a 10-person user interview; build a clickable prototype; put up a landing page with a price on it.

8. **Writeback**
 - Append to `.lonewolf/idea-context.md`.

## Quality gate (anti-slop)

Before reporting done:

- Is the demand-evidence claim backed by at least one citation that is not "founder intuition"?
- Is the competition list real, with named projects, not abstract "competitors exist"?
- Is feasibility called out only on real research-grade unknowns, not on "this sounds big"?
- Is distribution fit judged on channels the founder can actually reach, not hypothetical virality?
- Did the recommendation actually land on `go`, `go-with-pivot`, or `no-go`, not a hedge?
- Did the writeback happen?

If any answer is no, the skill keeps working.

## References

On-demand references (load when relevant to the user's question):

- `references/demand-evidence-questions.md`: Specific questions to test demand claims.
- `references/no-go-criteria.md`: Patterns that should trigger a `no-go` regardless of enthusiasm.

Knowledge docs:

- `data/ideas/`: Curated idea corpus, useful for pivot suggestions.

## Use in your agent

- Claude Code: `claude "/launchkit:validate-idea <your message>"`
- Codex: `codex "/validate-idea <your message>"`
- Grok Build: run `grok`, then `/validate-idea <your message>` in the session
- Cursor: paste a chat message that includes a phrase like "validate my idea", or load `~/.cursor/rules/validate-idea.mdc` and reference it.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
