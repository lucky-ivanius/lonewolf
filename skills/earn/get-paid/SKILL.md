---
name: get-paid
description: Wire up payments — fiat rails, crypto rails, or agent-economy earnings. Use when the user needs to accept money: Stripe, crypto checkout, or marketplace/escrow payouts.
---

## What this skill does

Wires the rail that moves money from the buyer to the founder, matched to what is being sold: fiat subscriptions and one-time charges, crypto payments, or agent-economy earnings (marketplace escrow, per-task settlement). Covers the unglamorous parts that actually break: webhooks, failed payments, refunds, payout custody, and the reconciliation view that says who paid for what.

## When to use it

- The price is decided and nothing collects it yet.
- Moving from "DM me to pay" to a real checkout.
- Listing an agent service that needs per-task settlement on a marketplace.
- Payments exist but webhooks/failed-charge handling are duct tape.

## When NOT to use it

- The price is undecided. Route to `pricing` — rails ship after the number.
- The question is "would anyone pay". Route to `will-real-users-pay`.
- Deep payment-security audit. `security-review` covers the webhook and custody checks; this skill wires them.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- `.lonewolf/pricing.md` (the model determines the rail).
- Where buyers are: card-comfortable web users, crypto-native, or other agents on a marketplace.
- The founder's entity/jurisdiction situation, as far as they know it (flag, don't lawyer).

## Outputs

Working payment rails, plus `.lonewolf/payments.md`:

```markdown
# Payments, <timestamp>
- rail(s): <stripe | crypto checkout | marketplace escrow | combination>
- what is charged: <product/tier/task -> price>
- webhook state: <events handled, verified how>
- failure handling: <dunning / retry / task-refund policy>
- payout custody: <where money lands, who holds keys/accounts, sweep policy>
- reconciliation: <where the who-paid-for-what view lives>
- open compliance flags: <tax registration, KYC thresholds — flagged for a professional, not resolved here>
```

## Decision table: pick the rail

| Selling | Buyer | Rail |
|---|---|---|
| SaaS subscription / one-time | Web users with cards | Stripe (or Paddle/LemonSqueezy as merchant-of-record if tax handling is the bigger fear) |
| Digital product, crypto-native audience | Wallet holders | Crypto checkout: direct transfer with verification, or a processor; stablecoins first, volatile assets are a treasury decision not a checkout default |
| Agent service, per-task | Humans or other agents on a marketplace | The marketplace's escrow/settlement protocol — never a DIY escrow |
| Hybrid | Mixed | Start with ONE rail where the first 10 buyers are; add the second at demonstrated demand |

## Workflow

1. **Confirm the rail from pricing + buyer.** One rail first. Two rails at launch doubles the failure surface for buyers who have not arrived yet.

2. **Wire the happy path.** Checkout -> charge -> access granted, in test mode end to end. For marketplace settlement: listing -> task accepted -> delivery -> settlement, exercised on the marketplace's test flow if one exists.

3. **Wire the webhooks properly.** Signature verification from day one (this is the #1 audited miss). Handle the minimum real set: payment succeeded, payment failed, subscription canceled / task disputed. Idempotent handlers — webhooks arrive twice.

4. **Handle failure like revenue depends on it (it does).** Card declines get a retry + email (dunning); crypto underpayments/overpayments get a defined policy; disputed tasks follow the marketplace's dispute flow with evidence attached. Write the refund policy in one paragraph and honor it mechanically.

5. **Decide payout custody.** Fiat: which account, and who has access. Crypto: which wallet, hot-vs-cold split, and a sweep policy so the checkout wallet never accumulates a treasury. Agent wallets that receive earnings get the same spending-limit treatment `build-ai-agent` mandates.

6. **Build the reconciliation view.** One place (a dashboard, a table, even a queried spreadsheet) answering: who paid, for what, when, and does access/delivery match. Revenue you cannot reconcile is revenue you cannot debug.

7. **Test with a real charge.** One real transaction at the real price on the live rail (refund it after if needed). Test mode passing is not the gate; money moving is.

8. **Flag compliance, don't play lawyer.** Note tax registration thresholds, KYC obligations, and jurisdiction questions in `open compliance flags` and tell the user which professional to ask. Do not silently skip; do not improvise legal advice.

9. **Writeback** and hand off: `launch-checklist` re-run if this changed the live product; `revenue-review` in 30 days.

## Quality gate (anti-slop)

- Did a real charge complete on the live rail (not just test mode)?
- Are webhook signatures verified and handlers idempotent?
- Does every failure path (decline, dispute, refund) have a defined, written behavior?
- Is payout custody decided deliberately, with limits on anything automated?
- Does the reconciliation view exist and match the test transactions?
- Are compliance unknowns flagged in writing rather than ignored?

If any answer is no, the skill keeps working.

## Use in your agent

- Claude Code: `claude "/launchkit:get-paid <your message>"`
- Codex: `codex "/get-paid <your message>"`
- Grok Build: run `grok`, then `/get-paid <your message>` in the session
- Cursor: paste a chat message like "wire up payments", or reference `~/.cursor/rules/get-paid.mdc`.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
