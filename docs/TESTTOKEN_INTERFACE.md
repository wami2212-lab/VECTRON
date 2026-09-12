# VECTRON (VCT) — Smart Contract Interface

This document outlines the core external functions, events, and state variables of the deployed VECTRON contract: a fixed-supply (1B VCT hard cap) token with tiered staking, linear vesting for investor/team allocations, and TWAP-protected auto-liquidity.

Deployed contract: `0x04F3e675068a93941d0b6cB13428c4a9C83dfd14` (BSC Mainnet).

## 1. Staking — Tiered Lock Periods

| Tier | Lock Duration | Point Multiplier |
|---|---|---|
| 1 | 15 days | 1.0x |
| 2 | 45 days | 1.5x |
| 3 | 90 days | 2.0x |

Each wallet can hold up to 25 concurrent stake slots (MAX_STAKE_SLOTS).

- stake(uint256 tier, uint256 amount) — locks tokens under the chosen tier, mints reward points (amount × multiplier).
- unstake(uint256 index) — withdraws a fully-matured slot, applies the current unstake fee.
- emergencyExit(uint256 index) — withdraws before maturity; keeps rewards on other slots, forfeits this slot's pro-rata reward share to treasury.
- claim() — claims accrued staking rewards without touching principal.

## 2. Investor & Team Vesting

10% at TGE (startSystem()), then the remaining 90% linearly over 12 weeks — applies uniformly to Seed, Private, Public, Team, and Treasury allocations.

- getVestedAmount(address account) — cumulative amount vested so far.
- claimMyVestedTokens() — transfers newly-vested tokens to caller.
- setSeedAllocationBatch / setPrivateAllocationBatch / setPublicAllocationBatch (owner-only) — bulk-assign allocations; lock permanently once startSystem() runs.

## 3. Fees

| Fee | Default | Hard Cap |
|---|---|---|
| Treasury tax (transfer) | 0.5% | — |
| Liquidity tax (transfer) | 0.5% | — |
| Combined transfer tax | 1% | 2% |
| Unstake exit fee | 1% | 3% |

## 4. TWAP-Protected Auto-Liquidity

Collected liquidity tax auto-swaps into BNB and adds liquidity once it crosses minTokensBeforeLiquidity (default 1.5M VCT) — but only if spot price hasn't diverged more than maxTwapDivergenceBPS (default 20%) from a manipulation-resistant TWAP, protecting against sandwich attacks.

## 5. Owner-Controlled Safety Mechanisms

- Two-step ownership transfer (transferOwnership / acceptOwnership)
- 48-hour timelocked rescue (initiateRescue / executeRescue / cancelRescue)
- LP lock (lockLiquidity, 150 days)
- Pause switch (setPaused)
