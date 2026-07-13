# Supply chain audit checklist

Covers npm/pnpm dependency auditing, contract dependency verification, and address pinning.

## npm / pnpm dependency audit

### Run the audit

```bash
# Check for known vulnerabilities
npm audit --production
# or
pnpm audit --prod

# Fix automatically where possible
npm audit fix
```

### What to check

| Item | Pass criteria |
|---|---|
| No critical or high vulnerabilities | `npm audit` reports 0 critical, 0 high |
| No abandoned packages (>2 years no publish) | Check last publish date on npmjs.com |
| No typosquatting risk | Package name matches the official name exactly (e.g., `@openzeppelin/contracts`, not `openzeppelin-contract`) |
| Lockfile committed | `pnpm-lock.yaml` or `package-lock.json` is in version control |
| No postinstall scripts from untrusted packages | Review `scripts.postinstall` in dependency `package.json` files |
| Peer dependency versions match | No conflicting peer dependency warnings |

### Verifying critical packages

For every load-bearing SDK (chain SDK, wallet kit, payment SDK, LLM client):

1. Confirm the exact package name against the vendor's official docs — not a search result.
2. Check weekly downloads and publisher identity on npmjs.com; a near-zero-download lookalike is a red flag.
3. Watch for legacy vs current names (vendors rename SDKs; the abandoned one stops getting security patches). If the project uses a legacy name, flag it for migration.

## Contract dependency verification

### Pin everything

Every contract dependency must be pinned to a specific revision, tag, or exact version — never a branch head.

```toml
# GOOD: pinned to a specific rev/tag
dependency = { git = "https://github.com/vendor/repo.git", rev = "abc1234" }

# BAD: floating on a branch — build is non-reproducible
dependency = { git = "https://github.com/vendor/repo.git", branch = "main" }
```

### Verify on-chain addresses

For any dependency that references a deployed contract address or package ID (a DEX router, an oracle, a token), confirm it matches the canonical deployment:

1. Get the expected address from the project's official docs or GitHub — never from a forum post or chat message.
2. Query the chain (explorer or RPC) to confirm the address exists and exposes the expected interface.
3. Compare bytecode or verification status if the explorer provides it.

### What to flag

| Finding | Severity |
|---|---|
| Contract dependency on `branch = "main"` or `branch = "develop"` | P1: supply chain risk, build is non-reproducible |
| Unrecognized git URL in the dependency manifest | P1: verify the source is the canonical repo |
| Deployed address does not match official docs | P0: possible address poisoning |
| npm package with `postinstall` script that runs network calls | P1: review the script for exfiltration |
| Dependency with known CVE at critical severity | P0: update or replace immediately |
| Dependency with known CVE at high severity | P1: update within the sprint |
| Abandoned dependency (no updates in 2+ years, low usage) | P2: evaluate alternatives |
