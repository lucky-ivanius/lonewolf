---
name: positioning
description: Land the one-sentence positioning — who it's for, what it beats, why now. Use when the user wants positioning, messaging, a tagline, or "how do I describe this".
---

## What this skill does

Forces the product into one sentence that names a specific user, a specific pain, and a specific alternative it beats — then stress-tests that sentence until a stranger could repeat it back. Everything downstream (landing copy, launch posts, the marketplace listing) inherits this sentence; getting it wrong here means rewriting everything later.

## When to use it

- Before writing any landing copy or launch post.
- The user cannot describe the product without a paragraph.
- The current description fits ten other products ("AI-powered platform for...").
- A pivot changed who the product is for.

## When NOT to use it

- No validated idea yet. Route to `validate-idea` — positioning an unvalidated idea is decorating a guess.
- The user wants the landing page written. Route to `landing-copy` (which requires this skill's output).
- The user wants a name or visual identity. Route to `brand-design`.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- `.lonewolf/idea-context.md` (competitive landscape and validation blocks if present).
- The user's current best attempt at describing the product.
- Who has actually used it so far, and what they said.

## Outputs

A positioning block written to `.lonewolf/positioning.md`:

```markdown
# Positioning, <timestamp>

## The sentence
<product> helps <specific user> <get specific outcome> without <the pain of the named alternative>.

## Proof points (max 3)
1. <observable fact that backs the sentence>
2. ...

## The named alternative
<what the user does today — a competitor, a spreadsheet, doing nothing — and the one reason they'd switch>

## Why now
<the thing that makes this possible or urgent today, in one sentence>

## Anti-positioning
- We are NOT <adjacent category people will wrongly file this under>.
- We do NOT serve <audience it is tempting to claim>.

## Words we use / words we ban
- Use: <the target user's own vocabulary, from real quotes if available>
- Ban: <generic filler this product must never say: "revolutionary", "seamless", "AI-powered" as a lead, ...>
```

## Workflow

1. **Read the context.** Idea context, validation verdicts, competitor map. If a `roast-my-product` verdict exists, read the positioning dimension's findings first.

2. **Extract the raw material.** Three questions to the user:
 - Who got real value from this so far — one actual person or segment, not "developers"?
 - What did they stop doing (or stop paying for) because of it?
 - What made them try it now instead of last year?

3. **Draft the sentence.** Fill the template: `<product> helps <user> <outcome> without <pain of alternative>`. The alternative must be named. "Without wasting time" is banned; "without re-typing every invoice into QuickBooks" passes.

4. **Stress-test it.**
 - **Swap test**: put a competitor's name in place of the product. If the sentence still holds, it is not positioning.
 - **Laugh test**: would a sophisticated person in this market nod or laugh?
 - **Repeat test**: say it once to someone cold; can they repeat it 5 minutes later?
 - **Exclusion test**: does it clearly NOT apply to someone? Positioning that excludes nobody attracts nobody.

5. **Write the anti-positioning.** Name the adjacent categories to refuse. This is what keeps the landing page and the listing from drifting back into generic.

6. **Writeback** to `.lonewolf/positioning.md` and hand off to `landing-copy` when the user is ready.

## Quality gate (anti-slop)

- Does the sentence name a specific user, outcome, AND alternative? All three or it fails.
- Does it survive the swap test against the nearest competitor?
- Is every proof point observable today, not aspirational?
- Is "why now" a fact about the world, not a fact about the founder's enthusiasm?
- Did the writeback happen?

If any answer is no, the skill keeps working.

## Use in your agent

- Claude Code: `claude "/launchkit:positioning <your message>"`
- Codex: `codex "/positioning <your message>"`
- Grok Build: run `grok`, then `/positioning <your message>` in the session
- Cursor: paste a chat message like "help me position this product", or reference `~/.cursor/rules/positioning.mdc`.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
