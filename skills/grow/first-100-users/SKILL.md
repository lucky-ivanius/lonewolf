---
name: first-100-users
description: Get and keep the first 100 users with manual, unscalable tactics. Use when the user has a live product and near-zero users, or asks how to find early users.
---

## What this skill does

Runs the manual acquisition lane: finding the first 100 users one at a time, onboarding them by hand, and converting their friction into the product's roadmap. This is the "do things that don't scale" phase, made concrete: who to contact, where, with what message, and what to do after they show up.

`distribution-plan` is the broadcast lane; this is the handshake lane. Early on, the handshake lane wins.

## When to use it

- The product is live with fewer than ~100 activated users.
- The user asks "where do I find early users" or "nobody is signing up".
- Signups exist but nobody activates — the manual onboarding loop diagnoses why.

## When NOT to use it

- The product does not deliver its core value yet. Route to `build-mvp`; recruiting users into a broken flow burns the exact people you need most.
- The user wants scalable channels and content cadence. Route to `distribution-plan`.
- Users churn after activating. Route to `retention-loop`.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- `.lonewolf/positioning.md` for the sentence and the target user.
- Where target users already gather (communities, threads, marketplaces) — from the competitive map if present.
- The founder's willingness: how many conversations per week they will actually have.

## Outputs

A living document at `.lonewolf/first-users.md`:

```markdown
# First 100 users, <timestamp>

## Target profile
<the ONE specific persona from positioning, with 3 places they demonstrably gather>

## Outreach ledger
| # | Who | Where found | Contacted | Replied | Activated | Notes |
|---|-----|-----------|-----------|---------|-----------|-------|

## The message
<the 3-sentence outreach template: their context, the one-sentence value, the no-pressure ask. Personalized per send — the template is a skeleton, not a broadcast.>

## Onboarding ritual
<what happens in the first 10 minutes after someone says yes: the call/DM walkthrough, the aha moment to reach, who does what>

## Friction log
- <date>: <user> hit <friction> -> <fix shipped or backlog item>

## Weekly counts
- contacted / replied / activated / retained-week-2
```

## Workflow

1. **Sharpen the target.** One persona. If positioning says "indie founders", narrow to "indie founders who just launched on an agent marketplace and have zero reviews". Specificity is what makes lists buildable.

2. **Build the first list of 25.** Real names from real places: community members who complained about the problem, people who launched adjacent products, engaged commenters on competitor threads. Public, findable, relevant — no scraped spam lists.

3. **Write the outreach skeleton.** Three sentences: (1) their specific context — proof this is not a blast; (2) the positioning sentence in their vocabulary; (3) a low-friction ask ("can I set it up for you?" beats "check it out"). Send individually. Volume target from the founder's real capacity, not fantasy.

4. **Onboard by hand.** Every yes gets the ritual: walk them to the aha moment personally, watch where they stumble, say thank you like it matters (it does). The goal is 100 users who felt the value, not 100 rows in a database.

5. **Log friction ruthlessly.** Every stumble goes in the friction log the same day. The friction log IS the early roadmap; route recurring items to `build-mvp` slices.

6. **Close the loop weekly.** Count contacted / replied / activated / retained. A reply rate under 10% means the message or the list is wrong — fix before sending more. An activation rate under 50% of replies means onboarding is broken — fix before recruiting more.

7. **Graduate.** At ~100 activated or when manual outreach saturates, the friction log feeds `retention-loop` and the working messages feed `distribution-plan` as content seeds.

## Quality gate (anti-slop)

- Is the target profile ONE persona with named gathering places, not a demographic blur?
- Is every outreach personalized (the recipient could tell it was written for them)?
- Is the ledger real — actual names and dates, not projections?
- Does every activated user have a friction-log entry or an explicit "clean run" note?
- Are the weekly counts computed from the ledger, not vibes?

If any answer is no, the skill keeps working.

## Use in your agent

- Claude Code: `claude "/launchkit:first-100-users <your message>"`
- Codex: `codex "/first-100-users <your message>"`
- Grok Build: run `grok`, then `/first-100-users <your message>` in the session
- Cursor: paste a chat message like "help me get my first users", or reference `~/.cursor/rules/first-100-users.mdc`.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
