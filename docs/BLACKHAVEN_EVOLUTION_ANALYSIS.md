# notanohmfork → Blackhaven Evolution

## Quick Summary

notanohmfork was the original concept (GitHub repo only). Blackhaven is the production version - a fully documented reserve-backed treasury functioning as a liquidity engine for MegaETH.

## Core Concept Evolution

### notanohmfork (Original)
- OHM-style DAT with upper-bound RBS only
- 1.2:1 backing ratio (deposit 1.2, get 1.0)
- MegaETH ecosystem focus
- Two-token model (RBT + sRBT)

### Blackhaven (Evolution)
- Reserve-backed treasury with self-reinforcing flywheel
- Value-accruing instruments: principal-protected convertible notes + fixed-term bonds
- Triple token system: HVN (governance) + sHVN (staked HVN, receives rewards) + RBT/BLK (reserve-backed)
- Captures treasury premiums from MegaETH ecosystem assets

## Key Changes

### 1. Product Structure

**notanohmfork:** Single bonding mechanism (1.2:1)

**Blackhaven:** 
- Haven Protected Notes (HPN): Deposit USDM/USDMy → get ERC-721 NFT
  - Principal-protected, no market risk
  - 7-day cooldown on all exits
- Fixed-Term Bonds: Exchange USDM/MEGA for vested RBT
- Aligned Bonds (LP Bonds): Deposit RBT-USDM LP → LP locked forever as POL

### 2. Token Model

**notanohmfork:** 
- RBT (backed token)
- sRBT (staked version)

**Blackhaven:**
- HVN (100M supply):
  - 12.5% Core Contributors (1yr cliff + 2yr vesting)
  - 7.5% Private Round (1yr cliff + 3mo vesting)
  - 22% Public Round (NO vesting, unlocked at TGE)
  - 18% Ecosystem (1yr linear vesting)
  - 40% Future Emissions & Rewards (no vesting)
- sHVN (staked HVN) - earns treasury growth rewards
- RBT = BLK (same token, reserve-backed)
- HPN positions as ERC-721 NFTs

### 3. Treasury Strategy

**notanohmfork:**
- Prioritized MegaETH ecosystem assets
- 10% OHM allocation (only confirmed percentage)
- Focus on MEGA accumulation ("blackhole effect")

**Blackhaven:**
- Dynamic allocation via HVN governance
- Whitelisted MegaETH DeFi strategies only
- ALL MEGA staked in MegaETH Sequencer → rewards cycle back to reserves
- MEGA Points earned from:
  - Forfeits (early HPN exits)
  - Early exit penalties
  - Capital allocations
  - Convert to MEGA → stake → compound

### 4. Price Mechanism

**notanohmfork:**
- Upper-bound RBS only
- No lower protection

**Blackhaven:**
- Premium Range Mechanism (PRM) - "Coming Soon"
- Captures RBT price range premiums
- Redirects surplus value to treasury reserves

### 5. User Experience

**notanohmfork:**
- Bond assets → Get RBT
- Simple flow

**Blackhaven:**
- Choose: HPN (protected) or Bonds (discounted)
- Multiple terms and options
- NFT receipts for positions

## What Stayed the Same

1. **Core Vision:** Liquidity engine for MegaETH ecosystem
2. **Treasury-First:** All value accrues to reserves
3. **MEGA Focus:** Creating demand sink for MEGA
4. **No Rebasing:** Clean token mechanics
5. **Yield Generation:** Multiple sources compound value

## Blackhaven's Five Core Objectives

1. **Protect Principal:** Ensure depositors' capital remains fully backed and redeemable
2. **Capture On-Chain Value:** Acquire rewards from whitelisted MegaETH DeFi strategies
3. **Accumulate Reserve Value:** Compound treasury through fees, forfeits, and rewards
4. **Anchor Liquidity:** Support and stabilize key MegaETH protocols
5. **Function as DAT Layer:** Self-sustaining treasury backbone for MegaETH

## What's New in Blackhaven

### Haven Protected Notes (HPN)
- Principal-protected positions as ERC-721 NFTs
- **ONLY** accepts USDM or USDMy deposits
- Accrues ecosystem yield + amplified MEGA Points (unavailable outside Blackhaven)
- Three exit options:
  1. Hold to Maturity: full principal + all rewards
  2. Early Redemption: principal minus fee, forfeit all rewards to treasury
  3. RBT Redemption: burn NFT for RBT equal to principal, forfeit rewards
- **ALWAYS** 7-day cooldown after maturity or early exit
- NOT market-priced instruments

### Fixed-Term Bonds (Aligned Bonds)
- Exchange quote assets (USDM/MEGA) for vested RBT at discount
- LP Bonds: deposit RBT-USDM LP tokens → LP stays in treasury FOREVER as POL
- User-selected vesting periods with varying discounts
- Early redemption possible (fee + forfeit unvested)
- Forfeited amounts strengthen treasury backing

### Proximity Market Integration
- Blackhaven actively participates in MegaETH sequencer and proximity markets
- Uses treasury MEGA to bid for sequencer-adjacent floorspace
- HVN governance decides how MEGA is positioned
- External participants can acquire HVN to influence MEGA utilization
- All actions transparent and verifiable onchain

### Baseline Markets Launch
- HVN launches on Baseline with guaranteed price floor (BLV)
- **Non-liquidatable leverage** - borrow against HVN without liquidation risk
- Capital-efficient borrowing scales liquidity automatically
- BLV only goes up, never down
- Liquidity cannot be pulled by third parties

## Why Evolve?

1. **Better UX:** Multiple products serve different users
2. **Risk Management:** Principal protection option
3. **Deeper Liquidity:** LP bonds create POL
4. **Governance:** HVN enables community control
5. **Integration:** Better MegaETH ecosystem fit

## TL;DR

notanohmfork → Blackhaven evolution key points:

**Products:**
- Single bonding → HPN (USDM/USDMy only) + Fixed-Term Bonds + LP Bonds

**Tokens:**
- RBT + sRBT → HVN (100M supply) + sHVN + RBT=BLK + HPN NFTs

**Treasury:**
- Basic MEGA accumulation → Active strategies + ALL MEGA staked in sequencer + MEGA Points system

**Unique Features:**
- HVN launches on Baseline with non-liquidatable leverage
- LP tokens locked FOREVER as POL
- 7-day cooldown on ALL HPN exits
- Forfeited rewards strengthen treasury

The evolution transformed a simple concept into a comprehensive liquidity engine.
