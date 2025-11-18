# Blackhaven Flow Diagrams

## RBT Minting and Backing Flow

```mermaid
sequenceDiagram
    participant User
    participant Product as Product Layer
    participant Treasury
    participant RBT as RBT Contract
    participant Oracle as Price Oracle
    participant Market as Secondary Market
    
    User->>Product: Deposit assets
    Product->>Treasury: Forward deposits
    Treasury->>Oracle: Get current backing ratio
    Oracle-->>Treasury: Backing = Treasury Value / RBT Supply
    Treasury->>RBT: Calculate RBT to mint
    RBT->>RBT: Mint new RBT
    RBT-->>User: Transfer RBT
    
    Note over Treasury: Deploy assets to strategies
    
    loop Daily Operations
        Treasury->>Treasury: Generate yield
        Treasury->>Oracle: Update backing value
        Oracle->>Market: Publish backing price
    end
    
    User->>Market: Trade RBT
    Market->>Oracle: Check backing value
    Oracle-->>Market: Current backing price
    Market-->>User: Execute trade
```

## Treasury Strategy Execution Flow

```mermaid
graph TD
    subgraph "Strategy Selection"
        GOVERNANCE[HVN Governance]
        WHITELIST[Strategy Whitelist]
        RISK_PARAMS[Risk Parameters]
    end
    
    subgraph "Execution Layer"
        ALLOCATOR[Strategy Allocator]
        MEGA_HANDLER[MEGA Handler]
        DEFI_ROUTER[DeFi Router]
    end
    
    subgraph "MegaETH Strategies"
        SEQ_STAKE[Sequencer<br/>Staking<br/>100% MEGA]
        LENDING[Lending<br/>Protocols]
        AMM[AMM<br/>Liquidity]
        YIELD_AGG[Yield<br/>Aggregators]
    end
    
    subgraph "Monitoring"
        HEALTH_CHECK[Health<br/>Monitor]
        REBALANCER[Auto<br/>Rebalancer]
        HARVESTER[Yield<br/>Harvester]
    end
    
    GOVERNANCE -->|approves| WHITELIST
    GOVERNANCE -->|sets| RISK_PARAMS
    
    WHITELIST --> ALLOCATOR
    RISK_PARAMS --> ALLOCATOR
    
    ALLOCATOR -->|all MEGA tokens| MEGA_HANDLER
    ALLOCATOR -->|stablecoins| DEFI_ROUTER
    
    MEGA_HANDLER -->|stakes| SEQ_STAKE
    DEFI_ROUTER -->|deploys| LENDING
    DEFI_ROUTER -->|provides| AMM
    DEFI_ROUTER -->|optimizes| YIELD_AGG
    
    SEQ_STAKE -->|status| HEALTH_CHECK
    LENDING -->|APY data| REBALANCER
    AMM -->|IL monitoring| HEALTH_CHECK
    YIELD_AGG -->|yield data| HARVESTER
    
    HARVESTER -->|compounds| ALLOCATOR
    REBALANCER -->|adjusts| ALLOCATOR
    HEALTH_CHECK -->|alerts| GOVERNANCE
```

## HPN Lifecycle State Machine

```mermaid
stateDiagram-v2
    [*] --> Initialized: User deposits
    
    Initialized --> Active: NFT minted
    
    Active --> Earning: Daily accrual
    
    Earning --> Earning: Accumulate rewards
    Earning --> MaturityReached: Term complete
    Earning --> EarlyExitRequest: User request
    Earning --> ConversionRequest: Convert to RBT
    
    MaturityReached --> CooldownPeriod: 7-day wait
    EarlyExitRequest --> CooldownPeriod: Process exit
    ConversionRequest --> RBTIssued: Immediate
    
    CooldownPeriod --> Claimable: Period complete
    
    Claimable --> Claimed: User claims
    RBTIssued --> [*]: Complete
    Claimed --> [*]: Complete
    
    note right of CooldownPeriod
        All exits except RBT conversion
        require 7-day cooldown
    end note
    
    note right of EarlyExitRequest
        2-3% penalty fee
        Forfeits all rewards
    end note
    
    note right of ConversionRequest
        Burns NFT
        Issues RBT at principal value
        Forfeits rewards
    end note
```

## Bond Vesting Mechanism

```mermaid
graph LR
    subgraph "Bond Purchase"
        USER[User]
        QUOTE[Quote Asset<br/>USDM/MEGA/LP]
        BOND_TERMS[Bond Terms<br/>7/30/90 days]
    end
    
    subgraph "Pricing Calculation"
        MARKET_PRICE[RBT Market Price]
        DISCOUNT_RATE[Discount Rate<br/>5-15%]
        BOND_PRICE[Bond Price]
    end
    
    subgraph "Vesting Schedule"
        VESTING_START[Day 0<br/>0% vested]
        VESTING_MID[Day 15<br/>50% vested]
        VESTING_END[Day 30<br/>100% vested]
    end
    
    subgraph "Claim Process"
        VESTED[Vested Amount]
        UNVESTED[Unvested Amount]
        CLAIM[Claim Function]
        USER_WALLET[User Wallet]
    end
    
    USER -->|selects| BOND_TERMS
    USER -->|provides| QUOTE
    
    MARKET_PRICE --> DISCOUNT_RATE
    DISCOUNT_RATE --> BOND_PRICE
    
    BOND_PRICE -->|creates position| VESTING_START
    VESTING_START -->|linear unlock| VESTING_MID
    VESTING_MID -->|linear unlock| VESTING_END
    
    VESTING_MID --> VESTED
    VESTING_MID --> UNVESTED
    
    VESTED --> CLAIM
    CLAIM --> USER_WALLET
    
    UNVESTED -->|if early exit| FORFEITED[Forfeited<br/>to Treasury]
```

## Protocol Owned Liquidity (POL) Architecture

```mermaid
graph TB
    subgraph "LP Bond Creation"
        USER_LP[LP Provider]
        DEX[DEX<br/>RBT-USDM Pool]
        LP_TOKEN[LP Token]
    end
    
    subgraph "Bond Contract"
        LP_DEPOSIT[LP Deposit<br/>Handler]
        LOCK_FOREVER[Permanent<br/>Lock]
        BOND_MINT[Bond<br/>Position]
    end
    
    subgraph "POL Management"
        POL_REGISTRY[POL<br/>Registry]
        FEE_COLLECTOR[Fee<br/>Collector]
        TREASURY_CREDIT[Treasury<br/>Credit]
    end
    
    subgraph "Benefits Distribution"
        TRADING_FEES[Trading Fees<br/>0.3% per swap]
        IMPERMANENT[Impermanent<br/>Gain/Loss]
        NET_VALUE[Net POL<br/>Value]
    end
    
    USER_LP -->|1. provides liquidity| DEX
    DEX -->|2. issues| LP_TOKEN
    USER_LP -->|3. deposits LP| LP_DEPOSIT
    
    LP_DEPOSIT -->|4. locks permanently| LOCK_FOREVER
    LP_DEPOSIT -->|5. creates| BOND_MINT
    LOCK_FOREVER -->|6. registers| POL_REGISTRY
    
    DEX -->|generates| TRADING_FEES
    DEX -->|experiences| IMPERMANENT
    
    TRADING_FEES --> FEE_COLLECTOR
    IMPERMANENT --> NET_VALUE
    FEE_COLLECTOR --> TREASURY_CREDIT
    NET_VALUE --> TREASURY_CREDIT
    
    BOND_MINT -->|vested RBT| USER_LP
    
    note right of LOCK_FOREVER
        LP tokens can never
        be withdrawn
    end note
    
    note right of TRADING_FEES
        All fees go to
        treasury forever
    end note
```

## Yield Distribution Flow

```mermaid
sequenceDiagram
    participant Strategies as DeFi Strategies
    participant Harvester as Yield Harvester
    participant Treasury
    participant Calculator as Distribution Calculator
    participant Backing as RBT Backing
    participant Rewards as sHVN Rewards
    participant Ops as Operations
    
    loop Every 24 hours
        Strategies->>Harvester: Check pending yields
        Note over Harvester: Aggregate all sources
        Harvester->>Treasury: Harvest yields
        
        Treasury->>Calculator: Calculate distribution
        Note over Calculator: Distribution configured<br/>via HVN governance
        
        Calculator->>Backing: Allocate to RBT backing
        Calculator->>Rewards: Allocate to sHVN pool
        Calculator->>Ops: Allocate to operations
        
        Backing->>Backing: Increase RBT backing value
        Rewards->>Rewards: Add to sHVN pool
        Ops->>Ops: Fund operations
        
        Note over Treasury: Update metrics
    end
```

---

*Technical flow diagrams illustrating Blackhaven protocol operations*
