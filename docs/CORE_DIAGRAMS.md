# notanohmfork - Core Diagrams

> 4 essential diagrams that explain everything.

---

## 1. Complete System - Everything In One

```mermaid
graph TB
    subgraph USERS["USERS"]
        U1["Has $MEGA<br/>Don't want to sell"]
        U2["Missed ICO<br/>Want exposure"]
        U3["Has $OHM<br/>Want to participate"]
    end
    
    subgraph MEGAETH["MegaETH Network"]
        SEQ["Sequencer Rotation<br/>Operators stake $MEGA"]
        PROX["Proximity Markets<br/>Apps lock $MEGA"]
        NETWORK_DEMAND["Network Demand<br/>More usage = More $MEGA needed"]
    end
    
    subgraph BONDING["Bonding Process"]
        DEPOSIT["User deposits 1.2 units<br/>($MEGA, $OHM, stables, etc)"]
        MINT["Protocol mints 1.0 RBT<br/>User receives token"]
        EXCESS["0.2 excess goes to Treasury<br/>Strengthens backing"]
    end
    
    subgraph TREASURY["Treasury - 120% Backing"]
        HOLDINGS["Asset Composition"]
        MEGA_T["$MEGA<br/>Blackhole effect<br/>Locked from circulation"]
        MNOTE_T["megaNOTE<br/>Yield-bearing vault<br/>From Avon"]
        USDM_T["USDm<br/>Stable backing<br/>Can deposit to Avon"]
        OHM_T["$OHM<br/>10% allocation<br/>Ohmies participate"]
        OTHER_T["Other Assets<br/>Diversified basket<br/>Yield opportunities"]
    end
    
    subgraph AVON["Avon Integration"]
        AVON_DEPOSIT["Treasury deposits USDm"]
        AVON_VAULT["Avon Vault"]
        AVON_MINT["Mint megaNOTE"]
        AVON_LEND["Order book lending<br/>to borrowers"]
        AVON_YIELD["Yield accrues<br/>to megaNOTE holders"]
    end
    
    subgraph RBT_TOKEN["RBT Token"]
        RBT["RBT<br/>Reserve-Backed Token<br/>Always backed 1.0-1.2"]
        MARKET["Trades on market"]
        PRICE_CHECK["Price monitoring"]
    end
    
    subgraph RBS_SYSTEM["RBS - Upper Bound Only"]
        MONITOR["Monitor RBT price<br/>vs backing value"]
        BELOW["Price ≤ Backing<br/>DO NOTHING"]
        ABOVE["Price > Backing<br/>ACTIVATE RBS"]
        SELL["Sell RBT from treasury"]
        CAPTURE["Capture premium"]
        REINVEST["Buy more assets<br/>Strengthen backing"]
    end
    
    subgraph REVENUE["Treasury Revenue"]
        REV1["$MEGA Appreciation<br/>Price goes up"]
        REV2["megaNOTE Yield<br/>Avon lending returns"]
        REV3["$MEGA Staking<br/>Ecosystem rewards"]
        REV4["Yield Farming<br/>DeFi protocols"]
        REV5["Arbitrage<br/>Market opportunities"]
        REV6["RBS Premium<br/>When price > backing"]
    end
    
    subgraph FLYWHEEL["Blackhole Effect"]
        LOCK["Treasury locks $MEGA"]
        SUPPLY["Circulating supply reduces"]
        PRICE_UP["$MEGA price appreciates"]
        MORE_DEMAND["More network demand"]
    end
    
    %% User Flow
    U1 --> DEPOSIT
    U2 --> DEPOSIT
    U3 --> DEPOSIT
    
    %% Bonding
    DEPOSIT --> MINT
    DEPOSIT --> EXCESS
    MINT --> RBT
    EXCESS --> HOLDINGS
    
    %% Treasury Holdings
    HOLDINGS --> MEGA_T
    HOLDINGS --> MNOTE_T
    HOLDINGS --> USDM_T
    HOLDINGS --> OHM_T
    HOLDINGS --> OTHER_T
    
    %% Avon Flow
    USDM_T --> AVON_DEPOSIT
    AVON_DEPOSIT --> AVON_VAULT
    AVON_VAULT --> AVON_MINT
    AVON_MINT --> MNOTE_T
    AVON_VAULT --> AVON_LEND
    AVON_LEND --> AVON_YIELD
    AVON_YIELD --> MNOTE_T
    
    %% Treasury backs RBT
    HOLDINGS -.->|Backs 1.2:1| RBT
    
    %% RBT Market
    RBT --> MARKET
    MARKET --> PRICE_CHECK
    
    %% RBS Logic
    PRICE_CHECK --> MONITOR
    MONITOR --> BELOW
    MONITOR --> ABOVE
    ABOVE --> SELL
    SELL --> CAPTURE
    CAPTURE --> REINVEST
    REINVEST --> HOLDINGS
    
    %% Revenue Streams
    REV1 --> HOLDINGS
    REV2 --> HOLDINGS
    REV3 --> HOLDINGS
    REV4 --> HOLDINGS
    REV5 --> HOLDINGS
    REV6 --> HOLDINGS
    
    %% MegaETH Integration
    SEQ --> NETWORK_DEMAND
    PROX --> NETWORK_DEMAND
    NETWORK_DEMAND --> MEGA_T
    
    %% Blackhole Effect
    MEGA_T --> LOCK
    LOCK --> SUPPLY
    SUPPLY --> PRICE_UP
    PRICE_UP --> REV1
    PRICE_UP --> MORE_DEMAND
    MORE_DEMAND --> NETWORK_DEMAND
```

**Key Points:**
- Users deposit 1.2, get 1.0 RBT, treasury gets 0.2 excess
- Treasury holds $MEGA (blackhole), megaNOTE (yield), USDm (stable), $OHM (10%), others
- RBS only activates when price > backing (upper bound only)
- Revenue from 6 sources: appreciation, yield, staking, farming, arbitrage, RBS
- MegaETH network growth → more $MEGA demand → treasury locks supply → price up

---

## 2. RBS - Upper Bound Only Mechanism

```mermaid
flowchart TD
    START([RBT Launches]) --> MARKET[RBT Trading on Market]
    
    MARKET --> MONITOR{Monitor Price}
    
    MONITOR --> CHECK{Compare Price<br/>vs<br/>Backing Value}
    
    CHECK -->|Price < Backing| SCENARIO_A["Scenario A: Below Backing<br/>Example: Backing = $10, Price = $8"]
    CHECK -->|Price = Backing| SCENARIO_B["Scenario B: At Backing<br/>Example: Backing = $10, Price = $10"]
    CHECK -->|Price > Backing| SCENARIO_C["Scenario C: Above Backing<br/>Example: Backing = $10, Price = $15"]
    
    SCENARIO_A --> NO_ACTION_A["❌ RBS INACTIVE<br/>Protocol does nothing<br/>No selling, no buying"]
    SCENARIO_B --> NO_ACTION_B["❌ RBS INACTIVE<br/>Protocol does nothing<br/>Market price = fair value"]
    
    SCENARIO_C --> RBS_ACTIVE["✅ RBS ACTIVATES<br/>Price premium detected"]
    
    RBS_ACTIVE --> STEP1["Step 1: Sell RBT from Treasury<br/>Example: Sell RBT at $15"]
    STEP1 --> STEP2["Step 2: Receive Payment<br/>Example: Get $15 per RBT sold"]
    STEP2 --> STEP3["Step 3: Calculate Premium<br/>Premium = $15 - $10 = $5"]
    STEP3 --> STEP4["Step 4: Use Proceeds<br/>Buy more backing assets<br/>$MEGA, megaNOTE, USDm, etc"]
    STEP4 --> STEP5["Step 5: Treasury Strengthens<br/>Backing now > 1.2:1"]
    STEP5 --> RESULT["Result: More sustainable<br/>Higher backing floor<br/>Protocol captures upside"]
    
    NO_ACTION_A --> WAIT_A["Continue monitoring<br/>No sell pressure<br/>Market finds price"]
    NO_ACTION_B --> WAIT_B["Continue monitoring<br/>Fair value maintained"]
    
    WAIT_A --> MONITOR
    WAIT_B --> MONITOR
    RESULT --> MONITOR
    
    NOTE1["Why Upper Bound Only?"]
    NOTE2["Traditional DATs expand AND contract<br/>= Sell pressure during consolidation"]
    NOTE3["notanohmfork only acts on upside<br/>= No downside interference<br/>= Only captures premium<br/>= Self-strengthening mechanism"]
    
    SCENARIO_C -.-> NOTE1
    NOTE1 -.-> NOTE2
    NOTE2 -.-> NOTE3
```

**Real Example:**

```
Initial State:
- 1000 RBT circulating
- Treasury has $12,000 backing ($12 per RBT)
- RBT trading at $10 on market

❌ RBS Inactive (price below backing)

---

Market Heats Up:
- MegaETH grows, $MEGA appreciates
- More users want RBT exposure
- RBT price rises to $18

✅ RBS Activates (price > backing)

Actions:
1. Sell 100 RBT from treasury at $18 = $1,800 received
2. Backing only needed: 100 × $12 = $1,200
3. Premium captured: $1,800 - $1,200 = $600
4. Use $600 to buy more $MEGA, megaNOTE, etc
5. Treasury now: $12,600 backing 900 RBT
6. New backing ratio: $12,600 / 900 = $14 per RBT

Result: Backing improved from $12 to $14 per RBT
```

---

## 3. Treasury Revenue Streams

```mermaid
graph TB
    subgraph TREASURY["Treasury"]
        TREASURY_CENTER["Current Holdings<br/>$MEGA + megaNOTE + USDm + $OHM + Others"]
        BACKING_VALUE["Total Backing Value<br/>Always ≥ 1.2 × RBT Supply"]
    end
    
    subgraph REVENUE_1["Revenue Stream 1: $MEGA Appreciation"]
        MEGA_START["Treasury holds $MEGA"]
        MEGA_DEMAND["MegaETH network grows<br/>More sequencer demand<br/>More proximity demand"]
        MEGA_LOCK["Treasury locks supply<br/>Blackhole effect"]
        MEGA_PRICE["$MEGA price increases"]
        MEGA_VALUE["Treasury value grows<br/>Passive appreciation"]
    end
    
    subgraph REVENUE_2["Revenue Stream 2: megaNOTE Yield"]
        MNOTE_START["Treasury holds megaNOTE"]
        MNOTE_AVON["megaNOTE = Avon vault token"]
        MNOTE_LEND["Avon lends USDm to borrowers"]
        MNOTE_INTEREST["Borrowers pay interest"]
        MNOTE_ACCRUE["Interest accrues to megaNOTE"]
        MNOTE_GROWTH["megaNOTE value grows<br/>Auto-compounds"]
    end
    
    subgraph REVENUE_3["Revenue Stream 3: $MEGA Staking"]
        STAKE_START["Treasury stakes $MEGA"]
        STAKE_ECOSYSTEM["MegaETH ecosystem rewards"]
        STAKE_INCENTIVES["Protocol incentives<br/>Network growth rewards"]
        STAKE_EARN["Earn staking rewards"]
    end
    
    subgraph REVENUE_4["Revenue Stream 4: Yield Farming"]
        FARM_START["Treasury deploys assets"]
        FARM_DEFI["MegaETH DeFi protocols"]
        FARM_LP["Liquidity provision<br/>Lending pools<br/>Vaults"]
        FARM_RETURN["Earn yields"]
    end
    
    subgraph REVENUE_5["Revenue Stream 5: Arbitrage"]
        ARB_START["Price differences"]
        ARB_DEPOSIT["Bond deposits create opportunities"]
        ARB_MARKET["Market inefficiencies"]
        ARB_CAPTURE["Capture arbitrage"]
    end
    
    subgraph REVENUE_6["Revenue Stream 6: RBS Premium"]
        RBS_START["RBT price > backing"]
        RBS_SELL["Sell RBT at premium"]
        RBS_DIFF["Premium = Market Price - Backing"]
        RBS_PROFIT["Capture premium"]
    end
    
    %% Flow 1
    MEGA_START --> MEGA_DEMAND
    MEGA_DEMAND --> MEGA_LOCK
    MEGA_LOCK --> MEGA_PRICE
    MEGA_PRICE --> MEGA_VALUE
    MEGA_VALUE --> BACKING_VALUE
    
    %% Flow 2
    MNOTE_START --> MNOTE_AVON
    MNOTE_AVON --> MNOTE_LEND
    MNOTE_LEND --> MNOTE_INTEREST
    MNOTE_INTEREST --> MNOTE_ACCRUE
    MNOTE_ACCRUE --> MNOTE_GROWTH
    MNOTE_GROWTH --> BACKING_VALUE
    
    %% Flow 3
    STAKE_START --> STAKE_ECOSYSTEM
    STAKE_ECOSYSTEM --> STAKE_INCENTIVES
    STAKE_INCENTIVES --> STAKE_EARN
    STAKE_EARN --> BACKING_VALUE
    
    %% Flow 4
    FARM_START --> FARM_DEFI
    FARM_DEFI --> FARM_LP
    FARM_LP --> FARM_RETURN
    FARM_RETURN --> BACKING_VALUE
    
    %% Flow 5
    ARB_START --> ARB_DEPOSIT
    ARB_DEPOSIT --> ARB_MARKET
    ARB_MARKET --> ARB_CAPTURE
    ARB_CAPTURE --> BACKING_VALUE
    
    %% Flow 6
    RBS_START --> RBS_SELL
    RBS_SELL --> RBS_DIFF
    RBS_DIFF --> RBS_PROFIT
    RBS_PROFIT --> BACKING_VALUE
    
    %% Treasury
    TREASURY_CENTER --> MEGA_START
    TREASURY_CENTER --> MNOTE_START
    TREASURY_CENTER --> STAKE_START
    TREASURY_CENTER --> FARM_START
    
    BACKING_VALUE --> GROWTH["Backing Grows<br/>RBT becomes more valuable<br/>Self-reinforcing cycle"]
```

**Revenue Breakdown:**

| Stream | Source | Type | Frequency |
|--------|--------|------|-----------|
| **$MEGA Appreciation** | Network growth → demand → locked supply | Passive | Continuous |
| **megaNOTE Yield** | Avon lending interest | Passive | Continuous |
| **$MEGA Staking** | Ecosystem rewards | Active | Periodic |
| **Yield Farming** | DeFi protocols on MegaETH | Active | Continuous |
| **Arbitrage** | Bond deposits, market inefficiencies | Active | Opportunistic |
| **RBS Premium** | Price > backing | Automatic | When triggered |

**Example Numbers (Hypothetical):**

```
Treasury: $10M
├─ $MEGA ($4M) → Appreciates 50% → +$2M
├─ megaNOTE ($3M) → 15% APY → +$450K/year
├─ $MEGA Staking → 5% rewards → +$200K/year
├─ Yield Farming → 10% APY → +$500K/year
├─ Arbitrage → Opportunistic → +$100K/year
└─ RBS Premium → When active → +$300K/event

Total: $3.55M+ annual growth (35%+ on $10M)
```

---

## 4. Avon Integration Example

```mermaid
sequenceDiagram
    participant Treasury as notanohmfork Treasury
    participant USDm as USDm Token
    participant Avon as Avon Protocol
    participant Vault as megaNOTE Vault
    participant Borrower as Borrower
    participant OrderBook as Order Book
    
    Note over Treasury: Treasury holds $100K USDm
    
    Treasury->>Avon: Deposit $100K USDm
    activate Avon
    
    Avon->>Vault: Route to megaNOTE Vault
    activate Vault
    
    Vault->>Treasury: Mint 100K megaNOTE tokens
    Note over Treasury,Vault: megaNOTE = receipt token<br/>Represents claim on vault
    
    deactivate Vault
    
    Note over Avon: Vault deploys capital to lending
    
    Avon->>OrderBook: Quote liquidity on order book
    activate OrderBook
    
    Note over OrderBook: Lending Pool Strategy:<br/>Interest Rate Curve<br/>Based on utilization
    
    Borrower->>OrderBook: Want to borrow $50K USDm
    Note over Borrower: Deposits collateral (ETH, BTC, etc)
    
    OrderBook->>Borrower: Match with best rates
    OrderBook->>Borrower: Lend $50K USDm
    
    Note over OrderBook: Utilization now: 50%<br/>Interest rate adjusts
    
    Borrower->>OrderBook: Pay interest (e.g., 12% APY)
    
    OrderBook->>Vault: Distribute interest
    deactivate OrderBook
    
    Note over Vault: Interest accrues to megaNOTE<br/>Value increases automatically
    
    Vault->>Vault: megaNOTE value grows
    Note over Vault: Originally: 1 megaNOTE = $1<br/>After yield: 1 megaNOTE = $1.12
    
    activate Vault
    Note over Treasury: Treasury still holds 100K megaNOTE<br/>But now worth $112K<br/>Gain: $12K (12% APY)
    deactivate Vault
    
    deactivate Avon
    
    rect rgb(240, 240, 240)
        Note over Treasury: Treasury can:<br/>1. Keep holding megaNOTE (yield compounds)<br/>2. Redeem for USDm + profit<br/>3. Use as collateral elsewhere
    end
    
    Note over Treasury: Result:<br/>+$12K in backing value<br/>No action needed (passive)<br/>RBT backing stronger
```

**Detailed Flow:**

### Phase 1: Deposit
```
Treasury: "I have $100K USDm, want yield"
         ↓
Avon: "Deposit to megaNOTE vault"
         ↓
Vault: "Here's 100K megaNOTE tokens"
         ↓
Treasury: "Got megaNOTE, it's yield-bearing"
```

### Phase 2: Lending
```
Vault: "I'll deploy this $100K to lending markets"
         ↓
Order Book: "Quoting rates based on utilization"
         ↓
Borrower: "I need $50K, here's my collateral"
         ↓
Order Book: "Matched at 12% APY interest rate"
         ↓
Borrower: "Paying interest over time"
```

### Phase 3: Yield Accrual
```
Time passes...
         ↓
Interest accumulates: $50K × 12% = $6K/year
         ↓
But vault has $100K total, 50% utilized
         ↓
Effective yield to vault: $6K / $100K = 6% APY
         ↓
megaNOTE value increases from $1.00 to $1.06
         ↓
Treasury's 100K megaNOTE now worth $106K
         ↓
Passive gain: $6K (no action needed)
```

### Phase 4: Compounding
```
Year 1: $100K → $106K (6% yield)
Year 2: $106K → $112.36K (6% on $106K)
Year 3: $112.36K → $119.10K (6% on $112.36K)
         ↓
Compounds automatically
         ↓
Treasury backing grows passively
         ↓
RBT becomes more valuable
```

**Why megaNOTE?**
- **Yield-bearing:** Auto-accrues lending returns
- **Liquid:** Can redeem for USDm anytime
- **Standard collateral:** Can use across MegaETH
- **Passive:** No active management needed
- **MegaETH native:** Built for the ecosystem

**Real Numbers Example:**

| Scenario | USDm Deposited | Utilization | Interest Rate | Annual Yield | megaNOTE Value |
|----------|----------------|-------------|---------------|--------------|----------------|
| Low Demand | $100K | 30% | 8% | $2.4K (2.4%) | $102.4K |
| Medium Demand | $100K | 60% | 12% | $7.2K (7.2%) | $107.2K |
| High Demand | $100K | 85% | 18% | $15.3K (15.3%) | $115.3K |

---

**All diagrams fact-based. No speculation.**

