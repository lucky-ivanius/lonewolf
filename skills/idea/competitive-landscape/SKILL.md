---
name: competitive-landscape
description: Map competitors for an idea, direct and adjacent. Use when the user wants a competitive landscape, competitor analysis, or to find their edge.
---

## What this skill does

Builds a structured competitive map for a candidate idea. Three rings: direct competitors (same product, same audience), adjacent players (same problem, different product shape or different audience), and the status quo (whatever users cobble together today — spreadsheets, Discord, a web2 incumbent, doing nothing). For each ring, lists the players, their stage, what they do well, what they miss, and the candidate's angle against them.

Output is a defensible angle (one sentence) backed by the map. Without a defensible angle, the candidate is "me too" and the skill says so.

## When to use it

- Mid-validation: the user has an idea and wants to see what already exists.
- Pre-pitch: the user has a draft pitch and wants to substantiate "we are different from X" claims.
- Post-launch: a competitor surfaced and the user wants to re-establish positioning.

## When NOT to use it

- The user has not picked an idea. Route to `find-next-idea`.
- The user wants validation overall, not just competition. Route to `validate-idea` (which calls this skill internally).

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- `.lonewolf/idea-context.md` if it exists.
- The category one-liner (e.g. "invoicing for freelance designers", "agent-to-agent payments infra", "on-chain loyalty for coffee shops").
- Optional: a list of competitors the user already knows about.

## Outputs

A competitive block appended to `.lonewolf/idea-context.md`:

```markdown
### Competitive landscape, <timestamp>

#### Direct competitors
- <project>: <stage>, <one-line summary>, <strength>, <gap>
- ...

#### Adjacent players
- <project>: <what they do>, <why they have not moved into this exact spot>
- ...

#### Status quo
- <tool or behavior>: <what users do today>, <where it falls short for the target user>
- ...

#### Defensible angle
- <one sentence stating the candidate's specific differentiation, naming at least one competitor it explicitly beats and how>

#### Risks from competition
- <if any incumbent could trivially extend into the candidate's territory, name them and the response>
```

## Workflow

1. **Define the category**
 - One sentence: what does the candidate do, and for whom?
 - Identify the closest comparable category (e.g. "lending" -> Aave, Compound in crypto; affirm-style BNPL off-chain).

2. **Map the direct ring**
 - Search launch platforms, app stores, GitHub, and category aggregators (see `references/where-to-look.md`).
 - List 3-7 named projects. For each: stage (idea, beta, live, dead), one-line summary, strength, observable gap.

3. **Map the adjacent ring**
 - Same problem on a different platform, or a neighboring product that could extend sideways.
 - For each adjacent player, ask: why have they not moved into this exact spot? (Often: focus, audience mismatch, margin too small for them — which can be the candidate's opening.)

4. **Map the status quo**
 - What do target users actually do today: a spreadsheet, a group chat, a web2 SaaS, a manual process, nothing.
 - For each: what does it offer that is hard to beat (free, familiar, already installed)? What does it fail at that the target user actively complains about?

5. **Identify the defensible angle**
 - One sentence. Names a specific competitor and a specific way the candidate beats them.
 - Tests: would a sophisticated person in this market nod, or laugh? If it does not pass the laugh test, sharpen. Apply `references/defensibility-tests.md`.

6. **Identify the risk**
 - Could a direct incumbent extend into this space in a single sprint? If yes, the moat is timing or distribution, not technology. Say so.
 - Could a well-funded adjacent player clone this in a quarter? If yes, the moat must be something they cannot copy: audience, data, integrations, or speed of iteration. Name which.

7. **Writeback**
 - Append to `.lonewolf/idea-context.md`.

## Quality gate (anti-slop)

Before reporting done:

- Is each direct competitor named with a real project, not a placeholder ("various invoicing tools")?
- Does the defensible angle name a specific competitor and a specific advantage, not generic phrases ("better UX", "faster")?
- Is the status quo listed (most products are competing with a spreadsheet or a group chat more than they realize)?
- Did the analysis honestly answer "could the incumbent trivially extend into this space"? If the answer is "we are not sure", that is a yellow flag worth recording.
- Did the writeback happen?

If any answer is no, the skill keeps working.

## References

On-demand references (load when relevant to the user's question):

- `references/where-to-look.md`: Sources for direct, adjacent, and status-quo mapping.
- `references/defensibility-tests.md`: Tests to apply to the candidate's angle.

## Use in your agent

- Claude Code: `claude "/launchkit:competitive-landscape <your message>"`
- Codex: `codex "/competitive-landscape <your message>"`
- Grok Build: run `grok`, then `/competitive-landscape <your message>` in the session
- Cursor: paste a chat message that includes a phrase like "competitive landscape", or load `~/.cursor/rules/competitive-landscape.mdc` and reference it.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
