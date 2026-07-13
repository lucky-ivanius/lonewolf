<div align="center">
  <img height="120x" src="assets/lonewolf.svg" />

  <h1 style="margin-top:20px;">Launch Kit by Lone Wolf</h1>
</div>

**Build something meaningful. Launch it for real.**

Building was never the hard part — launching is. Launch Kit is an open library of 36 production-grade skills that give Claude Code, Codex, Cursor, or Grok Build the judgment of someone who has launched before: idea pressure-testing, anti-slop product reviews, launch gates, distribution plans, and the pricing and revenue work that decides whether a launch lands. Most of it isn't about code at all.

Built around an explicit anti-slop bar. Every phase asks one question: **would this survive contact with a paying stranger?**

## Install

Via [skills.sh](https://skills.sh):

```bash
npx skills add luckyivanius/lonewolf
```

Or as a Claude Code plugin:

```text
/plugin marketplace add luckyivanius/lonewolf
/plugin install launchkit@lonewolf
```

Use the `/launchkit:` prefix in Claude Code (`/launchkit:roast-my-product`). Use the bare skill name in Codex, Cursor, and Grok Build. Skills auto-route by intent via `skills/SKILL_ROUTER.md`, so you mostly don't need to remember names.

## Drive it

```bash
claude "/launchkit:find-next-idea what should I build?"
claude "/launchkit:validate-idea invoicing for freelance designers"
claude "/launchkit:build-mvp help me build the first slice"
claude "/launchkit:roast-my-product be brutal"
claude "/launchkit:launch-checklist am I ready to ship?"
claude "/launchkit:distribution-plan 30 days, go"
```

## The journey

Six phases. Skills hand off through the filesystem (`.lonewolf/` in your project) so your agent never loses context between them. Handoff is optional; any skill runs standalone.

| # | Phase | What happens | Skills |
|---|---|---|---|
| 01 | **Learn** | Get the 0-to-1 vocabulary without a 50-tab research session | 2 |
| 02 | **Idea** | Pressure-test what you want to build before writing a line of code | 4 |
| 03 | **Build** | Scaffold, iterate, and review with a senior product bar | 18 |
| 04 | **Ship** | Go live without skipping the gates that catch real embarrassment | 5 |
| 05 | **Grow** | Earn first users, not just a launch tweet | 4 |
| 06 | **Earn** | Turn real users into real revenue, not vanity metrics | 3 |

```
find-next-idea        writes ->  .lonewolf/idea-context.md
clarify-intent        writes ->  .lonewolf/intent.md
plan-before-code      writes ->  .lonewolf/build-plan.md
build-mvp             appends -> .lonewolf/build-context.md
launch-checklist      reads  ->  all of the above
```

## Skills (36)

### Learn (2)
`founder-101` · `learn`

### Idea (4)
`find-next-idea` · `validate-idea` · `competitive-landscape` · `defi-market-research`

### Build (18)

**Intent loop.** Plan and verify against intent, the wrapper around all build work.
`clarify-intent` · `plan-before-code` · `verify-against-intent` · `navigate-skills`

**Foundations.**
`scaffold-project` · `build-mvp` · `build-ai-agent`

**Design and frontend.** Taste, not slop.
`brand-design` · `design-taste` · `frontend-design-guidelines` · `number-formatting` · `page-load-animations`

**Anti-slop and product.** The senior product person who won't sugarcoat.
`product-review` · `roast-my-product` · `validate-business-model` · `retention-loop` · `will-real-users-pay`

**Security.** `security-review`

### Ship (5)
`launch-checklist` · `create-pitch-deck` · `marketing-video` · `video-craft` · `apply-grant`

### Grow (4)
`positioning` · `landing-copy` · `distribution-plan` · `first-100-users`

### Earn (3)
`pricing` · `revenue-review` · `get-paid`

## Anti-slop, the actual differentiator

Every skill embeds a quality gate it refuses to skip:

- `/roast-my-product` — brutal critique, investor-grade
- `/validate-business-model` — who pays, how much, why they keep paying
- `/will-real-users-pay` — cheap pricing experiments before launch
- `/retention-loop` — what pulls users back on day 7, day 30
- `/launch-checklist` — refuses to declare GO against placeholder content
- `/landing-copy` — omits the proof section rather than invent testimonials

## The agent economy

Launch Kit is agent-economy native: `build-ai-agent` covers making your product an agent that earns on marketplaces like OKX.AI (identity, escrowed tasks, reputation), `get-paid` covers marketplace settlement alongside Stripe and crypto rails, and `founder-101` teaches the economics.

## What it is, what it isn't

It is:

- A single source of launch-tested product taste for any AI agent
- Open source, MIT
- Chain-agnostic and stack-agnostic: crypto-aware where it helps, never crypto-required

It isn't:

- A webapp, dashboard, or signup flow
- A code generator (most of launching — positioning, pricing, distribution, retention — isn't code)
- Financial or legal advice

## Credits

Skill architecture and several Build/Ship skills are curated, generalized ports from [Suiperpower](https://github.com/pivyme/suiperpower) by the PIVY team (MIT) — an excellent Sui-native original. Sui-specific material was removed or generalized; the Grow and Earn phases are original to Launch Kit.

## License

[MIT](LICENSE)
