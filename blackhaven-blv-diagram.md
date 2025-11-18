# Blackhaven BLV (Baseline Value) Diagram

## HVN on Baseline Markets - BLV Mechanism

```mermaid
graph TD
    subgraph "HVN Launch on Baseline"
        PUBLIC[Public Sale<br/>22M HVN<br/>No vesting]
        BASELINE[Baseline Markets]
        INITIAL_LIQ[Initial Liquidity]
    end
    
    subgraph "BLV Establishment"
        BLV_INIT[Initial BLV<br/>Price Floor Set]
        RESERVES[Locked Reserves<br/>Backs BLV]
        AMM[Autonomous AMM<br/>No manual LP]
    end
    
    subgraph "BLV Growth Mechanism"
        TRADES[Trading Activity]
        VOLUME[Volume Increases]
        BLV_RISE[BLV Rises]
        FLOOR[New Price Floor]
    end
    
    subgraph "Unique Features"
        NO_LIQ[Non-Liquidatable<br/>Loans]
        BORROW[Borrow up to BLV]
        LEVERAGE[Capital-Efficient<br/>Leverage]
    end
    
    PUBLIC -->|Proceeds to| BASELINE
    BASELINE -->|Establishes| INITIAL_LIQ
    INITIAL_LIQ -->|Creates| BLV_INIT
    BLV_INIT -->|Backed by| RESERVES
    
    RESERVES --> AMM
    AMM --> TRADES
    TRADES --> VOLUME
    VOLUME -->|Automatically| BLV_RISE
    BLV_RISE --> FLOOR
    
    FLOOR --> NO_LIQ
    FLOOR --> BORROW
    BORROW --> LEVERAGE
    
    LEVERAGE -->|More buying| TRADES
```

## BLV Mechanics Flow

```mermaid
sequenceDiagram
    participant User
    participant Baseline
    participant BLV
    participant Reserves
    participant Lending
    
    Note over Baseline: HVN Launch Day
    
    User->>Baseline: Buy HVN at launch
    Baseline->>Reserves: Lock liquidity
    Reserves->>BLV: Establish initial floor
    Note over BLV: Starting BLV = $0.90
    
    loop Market Activity
        User->>Baseline: Buy HVN
        Baseline->>Reserves: Add to reserves
        Reserves->>BLV: Increase floor
        Note over BLV: BLV only goes up
    end
    
    User->>Lending: Borrow against HVN
    Lending->>BLV: Check current BLV
    BLV-->>Lending: BLV = $1.50
    Lending-->>User: Loan up to $1.50 per HVN
    Note over User: No liquidation risk<br/>Loan secured by BLV
    
    User->>Baseline: Use loan to buy more HVN
    Note over Baseline: Creates positive feedback loop
```

## BLV vs Traditional Liquidity

```mermaid
graph LR
    subgraph "Traditional AMM"
        MM[Market Makers]
        LP_TRAD[LP Providers]
        RUG[Can Pull Liquidity]
        DUMP[Price Can Crash]
    end
    
    subgraph "Baseline BLV System"
        SYSTEM[System Managed]
        LOCKED[Liquidity Locked]
        FLOOR_ONLY[Price Floor Only Rises]
        PROTECTED[Downside Protected]
    end
    
    MM -->|Provide| LP_TRAD
    LP_TRAD -->|Risk of| RUG
    RUG -->|Causes| DUMP
    
    SYSTEM -->|Creates| LOCKED
    LOCKED -->|Ensures| FLOOR_ONLY
    FLOOR_ONLY -->|Provides| PROTECTED
```

## HVN Price Dynamics with BLV

```mermaid
graph TB
    subgraph "Price Components"
        MARKET[Market Price<br/>Supply/Demand]
        BLV_PRICE[BLV Floor Price<br/>Guaranteed Minimum]
        SPREAD[Price Spread<br/>Market - BLV]
    end
    
    subgraph "Price Scenarios"
        BULL[Bull Market<br/>Price >> BLV]
        NORMAL[Normal Market<br/>Price > BLV]
        BEAR[Bear Market<br/>Price = BLV]
    end
    
    subgraph "User Actions"
        HOLD[Hold HVN<br/>Protected by BLV]
        BORROW[Borrow vs HVN<br/>Up to BLV value]
        ARB[Arbitrage<br/>If Price < BLV]
    end
    
    MARKET --> BULL
    MARKET --> NORMAL
    MARKET --> BEAR
    
    BLV_PRICE --> BEAR
    
    BULL --> HOLD
    NORMAL --> BORROW
    BEAR --> ARB
    
    ARB -->|BLV prevents| IMPOSSIBLE[Impossible Scenario<br/>Price cannot go below BLV floor]
```

## Baseline Integration Benefits

```mermaid
graph LR
    subgraph "For HVN Holders"
        BENEFIT1[Protected Entry<br/>Known downside]
        BENEFIT2[Non-Liquidatable Loans<br/>Borrow safely]
        BENEFIT3[Rising Floor<br/>BLV increases with volume]
    end
    
    subgraph "For Blackhaven"
        BENEFIT4[No Liquidity Mining<br/>System handles LP]
        BENEFIT5[Permanent Liquidity<br/>Cannot be removed]
        BENEFIT6[Fair Launch<br/>No MM games]
    end
    
    subgraph "Synergies"
        SYN1[HVN governance value<br/>+<br/>BLV price protection]
        SYN2[Treasury growth<br/>+<br/>Rising price floor]
        SYN3[MegaETH alignment<br/>+<br/>Baseline innovation]
    end
    
    BENEFIT1 --> SYN1
    BENEFIT2 --> SYN2
    BENEFIT3 --> SYN3
    
    BENEFIT4 --> SYN1
    BENEFIT5 --> SYN2
    BENEFIT6 --> SYN3
```

## Example: HVN Launch to Maturity

```mermaid
graph TD
    subgraph "Day 1 - Launch"
        LAUNCH[HVN Public Sale<br/>Price: $1.00<br/>BLV: $0.90]
    end
    
    subgraph "Month 1"
        M1[Trading Volume: $10M<br/>Price: $2.50<br/>BLV: $1.20]
        BORROW1[Users can borrow<br/>$1.20 per HVN<br/>No liquidation risk]
    end
    
    subgraph "Month 6"
        M6[Trading Volume: $100M<br/>Price: $8.00<br/>BLV: $3.50]
        BORROW6[Users can borrow<br/>$3.50 per HVN<br/>Still no liquidation]
    end
    
    subgraph "Year 1"
        Y1[Trading Volume: $500M<br/>Price: $15.00<br/>BLV: $7.00]
        LEVERAGE[7x protected leverage<br/>from initial BLV]
    end
    
    LAUNCH --> M1
    M1 --> BORROW1
    M1 --> M6
    M6 --> BORROW6
    M6 --> Y1
    Y1 --> LEVERAGE
    
    LEVERAGE --> PROTECTION[Worst Case Protection<br/>HVN worth at least $7<br/>vs $0.90 at start]
```

---

*BLV mechanism ensures HVN holders always have a rising price floor, enabling unique DeFi strategies with protected downside*

