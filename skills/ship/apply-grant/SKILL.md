---
name: apply-grant
description: Draft an ecosystem grant application with milestones and sustainability plan. Use when the user wants to apply for a grant or draft a grant application.
---


## What this skill does

Drafts an ecosystem grant application (foundation, protocol program, or sponsor-led). The skill grounds every section in the project's actual context files (idea, deploy artifacts, business model). Refuses to draft a section that is not backed by source material; flags the missing input instead.

The skill does not submit the application; it produces text the user pastes into the grant portal. The user is responsible for the legal and financial commitments of any grant.

## When to use it

- After the project has at least a testnet deploy and a clear scope of work to fund.
- When the user is preparing an ecosystem foundation grant or a sponsor-led grant program.
- For renewal applications when prior milestones were completed.

## When NOT to use it

- Pre-build, when there is no project context to ground the application.
- For investor pitches or hackathon submissions. Route to `create-pitch-deck`.
- For accelerator applications (YC and similar). Different shape; the milestones sections transfer, the rest does not.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- `.lonewolf/idea-context.md`: chosen idea, target user, scope.
- `.lonewolf/deploy-context.md`: at least one testnet deploy.
- `.lonewolf/business-model.md`: required if the grant is for a commercial product.
- The grant size the user is requesting and the timeframe.
- The team roster with prior shipping evidence (links to repos, prior projects).
- The grant program target (foundation general, protocol grant program, sponsor-led grant).

## Outputs

A `.lonewolf/grant-application.md` with the application text:

```markdown
## Grant application, <timestamp>

### Grant program
- target: <foundation | protocol program | sponsor-led>
- size requested: <USD or token amount>
- timeframe: <months>
- contact: <name and email>

### Project
- name: <project name>
- one-liner: <one sentence>
- summary: <one paragraph, 80-150 words>
- repo: <URL>
- demo: <URL or "in development">
- package id: <0x... from deploy-context.md, with env>

### Why this matters to the ecosystem
- <one paragraph naming what the ecosystem gets that it could not get otherwise>

### Why this team
- <one paragraph naming team members, prior work, why this team will deliver>

### Deliverables
- D1: <concrete deliverable, with a definition of done>
- D2:...
- D3:...

### Milestones
- M1: <yyyy-mm-dd>, <deliverables>, <payout USD>
- M2:...
- M3:...

### Budget
- engineering: <USD>
- design: <USD>
- audit: <USD>
- infra: <USD>
- contingency: <USD, max 10%>

### Public-good rationale
- <one paragraph naming the open-source license, the public artifacts, the measurable benefit>

### Sustainability
- <one paragraph naming how the project continues after the grant period, honestly>

### Notes
- <facts cited from context files>
- <claims downgraded for lack of source>
```

## Workflow

1. **Confirm the grant program**
 - A foundation general grant, a protocol grant program, or a sponsor-led grant. Each has slightly different conventions; confirm the target program and read its published guidelines before drafting.
 - Confirm the size and timeframe up front. Different sizes have different scrutiny levels.

2. **Read context**
 - Pull project name, one-liner, summary from `idea-context.md`.
 - Pull deploy artifacts from `deploy-context.md`.
 - Pull business model from `business-model.md` if commercial.
 - If any required input is missing, list what is missing and route to the relevant skill. Do not draft on a missing foundation.

3. **Draft the project section**
 - Lead with the one-liner and the package id. Reviewers respect verifiable artifacts.
 - Summary is a paragraph: who it is for, what it does, why now.

4. **Why this matters to the ecosystem**
 - Concrete, named. "Adds tooling no one has built" is generic; "first open-source order-routing library for the ecosystem's main DEX, MIT, with TypeScript clients" is specific.
 - Cite if comparable projects exist (run `competitive-landscape` first if not yet done).

5. **Why this team**
 - List members, roles, and prior shipping evidence. Links to repos, prior projects, prior grants.
 - If the team is new, name what is true: a shipping cadence so far, a domain expertise, a community track record.

6. **Deliverables and milestones**
 - Three to five deliverables. Each has a definition of done that a reviewer can verify (a deployed package, a public repo, a documented integration).
 - Milestones are dates with payouts. Each milestone bundles deliverables. Aim for three milestones for grants under 6 months, four to six for longer.

7. **Budget**
 - Line items, dollar amounts. Engineering, design, audit, infra, contingency.
 - Contingency capped at 10%. A higher contingency reads as poor planning.
 - Tie line items to the deliverables: which deliverable consumes which line.

8. **Public-good rationale**
 - Name the open-source license (MIT preferred unless there is a reason).
 - Name the public artifacts (repo, docs, examples).
 - Name the measurable benefit to the ecosystem (downstream projects will use, integrations will land, security findings will be shared).

9. **Sustainability**
 - Honest answer. If the path is "we will charge for X starting M3", say so. If the path is "we will apply for follow-on grants", say so. If the path is "we will find a buyer or shut down", say so.
 - "We will figure it out" fails this section. Reviewers read it as a flag.

10. **Writeback and hand off**
 - Append `.lonewolf/grant-application.md`.
 - Recommend `roast-my-product` to stress-test the application.
 - Surface the grant portal URL when known.

## Quality gate (anti-slop)

Before reporting done:

- Is every required input present, or were missing inputs surfaced and routed?
- Does each deliverable have a verifiable definition of done?
- Does each milestone tie a date to a payout to a deliverable?
- Is the budget line-itemed, with contingency under 10%?
- Does sustainability section give a real answer, not "we will figure it out"?
- Did the writeback happen?

If any answer is no, the skill keeps working.

## References

On-demand references (load when relevant to the user's question):

- `references/milestone-templates.md`: Milestone shapes for 3, 6, and 12 month grants.
- `references/budget-line-items.md`: Common line items and pricing assumptions.

## Use in your agent

- Claude Code: `claude "/launchkit:apply-grant <your message>"`
- Codex: `codex "/apply-grant <your message>"`
- Grok Build: run `grok`, then `/apply-grant <your message>` in the session
- Cursor: paste a chat message that includes a phrase like "draft a grant application", or load `~/.cursor/rules/apply-grant.mdc` and reference it.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
