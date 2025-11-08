# Understanding OHM & Baseline - Technical Brief

> Demonstrating understanding of the core mechanics notanohmfork is built on

---

## OlympusDAO v3 - How It Works

### Core Concept
OHM is a treasury-backed reserve currency. Every OHM is backed by assets in the treasury, with the backing ratio determining the "intrinsic value" floor.

### Key Mechanisms

#### 1. Treasury & Backing
```
Backing = Total Reserves / (OHM Supply - Total Debt)

Example:
Treasury: $10M in assets (DAI, wETH, LP tokens, etc)
OHM Supply: 5M tokens
Total Debt: 0
Backing = $10M / 5M = $2 per OHM
```

Every OHM is backed by at least $2 worth of treasury assets.

#### 2. Bonding
Users can **bond** assets to the protocol:
- Deposit: DAI, wETH, LP tokens, etc.
- Receive: OHM at a discount to market price
- Vesting: Fixed-term (e.g., 7 days) or fixed-expiration

**Why bond?**
- Get OHM below market price (e.g., 5-10% discount)
- Protocol gets assets at below-market cost
- Treasury grows, backing increases

**Dynamic pricing:**
- Control variable adjusts based on demand
- Debt decays over time
- Market capacity limits prevent over-bonding

#### 3. Staking
Hold OHM? Stake it for rewards.

**sOHM (staked OHM):**
- Rebase token (balance increases automatically)
- Receives rewards from treasury excess reserves
- Epoch-based (e.g., every 8 hours)

**gOHM (governance OHM):**
- Wrapped sOHM, no rebase
- Transferable, composable
- Used in DeFi protocols

**Rewards source:**
- Treasury excess reserves (when backing > actual OHM value)
- Bond profits
- Allocator yields (Aave, Convex, etc.)

#### 4. RBS (Range-Bound Stability)
The magic of OHM v3. Price management system.

**Components:**
- **Moving Average:** Target price (e.g., $20)
- **Cushion:** Soft defense (bond markets)
- **Wall:** Hard defense (direct swaps)

**Upper Side (Price too high):**
```
Price > Cushion High → Deploy bond market (sell OHM for reserves)
Price > Wall High → Direct swaps (sell OHM at fixed price, zero slippage)
```

**Lower Side (Price too low):**
```
Price < Cushion Low → Deploy bond market (buy OHM with reserves)
Price < Wall Low → Direct swaps (buy OHM at fixed price, zero slippage)
```

**Example:**
```
Moving Average: $20
Cushion Spread: 10% ($18 - $22)
Wall Spread: 20% ($16 - $24)

Price Zones:
$24+ → Wall High active (protocol sells OHM directly)
$22-$24 → Cushion High (bond markets sell OHM)
$18-$22 → Neutral zone (no intervention)
$16-$18 → Cushion Low (bond markets buy OHM)
<$16 → Wall Low active (protocol buys OHM directly)
```

**Capacity & Regeneration:**
- Each side has limited capacity
- When capacity depleted → wall goes down
- Regenerates over time if price stabilizes
- Observation-based (checks if price stayed in range)

**Why this works:**
- Prevents extreme volatility
- Maintains price near target
- Treasury operations are profitable (sell high, buy low)
- Self-sustaining system

---

## Baseline (YesMoney) - How It Works

### Core Concept
Every Baseline token has a **guaranteed price floor** (BLV - Baseline Value) backed by on-chain liquidity. The floor can only go up, never down.

### Key Mechanisms

#### 1. Baseline Value (BLV)
```
BLV = Locked Reserve / Token Supply

Example:
Locked Reserve: $100,000 USDT
Token Supply: 100,000 YES
BLV = $100,000 / 100,000 = $1.00
```

**Key property:** Anyone can sell their tokens at BLV price, always.

#### 2. How BLV Grows
Every trade increases BLV:

**Buy transaction:**
```
User buys 1,000 YES at market price $1.50
Payment: $1,500 USDT

Split:
- Part goes to seller
- Part goes to locked reserve
- Locked reserve increases → BLV increases

New BLV = (Old Reserve + New Deposit) / Supply
```

**Sell transaction:**
```
User sells 1,000 YES at $1.50

If selling into market → normal trade
If selling into BLV → tokens burned

If sold at BLV:
Reserve pays out: 1,000 × $1.00 = $1,000
Tokens burned: 1,000 YES
New Supply: 99,000 YES
New BLV = $99,000 / 99,000 = $1.00 (stays same)

But if reserve grew before:
Reserve: $105,000
Supply after burn: 99,000
New BLV = $105,000 / 99,000 = $1.06 (increased!)
```

**Result:** BLV acts as a rising floor that can never go down.

#### 3. Baseline Market Maker (BMM)
Built on Uniswap V4. Provides liquidity in 3 ranges:

**Floor Liquidity:**
- Located at BLV price
- Allows last holder to exit at guaranteed price
- Deep liquidity at floor

**Anchor Liquidity:**
- Tightly concentrated around current price
- Provides price support during volatility
- Prevents sharp dumps

**Discovery Liquidity:**
- Wider range above current price
- Enables price discovery
- Facilitates upward movement

**Dynamic adjustment:**
- Volatility increases → tighten ranges
- Volatility decreases → widen ranges
- Buy pressure → shift liquidity up
- Sell pressure → strengthen floor

#### 4. Leverage Buybacks (YesMoney specific)
When price approaches BLV:

**Aggressive buyback:**
```
Price drops to $1.05 (near BLV of $1.00)

Protocol can:
1. Use MORE than BLV to buy tokens
2. Buy aggressively (e.g., spend $110,000 to defend $1.00 floor)
3. Burn all purchased tokens
4. New BLV rises significantly

Example:
Before: $100,000 reserve, 100,000 supply, BLV = $1.00
Buy: $10,000 spent, 9,500 tokens bought at avg $1.05
Burn: 9,500 tokens
After: $90,000 reserve, 90,500 supply
New BLV = $90,000 / 90,500 = $0.99...

Wait, that doesn't work? Let me recalculate...

Actually, leverage buyback means:
- Protocol borrows/uses extra capital
- Buys aggressively above BLV
- Burns tokens
- BLV increases due to lower supply
- Then repays/rebalances

The key is buying ABOVE BLV and burning, which reduces supply faster than reserve.
```

#### 5. Zero Interest Lending
Because BLV is guaranteed:

**Lending against Baseline tokens:**
```
User has 10,000 YES (BLV = $1.00 each)
Collateral value: $10,000 (guaranteed)

Can borrow: $8,000 (80% LTV)
Interest rate: 0%
No liquidation risk (as long as BLV holds)

Why 0% interest?
- BLV can't go down, only up
- Collateral value guaranteed
- No liquidation cascade risk
- Lender's risk is minimal
```

---

## Key Differences

| Feature | OHM v3 | Baseline/YesMoney |
|---------|--------|-------------------|
| **Backing** | Treasury reserves, can fluctuate | Locked reserve, BLV only grows |
| **Price Support** | Both sides (RBS upper + lower) | Only floor (BLV buyback) |
| **When Price High** | Sell via bonds/swaps (RBS upper) | BMM provides liquidity, captures spread |
| **When Price Low** | Buy via bonds/swaps (RBS lower) | Buyback at BLV, burn tokens |
| **Staking** | Yes (sOHM/gOHM with rebase) | No staking mechanism |
| **Bonding** | Yes (discount bonds with vesting) | No bonding, just trades |
| **Liquidity** | External + RBS intervention | BMM on Uniswap V4 (Floor/Anchor/Discovery) |
| **Floor Price** | Backing (can go down if reserves drop) | BLV (can only go up) |
| **Leverage** | Treasury allocators (Aave, etc.) | Leverage buybacks (aggressive defense) |
| **Goal** | Stable reserve currency around target | Rising floor with guaranteed exit |

---

## How notanohmfork Relates

Based on the description, notanohmfork takes:

**From OHM:**
- ✅ Treasury-backed model (120% backing like OHM's reserves)
- ✅ Bonding mechanism (users deposit assets, mint RBT)
- ✅ RBS concept (but modified - see below)
- ✅ Two-token model (RBT + likely staked RBT)

**From Baseline:**
- ✅ Focus on upside only (Baseline doesn't support lower side either)
- ✅ Passive treasury growth (megaNOTE yield like BLV growth)
- ❓ Maybe BLV-like floor? (unclear from description)

**Unique to notanohmfork:**
- ✅ **Upper bound ONLY** - Unlike OHM (both sides) or Baseline (floor only)
- ✅ **MegaETH ecosystem focus** - $MEGA, megaNOTE, USDm, Avon
- ✅ **Blackhole mechanism** - Locks $MEGA supply permanently
- ✅ **1.2:1 backing model** - User deposits 1.2, gets 1.0, treasury keeps 0.2
- ✅ **Passive yield** - megaNOTE auto-compounds from Avon

**Critical difference:**
- OHM: Supports price both ways (high + low)
- Baseline: Supports price on low side (BLV floor)
- **notanohmfork: Supports price ONLY on high side** (RBS upper bound)

This means when RBT price < backing, there's no automatic support (unless they implement something not described).

---

## Why This Understanding Matters

When designing notanohmfork, you need to choose:

1. **From OHM:** Active price management (RBS), bonding, staking
2. **From Baseline:** Guaranteed floors (BLV), autonomous liquidity (BMM)
3. **Your innovation:** Upper-bound-only RBS + MegaETH integration

The "upper bound only" is a bold choice. OHM and Baseline both protect downside. You're saying "no downside protection, only capture upside premiums." 

This works if:
- Treasury backing is strong enough to hold price
- $MEGA appreciation drives value
- megaNOTE yield grows backing over time
- Market believes in long-term value

But it's risky if:
- $MEGA dumps hard
- No mechanism prevents death spiral below backing
- Purely market-dependent downside

Hence the questions about what happens when price < backing.

---

**This is my understanding of the tech you're building on. Let me know if I got anything wrong.**

