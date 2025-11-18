# aRBT (Aligned RBT) Formula and Mechanism

## aRBT Formula Visualization

```mermaid
graph TB
    subgraph "Input Variables"
        RBT_LOCKED[RBT_locked<br/>Amount of RBT tokens committed]
        T_LOCK[T_lock<br/>Selected lock duration<br/>User chooses period]
        T_MAX[T_max<br/>Maximum lock duration<br/>Fixed at 2 years]
    end
    
    subgraph "aRBT Formula"
        FORMULA[aRBT_balance = RBT_locked × T_lock / T_max<br/><br/>Formula Breakdown:<br/>aRBT = RBT_amount × Lock_Period / Max_Period]
    end
    
    subgraph "Examples"
        EX1[Example 1:<br/>Lock 1000 RBT for 1 year<br/>aRBT = 1000 × 1 / 2 = 500 aRBT]
        EX2[Example 2:<br/>Lock 1000 RBT for 6 months<br/>aRBT = 1000 × 0.5 / 2 = 250 aRBT]
        EX3[Example 3:<br/>Lock 1000 RBT for 2 years<br/>aRBT = 1000 × 2 / 2 = 1000 aRBT<br/>Maximum]
    end
    
    RBT_LOCKED --> FORMULA
    T_LOCK --> FORMULA
    T_MAX --> FORMULA
    
    FORMULA --> EX1
    FORMULA --> EX2
    FORMULA --> EX3
```

## aRBT Creation Flow

```mermaid
sequenceDiagram
    participant User
    participant RBT as RBT Contract
    participant Lock as Lock Contract
    participant aRBT as aRBT Contract
    participant Formula as Formula Calculator
    
    User->>RBT: Hold RBT tokens
    User->>Lock: Choose lock duration<br/>T_lock (0 to 2 years)
    Lock->>Formula: Calculate aRBT amount<br/>RBT_locked × T_lock / T_max
    Formula->>Formula: T_max = 2 years<br/>Convert T_lock to years
    Formula->>aRBT: Mint aRBT tokens
    aRBT-->>User: Receive aRBT<br/>Non-transferable
    
    Note over User,aRBT: aRBT balance proportional<br/>to lock duration
```

## aRBT Value Chain

```mermaid
graph LR
    subgraph "Step 1: Lock RBT"
        LOCK[Lock RBT<br/>Choose duration<br/>T_lock]
    end
    
    subgraph "Step 2: Create aRBT"
        CREATE[Create aRBT<br/>Formula Applied<br/>aRBT = RBT × T_lock / T_max]
    end
    
    subgraph "Step 3: Stability"
        STABILITY[aRBT/Stability<br/>Locked RBT strengthens<br/>token liquidity]
    end
    
    subgraph "Step 4: Demand"
        DEMAND[Demand<br/>Enhanced trust<br/>Community alignment]
    end
    
    subgraph "Step 5: MegaETH Use"
        USE[MegaETH Use<br/>RBT accepted as<br/>core asset]
    end
    
    LOCK --> CREATE
    CREATE --> STABILITY
    STABILITY --> DEMAND
    DEMAND --> USE
    
    Note over LOCK,USE: Lock RBT → creates → aRBT/Stability → leads to → Demand → for → MegaETH Use
```

## aRBT Benefits and Mechanism

```mermaid
graph TD
    subgraph "User Action"
        USER[User locks RBT]
        DURATION[Select lock duration<br/>0 < T_lock ≤ 2 years]
    end
    
    subgraph "Formula Calculation"
        INPUT[RBT_locked × T_lock / T_max]
        MAX[T_max = 2 years<br/>730 days]
        RESULT[aRBT balance calculated]
    end
    
    subgraph "aRBT Properties"
        NON_TRANSFER[Non-transferable<br/>Cannot be sold or traded]
        REVENUE[Revenue Sharing<br/>Protocol fees distributed]
        UTILITY[Ecosystem Utility<br/>Pure utility token]
    end
    
    subgraph "Benefits"
        STABILITY_BENEFIT[RBT Stability<br/>Locked RBT strengthens liquidity]
        ALIGNMENT[Financial Alignment<br/>Direct link to protocol fees]
        TRUST[Ecosystem Trust<br/>Facilitates RBT acceptance]
    end
    
    USER --> DURATION
    DURATION --> INPUT
    INPUT --> MAX
    MAX --> RESULT
    
    RESULT --> NON_TRANSFER
    RESULT --> REVENUE
    RESULT --> UTILITY
    
    NON_TRANSFER --> STABILITY_BENEFIT
    REVENUE --> ALIGNMENT
    UTILITY --> TRUST
```

## aRBT Lock Duration Examples

```mermaid
graph LR
    subgraph "Short Lock"
        SHORT[Lock 1000 RBT<br/>for 3 months<br/>T_lock = 0.25 years]
        SHORT_ARBT[aRBT = 1000 × 0.25 / 2<br/>= 125 aRBT]
    end
    
    subgraph "Medium Lock"
        MEDIUM[Lock 1000 RBT<br/>for 1 year<br/>T_lock = 1 year]
        MEDIUM_ARBT[aRBT = 1000 × 1 / 2<br/>= 500 aRBT]
    end
    
    subgraph "Maximum Lock"
        MAX_LOCK[Lock 1000 RBT<br/>for 2 years<br/>T_lock = 2 years]
        MAX_ARBT[aRBT = 1000 × 2 / 2<br/>= 1000 aRBT<br/>Maximum possible]
    end
    
    SHORT --> SHORT_ARBT
    MEDIUM --> MEDIUM_ARBT
    MAX_LOCK --> MAX_ARBT
```

## Complete aRBT System

```mermaid
graph TB
    subgraph "RBT System"
        RBT_TOKEN[RBT Token<br/>Reserve-Backed<br/>Transferable]
    end
    
    subgraph "Lock Mechanism"
        LOCK_CONTRACT[Lock Contract<br/>User selects T_lock<br/>0 < T_lock ≤ 2 years]
        FORMULA_CALC[Formula Calculator<br/>aRBT = RBT_locked × T_lock / T_max<br/>T_max = 2 years]
    end
    
    subgraph "aRBT Token"
        ARBT_TOKEN[aRBT Token<br/>Non-transferable<br/>Pure utility]
        ARBT_BALANCE[aRBT Balance<br/>Proportional to lock duration]
    end
    
    subgraph "Revenue Sharing"
        PROTOCOL_FEES[Protocol Fees<br/>From operations]
        DISTRIBUTION[Distribution<br/>To aRBT holders]
    end
    
    subgraph "Ecosystem Impact"
        STABILITY_IMPACT[RBT Stability<br/>Strengthened liquidity]
        TRUST_IMPACT[Enhanced Trust<br/>MegaETH DeFi acceptance]
        DEMAND_IMPACT[Increased Demand<br/>For MegaETH use]
    end
    
    RBT_TOKEN -->|Lock| LOCK_CONTRACT
    LOCK_CONTRACT --> FORMULA_CALC
    FORMULA_CALC --> ARBT_TOKEN
    FORMULA_CALC --> ARBT_BALANCE
    
    ARBT_TOKEN --> PROTOCOL_FEES
    PROTOCOL_FEES --> DISTRIBUTION
    DISTRIBUTION --> ARBT_TOKEN
    
    ARBT_TOKEN --> STABILITY_IMPACT
    STABILITY_IMPACT --> TRUST_IMPACT
    TRUST_IMPACT --> DEMAND_IMPACT
```

---

## Formula Explanation

**aRBT Balance Formula:**
```
aRBT_balance = RBT_locked × (T_lock / T_max)
```

**Where:**
- `RBT_locked`: Amount of RBT tokens committed to lock
- `T_lock`: Selected lock duration (in years, 0 < T_lock ≤ 2)
- `T_max`: Maximum lock duration, fixed at 2 years

**Key Properties:**
- Non-transferable: aRBT cannot be sold or traded
- Proportional: Longer lock = more aRBT
- Maximum: Locking for 2 years gives 1:1 ratio (1000 RBT = 1000 aRBT)
- Utility: Pure utility token, no governance rights

**Value Chain:**
```
Lock RBT → creates → aRBT/Stability → leads to → Demand → for → MegaETH Use
```

---

*Visual representation of aRBT formula and mechanism based on official Blackhaven documentation*

