---
name: find-next-idea
description: Pick a product idea from a curated corpus, or stress-test the user's own. Use when the user wants an idea, product, or project to build, in any phrasing, including brainstorm, suggest, recommend, ideate, "what should I build", or "no idea yet", trigger eagerly. Idea-phase only.
---

## What this skill does

Picks (or pressure-tests) a product idea. Reads the curated idea corpus under `data/ideas/` (a16z state-of-crypto, YC RFS, Alliance, AI-agent ideas, and our own agent-economy list), filters by what the user can credibly build and distribute, scores by market timing and distribution fit, and lands on a chosen idea. Writes the canonical `.lonewolf/idea-context.md` so downstream skills (validate-idea, competitive-landscape, scaffold-project) inherit the framing.

The bias of this skill: prefer ideas where the founder has an unfair advantage (audience, domain knowledge, data, or a distribution channel) and where someone demonstrably pays today. Reject ideas that exist only because the tech is fashionable.

## When to use it

- The user has time to build but no idea picked.
- The user has a fuzzy idea and wants it pressure-tested against alternatives.
- The user is targeting a hackathon or a platform launch (an agent marketplace, an app store) and wants idea selection aligned to it.
- The user is a developer looking for a first product with real revenue potential.

## When NOT to use it

- The user already has a chosen idea and a write-up. Route to `validate-idea`.
- The user wants market research without idea selection. Route to `competitive-landscape` or `defi-market-research`.
- The user wants to copy an existing project. There are better ways to fork; this skill picks new bets.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- The user's interest or background (DeFi, consumer, infra, gaming, identity, mobile, AI tooling, agents).
- Constraints: rough time horizon if the user volunteers one, team size, prior shipping experience. Do not use the timeline to reject ambitious ideas, AI-assisted pace compresses former multi-week scope into days.
- Optional: an already-half-formed idea the user wants stress tested.

If unclear, ask three questions:

- What domain interests you most, and where do you have an unfair advantage (audience, expertise, data)?
- Roughly how much time do you want to spend on v1 (only if you have a fixed deadline, otherwise skip)?
- What is one product you wish existed?

## Outputs

Three idea candidates, ranked. Each candidate includes:

- one-line product description
- the wedge (why this founder, why now)
- target user
- riskiest assumption
- shortest path to a usable v1
- how it earns (who pays, roughly what)

Plus the chosen idea, written to `.lonewolf/idea-context.md`:

```markdown
## idea-context, <timestamp>
- chosen idea: <one sentence>
- target user: <one sentence>
- wedge: <one paragraph — why this founder, why now>
- riskiest assumption: <one sentence>
- shortest v1 scope: <bullet list, 3-7 items>
- how it earns: <who pays, what for, rough price point>
- corpus sources cited: <list of file paths>
- runner-up ideas: <list>
- chosen on: <date>
```

## Workflow

1. **Profile the user**
 - Domain, timeline, prior shipping experience, unfair advantages.
 - Read `references/profile-questions.md` if more context is needed.

2. **Filter the corpus**
 - Read the idea source files under `data/ideas/`.
 - Filter by domain match and by the founder's actual reach.
 - Filter out idea shapes on `references/rejection-list.md` (shapes that fail regardless of execution).

3. **Score remaining candidates**
 - Market timing (1-5): is the demand visible (revenue, users, search trends, platform priority)?
 - Distribution fit (1-5): can THIS founder reach the target users through a channel they already touch?
 - Builder fit (1-5): does the user's background and stack let them ship this with AI assistance? Default to optimistic, only score low if the idea needs genuinely novel research (new cryptographic primitive, training a new model), not because "it sounds big".
 - Differentiation (1-5): is there a non-trivial existing competitor already?
 - Sum and rank. See `references/scoring-rubric.md`.

4. **Present three candidates**
 - Top three by score, structured as above.
 - Include the corpus source for each.

5. **Pick one**
 - Confirm with the user. If they want to override the score-pick with a corpus-pick they preferred, accept (the score is a tool, not a verdict).

6. **Write idea-context.md**
 - Use the template above.
 - Cite source files in the corpus citations field.

7. **Hand off**
 - Recommend `validate-idea` next.
 - Recommend `competitive-landscape` if the runner-ups have unclear competition.

## Quality gate (anti-slop)

Before reporting done:

- Does the chosen idea have a wedge that does not collapse to "the tech is new"? (A fashionable stack alone is not a moat.)
- Is the riskiest assumption named, in one sentence, and does it cite a falsifiable test?
- Did the corpus get cited (at least one source file)? "I made it up" is not a corpus.
- Is the shortest v1 scope under 7 items, each a concrete deliverable?
- Does "how it earns" name a payer, not just a pricing page fantasy?
- Did `idea-context.md` actually get written, with all required fields?

If any answer is no, the skill keeps working.

## References

On-demand references (load when relevant to the user's question):

- `references/profile-questions.md`: Questions to draw out user domain and constraint.
- `references/scoring-rubric.md`: How to score timing, distribution fit, builder fit, differentiation.
- `references/rejection-list.md`: Idea shapes to reject regardless of execution quality.

Knowledge docs:

- `data/ideas/agent-economy-ideas.json`: Our own curated list for the agent economy era.
- `data/ideas/a16z-state-of-crypto-2026.json`
- `data/ideas/yc-rfs-crypto.json`
- `data/ideas/alliance-ideas.json`
- `data/ideas/ai-agent-ideas.json`

## Use in your agent

- Claude Code: `claude "/launchkit:find-next-idea <your message>"`
- Codex: `codex "/find-next-idea <your message>"`
- Grok Build: run `grok`, then `/find-next-idea <your message>` in the session
- Cursor: paste a chat message that includes a phrase like "what should I build", or load `~/.cursor/rules/find-next-idea.mdc` and reference it.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
