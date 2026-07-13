---
name: number-formatting
description: Format token amounts, USD, gas, or big-number conversion in an app UI. Use when the user wants to fix or implement number formatting.
---

## What this skill does

Drops in a small set of number formatting helpers and applies them across the UI. Covers token amounts (any decimals, base-unit conversion), USD values, percentages, gas, balances with thousands separators, and address ellipsis. The goal is one consistent formatter set so prices, balances, and fees all read alike across the app.

Numbers are the most-glanced-at element in a product UI. Getting them wrong (raw base units shown to a user, scientific notation in the middle of a price, jiggling proportional figures) is a credibility kill.

## When to use it

- The app displays raw base units (wei, lamports, satoshi, cents) anywhere a user can see them.
- Numbers update in place and the columns or values jiggle (proportional figures).
- USD values render with floating decimals (`$1234.567890123`).
- Percentages render without a sign or with too many decimals.
- The user wants a consistent formatting layer instead of inline `Number.toFixed` calls.

## When NOT to use it

- The frontend has not been scaffolded yet, route to `scaffold-project`.
- Pure backend concern, formatting belongs at the UI layer.
- The contract or API returns raw integers and the user wants to format server-side (keep raw integers at the data layer; format at render).

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- The current frontend code (TS or JS).
- The set of tokens or currencies the app displays, with their decimals.
- Optional: existing formatter utility, if one is already in the codebase (audit and consolidate).

## Outputs

- A `web/lib/format.ts` (or similar location) module exporting:
 - `formatToken(amount: bigint, decimals: number, opts?)`
 - `formatNative(amount: bigint, opts?)` (wrapper preset for the chain's native token, if any)
 - `formatUSD(value: number | string, opts?)`
 - `formatPercent(value: number, opts?)`
 - `formatGas(amount: bigint)` (renders `~0.0001 <SYMBOL>` shape)
 - `ellipsizeAddress(addr: string)` and `ellipsizeId(id: string)`
- Component callsites updated to use the new helpers.
- Tabular-figures CSS applied at the right element scope.

## Workflow

1. **Inventory existing formatting**
 - Grep for `toFixed`, `toLocaleString`, `BigInt`, manual decimals.
 - Note divergent patterns. Pick one canonical shape per surface.

2. **Drop in the formatters**
 - Use the templates in `references/format-helpers.md`.
 - Place under `web/lib/format.ts` (or the project's lib root).

3. **Apply tabular figures**
 - Add `font-feature-settings: "tnum"` (or Tailwind `tabular-nums`) to numeric elements at the component level. Not globally, since tabular figures look wrong in body prose.

4. **Update callsites**
 - Replace inline `(amount / 10n ** 18n).toString()` with `formatToken(amount, 18)`.
 - Replace `value.toFixed(2)` with `formatUSD(value)`.
 - Replace `(rate * 100).toFixed(2) + '%'` with `formatPercent(rate)`.

5. **Address handling**
 - Replace inline `addr.slice(0, 6) + '...' + addr.slice(-4)` with `ellipsizeAddress(addr)`.
 - For contract addresses, tx hashes, and IDs use `ellipsizeId`.

6. **Verification**
 - Test small numbers (0.000001 USDC), large numbers (1,234,567.89 USDC), zero, max uint64.
 - Test base-unit conversion with rounding (e.g. 1,234,567,890,000,000,000 wei renders as 1.23456789 ETH, not scientific).
 - Test that the same number rendered twice in different components looks identical character-for-character.

## Quality gate (anti-slop)

Before reporting done:

- Are there any remaining direct `toFixed` or manual division by 10n ** decimals in the codebase? If yes, the consolidation is incomplete.
- Does every numeric column use tabular figures?
- Do gas, token, and USD values have distinguishable formatting (so users do not confuse 0.001 ETH gas with 0.001 USDC)?
- Do edge cases work (zero, very small, very large, undefined input gracefully renders as "--" or "0")?
- Are addresses and IDs both ellipsized with the same head/tail counts?

If any answer is no, the skill keeps working.

## References

On-demand references (load when relevant to the user's question):

- `references/format-helpers.md`: Drop-in TypeScript helpers for tokens, USD, percent, gas, addresses.
- `references/decimals-by-token.md`: Common token decimals across chains, and how to look them up at runtime.

## Use in your agent

- Claude Code: `claude "/launchkit:number-formatting <your message>"`
- Codex: `codex "/number-formatting <your message>"`
- Grok Build: run `grok`, then `/number-formatting <your message>` in the session
- Cursor: paste a chat message that includes a phrase like "format numbers in my UI", or load `~/.cursor/rules/number-formatting.mdc` and reference it.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
