# notanohmfork Architecture

> Visual breakdown of the system. Clean, simple, complete.

---

## The Big Picture

```mermaid
graph LR
    User[User] -->|Bond assets| Protocol[notanohmfork]
    Protocol -->|Mint| RBT[RBT Token]
    Protocol -->|Hold| Treasury[Treasury]
    Treasury -->|$MEGA + megaNOTE + USDm + $OHM| Assets[MegaETH Assets]
    Assets -->|Yield + Appreciation| Treasury
    Treasury -->|Back| RBT
```

---

## Layer 1: User Actions

```mermaid
stateDiagram-v2
    [*] --> ChoosePath
    
    ChoosePath --> MegaHolder: Has $MEGA
    ChoosePath --> MissedICO: Wants exposure
    ChoosePath --> OhmHolder: Has $OHM
    
    MegaHolder --> Bond: Don't want to sell
    MissedICO --> Bond: Deposit assets
    OhmHolder --> Bond: Participate
    
    Bond --> Receive: Get 1.0 RBT
    Receive --> Hold: Backed 1.2:1
    
    Hold --> [*]
```

---

## Layer 2: Bonding Math

```mermaid
sequenceDiagram
    participant U as User
    participant B as Bond
    participant T as Treasury
    participant R as RBT
    
    U->>B: Deposit 1.2 units
    B->>T: Send 0.2
    B->>R: Mint 1.0
    R->>U: Receive 1.0 RBT
    
    Note over T,R: Every RBT backed 1.0-1.2
```

---

## Layer 3: Treasury Composition

```mermaid
pie title Treasury Holdings
    "$MEGA" : 40
    "megaNOTE" : 30
    "USDm" : 15
    "$OHM" : 10
    "Other" : 5
```

> Note: Exact percentages unknown. Only 10% $OHM confirmed. Priority = MegaETH ecosystem.

---

## Layer 4: Price Mechanism (RBS)

```mermaid
flowchart TD
    Start([RBT Price]) --> Check{Price vs Backing?}
    
    Check -->|Below| NoAction[Do Nothing]
    Check -->|Equal| NoAction
    Check -->|Above| Activate[RBS Activates]
    
    Activate --> Sell[Sell RBT from Treasury]
    Sell --> Capture[Capture Premium]
    Capture --> Strengthen[Buy More Assets]
    Strengthen --> Loop[Backing Stronger]
    
    NoAction --> Monitor[Continue Monitoring]
    Loop --> Monitor
    Monitor --> Check
```

**Key: Upper bound ONLY. No downside action.**

---

## Layer 5: Revenue Streams

```mermaid
graph TD
    subgraph Sources
        A[$MEGA Appreciation]
        B[megaNOTE Yield]
        C[Yield Farming]
        D[Arbitrage]
        E[Staking Rewards]
    end
    
    subgraph Treasury
        T[Treasury]
    end
    
    A --> T
    B --> T
    C --> T
    D --> T
    E --> T
    
    T --> Growth[Backing Growth]
```

---

## Layer 6: MegaETH Integration

### $MEGA Flow

```mermaid
graph LR
    Network[MegaETH Network] -->|Demand| MEGA[$MEGA Token]
    MEGA -->|Bond| Treasury[notanohmfork Treasury]
    Treasury -->|Lock| Supply[Reduced Supply]
    Supply -->|Blackhole| Price[Price Up]
    Price -->|Triggers| RBS[RBS Mechanism]
```

### USDm Flow

```mermaid
graph LR
    Treasury[Treasury] -->|Hold| USDm[USDm]
    USDm -->|Deposit| Avon[Avon]
    Avon -->|Mint| megaNOTE[megaNOTE]
    megaNOTE -->|Yield| Treasury
```

### megaNOTE Mechanics

```mermaid
sequenceDiagram
    Treasury->>Avon: Deposit USDm
    Avon->>Treasury: Mint megaNOTE
    Note over Avon: Lending happens
    Note over megaNOTE: Auto-accrues yield
    megaNOTE->>Treasury: Value grows
```

---

## Layer 7: The Flywheel

```mermaid
graph TB
    Start[MegaETH Grows] --> Demand[More $MEGA Demand]
    Demand --> Treasury[Treasury Bonds $MEGA]
    Treasury --> Lock[Supply Locked]
    Lock --> Price[$MEGA Price Up]
    Price --> RBS[RBS Triggers]
    RBS --> Revenue[Capture Premium]
    Revenue --> Stronger[Treasury Stronger]
    Stronger --> More[More Bonding]
    More --> Start
    
    Yield[megaNOTE Yield] -.-> Stronger
```

---

## Layer 8: System States

```mermaid
stateDiagram-v2
    [*] --> Launch
    
    Launch --> Bonding: Users deposit
    Bonding --> Treasury: Assets accumulate
    Treasury --> Backing: RBT backed
    
    Backing --> Normal: Price = Backing
    Backing --> Premium: Price > Backing
    
    Normal --> Monitoring: No action
    Premium --> RBS_Active: Sell RBT
    
    RBS_Active --> Capture: Get premium
    Capture --> Strengthen: Buy assets
    Strengthen --> Backing
    
    Monitoring --> Backing
```

---

## Layer 9: Comparison Matrix

| Feature | Traditional OHM | notanohmfork |
|---------|----------------|--------------|
| **RBS Direction** | Both (buy + sell) | Upper bound only |
| **Consolidation** | Can create sell pressure | No action |
| **Focus** | General reserve | MegaETH ecosystem |
| **Primary Assets** | Diversified | $MEGA, megaNOTE, USDm |
| **Mechanism** | Expand/contract supply | Capture upside only |

---

## Layer 10: Three Key Flows

### Flow A: User → RBT

```
User has assets → Bond to protocol (1.2 ratio)
                        ↓
                  Receive 1.0 RBT
                        ↓
                  Backed by treasury
                        ↓
                  Exposure to MegaETH
```

### Flow B: Treasury → Yield

```
Treasury holds USDm → Deposit to Avon → Mint megaNOTE
                                              ↓
                                        Auto-accrues yield
                                              ↓
                                        Treasury grows
                                              ↓
                                        RBT backing stronger
```

### Flow C: $MEGA → Blackhole

```
MegaETH network → $MEGA demand → notanohmfork bonds $MEGA
                                         ↓
                                   Locks in treasury
                                         ↓
                                   Supply reduces
                                         ↓
                                   Price appreciates
                                         ↓
                                   RBS captures upside
```

---

## The Three Pillars

```mermaid
graph TB
    subgraph Pillar1["1. Bonding"]
        B1[1.2:1 ratio]
        B2[Multiple assets]
        B3[Instant liquidity]
    end
    
    subgraph Pillar2["2. Treasury"]
        T1[$MEGA locked]
        T2[megaNOTE yield]
        T3[120% backing]
    end
    
    subgraph Pillar3["3. RBS"]
        R1[Upper bound only]
        R2[Premium capture]
        R3[Self-strengthening]
    end
    
    Pillar1 --> Pillar2
    Pillar2 --> Pillar3
    Pillar3 --> Pillar1
```

---

## Timeline View

```mermaid
timeline
    title notanohmfork Lifecycle
    Launch : Bond opens : Users deposit : RBT mints
    Growth : Treasury accumulates : $MEGA locks : Supply reduces
    Maturity : RBS activates : Premium captured : Yield compounds
    Sustainable : Self-reinforcing : Passive growth : Ecosystem aligned
```

---

## What Makes It Different

```mermaid
mindmap
  root((notanohmfork))
    Built on MegaETH
      Real-time chain
      Sub-cent gas
      Proximity markets
    Upper Bound RBS
      No downside action
      Premium capture only
      Self-strengthening
    Ecosystem Focus
      $MEGA blackhole
      megaNOTE yield
      USDm integration
    120% Backing
      1.2:1 ratio
      Excess to treasury
      Always overcollateralized
```

---

## Quick Reference

### Ratios
- **Deposit:** 1.2 units
- **Receive:** 1.0 RBT
- **Treasury gets:** 0.2 excess
- **Backing:** Always 1.0 - 1.2

### Assets (Priority Order)
1. $MEGA (blackhole)
2. megaNOTE (passive yield)
3. USDm (stable backing)
4. $OHM (10% allocation)
5. Others (diversification)

### Mechanisms
- **Bonding:** Instant, 1.2:1 ratio
- **RBS:** Upper bound only, activates when price > backing
- **Yield:** Passive, from megaNOTE auto-compounding

### Integrations
- **$MEGA:** Bond & lock → reduce supply
- **USDm:** Hold & deposit → get megaNOTE
- **Avon:** Deposit USDm → mint megaNOTE → yield accrues
- **$OHM:** 10% allocation → ohmies participate

---

## Facts vs Unknown

### ✅ Confirmed
- 1.2:1 backing ratio
- Upper bound RBS only
- 10% $OHM allocation
- MegaETH ecosystem priority
- megaNOTE = Avon yield vault
- $MEGA blackhole mechanism

### ❓ Unknown
- Exact asset allocations (except $OHM)
- Specific yield rates
- Launch date
- RBS technical parameters
- Governance structure

---

**Simple. Clean. Complete.**

