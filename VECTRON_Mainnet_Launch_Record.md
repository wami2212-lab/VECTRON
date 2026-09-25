# VECTRON (VCT) — Mainnet Launch Record

**Launch Date:** September 11, 2026
**Network:** BNB Smart Chain (BSC) Mainnet — Chain ID 56

---

## Contract Identity

| Field | Value |
|---|---|
| Contract Address | 0x60d78404f645d1e9140b605ba8c7641f67e841fc |
| Token Name | VECTRON |
| Token Symbol | VCT |
| Decimals | 18 |
| Max Supply | 1,000,000,000 VCT (1B, hard cap) |
| Deploy Transaction Hash | 0x552d392dc8203c615f715ef584e79c32954be8747057f8051e6c513a5f0da400  |
| Deploy Block | 123919204 |
| Deployer / Owner Wallet | `0xbEc4B6356267fE4791b5C0b5b1325B5B07aE0ad9` (Trezor hardware wallet) |

**BscScan (Mainnet):**
]https://bscscan.com/address/0x60d78404f645d1e9140b605ba8c7641f67e841fc)
**Write Contract directly from BscScan (no Remix dependency):**
https://bscscan.com/address/0x60d78404f645d1e9140b605ba8c7641f67e841fc#writeContract`

---

## Constructor Arguments (as deployed)

| Parameter | Address |
|---|---|
| `_router` (PancakeSwap V2 Router) | `0x10ED43C718714eb63d5aA57B78B54704E256024E` |
| `_teamWallet` | `0xB09Fe4e0a7eAa5CCa8D61689E4De8C9305F550ca` |
| `_treasury` | `0x3D919111dC1B6468d5515eE89f5c088aD833C8C8` |

---

## Key Wallet Addresses

| Role | Address |
|---|---|
| Owner (Trezor) | `0xbEc4B6356267fE4791b5C0b5b1325B5B07aE0ad9` |
| Team Wallet | `0xB09Fe4e0a7eAa5CCa8D61689E4De8C9305F550ca` |
| Treasury Wallet | `0x3D919111dC1B6468d5515eE89f5c088aD833C8C8` |

---

## Compiler Settings (for verification / recompilation)

- **Compiler Version:** Solidity `0.8.19`
- **Optimization:** Enabled
- **Optimizer Runs:** 200
- **EVM Version:** default

---

## Initial Mint (confirmed on-chain at deploy)

| Recipient | Amount | Purpose |
|---|---|---|
| Owner wallet (`0xbEc4...E0ad9`) | 150,000,000 VCT | Liquidity allocation — live at TGE to seed the DEX pool |
| Contract itself | 850,000,000 VCT | Vesting pool — Seed / Private / Public / Team / Treasury allocations |

Owner wallet balance independently verified post-deploy via `balanceOf()` read call: `150,000,000,000,000,000,000,000,000` (150M × 10^18) — matches deploy log exactly.

---

## Verification Status

- **Sourcify:** Verification submitted and successful at deploy time.
- **BscScan verify + publish source:** Recommended next step — do this before opening investor rounds, so anyone can inspect the exact deployed code.

---

## Fee Structure (as deployed, adjustable within caps)

| Fee | Default | Hard Cap |
|---|---|---|
| Treasury tax (transfer) | 0.5% | — |
| Liquidity tax (transfer) | 0.5% | — |
| **Combined transfer tax** | **1%** | **2%** (enforced in `setFees()`) |
| Unstake exit fee | 1% | 3% (enforced in `setFees()`) |

---

## Launch Sequence — Status & Next Steps

- [x] Contract deployed to BSC Mainnet
- [x] Initial mint confirmed (150M owner / 850M contract)
- [x] Sourcify verification successful
- [ ] BscScan source verification (publish)
- [ ] Open Seed round (collect BNB via Google Form/Sheet)
- [ ] Open Private round
- [ ] Open Public round
- [ ] Batch-set all investor allocations via verification script (`setSeedAllocationBatch`, `setPrivateAllocationBatch`, `setPublicAllocationBatch`) — **must complete before `startSystem()`**, allocation functions lock permanently once the system starts
- [ ] Seed PancakeSwap liquidity pool using collected round funds + owner VCT allocation
- [ ] Call `lockLiquidity()` — locks LP for 150 days
- [ ] Call `startSystem()` — finalizes allocations, starts 12-week vesting clock, sweeps any unallocated vesting tokens to treasury
- [ ] Call `setExchangePair()` — **last step** — registers the PancakeSwap pair for tax collection / enables live trading

---

## Security Notes

- Trezor hardware wallet seed phrase: stored offline, physically secured — **never digital, never in this document, never in chat.**
- Owner wallet retains centralized control: pause/unpause, fee adjustment (within caps), 48-hour timelocked rescue of unallocated tokens, exchange pair toggling. Worth disclosing transparently to investors.

---

*This record was compiled on launch day, September 11, 2026, as a permanent reference for the VECTRON project.*
