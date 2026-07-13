---
name: landing-copy
description: Write landing page copy that converts — hero, proof, CTA. Use when the user wants landing page copy, a hero headline, or website words.
---

## What this skill does

Writes the words for a landing page that a specific stranger reads and understands in eight seconds. Produces the full copy block: hero headline, subhead, proof section, how-it-works, pricing framing, FAQ, and CTA — all inheriting from `.lonewolf/positioning.md`. Copy only; layout and visual design belong to `frontend-design-guidelines` and `design-taste`.

## When to use it

- The product works and needs a landing page.
- The existing landing page reads like every other product in the category.
- The marketplace listing description needs buyer-facing words.
- Copy refresh after a pivot or repositioning.

## When NOT to use it

- No positioning sentence exists. Route to `positioning` first — copy without positioning is decoration on a guess.
- The user wants the page built or styled. Copy hands off to the build skills.
- The user wants launch posts / threads. That is `distribution-plan` territory.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- `.lonewolf/positioning.md` (required; refuse without it, offer `positioning` first).
- Real user quotes, metrics, or demo assets if they exist.
- The single action the page should drive (sign up, install, book, buy).

## Outputs

A copy document at `.lonewolf/landing-copy.md`:

```markdown
# Landing copy, <timestamp>

## Hero
- Headline: <max 8 words, the outcome, in the user's vocabulary>
- Subhead: <one sentence, the mechanism + the named alternative it replaces>
- CTA button: <verb + object, e.g. "Roast my product", never "Get started">
- Hero visual note: <what the screenshot/demo must show — the product at its moment of value>

## Social proof strip
<real logos, real numbers, real quotes only — or OMIT the section. Fabricated proof is a firing offense.>

## Problem section
<three lines naming the pain in the user's own words>

## How it works
<three steps max, each step = verb + concrete artifact>

## Proof / demo section
<the one flow to show, and the caption that states the outcome>

## Pricing framing
<the price, what it includes, and the anchor (what it replaces or saves)>

## FAQ (max 5)
<the real objections: "does it work with X", "what happens to my data", "why not just use <alternative>">

## Final CTA
<restate the outcome + the button>

## Banned on this page
<the ban-list from positioning.md, enforced>
```

## Workflow

1. **Read positioning.** The headline is the positioning outcome restated in the user's vocabulary. If the positioning sentence is weak, stop and route back.

2. **Write the hero first, ten candidates.** Ten headline variants, then cut to the one a distracted stranger parses in one read. Concrete beats clever every time it is close.

3. **Draft the page top to bottom** using the output schema. Every section earns its place; a section with nothing real to say gets omitted, not padded.

4. **The proof rule.** Numbers, quotes, and logos appear only if real and verifiable. A page with zero proof and honest copy beats a page with invented testimonials — and the second one is not an option here.

5. **Read-aloud pass.** Read the whole page out loud. Kill every sentence that a human would not say to another human across a table.

6. **The eight-second test.** Show only the hero to someone (or simulate a cold read): can they answer "what is it, who is it for, what do I click"? If not, rewrite the hero, not the reader.

7. **Writeback** to `.lonewolf/landing-copy.md`. Hand off to the build skills for implementation, and `distribution-plan` for the traffic that will hit this page.

## Quality gate (anti-slop)

- Does the headline state an outcome in under 8 words, without the words from the ban list?
- Is the named alternative from positioning visible on the page?
- Is every proof element real? (Zero fabricated numbers, quotes, or logos — omit over invent.)
- Is the CTA a specific verb + object, not "Get started" / "Learn more"?
- Does every FAQ answer an objection someone actually raised or predictably will?
- Did the writeback happen?

If any answer is no, the skill keeps working.

## Use in your agent

- Claude Code: `claude "/launchkit:landing-copy <your message>"`
- Codex: `codex "/landing-copy <your message>"`
- Grok Build: run `grok`, then `/landing-copy <your message>` in the session
- Cursor: paste a chat message like "write my landing page copy", or reference `~/.cursor/rules/landing-copy.mdc`.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
