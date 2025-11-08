# Questions for notanohmfork Team

> Based on deep study of OHM v3 and Baseline Markets

---

## Understanding Check

### What I've Studied:

**OlympusDAO v3:**
- ✅ Range-Bound Stability (RBS) with Cushion + Wall
- ✅ Cushion = Bond markets (soft defense, both sides)
- ✅ Wall = Direct swaps at fixed price (hard defense, both sides)
- ✅ Treasury management (deposits, reserves, debt tracking)
- ✅ Bonding mechanics (quote token → OHM, with vesting)
- ✅ Staking (OHM → sOHM/gOHM, rebase rewards)
- ✅ Operator policy (heart-triggered market operations)
- ✅ Regeneration logic (capacity, thresholds, observations)

**Baseline Markets (YesMoney):**
- ✅ Baseline Value (BLV) - guaranteed floor price
- ✅ BLV grows with every trade (locked reserve accumulation)
- ✅ Baseline Market Maker (BMM) on Uniswap V4
- ✅ Three liquidity ranges: Floor + Anchor + Discovery
- ✅ Leverage buybacks (aggressive buy + burn when price → BLV)
- ✅ Volatility-aware dynamic adjustments
- ✅ 0% interest, unliquidatable lending against BLV collateral

---

## Critical Questions

### 1. Two-Token Model

**Source text says:** "An onchain OHM-style DAT using a two-token model"

**Question:** What are the two tokens?
- Is it RBT + something else?
- Or is "two-token" referring to RBT + the backing assets ($MEGA, megaNOTE, etc)?
- Is there a governance token separate from RBT?

---

### 2. Upper Bound ONLY - What About Downside?

**Source text says:** "RBS mechanism that only triggers on the upper bound of price movement"

**OHM has:**
- Upper bound (high wall/cushion) → Protocol SELLS OHM when price too high
- Lower bound (low wall/cushion) → Protocol BUYS OHM when price too low

**Baseline has:**
- BLV floor → Always buys at BLV (buyback + burn)

**Question for notanohmfork:**
- When RBT price < backing (1.2:1), what happens?
- Is there NO protection like OHM's lower wall?
- Or is there a BLV-like mechanism (always buy at backing)?
- Or completely free-float below backing?

**This is critical because:**
- OHM: Supports price both ways
- Baseline: Supports with BLV floor
- notanohmfork: "Upper bound only" = no lower support?

---

### 3. Staking Mechanism

**OHM has:**
- sOHM (staked OHM, rebase token)
- gOHM (wrapped, no rebase, transferable)
- Rewards from treasury excess reserves
- Epoch-based distribution

**Question:** Will notanohmfork have staking?
- If yes, what's the token? sRBT? gRBT?
- Where do rewards come from?
  - Treasury yield (megaNOTE, $MEGA appreciation)?
  - Bond profits (0.2 excess from mints)?
  - RBS premium capture?
- Rebase or fixed-supply staking token?

---

### 4. Bonding vs Direct Minting

**OHM bonding:**
- Market-based pricing (dynamic control variable, debt decay)
- Fixed-term or fixed-expiration vesting
- Discount to market price
- Market capacity limits

**notanohmfork text says:**
- "Bond deposits mint a reserve-backed token (RBT) supported by 120% collateral"
- "When users deposit, they mint 1.2:1.2 in theory, but the extra 0.2 is allocated to the treasury"

**Question:** Is this bonding or direct minting?
- **Option A:** Direct mint (user deposits 1.2 → instantly get 1.0 RBT)
- **Option B:** Bond market (user deposits, gets claim, vests over time)
- How is price determined if direct mint?
- If bond market, what are the terms (duration, discount, etc)?

---

### 5. RBS Technical Parameters

**OHM v3 RBS has:**
- Cushion spread (% above/below moving average)
- Wall spread (% above/below moving average)
- Cushion factor (% of capacity for bond markets)
- Reserve factor (% of reserves for capacity)
- Regen parameters (wait time, threshold, observations)

**Question:** What are notanohmfork's RBS parameters?
- At what % above backing does RBS trigger? (e.g., 105%? 110%? 120%?)
- How much RBT is sold per activation?
- Is there a cushion-like mechanism (bond markets) or direct sales only?
- Regeneration logic for capacity?

---

### 6. Backing Calculation

**OHM formula:**
```
Backing = Total Reserves / (OHM Supply - Total Debt)
Excess Reserves = Total Reserves - (OHM Supply - Total Debt)
```

**notanohmfork text says:**
- "Each circulating RBT is backed 1.2:1"
- "When users deposit, they mint 1.2:1.2 in theory, but the extra 0.2 is allocated to the treasury, ensuring every circulating token remains backed between 1 and 1.2"

**Question:** How is backing tracked?
```
Option A: Backing = Treasury Value / RBT Supply
         Always should be ≥ 1.2

Option B: Backing = (Initial Deposit - 0.2) / 1.0 RBT
         So each RBT has "birth backing" of 1.0?
         And treasury holds the 0.2 separately?

Option C: Something else?
```

**Follow-up:**
- Does backing ratio change per RBT over time?
- Or is there a global backing ratio for all RBT?
- How do treasury revenues (megaNOTE yield, $MEGA appreciation) affect backing?

---

### 7. Blackhole Mechanism Details

**Source text says:**
- "Obviously, this requires the MegaETH ecosystem to work together to create a blackhole-style liquidity capture mechanism that feeds the treasury while reducing the circulating supply of $MEGA"

**Question:** How does this actually work?
- notanohmfork treasury bonds $MEGA → locks it
- Does treasury STAKE the $MEGA (for sequencer rotation)?
- Does treasury LOCK the $MEGA (for proximity markets)?
- Or just holds passively?
- Can treasury ever sell $MEGA or is it permanently locked?
- What happens if $MEGA depreciates significantly?

---

### 8. megaNOTE Integration

**What we know:**
- megaNOTE = Avon's yield-bearing vault token (deposit USDm → mint megaNOTE)
- Auto-accrues yield from Avon lending
- Treasury holds megaNOTE

**Question:** How does treasury get megaNOTE?
```
Option A: From bond deposits
         - User deposits USDm → treasury mints RBT
         - Treasury converts USDm → megaNOTE

Option B: From treasury management
         - Treasury starts with various assets
         - Actively deploys to Avon for megaNOTE

Option C: Mix of both?
```

**Follow-up:**
- Can users directly bond megaNOTE for RBT?
- Or only bond USDm which treasury converts?

---

### 9. Baseline-Inspired Features

**Baseline has unique features notanohmfork might adopt:**

**Question A: BLV-like floor?**
- Will RBT have a guaranteed buy-back at backing value (like BLV)?
- Or completely market-dependent below backing?

**Question B: Leverage buybacks?**
- When RBT below backing, can treasury use MORE than backing value to aggressively buy?
- Or strictly limited to available capacity?

**Question C: Liquidity ranges?**
- Will there be Floor + Anchor + Discovery liquidity (like BMM)?
- Or single-price RBS activation?

**Question D: Lending against RBT?**
- Can RBT be used as collateral for 0% interest loans (like Baseline)?
- What's the LTV if yes?

---

### 10. Revenue Distribution

**Treasury revenue sources (from text):**
1. $MEGA appreciation
2. megaNOTE yield
3. Arbitrage from deposits
4. Yield farming
5. Staking rewards ($MEGA ecosystem)
6. RBS premium capture

**Question:** Where do these revenues go?
```
Option A: All to backing (strengthen RBT value)
Option B: Split between backing + stakers (if staking exists)
Option C: Split between backing + protocol-owned liquidity
Option D: Treasury discretion based on policy
```

---

### 11. Initial Bootstrap

**Question:** How does protocol launch?
- Initial treasury composition? (seed funding?)
- First RBT mints at what ratio?
- Who can bond initially? (whitelist? public?)
- How is backing established from day 1?

---

### 12. Comparison Matrix Request

Can you provide a comparison showing:

| Feature | OHM v3 | Baseline | notanohmfork |
|---------|--------|----------|--------------|
| **Price Support** | Both sides (wall + cushion) | BLV floor only | Upper bound only (?) |
| **Backing** | Treasury reserves | Locked reserve (BLV) | 120% backing |
| **When Price High** | Sell OHM (bonds/swaps) | Sell with BMM | RBS sells RBT ✓ |
| **When Price Low** | Buy OHM (bonds/swaps) | Buy at BLV (buyback) | ??? |
| **Staking** | sOHM/gOHM rebase | No staking | ??? |
| **Bonding** | Market-based, vesting | No bonding | Direct mint (?) |
| **Liquidity** | External DEX + RBS | BMM on Uni V4 | ??? |
| **Revenue** | Bonds, RBS, allocators | Trading spreads, BLV growth | megaNOTE yield, $MEGA, RBS |

---

### 13. Risk Scenarios

**Question:** What happens in these scenarios?

**Scenario A: $MEGA crashes 50%**
- Treasury loses value in $MEGA holdings
- Does backing ratio drop?
- Does this trigger any mechanism?
- Can RBT be redeemed at backing still?

**Scenario B: megaNOTE vault exploited**
- Avon protocol has a hack
- megaNOTE value → 0
- What % of treasury is affected?
- How is RBT backing maintained?

**Scenario C: No demand for RBT**
- Price = backing (1.0)
- No RBS premium to capture
- Revenue only from megaNOTE yield
- Is system sustainable long-term?

**Scenario D: Extreme sell pressure**
- RBT price drops to 0.5 (below backing)
- No lower bound protection
- Treasury can't intervene
- What prevents death spiral?

---

### 14. Governance

**OHM has:**
- Governor contracts (proposals, voting)
- Timelock for changes
- Community-driven policy

**Question:** Will notanohmfork have governance?
- Who controls RBS parameters?
- Who decides treasury allocation?
- How are upgrades managed?
- Is there a DAO or team-controlled?

---

### 15. Technical Implementation

**Question:** What's the tech stack?
- Built on MegaETH L2 (obviously)
- Using what AMM? (Uniswap V3/V4? Custom?)
- Bond market infrastructure? (Bond Protocol? Custom?)
- Oracle for price feeds? (Chainlink? MegaETH native?)
- Smart contract architecture? (modules like OHM v3? monolithic?)

---

## Summary of Key Unknowns

**From OHM perspective:**
1. ❓ Lower bound protection? (OHM has it, notanohmfork text says "upper only")
2. ❓ Staking mechanism? (OHM has sOHM/gOHM)
3. ❓ Bonding vs direct mint?
4. ❓ Governance structure?

**From Baseline perspective:**
1. ❓ BLV-like guaranteed floor? (Baseline has it)
2. ❓ Leverage buybacks? (YesMoney has it)
3. ❓ Dynamic liquidity ranges? (BMM has Floor/Anchor/Discovery)
4. ❓ 0% interest lending? (Baseline enables this)

**notanohmfork-specific:**
1. ❓ Two-token model clarification
2. ❓ Blackhole mechanism implementation
3. ❓ Revenue distribution logic
4. ❓ Risk mitigation strategies
5. ❓ Launch parameters

---

**Please clarify these points so we can create accurate architecture diagrams.**

