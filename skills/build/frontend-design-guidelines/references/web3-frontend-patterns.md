# Web3 frontend patterns (skip for non-crypto products)

Surfaces every crypto app has and how to render them well. Chain-agnostic; swap in your chain's native token symbol and block explorer.

## Wallet connect

- Single button at top-right when disconnected, labeled "Connect wallet" (not "Login", not "Sign in").
- On connect, show: ellipsized address, network (testnet / mainnet) with a colored dot, native token balance.
- Disconnect under a dropdown next to the address. Confirm only on testnet (mainnet should be one-click for trust).
- If multiple wallets are supported, present a sheet of options with the most-used wallet for your chain first. Do not pre-select.

## Address display

- Ellipsis format: `0x1234...abcd` (4 chars head, 4 chars tail). NOT 6+6, that is too wide.
- Always pair with a copy button, focus-visible, with a "Copied" toast on click.
- On hover: tooltip with full address.
- For long identifiers (contract addresses, tx hashes), use the same format and link to the explorer.

## Network indicator

- Single small pill at top-right, near the wallet button.
- Mainnet: green dot, "Mainnet". Testnet: yellow dot, "Testnet".
- Click to switch (if app supports).

## Transaction state

Four states, each with its own visual treatment:

1. **Signing**: modal or inline indicator with "Confirm in your wallet". Disable the trigger button.
2. **Pending**: spinner, "Submitting transaction". Show the tx hash as soon as it exists, link to the explorer.
3. **Success**: green check, "Transaction confirmed", show what changed for the user, link to the explorer, dismiss after 5s.
4. **Failed**: red X, the failure reason in plain language (translate raw revert/abort codes), retry CTA, link to the explorer for the failed tx.

## Gas display

- Show gas in the native token with at most 6 decimals, NOT in raw base units (wei/lamports), NOT scientific notation.
- For "estimate", show as `~0.001 ETH`. For "actual", drop the tilde.
- See the `number-formatting` skill for the helper that does this.

## Asset display

For an asset the user owns:

- Type or collection name at top.
- Identifier below, ellipsized, with copy.
- Type-specific fields (a token shows balance, an NFT shows image + name, a generic record shows fields under a disclosure).
- Action menu: transfer, view in explorer, custom actions per type.

## Coin / token display

- Always tabular figures.
- Symbol after the number, separated by a thin space. `1,234.56 USDC`, not `USDC1234.56`.
- For balances: trailing zeros trimmed unless the user is in a "trade" surface where alignment matters.
- For input: dropdown of the user's tokens, selected token's balance shown above the input, a "Max" button.

## Empty wallet state

If the connected wallet has nothing relevant:

- Title: "No <thing> yet".
- Body: one sentence explaining how to get one.
- CTA: directs to the action that creates one. If the action requires testnet funds, link to the faucet explicitly.

## Faucet flow (testnet only)

- A small persistent button or banner: "Need testnet funds? Faucet".
- Click triggers the faucet request (or links to the chain's faucet), shows a toast on success.
- If the user already has enough testnet balance, hide the banner.

## Failure copy patterns

- Insufficient gas: "Not enough <TOKEN> for gas. You need ~X; you have Y."
- User rejected: "Transaction not confirmed. <Retry button>".
- Network error: "Could not reach the network. Check your connection."
- Asset not found: "This item no longer exists or you do not own it."

## Anti-patterns

- Showing the raw full-length address inline.
- Showing base-unit values (wei, lamports) directly to users.
- Showing the transaction hash as the only feedback (wrap it: "Transaction sent. Hash: 0x...").
- Letting a network switch fail silently.
- Using "wallet" and "address" interchangeably (they are not the same; an address is owned by a wallet).
