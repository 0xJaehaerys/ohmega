# Blackhaven Architecture Diagrams

## RBT (Reserve-Backed Token) Architecture Model

```mermaid
graph LR
    subgraph "User Entry Points"
        USER1[User]
        USER2[User]
        USER3[User]
        USER4[User]
    end
    
    subgraph "Product Layer"
        HPN[HPN Contract<br/>ERC-721]
        BONDS[Bond<br/>Depository]
        MARKET[Secondary<br/>Market]
    end
    
    subgraph "Treasury Core"
        TREASURY[Treasury<br/>Contract]
        RESERVE[Reserve<br/>Assets]
        STRATEGIES[DeFi<br/>Strategies]
    end
    
    subgraph "RBT System"
        RBT_CONTRACT[RBT<br/>Contract]
        BACKING[Backing<br/>Calculator]
        SUPPLY[Supply<br/>Controller]
    end
    
    subgraph "Value Accrual"
        YIELD[Yield<br/>Generation]
        FEES[Protocol<br/>Fees]
        FORFEIT[Forfeited<br/>Rewards]
    end
    
    USER1 -->|deposits USDM| HPN
    USER2 -->|deposits assets| BONDS
    USER3 -->|buys/sells| MARKET
    
    HPN -->|forwards deposits| TREASURY
    BONDS -->|forwards deposits| TREASURY
    
    TREASURY -->|deploys capital| STRATEGIES
    TREASURY -->|manages| RESERVE
    
    HPN -->|can convert to| RBT_CONTRACT
    BONDS -->|mints discounted| RBT_CONTRACT
    
    STRATEGIES -->|generates| YIELD
    HPN -->|early exit fees| FEES
    HPN -->|unclaimed rewards| FORFEIT
    
    YIELD --> RESERVE
    FEES --> RESERVE
    FORFEIT --> RESERVE
    
    RESERVE -->|determines| BACKING
    BACKING -->|informs| SUPPLY
    SUPPLY -->|controls| RBT_CONTRACT
    
    RBT_CONTRACT -->|trades on| MARKET
    
    style TREASURY fill:#fff,stroke:#000,stroke-width:2px
    style RBT_CONTRACT fill:#fff,stroke:#000,stroke-width:2px
```

## Treasury Operations Architecture

```mermaid
graph TD
    subgraph "Inflow Sources"
        HPN_DEPOSITS[HPN<br/>Deposits]
        BOND_PROCEEDS[Bond<br/>Proceeds]
        EXIT_FEES[Exit<br/>Fees]
        TRADING_FEES[Trading<br/>Fees]
    end
    
    subgraph "Treasury Contract"
        CONTROLLER[Treasury<br/>Controller]
        ALLOCATOR[Strategy<br/>Allocator]
        ACCOUNTING[Accounting<br/>Module]
    end
    
    subgraph "Deployment Targets"
        MEGA_STAKING[MEGA<br/>Sequencer<br/>Staking]
        DEFI_1[Lending<br/>Markets]
        DEFI_2[AMM<br/>Positions]
        DEFI_3[Yield<br/>Aggregators]
    end
    
    subgraph "Output Distribution"
        RBT_BACKING[RBT<br/>Backing<br/>Growth]
        SHVN_REWARDS[sHVN<br/>Reward<br/>Pool]
        OPERATIONS[Protocol<br/>Operations]
    end
    
    HPN_DEPOSITS --> CONTROLLER
    BOND_PROCEEDS --> CONTROLLER
    EXIT_FEES --> CONTROLLER
    TRADING_FEES --> CONTROLLER
    
    CONTROLLER --> ALLOCATOR
    CONTROLLER --> ACCOUNTING
    
    ALLOCATOR -->|100% of MEGA| MEGA_STAKING
    ALLOCATOR -->|USDM allocation| DEFI_1
    ALLOCATOR -->|LP strategies| DEFI_2
    ALLOCATOR -->|Optimized yield| DEFI_3
    
    MEGA_STAKING -->|staking rewards| ACCOUNTING
    DEFI_1 -->|lending yield| ACCOUNTING
    DEFI_2 -->|LP fees| ACCOUNTING
    DEFI_3 -->|aggregated yield| ACCOUNTING
    
    ACCOUNTING -->|70%| RBT_BACKING
    ACCOUNTING -->|20%| SHVN_REWARDS
    ACCOUNTING -->|10%| OPERATIONS
```

## HPN (Haven Protected Notes) Architecture

```mermaid
graph LR
    subgraph "User Interface"
        USER[User]
        NFT[HPN NFT<br/>ERC-721]
    end
    
    subgraph "HPN Contract"
        DEPOSIT_HANDLER[Deposit<br/>Handler]
        POSITION_TRACKER[Position<br/>Tracker]
        REWARD_ACCRUAL[Reward<br/>Accrual]
        EXIT_HANDLER[Exit<br/>Handler]
    end
    
    subgraph "State Management"
        POSITIONS[(Position<br/>Storage)]
        REWARDS[(Reward<br/>Storage)]
        COOLDOWNS[(Cooldown<br/>Storage)]
    end
    
    subgraph "Integration Layer"
        TREASURY_INT[Treasury<br/>Interface]
        RBT_INT[RBT<br/>Interface]
        ORACLE[Price<br/>Oracle]
    end
    
    USER -->|1. deposit USDM/USDMy| DEPOSIT_HANDLER
    DEPOSIT_HANDLER -->|2. mint NFT| NFT
    DEPOSIT_HANDLER -->|3. record position| POSITION_TRACKER
    POSITION_TRACKER --> POSITIONS
    
    DEPOSIT_HANDLER -->|4. forward funds| TREASURY_INT
    
    POSITION_TRACKER -->|5. track rewards| REWARD_ACCRUAL
    REWARD_ACCRUAL --> REWARDS
    
    USER -->|option A: hold to maturity| EXIT_HANDLER
    USER -->|option B: early exit| EXIT_HANDLER
    USER -->|option C: convert to RBT| EXIT_HANDLER
    
    EXIT_HANDLER -->|check position| POSITIONS
    EXIT_HANDLER -->|check rewards| REWARDS
    EXIT_HANDLER -->|initiate cooldown| COOLDOWNS
    
    EXIT_HANDLER -->|if converting| RBT_INT
    EXIT_HANDLER -->|get pricing| ORACLE
    
    COOLDOWNS -->|after 7 days| USER
    
    style HPN_CONTRACT fill:#fff,stroke:#000,stroke-width:2px
```

## Fixed-Term Bonds Architecture

```mermaid
graph TB
    subgraph "Bond Types"
        REGULAR_BOND[Regular Bond<br/>USDM/MEGA -> RBT]
        LP_BOND[LP Bond<br/>LP Token -> RBT]
    end
    
    subgraph "Bond Depository"
        PRICING_ENGINE[Bond<br/>Pricing<br/>Engine]
        VESTING_CONTROLLER[Vesting<br/>Controller]
        BOND_REGISTRY[Bond<br/>Registry]
    end
    
    subgraph "Vesting Mechanism"
        VESTING_POSITIONS[(Vesting<br/>Positions)]
        LINEAR_RELEASE[Linear<br/>Release<br/>Logic]
        CLAIM_HANDLER[Claim<br/>Handler]
    end
    
    subgraph "Treasury Integration"
        TREASURY_DEPOSITS[Treasury<br/>Deposits]
        POL_MANAGER[POL<br/>Manager]
        LP_LOCKER[LP Token<br/>Locker]
    end
    
    subgraph "User Flow"
        USER_REGULAR[User A]
        USER_LP[User B]
    end
    
    USER_REGULAR -->|deposits USDM| REGULAR_BOND
    REGULAR_BOND --> PRICING_ENGINE
    
    USER_LP -->|1. creates LP| LP_CREATION[LP Creation]
    LP_CREATION -->|2. deposits LP token| LP_BOND
    LP_BOND --> PRICING_ENGINE
    
    PRICING_ENGINE -->|calculates discount| VESTING_CONTROLLER
    VESTING_CONTROLLER -->|creates position| BOND_REGISTRY
    BOND_REGISTRY --> VESTING_POSITIONS
    
    REGULAR_BOND -->|assets to treasury| TREASURY_DEPOSITS
    LP_BOND -->|LP tokens locked forever| LP_LOCKER
    LP_LOCKER --> POL_MANAGER
    
    VESTING_POSITIONS --> LINEAR_RELEASE
    LINEAR_RELEASE -->|daily unlock| CLAIM_HANDLER
    CLAIM_HANDLER -->|vested RBT| USER_REGULAR
    CLAIM_HANDLER -->|vested RBT| USER_LP
    
    POL_MANAGER -->|generates fees| TREASURY_DEPOSITS
    
    style POL_MANAGER fill:#fff,stroke:#000,stroke-width:3px
    style LP_LOCKER fill:#fff,stroke:#000,stroke-width:3px
```

## Complete System Integration

```mermaid
graph LR
    subgraph "MegaETH Layer"
        MEGAETH[MegaETH<br/>10ms blocks]
        SEQUENCER[Sequencer]
        PROXIMITY[Proximity<br/>Markets]
    end
    
    subgraph "Blackhaven Core"
        TREASURY_CORE[Treasury]
        RBT_SYSTEM[RBT]
        HVN_GOVERNANCE[HVN<br/>Governance]
    end
    
    subgraph "Products"
        HPN_PRODUCT[HPN]
        BONDS_PRODUCT[Bonds]
        STAKING[sHVN<br/>Staking]
    end
    
    subgraph "External Integrations"
        BASELINE[Baseline<br/>Markets]
        DEFI_PROTOCOLS[MegaETH<br/>DeFi]
        ORACLES[Price<br/>Oracles]
    end
    
    MEGAETH --> TREASURY_CORE
    TREASURY_CORE -->|stakes MEGA| SEQUENCER
    HVN_GOVERNANCE -->|controls allocation| PROXIMITY
    
    HPN_PRODUCT --> TREASURY_CORE
    BONDS_PRODUCT --> TREASURY_CORE
    TREASURY_CORE --> RBT_SYSTEM
    
    HVN_GOVERNANCE -->|launched on| BASELINE
    TREASURY_CORE -->|deploys to| DEFI_PROTOCOLS
    ORACLES -->|price feeds| TREASURY_CORE
    
    STAKING --> HVN_GOVERNANCE
    RBT_SYSTEM -->|used in| DEFI_PROTOCOLS
    
    SEQUENCER -->|rewards| TREASURY_CORE
    PROXIMITY -->|MEV revenue| TREASURY_CORE
    DEFI_PROTOCOLS -->|yield| TREASURY_CORE
```

---

*Technical architecture diagrams for Blackhaven protocol on MegaETH*
