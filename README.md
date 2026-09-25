<div align="center">

# Bidwallfi

**Every other bid wall gets pulled. This one can't.**

A locked share of every trade's fee becomes buy orders under the price. No owner. No withdraw. It only grows.

[![chain](https://img.shields.io/badge/chain-Robinhood%20Chain%204663-C6F034?style=flat-square&labelColor=0B1F33)](https://robin.etherscan.io)
[![venue](https://img.shields.io/badge/venue-Pons%20V2-C6F034?style=flat-square&labelColor=0B1F33)](https://bidwallfi.fun/docs)
[![bricks](https://img.shields.io/badge/bricks-Uniswap%20v3%20ranges-C6F034?style=flat-square&labelColor=0B1F33)](https://bidwallfi.fun/docs)
[![contracts](https://img.shields.io/badge/contracts-Solidity%200.8.28%20%C2%B7%20Foundry-C6F034?style=flat-square&labelColor=0B1F33)](https://github.com/bidwallfi)
[![ticker](https://img.shields.io/badge/ticker-%24BID-C6F034?style=flat-square&labelColor=0B1F33)](https://x.com/bidwallfifun)

[Site](https://bidwallfi.fun) · [App](https://app.bidwallfi.fun) · [Docs](https://bidwallfi.fun/docs) · [X @bidwallfifun](https://x.com/bidwallfifun) · [GitHub](https://github.com/bidwallfi)

</div>

---

## What is Bidwallfi

Every launchpad coin has the same weak point: the fee stream. Creator fees land in a wallet one person controls, and the bigger the volume, the bigger the pile that can be sold into the chart. "LP locked" and "renounced" describe the past. They say nothing about who holds the fees today.

Bidwallfi is a factory for vaults on Robinhood Chain. Launch a coin through it, or opt one in, and a fixed share of every trade's creator fee becomes **bricks**: real Uniswap v3 buy positions stacked under the price, held by a vault with no owner and no withdraw function. Every trade thickens the wall. Every run-up that holds leaves a new layer behind. Nobody can pull it.

**$BID** is the token that runs the platform, and it lives behind the same wall from its first block.

## Features

| | |
|---|---|
| **Launch with a wall** | One signature deploys the coin, its vault and its rules together. The vault is the fee recipient before the first trade. |
| **Opt in an existing coin** | Two transactions from the wallet that currently receives the fees. From the next trade on, the ratchet runs. |
| **The missing function** | No withdraw, no owner, no admin, no pause, no upgrade. Verify it on the explorer in ten seconds. |
| **Pump-proof placement** | Bricks are laid against the lowest price recorded over the last two 30-minute windows, never the spot price. |
| **The ratchet** | The anchor only steps up after a run that held. Old layers stay put. The top of the wall can only climb. |
| **Floor Wall Live** | Every coin page renders its wall in real time. Sells arrive as waves and break on the bricks. |
| **Share cards** | "The wall held", "New layer", "Level up" and weekly recaps generate themselves from chain events. |
| **Floor Wars** | Weekly season. Walls compete on growth. The prize lands as bricks, never in a wallet. |
| **Permissionless keepers** | Anyone can call claim, build, harvest and compound. The caller is tipped from the platform cut. |
| **Read API and badge** | Every wall, brick and fill, readable without a key. Embeddable SVG badge for coin sites. |

## How it works

```
  trader buys or sells on Pons
            |
            v
     1% fee on the trade ----------> 30% Pons protocol
            |
            v
  70% creator share (+ creator tax) lands in the coin's vault
            |
            v
   claim()  anyone may call; the vault does the split
            |
            +----> (100 - X)% -----> creator payout address
            |
            +----> X% ratchet share  (20% to 100%, fixed at deployment)
                      |
                      +----> 90% -----> BRICKS under this coin
                      |
                      +----> 10% platform cut
                                 +-- 40% bricks under $BID
                                 +-- 40% treasury (audits, ops, rebates)
                                 +-- 20% Floor Wars prize pool

  every wei claimed = payout + bricks + cut
```

### Bricks

A brick is a single-sided Uniswap v3 range position holding ETH in a band about 2% wide below the price. Five rungs sit at **5, 10, 20, 35 and 50 percent** under the anchor. Each build splits new ETH evenly across the five and mints the positions straight to the vault.

Price falls into a brick: the brick buys the coin. The vault harvests the filled brick and keeps the coin. Nothing the wall bought is ever sold back. Swap fees earned by bricks are compounded into new bricks.

```
  price now ------------------------------ 1.00
  brick 1   ########   0.93 to 0.97   buys if price falls here
  brick 2   ########   0.88 to 0.92
  brick 3   ########   0.78 to 0.82
  brick 4   ########   0.63 to 0.67
  brick 5   ########   0.48 to 0.52
```

### The anchor and the pump guard

The obvious attack on an on-chain bid wall is to pump the price right before a build so the bids land too high, then sell into them.

Every claim, build and price record stores the **lowest** main-pool price seen in the current and the previous 30-minute window. The anchor is taken from that minimum. A pump raises the price. It never raises the minimum.

The anchor steps up only when the minimum itself holds 25% above it, which means a run that stuck rather than a wick. It resets down only after every rung has been crossed. Between those, builds add to the same five ranges. A 10-minute cooldown and a 5 ETH cap per build sit on top.

Tested on a fork of Robinhood Chain against the real venue: a 2 ETH buy pushed the price +118% right before `build()`, and the bricks landed at the pre-pump level.

### What nobody can do

| Action | Result |
|---|---|
| Withdraw | No function exists. Not for the creator, not for the team, not for anyone. |
| Change X | Fixed at deployment. Minimum 20%. |
| Upgrade the vault | No proxy, no admin, no owner. |
| Move a brick | Position NFTs belong to the vault and have no exit. |
| Bend the rules | Keepers can only trigger calls. The vault decides where every wei goes. |

The creator can change one thing: their own payout address. It only ever touches their share.

## Floor Wars

| Rule | Detail |
|---|---|
| Season | One week, Monday 00:00 UTC to Sunday 23:59 UTC |
| Entry | Every coin with a wall of at least 0.5 ETH at week start, automatically |
| Score | New bricks placed this week divided by wall size at week start. Only bricks funded by real claims count. |
| Prize | The smaller of this week's pool and the winner's own growth, placed as bricks in the winner's vault |
| Anti-wash | At X = 30%, every 1 ETH of fake bricks costs about 2.7 ETH of net fees. The prize caps at 1x own growth, so washing to win always loses. |

Wall levels, from locked bricks only: **Sandbag** under 0.5 ETH, **Seawall** 0.5 to 5 ETH, **Breakwater** 5 to 25 ETH, **Bedrock** 25 ETH and up. Bricks never leave, so a wall can only move up.

## $BID

| | |
|---|---|
| Supply | 1,000,000,000 $BID |
| Launch | Fair launch on Pons through the Bidwallfi factory. No presale, no allowlist, no team allocation. |
| Creator tax | 2%, fixed at launch |
| $BID's vault | X = 100%, no platform cut. Every wei of creator income becomes bricks under $BID. |
| Team income | None from $BID trading. The team is paid only from the treasury share of other coins' cut. |

Holding $BID buys discounts, cosmetics and a voice on venues. It never buys Floor Wars score, board placement or different vault rules. The wall is the same wall for everyone.

| Tier | Hold | Perks |
|---|---|---|
| Visitor | 0 | Every wall, page, badge and card. Standard 10% cut. Floor Wars eligible. |
| Bricklayer | 1M (0.1%) | Effective cut 9% via weekly rebate. Bricklayer mark. Fill alerts. |
| Mason | 5M (0.5%) | Effective cut 8%. Custom wall skins. New venues open to Masons first. |
| Architect | 10M (1%) | Effective cut 7%. Vote on the venue allowlist. Founding-wall mark. |

The vault always takes the same 10% on-chain. Rebates are paid weekly from the treasury based on a 7-day time-weighted balance, so nothing can be borrowed for one block to game a claim.

## Architecture

```
  +--------------------+          +----------------------------------+
  |  bidwallfi.fun     |          |  ROBINHOOD CHAIN (4663)          |
  |  app.bidwallfi.fun |  sign    |  RatchetFactory                  |
  |  Next.js + viem    |--------->|  RatchetVault x N                |
  |  wallet: EIP-6963  |          |  FloorWarsPool                   |
  +---------+----------+          |  Pons factory + fee escrow       |
            |                     |  Uniswap v4 (trading) / v3 (wall)|
            | read                +---------------+------------------+
            v                                     | logs
  +--------------------+          +---------------v------------------+
  |  Read API          |<---------|  Indexer                         |
  |  JSON, badge, SSE  |          |  events, swaps, fills            |
  +---------+----------+          +----------------------------------+
            |
            v                     +----------------------------------+
  +--------------------+          |  Keeper (ours, and anyone's)     |
  |  Floor Wall Live   |          |  claim · record · build ·        |
  +--------------------+          |  harvest · compound · settle     |
                                  +----------------------------------+
```

| Contract | Role |
|---|---|
| `RatchetFactory` | Deploys one vault per coin at a predictable CREATE2 address. Wraps the Pons launch. One power, once: naming the $BID vault. No owner after that. |
| `RatchetVault` | The coin's fee recipient. Splits every claim, lays bricks, harvests fills. Holds every position NFT for life. |
| `RatchetVaultDeployer` | Holds the vault creation code so the factory stays under the 24 KB limit. |
| `FloorWarsPool` | Holds the prize share. ETH leaves in exactly one way: as bricks in a vault. |

## Tech stack

| Layer | Technology |
|---|---|
| Chain | Robinhood Chain (EVM, chainId 4663) |
| Contracts | Solidity 0.8.28, Foundry, `via_ir`, unit + fuzz + invariant + fork tests |
| Venue | Pons V2 bonding curve, graduates at 4.2 ETH into a hooked Uniswap v4 pool |
| Bricks | Uniswap v3, 1% fee tier, coin/WETH, single-sided ranges |
| App | Next.js 16, viem, EIP-6963 wallet discovery, Tailwind 4 |
| Worker | TypeScript, viem: keeper, monitor, indexer into managed Postgres |
| Data | The chain is the source of truth. The database is a cache the indexer can rebuild from block zero. |

## Security

- Every state-changing function is reentrancy guarded.
- The numbers are constants in bytecode. There is no setter for any of them.
- `build()` refuses unless spot sits within the price guard, the cooldown has passed and a price record exists in an earlier window.
- A payout address that refuses ETH can never block the bricks. The share waits.
- Fork tests run against the real Pons and Uniswap contracts on Robinhood Chain.
- Verified source on the explorer for every contract. External audit before mainnet. Public bug bounty from launch day.

## Honest limits

- Bricks are bids, not a price promise. A large enough sell goes through every brick.
- The Pons protocol owner can re-point a coin's future fee stream after a public 3-day timelock. Bricks already placed stay put. Every coin page shows this.
- Bricks sit in a side pool, so support reaches the main pool through arbitrage.
- A coin with little volume builds a small wall.
- Contracts are immutable. A bug means a new factory, and old vaults keep their rules.

## Roadmap

| Phase | Scope |
|---|---|
| 1 · MVP | Contracts and tests, full fork cycle, external audit, verified source, bug bounty, app with the board, coin page, launch and opt-in, Floor Wall Live, keeper, indexer. $BID launches through the factory as the first wall on the board. |
| 2 · Growth | Opt-in open to every coin on Pons. Floor Wars season 1. Weekly tier rebates. Embeddable badge and public read API. Wall levels. |
| 3 · Scale | More brick venues on Robinhood Chain, each with its own audited builder. Other launchpads integrate the vault factory. Wall analytics. White-label Floor Wall widget. |
| 4 · Ecosystem | The vault as an open standard for any EVM launchpad. Architect votes on venues. Bedrock program for walls above 25 ETH. Grants for tools on public wall data. |

## Official links

| | |
|---|---|
| Site | https://bidwallfi.fun |
| App | https://app.bidwallfi.fun |
| Docs | https://bidwallfi.fun/docs |
| X | https://x.com/bidwallfifun |
| GitHub | https://github.com/bidwallfi |
| Explorer | https://robin.etherscan.io |

The contract address is posted only from @bidwallfifun and pinned on bidwallfi.fun at the same moment. There is no presale, no allowlist and no private round. Anything else is fake.

---

<div align="center">
<sub>Bidwallfi is software that places liquidity positions on Robinhood Chain according to fixed on-chain rules. Bricks are bids, not a price floor or a promise of any price. The Pons protocol owner can reroute a coin's future fee stream after a public 3-day timelock; bricks already placed are not affected. Smart contracts can contain bugs despite testing and audits. Nothing here is financial, investment or legal advice. $BID is a utility token for discounts, cosmetics and venue votes inside the product; it confers no ownership, rights or claims.</sub>
</div>
