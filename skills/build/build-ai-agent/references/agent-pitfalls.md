# Agent pitfalls

Common mistakes when building AI agents that act and transact, and how to avoid them.

## Hardcoded private keys and secrets

The most common and most dangerous mistake. Agent keys and API secrets end up in source code, .env files checked into git, or plaintext config.

Fix:
- Use KMS (AWS KMS, GCP KMS, Vault) for server-side agents.
- Add `.env` and any key files to `.gitignore` before the first commit.
- Rotate keys on any suspected exposure.

## No spending limits or kill switch

An autonomous agent with unrestricted access to a funded wallet or a metered API can drain it. LLM hallucinations, prompt injection, or logic bugs can trigger unexpected spending.

Fix:
- Set a per-action spending cap enforced in the agent's execution code, not in the prompt.
- Set a per-session or per-day cumulative limit.
- Implement a kill switch (a flag in env or a control record) that halts all agent actions.
- For high-value agents, require human approval above a threshold.

## Trusting LLM output for action parameters

The agent uses raw model output (amounts, addresses, URLs, function targets) directly in action execution without validation.

Fix:
- Validate every parameter before executing.
- Amounts: check against known bounds, parse as integers/BigInt, reject negative or unreasonable values.
- Addresses and URLs: validate format and allowlist destinations.
- Tools: allowlist the functions the agent is permitted to call. Never let the model construct arbitrary call targets.

## Prompt injection through tool results

Untrusted content (a web page, a user upload, another agent's message) contains instructions, and the agent follows them because instructions and data share the same context.

Fix:
- Mark tool results as data, not instructions, in the prompt structure.
- Strip or neutralize instruction-shaped content in untrusted inputs before it reaches privileged context.
- Privileged tools (spending, deletion, messaging) should never fire from a turn dominated by untrusted content without a validation step.

## Partial-failure flows

Multi-step actions built as independent calls: step 2 fails after step 1 succeeded, and the agent is in an inconsistent state (paid but not delivered, booked but not confirmed).

Fix:
- Use atomic mechanisms where the platform offers them (batched transactions, escrow, database transactions).
- Where atomicity is impossible, design explicit rollback or reconciliation logic for every partial-failure path.
- Persist step state so a crashed agent can resume rather than repeat.

## No cost accounting

The agent works, but nobody knows what a run costs. Heavy users become unprofitable silently; a retry loop burns a week of margin overnight.

Fix:
- Log tokens, API calls, and fees per run from the first day.
- Alert on cost-per-run regression, not just on total spend.
- Check the unit economics against the price in `validate-business-model`'s Q4.

## Retry loops on failing actions

An action fails deterministically (bad params, insufficient balance) and the agent retries in a loop, burning money and rate limits.

Fix:
- Simulate or validate before executing where the platform allows (dry runs, gas estimation, param validation).
- Distinguish retryable errors (network, 5xx) from deterministic ones (validation, insufficient funds). Never auto-retry deterministic failures.
- Cap retries and surface the failure to a human after the cap.

## Memory bleed across contexts

Using one memory store for different clients or tasks. Context from one client leaks into another's session — a correctness bug and a privacy incident at once.

Fix:
- Use a unique namespace per client or task type.
- Clear or archive stale memories when the agent's task changes.
- Never store one client's data where another client's queries can retrieve it.

## Polling what should be event-driven (and vice versa)

Agents that hammer an API on a 1-second timer for state that changes hourly, or agents that rely on a webhook that silently stopped arriving.

Fix:
- Prefer webhooks/subscriptions where offered, with a slow polling fallback as liveness insurance.
- Match polling intervals to how fast the state actually changes.

## Over-scoping the agent

Giving the agent permission to call any tool on any resource. This maximizes the blast radius of any bug or prompt injection.

Fix:
- Define an explicit allowlist of tools and resources.
- Log every action the agent takes, with full parameters, for audit.
- Start narrow, expand permissions only when the use case requires it.

## Shipping without a golden task set

Every prompt tweak and model upgrade changes behavior, and without a replayable eval the regressions ship to customers.

Fix:
- Maintain 5-10 golden tasks with expected outcomes.
- Replay them on every prompt, tool, or model change, before deploy.
- Track pass rate over time; a slow slide is as dangerous as a sudden break.
