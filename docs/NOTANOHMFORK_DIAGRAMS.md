# notanohmfork - Architecture Diagrams

> Based ONLY on facts from source materials. No speculation.

---

## 1. Core System Architecture

```mermaid
graph TB
    subgraph "User Layer"
        U1[Bonders]
        U2[RBT Holders]
        U3[$MEGA Holders]
    end
    
    subgraph "notanohmfork Protocol"
        BOND[Bond Contract]
        RBT[RBT Token<br/>Reserve-Backed Token]
        TREAS[Treasury<br/>120% Backing]
        RBS[RBS Mechanism<br/>Upper Bound Only]
    end
    
    subgraph "MegaETH Ecosystem"
        MEGA[$MEGA Token]
        USDM[USDm Stablecoin]
        AVON[Avon Lending]
        MNOTE[megaNOTE<br/>Yield Vault]
    end
    
    subgraph "External"
        OHM[$OHM<br/>10% allocation]
    end
    
    U1 -->|Deposit Assets| BOND
    BOND -->|Mint 1.0 RBT| U1
    BOND -->|0.2 to Treasury| TREAS
    
    TREAS -->|Backs| RBT
    TREAS -.->|Holds| MEGA
    TREAS -.->|Holds| MNOTE
    TREAS -.->|Holds| USDM
    TREAS -.->|Holds 10%| OHM
    
    RBS -->|Premium Price?| RBT
    RBS -->|Sell RBT| TREAS
    
    USDM -->|Deposit| AVON
    AVON -->|Mint| MNOTE
    MNOTE -->|Passive Yield| TREAS
    
    U2 -.->|Trade| RBT
    U3 -.->|Bond| MEGA
```

---

## 2. RBT Bonding Mechanism (1.2:1 Model)

```mermaid
sequenceDiagram
    participant User
    participant Bond
    participant RBT
    participant Treasury
    
    User->>Bond: Deposit 1.2 units of asset
    
    Note over Bond,Treasury: Total Value: 1.2
    
    Bond->>RBT: Mint 1.0 RBT
    RBT->>User: Receive 1.0 RBT
    
    Bond->>Treasury: Transfer 0.2 excess
    
    Note over Treasury: Every circulating RBT<br/>backed between 1.0 - 1.2
    Note right of Treasury: ✓ User has 1.0 RBT<br/>✓ Backed by 1.2 assets<br/>✓ 0.2 strengthens treasury
```

---

## 3. RBS - Upper Bound ONLY

```mermaid
stateDiagram-v2
    [*] --> Normal: RBT launches
    
    Normal --> CheckPrice: Monitor price
    
    CheckPrice --> BelowBacking: Price < Backing
    CheckPrice --> AtBacking: Price = Backing  
    CheckPrice --> Premium: Price > Backing
    
    BelowBacking --> DoNothing: ❌ RBS inactive
    AtBacking --> DoNothing: ❌ RBS inactive
    Premium --> ActivateRBS: ✓ RBS ACTIVE
    
    DoNothing --> CheckPrice: Continue monitoring
    
    ActivateRBS --> SellRBT: Sell treasury RBT
    SellRBT --> CaptureProceeds: Capture premium
    CaptureProceeds --> StrengthenBacking: Add to reserves
    StrengthenBacking --> CheckPrice
    
    note right of DoNothing
        "Unlike traditional DATs that 
        expand supply dynamically, 
        this design activates ONLY 
        when token trades at premium"
    end note
    
    note right of ActivateRBS
        "Protocol sells additional RBTs 
        from treasury, capturing premium 
        and cycling proceeds back into 
        reserve"
    end note
```

---

## 4. Treasury Flow & Asset Composition

```mermaid
graph LR
    subgraph "Inflows"
        B1[Bond Deposits<br/>0.2 excess per mint]
        B2[RBS Premium Sales]
        B3[$MEGA Appreciation]
        B4[Yield Farming]
        B5[Arbitrage]
    end
    
    subgraph "Treasury Assets"
        T[Treasury<br/>120% backing]
    end
    
    subgraph "Holdings - Priority: MegaETH"
        H1[$MEGA<br/>blackhole mechanism]
        H2[megaNOTE<br/>passive growth]
        H3[USDm<br/>stable backing]
        H4[$OHM<br/>10% allocation]
        H5[Other Volatiles<br/>diversified basket]
    end
    
    subgraph "Passive Yield Generation"
        Y1[megaNOTE]
        Y2[Auto-compounds]
        Y3[From Avon lending]
    end
    
    B1 --> T
    B2 --> T
    B3 --> T
    B4 --> T
    B5 --> T
    
    T --> H1
    T --> H2
    T --> H3
    T --> H4
    T --> H5
    
    H2 --> Y1
    Y1 --> Y2
    Y2 --> Y3
    Y3 -.->|Grows reserve| T
```

---

## 5. MegaETH Integration Points

```mermaid
graph TB
    subgraph "notanohmfork"
        RBT[RBT Token]
        TREAS[Treasury]
    end
    
    subgraph "$MEGA Integration"
        M1[Sequencer Rotation<br/>Stake $MEGA]
        M2[Proximity Markets<br/>Lock $MEGA]
        M3[Demand Driver]
        M4[notanohmfork bonds $MEGA]
        M5[Reduces circulating supply]
    end
    
    subgraph "USDm Integration"
        U1[Native Stablecoin]
        U2[Backed by BUIDL]
        U3[Yield from reserves]
        U4[Deposit to Avon]
    end
    
    subgraph "Avon Integration"
        A1[Order Book Lending]
        A2[Deposit USDm]
        A3[Mint megaNOTE]
        A4[Yield-bearing]
    end
    
    subgraph "megaNOTE"
        N1[Receipt Token]
        N2[Auto-accrues yield]
        N3[Standard collateral]
    end
    
    M3 -->|Growth| M1
    M3 -->|Growth| M2
    TREAS -->|Holds| M4
    M4 --> M5
    M5 -.->|"blackhole-style<br/>liquidity capture"| M3
    
    U1 --> U2
    U2 --> U3
    TREAS -->|Holds| U1
    U1 --> U4
    U4 --> A1
    
    A1 --> A2
    A2 --> A3
    A3 --> A4
    
    A3 --> N1
    N1 --> N2
    N2 --> N3
    TREAS -->|Holds| N1
    N1 -.->|Passive growth| TREAS
```

---

## 6. User Workflow - Bonding

```mermaid
sequenceDiagram
    actor User
    participant UI as notanohmfork UI
    participant Bond as Bond Contract
    participant Treasury
    participant RBT as RBT Token
    participant MegaETH as $MEGA/$OHM/etc
    
    User->>UI: Want exposure to MegaETH
    
    alt Has $MEGA
        User->>UI: Select $MEGA bond
        Note over User,UI: "If you aped ICO but don't<br/>plan selling, deposit into<br/>MegaOHM instead"
    else Has $OHM
        User->>UI: Select $OHM bond
        Note over User,UI: "10% of reserves allocated<br/>for $OHM deposits"
    else Other assets
        User->>UI: Select asset bond
    end
    
    UI->>Bond: Call deposit()
    User->>Bond: Transfer 1.2 units
    
    Bond->>Treasury: Send 0.2 excess
    Note over Treasury: Strengthens backing
    
    Bond->>RBT: Mint 1.0 RBT
    RBT->>User: Receive 1.0 RBT
    
    Note over User,RBT: ✓ RBT backed 1.2:1<br/>✓ Exposure to MegaETH ecosystem<br/>✓ RBS protects upside
    
    User->>User: Hold RBT
    
    opt If RBT trades at premium
        Bond->>Treasury: RBS activates
        Treasury->>UI: Sell RBT on market
        UI->>Treasury: Capture premium
        Note over Treasury: Backing strengthens further
    end
```

---

## 7. Treasury Revenue Cycle

```mermaid
flowchart TD
    START([Treasury Holds Assets]) --> MEGA{$MEGA in Treasury}
    START --> MNOTE{megaNOTE in Treasury}
    START --> OTHER{Other Assets}
    
    MEGA -->|Appreciates| PRICE[Price > backing]
    PRICE -->|Triggers| RBS[RBS Mechanism]
    RBS -->|Sells RBT| PROCEEDS[Capture Premium]
    PROCEEDS -->|Buys more assets| STRENGTHEN[Strengthen Treasury]
    
    MNOTE -->|Deposits USDm| AVON[Avon Lending]
    AVON -->|Auto-accrues| YIELD[Yield Generation]
    YIELD -->|Passive growth| STRENGTHEN
    
    OTHER -->|Staking Rewards| REWARDS[$MEGA incentives]
    OTHER -->|Yield Farming| FARM[DeFi yields]
    OTHER -->|Arbitrage| ARB[Market opportunities]
    
    REWARDS --> STRENGTHEN
    FARM --> STRENGTHEN
    ARB --> STRENGTHEN
    
    STRENGTHEN -->|Compounds| START
```

---

## 8. $MEGA "Blackhole" Effect

```mermaid
graph TB
    subgraph "MegaETH Network Growth"
        G1[More Sequencers<br/>stake $MEGA]
        G2[More Apps<br/>lock $MEGA for proximity]
        G3[More Users<br/>drive demand]
    end
    
    subgraph "notanohmfork Treasury"
        T1[Bonds $MEGA]
        T2[Locks in Treasury]
        T3[Removes from circulation]
    end
    
    subgraph "Market Impact"
        M1[Reduced Supply]
        M2[$MEGA Appreciation]
        M3[RBS Triggers]
        M4[More Revenue]
    end
    
    G1 --> G3
    G2 --> G3
    G3 -->|Demand| M1
    
    T1 --> T2
    T2 --> T3
    T3 --> M1
    
    M1 --> M2
    M2 --> M3
    M3 --> M4
    M4 -.->|Reinvest| T1
    
    Note1[Quote: Obviously this requires<br/>MegaETH ecosystem to work together<br/>to create blackhole-style liquidity<br/>capture mechanism]
    
    M1 -.-> Note1
```

---

## 9. Comparison: Traditional DAT vs notanohmfork

```mermaid
graph LR
    subgraph "Traditional OHM-style DAT"
        T1[Expands on demand]
        T2[Contracts on supply]
        T3[RBS both directions]
        T4[Can create sell pressure]
    end
    
    subgraph "notanohmfork"
        N1[Upper bound ONLY]
        N2[No lower bound action]
        N3[Captures upside]
        N4[No sell pressure in consolidation]
    end
    
    T1 -.->|vs| N1
    T2 -.->|vs| N2
    T3 -.->|vs| N3
    T4 -.->|vs| N4
```

---

## 10. Complete System Flow

```mermaid
flowchart TB
    START([User wants MegaETH exposure])
    
    START --> CHOICE{Choose path}
    
    CHOICE -->|Missed ICO| BOND1[Bond assets for RBT]
    CHOICE -->|Has $MEGA| BOND2[Bond $MEGA instead of selling]
    CHOICE -->|OHM holder| BOND3[Bond $OHM - 10% allocation]
    
    BOND1 --> MINT[Mint RBT at 1.2:1]
    BOND2 --> MINT
    BOND3 --> MINT
    
    MINT --> TREAS[0.2 to Treasury]
    MINT --> USER[1.0 to User]
    
    TREAS --> ALLOCATE{Allocate to MegaETH assets}
    
    ALLOCATE -->|Priority| MEGA2[$MEGA - blackhole]
    ALLOCATE --> MNOTE2[megaNOTE - yield]
    ALLOCATE --> USDM2[USDm - stable]
    ALLOCATE --> OHM2[$OHM - 10%]
    
    MNOTE2 --> AVON2[Deposit USDm to Avon]
    AVON2 --> YIELD2[Auto-accrues yield]
    
    MEGA2 --> APPRECIATE[If $MEGA appreciates]
    APPRECIATE --> CHECK{RBT price > backing?}
    
    CHECK -->|No| HOLD[Continue holding]
    CHECK -->|Yes| RBS2[RBS activates]
    
    RBS2 --> SELL[Sell RBT from treasury]
    SELL --> CAPTURE[Capture premium]
    CAPTURE --> REINVEST[Strengthen backing]
    
    YIELD2 --> REINVEST
    REINVEST --> TREAS
    
    HOLD --> USER2[User holds RBT]
    USER2 --> EXPOSURE[Exposure to MegaETH ecosystem]
```

---

## Key Facts Summary

### What RBT IS:
- Reserve-Backed Token
- 1.2:1 backing ratio
- Minted via bond deposits
- 0.2 excess to treasury per mint
- Upper-bound RBS only

### What megaNOTE IS:
- Avon's yield-bearing vault token
- Minted by depositing USDm
- Auto-accrues yield from lending
- Treasury holds for passive growth

### Treasury Composition (KNOWN):
- **10% $OHM** (explicitly stated)
- **Priority to MegaETH ecosystem** (stated)
- **Basket of stable + volatile assets** (stated)
- **megaNOTE for passive yield** (stated)
- **$MEGA for blackhole effect** (stated)
- **USDm for stable backing** (stated)

### Revenue Sources (STATED):
1. $MEGA appreciation triggering RBS
2. Arbitrage from deposits
3. Yield farming
4. Staking rewards from $MEGA ecosystem

### What's NOT Known:
- ❌ Exact % allocations (except 10% OHM)
- ❌ Specific APY numbers
- ❌ Launch timeline
- ❌ Technical parameters (RBS thresholds, etc)
- ❌ Governance model

---

**All diagrams based on source text. No speculation.**

