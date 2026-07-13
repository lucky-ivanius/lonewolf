# Token decimals quick reference

Common decimals encountered across major chains. Always confirm against the token's on-chain metadata at runtime; this list is a quick reference, not authoritative.

## Native tokens

- **ETH** (and most EVM natives: OKB, BNB, MATIC): 18 decimals. Base unit is wei (1 ETH = 10^18 wei).
- **BTC**: 8 decimals. Base unit is satoshi.
- **SOL**: 9 decimals. Base unit is lamports.

## Stablecoins

- **USDC**: 6 on most chains.
- **USDT**: 6 on most chains.
- Watch for wrapped/bridged variants of the "same" stablecoin living alongside the native issue with a different contract address. Disambiguate by address, not symbol.

## Wrapped or bridged

- **WETH**: 18 on Ethereum-native deployments, but bridged versions on non-EVM chains are often 8 or 9. Always verify per bridge.
- **WBTC**: 8.
- Bridged token decimals vary by bridge; never assume symmetry with the origin chain.

## How to look up at runtime

```ts
// EVM (viem)
const decimals = await client.readContract({
 address: tokenAddress,
 abi: erc20Abi,
 functionName: "decimals",
});
```

Cache decimals per token address for the session. They do not change post-deploy (and if they do for a token you depend on, that is a red flag).

## Decimals selection for new tokens

If the product issues its own token or points system:

- 6 is a comfortable default for utility / payment units (matches USDC, human-readable amounts).
- 18 mirrors EVM natives. Pick for maximal ecosystem compatibility on EVM chains.
- 0 only for whole-unit-only tokens (points, non-divisible utility).

## Gotchas

- The same logical token can exist as multiple contracts (a native version and one or more bridged versions) with the same symbol but different decimals. Always disambiguate by contract address.
- Display rounding: never round token values silently. If you display 4 decimals out of 18, also link to the explorer for the full value.
- Fiat is not exempt: store cents (or the currency's minor unit) as integers, format at render.
