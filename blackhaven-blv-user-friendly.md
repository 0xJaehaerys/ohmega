# Blackhaven BLV - User-Friendly Visual Guide

## How BLV (Baseline Value) Works - Simple Explanation

```mermaid
graph TB
    subgraph "BLV Formula"
        FORMULA[BLV = Locked Reserve / Token Supply<br/>Guaranteed Price Floor]
        EXAMPLE[Example:<br/>Reserve: $100,000<br/>Supply: 100,000 HVN<br/>BLV = $1.00]
    end
    
    subgraph "How BLV Protects You"
        PROTECTION[Price Protection<br/>HVN can NEVER go below BLV<br/>Even in worst case]
        BUYBACK[Automatic Buyback<br/>When price drops to BLV<br/>Protocol buys and burns tokens]
        GROWTH[BLV Only Goes Up<br/>Every trade increases reserve<br/>BLV rises over time]
    end
    
    subgraph "What Happens When Price Drops"
        DROP[Market Price Drops<br/>to $0.50]
        CHECK[Price approaches BLV<br/>$1.00]
        ACTION[Protocol Activates<br/>Buyback at BLV]
        BURN[Tokens Bought<br/>and Burned]
        NEW_BLV[New BLV Calculated<br/>Higher than before]
    end
    
    FORMULA --> EXAMPLE
    EXAMPLE --> PROTECTION
    PROTECTION --> BUYBACK
    PROTECTION --> GROWTH
    
    DROP --> CHECK
    CHECK --> ACTION
    ACTION --> BURN
    BURN --> NEW_BLV
```

## BLV Price Dynamics - Visual Graph

```mermaid
graph LR
    subgraph "Price vs BLV Over Time"
        T0[Day 0<br/>Price: $1.00<br/>BLV: $0.90]
        T1[Month 1<br/>Price: $2.50<br/>BLV: $1.20]
        T2[Month 3<br/>Price: $5.00<br/>BLV: $2.10]
        T3[Month 6<br/>Price: $8.00<br/>BLV: $3.50]
        T4[Year 1<br/>Price: $15.00<br/>BLV: $7.00]
    end
    
    subgraph "BLV Floor Protection"
        FLOOR[BLV Floor<br/>Only Goes Up<br/>Never Down]
    end
    
    T0 -->|BLV Rises| T1
    T1 -->|BLV Rises| T2
    T2 -->|BLV Rises| T3
    T3 -->|BLV Rises| T4
    
    T0 -.->|Protected by| FLOOR
    T1 -.->|Protected by| FLOOR
    T2 -.->|Protected by| FLOOR
    T3 -.->|Protected by| FLOOR
    T4 -.->|Protected by| FLOOR
```

## BLV Growth Mechanism - How It Works

```mermaid
sequenceDiagram
    participant User
    participant Market
    participant Baseline
    participant Reserve
    participant BLV
    
    Note over User,BLV: Every Trade Increases BLV
    
    User->>Market: Buy HVN at $2.00
    Market->>Baseline: Process trade
    Baseline->>Reserve: Add portion to locked reserve
    Reserve->>BLV: Calculate new BLV
    BLV-->>User: BLV increased from $1.20 to $1.25
    
    Note over User,BLV: Price Drops Near BLV
    
    Market->>User: Price drops to $1.10
    User->>Baseline: Check BLV protection
    Baseline->>BLV: Current BLV = $1.20
    BLV-->>Baseline: Price above BLV, no action
    
    Note over User,BLV: Price Hits BLV - Buyback Activates
    
    Market->>User: Price drops to $1.20 (BLV)
    User->>Baseline: Sell at BLV
    Baseline->>Reserve: Pay $1.20 per HVN
    Baseline->>Baseline: Burn purchased tokens
    Reserve->>BLV: Recalculate BLV
    BLV-->>User: New BLV = $1.25 (increased!)
    
    Note over User,BLV: BLV Can Never Go Down
```

## BLV Zones - How Baseline Market Maker Works

```mermaid
graph TB
    subgraph "Price Zones Relative to BLV"
        PREMIUM[Premium Zone<br/>Price > BLV × 1.5<br/>Protocol may sell<br/>Capture premium]
        NEUTRAL[Neutral Zone<br/>BLV × 1.1 < Price < BLV × 1.5<br/>Market driven<br/>Normal trading]
        BLV_ZONE[BLV Zone<br/>Price < BLV × 1.1<br/>Buyback active<br/>Defend floor]
    end
    
    subgraph "BLV Floor"
        FLOOR[BLV Floor<br/>Price = BLV<br/>Guaranteed buyback<br/>Tokens burned]
    end
    
    PREMIUM --> NEUTRAL
    NEUTRAL --> BLV_ZONE
    BLV_ZONE --> FLOOR
    
    FLOOR -.->|Can never go below| IMPOSSIBLE[Price Below BLV<br/>Impossible]
```

## Example: BLV Growth Timeline

```mermaid
graph LR
    subgraph "Launch"
        L0[Day 0<br/>Reserve: $100K<br/>Supply: 100K HVN<br/>BLV: $1.00<br/>Price: $1.00]
    end
    
    subgraph "Month 1"
        L1[Trading Volume<br/>Reserve: $120K<br/>Supply: 100K HVN<br/>BLV: $1.20<br/>Price: $2.50]
    end
    
    subgraph "Month 6"
        L6[More Trading<br/>Reserve: $350K<br/>Supply: 100K HVN<br/>BLV: $3.50<br/>Price: $8.00]
    end
    
    subgraph "Year 1"
        L12[Steady Growth<br/>Reserve: $700K<br/>Supply: 100K HVN<br/>BLV: $7.00<br/>Price: $15.00]
    end
    
    subgraph "Buyback Event"
        BUYBACK[Price drops to $7.00<br/>Protocol buys 10K HVN<br/>Burns 10K HVN<br/>New Supply: 90K HVN<br/>New BLV: $7.78]
    end
    
    L0 -->|Trades increase reserve| L1
    L1 -->|Trades increase reserve| L6
    L6 -->|Trades increase reserve| L12
    L12 -->|Buyback + burn| BUYBACK
```

## BLV vs Traditional Tokens - Side by Side

```mermaid
graph TB
    subgraph "Traditional Token (No BLV)"
        TRAD_BUY[You Buy Token<br/>$1.00]
        TRAD_DROP[Market Crashes<br/>Price: $0.20]
        TRAD_LOSS[You Lose 80%<br/>No Protection]
        TRAD_LOAN[Your Loan<br/>Liquidated]
    end
    
    subgraph "HVN with BLV"
        HVN_BUY[You Buy HVN<br/>$1.00<br/>BLV: $0.90]
        HVN_DROP[Market Crashes<br/>Price: $0.20]
        HVN_BLV[BLV Still $0.90<br/>Protected Floor]
        HVN_BUYBACK[Protocol Buys<br/>at BLV $0.90]
        HVN_SAFE[You're Protected<br/>Can sell at BLV<br/>Loan Safe]
    end
    
    TRAD_BUY --> TRAD_DROP
    TRAD_DROP --> TRAD_LOSS
    TRAD_DROP --> TRAD_LOAN
    
    HVN_BUY --> HVN_DROP
    HVN_DROP --> HVN_BLV
    HVN_BLV --> HVN_BUYBACK
    HVN_BUYBACK --> HVN_SAFE
```

## Safe Borrowing Against BLV

```mermaid
sequenceDiagram
    participant You
    participant Baseline
    participant BLV
    participant Lending
    
    Note over You,Lending: You Want to Borrow Safely
    
    You->>Baseline: I have 1000 HVN
    Baseline->>BLV: Check current BLV
    BLV-->>Baseline: BLV = $3.50 per HVN
    Baseline->>Lending: Collateral value = $3,500
    Lending-->>You: Borrow up to $3,500<br/>0% interest possible<br/>NO liquidation risk
    
    Note over You,Lending: Even if Market Crashes
    
    You->>You: Market crashes<br/>Price: $0.50
    You->>BLV: Check my protection
    BLV-->>You: BLV still $3.50<br/>Your collateral still worth $3,500<br/>Loan is safe
    
    Note over You,Lending: BLV Can Only Go Up
    
    You->>BLV: What if BLV increases?
    BLV-->>You: BLV now $4.00<br/>Your collateral worth $4,000<br/>Can borrow more!
```

## Key Benefits Summary

```mermaid
graph TD
    subgraph "BLV Benefits"
        BENEFIT1[Guaranteed Floor<br/>Price can never go below BLV<br/>Know your worst case]
        BENEFIT2[Automatic Buyback<br/>Protocol defends BLV<br/>Buys and burns when needed]
        BENEFIT3[Rising Floor<br/>BLV only increases<br/>Every trade helps]
        BENEFIT4[Safe Leverage<br/>Borrow up to BLV value<br/>No liquidation risk]
        BENEFIT5[Locked Liquidity<br/>Reserve cannot be removed<br/>Permanent protection]
    end
    
    subgraph "What This Means"
        MEANING1[You Can Plan<br/>Know minimum value]
        MEANING2[Downside Protected<br/>Automatic defense]
        MEANING3[Floor Grows<br/>Protection increases]
        MEANING4[Use Leverage<br/>Without fear]
        MEANING5[Stable Foundation<br/>No rug pulls]
    end
    
    BENEFIT1 --> MEANING1
    BENEFIT2 --> MEANING2
    BENEFIT3 --> MEANING3
    BENEFIT4 --> MEANING4
    BENEFIT5 --> MEANING5
```

---

## Simple Explanation

**BLV (Baseline Value) = Your Guaranteed Safety Net**

- **What it is**: BLV = Locked Reserve / Token Supply. A guaranteed minimum price that can NEVER go down
- **How it grows**: Every trade adds to the locked reserve, increasing BLV. When price drops to BLV, protocol buys tokens and burns them, reducing supply and increasing BLV further
- **Why it matters**: Even if the market crashes, you're protected at BLV level. You can always sell at BLV price
- **Bonus**: You can borrow money using HVN as collateral, up to BLV value, with NO liquidation risk because BLV can only go up

**Think of it like**: A house with a guaranteed minimum value that only increases. Even if the market crashes, you know your house is worth at least BLV amount. And you can borrow against that guaranteed value safely, with no risk of losing your house.

---

*Simple visual guide to understanding BLV protection for HVN holders based on Baseline Markets mechanism*
