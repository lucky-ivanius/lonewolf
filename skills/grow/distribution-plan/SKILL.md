---
name: distribution-plan
description: Build a 30-day channel-by-channel plan to reach the first users. Use when the user wants a launch plan, marketing plan, growth plan, or "how do I get users".
---

## What this skill does

Produces a 30-day distribution plan built on channels the founder can actually reach, with a weekly cadence, per-channel success metrics, and kill criteria. The plan is a schedule of concrete actions ("post the teardown thread in r/SaaS Tuesday 9am ET"), not a strategy essay ("leverage social media").

Launching is a distribution event. The launch day is one row in this plan, not the plan.

## When to use it

- The product is live or passing `launch-checklist` and nobody knows it exists.
- The user says "how do I get users", "marketing plan", "launch plan".
- A launch happened, flopped quietly, and the user wants a real second push.

## When NOT to use it

- The product's core flow does not work yet. Route to `build-mvp` — distribution multiplies the product, including a broken one.
- No positioning exists. Route to `positioning`; every channel action needs the sentence.
- The user wants hands-on first-user tactics (DMs, onboarding calls). Route to `first-100-users` — that skill is the manual companion to this plan.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- `.lonewolf/positioning.md` (required).
- The founder's honest channel inventory: follower counts, communities they participate in, email lists, marketplace presence, friends with audiences.
- Hours per week the founder will actually spend on distribution.

## Outputs

A plan at `.lonewolf/distribution-plan.md`:

```markdown
# Distribution plan, <timestamp>

## Channel inventory (honest)
- <channel>: <reach today, e.g. "X: 340 followers", "r/indiehackers: lurker", "OKX.AI marketplace: listed">

## The bet
<2-3 channels maximum for the 30 days, and one sentence on why each can work for THIS product>

## Week 1: <theme, e.g. "warm circle + listing live">
- [ ] <day>: <concrete action> -> metric: <number to hit>
- ...

## Week 2-4: <same structure>

## Launch day (one day, not the plan)
- [ ] <the launch-platform post, the thread, the community posts — each with time and link target>

## Metrics
- North star for the 30 days: <ONE number, e.g. "25 activated users" or "5 paying">
- Per-channel check: <signups or replies attributable per channel>

## Kill criteria
- <channel>: if <metric> < <threshold> by day <N>, stop and reallocate to <channel>.

## Content bank
<the 5-10 content pieces the plan needs, each tied to a slot: teardown thread, demo clip, before/after, build-in-public update, comparison post>
```

## Workflow

1. **Inventory honestly.** List every channel with today's real reach. "We'll go viral on X" with 40 followers is not a channel; a 200-member niche Discord where the founder is known is.

2. **Pick 2-3 channels, max.** Criteria: (a) target users demonstrably there, (b) founder can produce the channel's native format consistently, (c) results measurable within days. Depth beats spray.

3. **Schedule 30 days.** Weekly themes, daily-or-every-other-day concrete actions. Every action names its artifact (the post, the DM template, the demo clip) and its metric.

4. **Plan launch day as one row.** Product Hunt / HN / community launches get one well-prepared day each — assets ready, first comment drafted, replies staffed — inside a month of compounding work, not instead of it.

5. **Set kill criteria upfront.** Each channel gets a number and a date. Channels that miss get cut without ceremony; the hours move to what worked.

6. **Build the content bank.** Write the list of pieces the schedule needs; note which the agent can draft vs which need the founder's voice (founder-voice pieces convert better in early-stage channels — mark them honestly).

7. **Writeback** and hand off: `first-100-users` for the manual outreach lane running alongside this plan.

## Quality gate (anti-slop)

- Is every channel in the bet backed by CURRENT reach or a named, credible entry path — not hoped-for virality?
- Is every action concrete enough to do without further planning (artifact + place + day)?
- Is there exactly ONE north-star number for the 30 days?
- Do kill criteria exist with real thresholds and dates?
- Does launch day occupy one day, not the whole plan?
- Did the writeback happen?

If any answer is no, the skill keeps working.

## Use in your agent

- Claude Code: `claude "/launchkit:distribution-plan <your message>"`
- Codex: `codex "/distribution-plan <your message>"`
- Grok Build: run `grok`, then `/distribution-plan <your message>` in the session
- Cursor: paste a chat message like "build my 30-day launch plan", or reference `~/.cursor/rules/distribution-plan.mdc`.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
