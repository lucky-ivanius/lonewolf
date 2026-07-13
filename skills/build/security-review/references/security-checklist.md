# Security checklist: STRIDE + OWASP for products

Source: OWASP Top 10 (2021), OWASP API Security Top 10 (2023), adapted for modern product patterns (web, AI, crypto).

## STRIDE categories

### Spoofing (identity)

| Check | Detail |
|---|---|
| Session/token validation | Tokens verified server-side on every authenticated request; short TTL with refresh |
| OAuth flow validation | Verify the provider JWT (issuer, audience, expiry) before creating a session |
| Wallet signature verification | For crypto products: verify signatures on every authenticated request, bind to a nonce to prevent replay |
| Sponsored/relayed tx sender spoofing | Relayed transactions must verify the sender matches the intended user, not just that gas/fees are covered |
| Admin impersonation | Admin status checked by verified role or capability, not by convention or client claim |

### Tampering (data integrity)

| Check | Detail |
|---|---|
| Request tampering | State-changing requests validated server-side; never trust client-computed amounts, prices, or permissions |
| Composed-transaction injection | If users can compose or batch calls that you sign or sponsor, restrict the allowed commands |
| Upgrade tampering | If contracts or infra are upgradeable, restrict who can upgrade and record what changes are allowed |
| Frontend state tampering | Client-side state (balances, permissions) must be re-verified server-side or on-chain before critical operations |

### Repudiation (audit trail)

| Check | Detail |
|---|---|
| Critical-operation logging | Critical operations emit structured logs (or on-chain events) with correlation IDs. No PII in logs |
| Receipt tracking | Store transaction digests / payment IDs for critical operations so outcomes can be verified later |

### Information disclosure

| Check | Detail |
|---|---|
| Public-by-default storage | On-chain data and public buckets are readable by anyone. Do not store secrets there |
| API response filtering | Responses should not leak internal data (internal IDs, other users' records) to unauthorized callers |
| Error message leakage | Errors should not reveal internal logic or stack traces. Frontend displays generic errors |
| Prompt/context leakage | For LLM products: system prompts and other users' context must not be extractable through model output |
| Address correlation | For crypto products: warn users that transactions are public and addresses are linkable |

### Denial of service

| Check | Detail |
|---|---|
| Paid-resource exhaustion | Anything that costs you per use (inference, gas sponsorship, storage, SMS) needs per-user and global limits |
| Rate limiting | All public endpoints rate-limited; handle upstream 429s gracefully with fallbacks |
| Hot-spot contention | A single global record or shared object that every request touches becomes a bottleneck. Shard or partition |

### Elevation of privilege

| Check | Detail |
|---|---|
| Missing ownership checks | Endpoints and entry functions that accept a resource ID must verify the caller is authorized for that resource |
| Capability/credential leaks | Admin capabilities and service credentials must not pass through user-controllable paths |
| Upgrade authority exposure | Exposed upgrade/deploy credentials allow arbitrary code changes. Multisig or locked storage |
| Scope escalation | Tokens and proofs must be scoped to intended actions, not reusable across contexts |
| Agent privilege escalation | For agent products: tool access granted to the model must be least-privilege; untrusted input must not steer privileged tools |

## OWASP Top 10 mapped to product patterns

| OWASP | Product equivalent | What to check |
|---|---|---|
| A01: Broken Access Control | Missing server-side authz, missing capability checks in contracts | Every privileged path enforced server-side or on-chain |
| A02: Cryptographic Failures | Secrets in public storage, weak key derivation | Never store secrets client-side or on-chain unencrypted |
| A03: Injection | SQL/command injection, prompt injection, composed-tx injection | Parameterize queries; isolate untrusted content from privileged model context; restrict sponsored calls |
| A04: Insecure Design | No threat model, no abuse cases | Run STRIDE before shipping. Write negative tests for unauthorized access |
| A05: Security Misconfiguration | Default configs, open CORS, debug mode in prod | Restrict CORS origins, disable debug endpoints, review defaults |
| A06: Vulnerable Components | Outdated deps, floating versions | Pin all dependencies. Run `npm audit`. Verify contract addresses |
| A07: Authentication Failures | No session expiry, weak OAuth config | Bound token TTL, validate JWTs, rate-limit auth |
| A08: Integrity Failures | Unsigned upgrades, untrusted CI/CD | Restrict upgrade authority. Pin CI dependencies. Verify build provenance |
| A09: Logging Failures | No audit trail | Structured logs (or events) for critical ops, correlation IDs |
| A10: SSRF | Backend fetches user-supplied URLs | Allowlist external URLs. Block internal/localhost. Validate URL schemes |

## OWASP API Security Top 10 (quick check)

| # | Vulnerability | Check |
|---|---|---|
| API1 | Broken Object Level Auth | Every resource access verifies ownership or role |
| API2 | Broken Authentication | Token TTL bounded, signatures verified, rate limiting on auth |
| API3 | Broken Object Property Auth | Field-level write access verified, not just record-level |
| API4 | Unrestricted Resource Consumption | Limits on paid resources (inference, gas, storage), pagination on queries |
| API5 | Broken Function Level Auth | Admin-only functions require verified role/capability, not just a hidden route |
| API6 | Unrestricted Business Flows | Rate-limit minting, claiming, referral, and other value-creation flows |
| API7 | SSRF | Backend URL fetching must allowlist targets |
| API8 | Security Misconfiguration | No debug mode in prod, CORS restricted, error messages generic |
| API9 | Improper Inventory | Document all deployed services and contracts, track versions |
| API10 | Unsafe API Consumption | Validate responses from third-party APIs before acting on them |
