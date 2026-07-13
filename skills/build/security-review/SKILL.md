---
name: security-review
description: Run a structured security review on a product (STRIDE, OWASP, supply chain, key management). Use when the user wants a security audit, threat model, or hardening pass before launch.
---

## What this skill does

Runs a structured infrastructure security audit on a project. Walks through STRIDE threat modeling, OWASP-mapped checks, dependency supply chain verification, API hardening, key management, and frontend security. Produces a findings report with severity ratings and a remediation plan. Every P0 finding must have a fix or an accepted-risk decision before the audit is declared complete.

## When to use it

- The user wants a security review of their full application (contracts + frontend + backend + infra).
- The user is preparing for an external security audit.
- The user says "threat model", "STRIDE", "OWASP", or "security audit".
- The user wants to harden their app before public launch or mainnet deployment.
- The user wants a supply chain or dependency audit.

## When NOT to use it

- If the user has not scaffolded a project yet, use `scaffold-project` first.
- If the user wants a UX review, use `product-review`.
- If the user wants to ship, use `launch-checklist` (which gates on this skill's P0s).

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- A project with at least one of: smart contracts, a frontend, a backend API, or deployment config.
- Optional: `.lonewolf/build-context.md` from prior skills. Read it if present.
- Optional: deployment target (staging, production, testnet, mainnet) and endpoints in use.

If the project scope is unclear, interview the user for:

- What components exist? (contracts, frontend, backend API, workers, agents)
- What auth mechanism is in use? (OAuth, passkeys, wallet signatures, API keys)
- Are there admin roles or privileged operations?
- What third-party services does the app call? (LLM APIs, RPC nodes, storage, data providers)
- Is this pre-launch or already live?

## Outputs

- A structured findings report appended to `.lonewolf/build-context.md` with severity levels (P0 critical, P1 high, P2 medium, P3 low).
- A remediation plan with concrete fix instructions for each P0 and P1 finding.
- Append to `.lonewolf/build-context.md`:

 ```markdown
 ## security-review session, <timestamp>
 - scope: <components audited>
 - findings: P0=<n> P1=<n> P2=<n> P3=<n>
 - P0 findings resolved: <yes | no, list remaining>
 - threat model: STRIDE completed for <components>
 - supply chain: <clean | issues found>
 - open issues: <list>
 ```

## Workflow

### 1. Context gathering

- Read `.lonewolf/build-context.md` if it exists.
- Inventory the project: list contract packages, source directories, backend code, config files, deployment manifests.
- Identify the attack surface: public endpoints, admin functions, external integrations, user-facing APIs, webhook receivers.

### 2. STRIDE threat model

For each component, walk through the six STRIDE categories. See `references/security-checklist.md` for the full table.

| Category | Question |
|---|---|
| **S**poofing | Can an attacker impersonate a user or admin? |
| **T**ampering | Can an attacker modify stored state, transactions, or API requests? |
| **R**epudiation | Can actions be denied without audit trail? |
| **I**nformation disclosure | Can sensitive data leak from storage, API responses, or frontend state? |
| **D**enial of service | Can an attacker exhaust rate limits, quotas, or paid resources (inference, gas, storage)? |
| **E**levation of privilege | Can a user escalate to admin via missing auth checks or leaked credentials? |

Document findings per component. Assign severity.

### 3. Authentication and session audit

- Check auth mechanism: session handling, token validation, wallet signature verification, API key validation.
- Verify session expiry and refresh logic. Short-lived tokens must have bounded TTL.
- Check for user enumeration in error messages.
- Verify rate limiting on auth endpoints.

### 4. Authorization audit

- List all privileged operations (admin functions, treasury or billing access, config changes).
- Verify each is enforced server-side (or on-chain), not just hidden in the UI.
- Check object-level access: can user A read or mutate user B's records by changing an ID?
- For contracts: verify privileged functions require the correct capability/role, and shared state cannot be mutated by unauthorized callers.

### 5. Input validation

- Check API endpoints and contract entry points: are all parameters validated (bounds, types, sizes)?
- Check frontend inputs: is server-side validation present, not just client-side?
- Check for injection vectors (SQL, command, path traversal) and — for LLM-backed products — prompt injection: does untrusted content reach the model with tool access or privileged context?
- For sponsored/relayed transactions: can a user inject unexpected calls into what the sponsor signs or pays for?

### 6. Dependency supply chain audit

- Run `npm audit` (or equivalent) on the project. Flag high and critical findings.
- Check that all dependencies are pinned via a committed lockfile; contract dependencies pinned to a specific rev or tag, not a floating branch.
- Verify contract addresses / package IDs of on-chain dependencies against canonical docs.
- Check for known-compromised or abandoned dependencies.
- See `references/supply-chain-audit.md` for the full checklist.

### 7. External service and API security

- Identify all external endpoints in use (LLM APIs, RPC nodes, storage, data providers).
- Check for hardcoded endpoints that could be MITM'd; prefer configured, TLS-verified endpoints.
- Verify API keys are not committed to source and are scoped to least privilege.
- Check CORS configuration on any custom backend.
- Verify rate limiting, timeout, and error handling for third-party failures.

### 8. Key and secret management

- Check how private keys, mnemonics, and API secrets are handled (never in source, never in logs, never in client bundles).
- Verify `.env` files are in `.gitignore`.
- Check spending limits on anything that auto-pays: sponsored gas, metered APIs, agent wallets. Unbounded sponsorship = drain attack.
- Verify admin credentials and upgrade authority are held deliberately (multisig or documented single holder), not left wherever the deploy script put them.

### 9. Frontend security

- Check for XSS vectors: is user input rendered without escaping?
- Check for CSRF protection on state-changing requests.
- Verify Content Security Policy headers.
- Check that wallet or auth integration does not expose private keys or session tokens.
- Verify that sensitive data is not cached in localStorage without encryption.

### 10. Remediation plan and writeback

- Compile all findings into a severity-ordered list.
- For each P0 and P1 finding, write a concrete fix with code or config changes.
- For P2 and P3, document the finding and recommended fix timeline.
- Append the session record to `.lonewolf/build-context.md`.

### 11. Closing handoff

- If `.lonewolf/intent.md` exists and the session was non-trivial (new component, new integration, or material changes to public surfaces), recommend `verify-against-intent` as the next step so drift is caught before shipping.
- If no `intent.md` exists and the session was non-trivial, surface that gap once: offer `clarify-intent` to backfill, do not force it.

## Quality gate (anti-slop)

Before reporting done, the skill asks itself the following and refuses to declare success if any answer is no:

- Was every component in the project inventoried and audited?
- Did the STRIDE threat model cover all six categories for each component?
- Does every P0 finding have either a fix committed or an explicit accepted-risk decision from the user?
- Were privileged operations checked for server-side (or on-chain) enforcement, not UI-only?
- Was the dependency supply chain actually checked (not just assumed clean)?
- Were spending limits verified on anything that auto-pays (sponsorship, metered APIs, agent wallets)?
- Is the findings report written to `.lonewolf/build-context.md`, not just discussed verbally?

If any answer is no, the skill reports the gap and works through it before claiming the audit is complete.

## References

On-demand references (load when relevant to the user's question):

- `references/security-checklist.md`: STRIDE categories with web and crypto items, OWASP top 10 mapped to product patterns.
- `references/supply-chain-audit.md`: npm audit workflow, dependency pinning, contract address verification.

External docs (fetch at runtime for the latest guidance):

- OWASP Top 10: https://owasp.org/Top10/
- OWASP API Security: https://owasp.org/API-Security/editions/2023/en/0x11-t10/
- OWASP Cheat Sheet Series: https://cheatsheetseries.owasp.org/

## Use in your agent

- Claude Code: `claude "/launchkit:security-review <your message>"`
- Codex: `codex "/security-review <your message>"`
- Grok Build: run `grok`, then `/security-review <your message>` in the session
- Cursor: paste a chat message that includes a phrase like "security audit" or "threat model", or load `~/.cursor/rules/security-review.mdc` and reference it.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
