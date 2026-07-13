---
name: launch-checklist
description: Run the go-live gates before a public launch or mainnet deploy. Use when the user wants to ship, launch, deploy to production, or go public.
---

## What this skill does

Runs the gates that separate "it works on my machine" from "strangers can use this and nothing embarrassing happens". Walks infrastructure, product, trust, and (for crypto products) on-chain gates, each with a concrete pass condition. Refuses to declare the product launched while a blocking gate fails — the skill is on the founder's side, and the founder's side is not "ship it broken".

This is the go-live gate-check, not the marketing plan. For the launch-day distribution work, route to `distribution-plan`.

## When to use it

- The product demos end to end and the user wants to go public.
- Deploying contracts to mainnet.
- The user says "launch", "go live", "ship it", "deploy to production".
- Re-run after any major change before the next announcement.

## When NOT to use it

- The core flow does not work end to end yet. Route to `build-mvp`.
- The user wants launch marketing. Route to `distribution-plan` and `landing-copy`.
- The user wants a security deep-dive. Route to `security-review` (this checklist gates on its output).

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- The project tree and its deploy configuration.
- `.lonewolf/build-context.md` and `.lonewolf/deploy-context.md` if present.
- Prior anti-slop outputs: `security-review` findings, `validate-business-model` verdict, `retention-loop` definition.

## Outputs

A gate report, and an updated `.lonewolf/deploy-context.md`:

```markdown
## launch-checklist, <timestamp>
- verdict: GO | NO-GO
- blocking failures: <list, or "none">
- non-blocking warnings: <list>
- deployed surfaces: <URL / contract addresses per environment>
- rollback plan: <one paragraph>
```

## The gates

### Infrastructure gates (blocking)

- [ ] Production build passes from a clean checkout — not from the dev machine's warm cache.
- [ ] Domain live with TLS; www and apex both resolve.
- [ ] Error tracking wired (Sentry or equivalent) and a test error visibly arrives.
- [ ] Analytics wired: page views and the ONE core-flow completion event.
- [ ] Uptime monitoring on the main URL and any API the product depends on.
- [ ] Secrets in the deploy environment, not in the repo; `.env` files gitignored and rotated if ever committed.
- [ ] Rollback path exists and has been stated in one paragraph: what gets reverted, how fast, by whom.

### Product gates (blocking)

- [ ] The core flow completes on a phone, on Fast-3G throttling, in a fresh browser session.
- [ ] Every async surface has loading, empty, and error states (spot-check the three worst).
- [ ] Sign-up/sign-in works with a brand-new account, not the founder's test account.
- [ ] OG tags, favicon, and title render correctly in a link preview (X, Telegram, Slack).
- [ ] The pricing page (if paid) states the actual price and the payment flow completes a real test charge.

### Trust gates (blocking)

- [ ] `security-review` ran and every P0 is fixed or explicitly accepted in writing.
- [ ] Terms and privacy pages exist if the product stores user data or takes payment.
- [ ] Support contact (email or form) is reachable and monitored.
- [ ] No placeholder content anywhere a user can see: lorem ipsum, fake testimonials, dead links, "coming soon" buttons that go nowhere.

### On-chain gates (blocking for crypto products; skip otherwise)

- [ ] Contracts deployed to testnet first and the core flow exercised there with a fresh wallet.
- [ ] Contract addresses recorded in `.lonewolf/deploy-context.md` per network.
- [ ] Upgrade/admin authority decided and documented (multisig, timelock, or burned) — not left on the deploy key.
- [ ] Verified source on the block explorer.
- [ ] Spending limits and kill switch tested on anything that auto-pays (sponsorship, agent wallets).

### Anti-slop gates (blocking, the Launch Kit signature)

- [ ] `validate-business-model` verdict exists. Launching with a known-broken model is allowed, but only as a written, conscious choice.
- [ ] `retention-loop` is articulated: what pulls a user back on day 7. "We'll see" fails the gate.
- [ ] The positioning sentence exists and a stranger reading the landing page can repeat it back.

### Non-blocking warnings

- Lighthouse performance below 80 on mobile.
- No sitemap / robots.txt.
- Accessibility spot-check failures (focus rings, contrast) — fix within the first week if not now.

## Workflow

1. Read the context files and prior anti-slop outputs.
2. Walk every applicable gate, verifying on disk / in the browser / on chain, not from memory. Record evidence per gate.
3. Compose the verdict: any blocking gate failed -> NO-GO with the ranked fix list; all pass -> GO.
4. On GO: write `.lonewolf/deploy-context.md`, then hand off to `distribution-plan` (the launch is a distribution event, not a technical one).
5. On NO-GO: hand off to the skill that fixes the largest gap; re-run after.

## Quality gate (anti-slop)

- Did every gate get verified with evidence (a URL, a screenshot-able state, a tx hash), not "should be fine"?
- Did the verdict actually land on GO or NO-GO, not a hedge?
- Is the rollback plan written down, not implied?
- Did the writeback to `.lonewolf/deploy-context.md` happen?

If any answer is no, the skill keeps working.

## Use in your agent

- Claude Code: `claude "/launchkit:launch-checklist <your message>"`
- Codex: `codex "/launch-checklist <your message>"`
- Grok Build: run `grok`, then `/launch-checklist <your message>` in the session
- Cursor: paste a chat message like "run the launch checklist", or reference `~/.cursor/rules/launch-checklist.mdc`.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
