---
name: defi-market-research
description: Research DeFi TVL, protocols, and yield for any chain via DefiLlama. Use when the user wants DeFi data, TVL, or protocol research.
---

## What this skill does

Fetches live DeFi metrics from DefiLlama's free API (TVL, DEX volumes, lending rates, stablecoin flows) for a target chain and compares it against comparison chains to surface concrete market gaps. Output is a ranked list of opportunities where the target chain has low presence in categories that show high demand elsewhere. Every opportunity cites a specific metric.

## When to use it

- The user wants data-driven DeFi ideas (not vibes, not "what's hot").
- The user is evaluating which DeFi vertical to enter.
- The user wants to compare one chain's DeFi maturity against other chains.

## When NOT to use it

- The user already has an idea and wants to build it. Route to `validate-idea` or a build-phase skill.
- The user wants general idea search beyond DeFi. Route to `find-next-idea`.
- The product is not crypto at all. This skill has nothing for it.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- The target chain (e.g. Ethereum, Solana, Base, Sui, X Layer). DefiLlama chain identifiers are case-sensitive.
- The user's DeFi interest (broad survey, specific vertical like lending or DEX, yield farming).
- Optional: verticals to focus on (DEXes, lending, stablecoins, derivatives, yield).
- Optional: chains to compare against (defaults to Ethereum + Solana).

## Outputs

A research block written to `.lonewolf/idea-context.md` (or `.lonewolf/research-defillama-<timestamp>.md` if no idea is chosen yet):

```markdown
## DeFi market research (DefiLlama), <timestamp>

### <chain> overview
- Total TVL: $X (rank #Y across all chains)
- 24h DEX volume: $X
- Stablecoin market cap: $X
- Active protocols tracked: N

### Cross-chain comparison
| Metric | <chain> | Ethereum | Solana |
|----------------|---------|----------|---------|
| TVL | ... | ... | ... |
| DEX 24h volume | ... | ... | ... |
| Lending TVL | ... | ... | ... |
| Stablecoin cap | ... | ... | ... |

### Market gaps (ranked by opportunity size)
1. <gap>: <target-chain metric> vs <comparison-chain metric>, <why this is an opportunity>
2. ...

### Top yield opportunities
- <pool>: <APY>, <TVL>, <protocol>, <risk notes>

### Risks
- <risk>: <mitigation>

### Data sources
- <endpoint called, timestamp, response summary>
```

## Workflow

1. **Confirm scope**
 - Which target chain? Broad DeFi survey, or a specific vertical? Which comparison chains?

2. **Fetch target-chain baseline metrics**
 - `GET https://api.llama.fi/v2/chains` for the chain's TVL and rank.
 - `GET https://api.llama.fi/v2/historicalChainTvl/<Chain>` for TVL trend (is it growing, flat, declining?).
 - `GET https://stablecoins.llama.fi/stablecoincharts/<Chain>` for stablecoin market cap trend.
 - Note: chain identifiers are case-sensitive in all DefiLlama endpoints. Confirm the exact spelling in the `/v2/chains` response first.

3. **Fetch protocol-level data**
 - `GET https://api.llama.fi/protocols` and filter where `chains` includes the target chain.
 - Group protocols by category (DEX, lending, yield, derivatives, stablecoin, liquid staking).
 - Note protocol count per category. Categories with fewer than 3 protocols are potential gaps.

4. **Fetch DEX and fee data**
 - `GET https://api.llama.fi/overview/dexs/<Chain>` for DEX volume breakdown by protocol.
 - `GET https://api.llama.fi/overview/fees/<Chain>` for fee and revenue breakdown by protocol.
 - Identify which DEX dominates volume. A single protocol with >60% share signals aggregator or alternative DEX opportunity.

5. **Fetch yield data**
 - `GET https://yields.llama.fi/pools` and filter where `chain === "<Chain>"`.
 - Sort by APY (descending) and by TVL (descending) separately.
 - Flag pools with high APY but low TVL (potential early opportunities or risk signals).
 - Flag pools with high TVL but low APY (stable but possibly oversaturated).

6. **Compare against the comparison chains**
 - Run the same chain-level queries for each comparison chain.
 - Build the cross-chain comparison table.
 - For each DeFi category, compute: number of protocols, total TVL, total volume.
 - Categories where the target chain has <10% of a comparison chain's protocol count are strong gap signals.

7. **Identify and rank gaps**
 - For each gap, state: what the gap is, the specific metric that proves it, why a builder could fill it.
 - Contextualize with the named protocols from the `/protocols` response — cite real names, not "several DEXes".
 - Rank by: (demand on comparison chains) x (inverse of target-chain presence in that category).

8. **Surface risks**
 - TVL concentration: if one protocol holds >50% of the chain's TVL, ecosystem risk is high.
 - Stablecoin depth: low stablecoin cap limits DeFi composability.
 - Bridge dependency: DeFi depends on bridged assets; map the bridge risk.

9. **Cite everything**
 - Every number links to the API endpoint that produced it.
 - Include the query timestamp. DeFi data moves fast; stale numbers mislead.

10. **Writeback**
 - Append to the chosen output file.

## Quality gate (anti-slop)

Before reporting done:

- Does every opportunity cite a specific metric from a specific DefiLlama endpoint?
- Does the cross-chain comparison use data from the same time window (not mixing cached and live)?
- Did the analysis include at least one risk per gap, not just upside?
- Are yield numbers accompanied by TVL context (high APY on $1k TVL is not an opportunity)?
- Did the writeback happen?

If any answer is no, the skill keeps working.

## References

On-demand references (load when relevant):

- `references/defillama-endpoints.md`: All verified DefiLlama API endpoints with response shapes.

## Use in your agent

- Claude Code: `claude "/launchkit:defi-market-research <your message>"`
- Codex: `codex "/defi-market-research <your message>"`
- Grok Build: run `grok`, then `/defi-market-research <your message>` in the session
- Cursor: paste a chat message that includes a phrase like "DeFi market research", or load `~/.cursor/rules/defi-market-research.mdc` and reference it.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
