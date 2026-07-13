---
name: build-ai-agent
description: Build an AI agent that does real work — tool use, memory, payments, marketplace listing. Use when the user wants to build an AI agent or agent-powered product.
---

## What this skill does

Guides the user through building an AI agent that does real work end to end. Picks the right runtime pattern, model strategy, tool surface, memory system, and — when the agent handles money — custody and spending controls. Covers listing the agent on an agent marketplace (such as OKX.AI) so it can be hired and paid. Refuses to ship an agent that cannot complete one real task end to end.

## When to use it

- The user wants to build an agent that performs tasks, calls tools, or transacts.
- The user wants their product to participate in an agent economy (be hired by users or other agents, get paid).
- The user wants agent-managed wallets or spending controls.
- The user wants persistent agent memory across sessions.
- The user wants multi-agent coordination.

## When NOT to use it

- If the user has not picked a project yet, use `find-next-idea` first.
- If the user has not scaffolded a project, use `scaffold-project` first.
- If the "agent" is really one LLM call behind a button, build it as a feature inside `build-mvp` — no agent runtime needed.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- A project (backend service and/or contracts).
- Optional: `.lonewolf/build-context.md` from `scaffold-project`. Read it if present.
- The agent's purpose and autonomy level.

If unclear, interview the user for:

- What does the agent do in one sentence?
- Does the agent act on behalf of a user (delegated) or independently (autonomous)?
- Does the agent need persistent memory across sessions?
- Does the agent manage funds? If yes, what are the custody constraints and loss bounds?
- Will the agent be listed for hire on a marketplace, and who pays it?

## Outputs

- A working runtime loop with a bounded tool surface.
- Wallet or payment setup with spending limits, if the agent handles money.
- Memory integration, if the agent needs persistence.
- An eval harness with a golden task set.
- Append to `.lonewolf/build-context.md`:

 ```markdown
 ## build-ai-agent session, <timestamp>
 - agent purpose: <one sentence>
 - runtime: <single-loop | workflow | multi-agent>
 - model strategy: <one model | router | local>
 - custody: <none | user-delegated | agent wallet with limits>
 - memory: <none | session | persistent store>
 - marketplace: <none | which, listing status>
 - open issues: <list>
 ```

## Decision table: pick agent architecture

| Use case | Runtime | Custody | Memory | Coordination |
|---|---|---|---|---|
| Task-for-hire service agent | Single loop + queue | Payments via marketplace escrow | Per-client namespace | Marketplace protocol |
| Trading bot (server-side) | Scheduled loop | Agent wallet, hard limits | Persistent store | Events/webhooks |
| User-facing assistant | Request-response loop | User-delegated, per-action approval | Per-user store | None |
| Back-office automation | Workflow (steps, retries) | None (no funds) | Job state only | Queue |
| Multi-agent pipeline | Orchestrator + workers | One treasury, per-agent budgets | Shared store, namespaced | Task queue / bus |

Confirm the architecture with the user before writing code. If the use case does not fit a row, compose from the columns.

## Workflow

1. **Context gathering**
 - Read `.lonewolf/build-context.md` if it exists.
 - Confirm the agent's purpose in one sentence.
 - Determine autonomy level: delegated (user approves each action) vs fully autonomous.

2. **Pick architecture**
 - Walk through the decision table above with the user.
 - Confirm runtime, model strategy, custody, memory, and coordination choices.
 - If the agent manages real funds, require explicit discussion of custody risk, loss bounds, and kill switches before any code.

3. **Runtime and tool surface**
 - Define the tools as typed functions with least-privilege access. Allowlist, never "the agent can call anything".
 - Every tool that spends money or mutates external state gets its own limit and audit log line.
 - Untrusted content (web pages, user uploads, other agents' messages) must not be able to steer privileged tools — separate it from instructions in the context.

4. **Model strategy**
 - Default to the strongest model for the core reasoning step and a cheap model for classification/routing steps.
 - Log tokens and cost per run from day one; cost-per-task is a product number, not an ops detail.

5. **Custody and payments (if applicable)**
 - Delegated: the user's wallet signs; the agent only proposes.
 - Agent wallet: keys in env/KMS, never in source. Per-transaction cap, daily cap, kill switch. Fund minimally.
 - Marketplace escrow: prefer the marketplace's payment/escrow protocol over rolling your own.

6. **Memory (if applicable)**
 - Session memory: keep in the job record.
 - Persistent memory: one namespace per client or task type; memories from one context must not bleed into another.

7. **Marketplace listing (if applicable)**
 - Write the service description as a buyer would read it: what goes in, what comes out, price, turnaround.
 - Wire the marketplace's task lifecycle (accept, deliver, dispute) before polishing anything else — liveness beats features on a marketplace.

8. **Test end to end**
 - The agent must complete at least one real task end to end in a staging environment (real model, real tools, real payment flow on testnet if funds are involved).
 - Build a golden task set (5-10 tasks with expected outcomes) and replay it after every prompt or model change.
 - If multi-agent, verify two agents complete a handoff through the real mechanism.

9. **Writeback**
 - Append session details to `.lonewolf/build-context.md`.
 - List open issues, especially custody risks if the agent manages funds.

10. **Closing handoff**
 - If `.lonewolf/intent.md` exists and the session was non-trivial, recommend `verify-against-intent` next so drift is caught before shipping.
 - If the agent will charge money, recommend `pricing` and `will-real-users-pay` before the listing goes live.

## Quality gate (anti-slop)

Before reporting done, the skill asks itself the following and refuses to declare success if any answer is no:

- Has the agent completed at least one real task end to end (not a mocked run)?
- Is the tool surface an explicit allowlist with least privilege?
- If the agent manages funds, is there a spending cap AND a kill switch?
- Are keys and secrets in env/KMS, never hardcoded or committed?
- Does a golden-task replay exist, and does it pass?
- Is cost per run measured and compatible with the intended price?
- Is untrusted content separated from instructions in the context?
- Is the agent's scope clearly bounded (what it can and cannot do)?

If any answer is no, the skill reports the gap and works through it before claiming the session is complete.

## References

On-demand references (load when relevant to the user's question):

- `references/agent-pitfalls.md`: Common mistakes when building AI agents that act and transact.

## Use in your agent

- Claude Code: `claude "/launchkit:build-ai-agent <your message>"`
- Codex: `codex "/build-ai-agent <your message>"`
- Grok Build: run `grok`, then `/build-ai-agent <your message>` in the session
- Cursor: paste a chat message that includes a phrase like "build an AI agent", or reference `~/.cursor/rules/build-ai-agent.mdc`

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
