# Blackhaven Protocol Diagrams

## 1. HVN and sHVN System

```mermaid
graph TB
    subgraph "HVN Token"
        HVN_SUPPLY[100M Total Supply]
        HVN_DIST[Token Allocation<br/>22% Public - No vesting<br/>12.5% Core - 1yr cliff + 2yr vest<br/>7.5% Private - 1yr cliff + 3mo vest<br/>18% Ecosystem - 1yr vest<br/>40% Future emissions]
    end
    
    subgraph "HVN Governance"
        GOVERNANCE[Governance Rights<br/>Vote on:<br/>- Treasury strategy<br/>- DeFi whitelist<br/>- MEGA allocation<br/>- Reward rates<br/>- Proximity Market usage]
        BASELINE[Launched on Baseline Markets<br/>BLV price floor<br/>Non-liquidatable leverage]
        PROXIMITY[Control Proximity Markets<br/>HVN governance decides<br/>how staked MEGA is used]
    end
    
    subgraph "sHVN Staking"
        STAKING[Stake HVN]
        SHVN[sHVN Token<br/>Staked HVN<br/>Non-transferable]
        REWARDS[Treasury Growth Rewards<br/>Configured post-launch<br/>via HVN governance]
    end
    
    HVN_SUPPLY --> HVN_DIST
    HVN_DIST --> GOVERNANCE
    HVN_DIST --> BASELINE
    HVN_DIST --> PROXIMITY
    
    GOVERNANCE --> STAKING
    STAKING --> SHVN
    SHVN --> REWARDS
    
    Note over GOVERNANCE: Only HVN controls governance<br/>sHVN does NOT control governance
    Note over SHVN: sHVN receives rewards<br/>from treasury growth
```

## 2. RBT and aRBT System

```mermaid
graph TB
    subgraph "RBT Token"
        RBT[RBT Contract<br/>Reserve-Backed Token<br/>Transferable]
        BACKING[Backing Calculation<br/>Backing = Treasury Value / RBT Supply]
        MINTING[Proportional Minting<br/>Preserves reserve ratio<br/>as treasury grows]
    end
    
    subgraph "RBT Sources"
        HPN_SOURCE[From HPN<br/>Convert to RBT<br/>at principal value]
        BOND_SOURCE[From Bonds<br/>Discounted RBT<br/>with vesting]
        LP_BOND_SOURCE[From LP Bonds<br/>Discounted RBT<br/>with vesting]
    end
    
    subgraph "aRBT System"
        LOCK[Lock RBT<br/>User selects T_lock<br/>0 < T_lock ≤ 2 years]
        FORMULA[aRBT Formula<br/>aRBT_balance = RBT_locked × T_lock / T_max<br/><br/>Where:<br/>RBT_locked = amount committed<br/>T_lock = selected lock duration<br/>T_max = 2 years maximum]
        ARBT[aRBT Token<br/>Aligned RBT<br/>Non-transferable<br/>Pure utility]
    end
    
    subgraph "aRBT Value Chain"
        CHAIN1[Lock RBT]
        CHAIN2[creates]
        CHAIN3[aRBT/Stability]
        CHAIN4[leads to]
        CHAIN5[Demand]
        CHAIN6[for]
        CHAIN7[MegaETH Use]
    end
    
    subgraph "aRBT Benefits"
        STABILITY[RBT Stability<br/>Locked RBT strengthens liquidity]
        REVENUE[Revenue Sharing<br/>Protocol fees to aRBT holders]
        TRUST[Ecosystem Trust<br/>Facilitates RBT acceptance<br/>in MegaETH DeFi]
    end
    
    HPN_SOURCE --> RBT
    BOND_SOURCE --> RBT
    LP_BOND_SOURCE --> RBT
    
    RBT --> BACKING
    BACKING --> MINTING
    MINTING --> RBT
    
    RBT -->|User locks| LOCK
    LOCK --> FORMULA
    FORMULA --> ARBT
    
    CHAIN1 --> CHAIN2
    CHAIN2 --> CHAIN3
    CHAIN3 --> CHAIN4
    CHAIN4 --> CHAIN5
    CHAIN5 --> CHAIN6
    CHAIN6 --> CHAIN7
    
    ARBT --> CHAIN3
    CHAIN7 --> TRUST
    
    ARBT --> STABILITY
    ARBT --> REVENUE
    ARBT --> TRUST
    
    RBT -->|Used in| DEFI[MegaETH DeFi Ecosystem]
    TRUST --> DEFI
```

## 3. Treasury System

```mermaid
graph TD
    subgraph "Treasury Inflows"
        HPN_DEPOSITS[HPN Deposits<br/>USDM/USDMy]
        BOND_DEPOSITS[Bond Deposits<br/>USDM/MEGA]
        LP_BOND_DEPOSITS[LP Bond Deposits<br/>LP Tokens]
        YIELD[Generated Yields<br/>From DeFi Strategies]
        SEQUENCER_REWARDS[Sequencer Rewards<br/>From MEGA Staking]
        MEGA_POINTS[MEGA Points<br/>Converted to MEGA]
        EXIT_FEES[Exit Fees<br/>HPN: 2-3%<br/>Bonds: 3.3%]
        FORFEITED[Forfeited Rewards<br/>From early exits]
        POL_FEES[POL Trading Fees<br/>From RBT-USDM Pool]
        PRM[Premium Range Mechanism<br/>Coming Soon]
    end
    
    subgraph "Treasury Contract"
        TREASURY[Treasury Contract<br/>Manages all assets]
        MEGA_HANDLER[MEGA Handler<br/>100% to Sequencer]
        DEFI_ALLOCATOR[DeFi Allocator<br/>Whitelisted Strategies]
        ACCOUNTING[Accounting Module]
    end
    
    subgraph "Deployment"
        SEQUENCER[MEGA Sequencer Staking<br/>100% of all MEGA]
        DEFI_STRATEGIES[Whitelisted MegaETH DeFi<br/>USDM/USDMy only]
        POL_REGISTRY[POL Registry<br/>LP Tokens Locked Forever]
    end
    
    subgraph "Treasury Outputs"
        RBT_BACKING[RBT Backing Growth<br/>Proportional minting]
        SHVN_REWARDS[sHVN Reward Pool<br/>From treasury growth<br/>Configured via governance]
    end
    
    HPN_DEPOSITS --> TREASURY
    BOND_DEPOSITS --> TREASURY
    LP_BOND_DEPOSITS --> TREASURY
    YIELD --> TREASURY
    SEQUENCER_REWARDS --> TREASURY
    MEGA_POINTS --> TREASURY
    EXIT_FEES --> TREASURY
    FORFEITED --> TREASURY
    POL_FEES --> TREASURY
    PRM -.-> TREASURY
    
    TREASURY --> MEGA_HANDLER
    TREASURY --> DEFI_ALLOCATOR
    TREASURY --> ACCOUNTING
    
    MEGA_HANDLER -->|ALL MEGA| SEQUENCER
    DEFI_ALLOCATOR -->|USDM/USDMy| DEFI_STRATEGIES
    LP_BOND_DEPOSITS -->|LP tokens| POL_REGISTRY
    
    SEQUENCER -->|Rewards| ACCOUNTING
    DEFI_STRATEGIES -->|Yields| ACCOUNTING
    POL_REGISTRY -->|Trading fees| ACCOUNTING
    
    ACCOUNTING -->|Distribution configured<br/>via HVN governance| RBT_BACKING
    ACCOUNTING -->|Distribution configured<br/>via HVN governance| SHVN_REWARDS
```

## 4. HPN (Haven Protected Notes)

```mermaid
sequenceDiagram
    participant User
    participant HPN as HPN Contract
    participant NFT as ERC-721 NFT
    participant Treasury
    participant Cooldown as 7-Day Cooldown
    
    User->>HPN: Deposit USDM/USDMy
    HPN->>NFT: Mint unique NFT<br/>Records principal, terms
    HPN->>Treasury: Forward deposits
    
    loop Daily Accrual
        Treasury->>NFT: Accrue yield
        Treasury->>NFT: Accrue MEGA points<br/>Amplified via TVL
    end
    
    alt Hold to Maturity
        User->>HPN: Request claim at maturity
        HPN->>Cooldown: Start 7-day cooldown
        Cooldown-->>User: Principal + All rewards
    else Early Exit
        User->>HPN: Request early exit
        Note over HPN: 2-3% fee charged
        HPN->>Treasury: Forfeit ALL rewards
        HPN->>Cooldown: Start 7-day cooldown
        Cooldown-->>User: Principal - fee
    else Convert to RBT
        User->>HPN: Convert to RBT
        HPN->>NFT: Burn NFT
        HPN->>Treasury: Forfeit ALL rewards
        HPN-->>User: RBT = Principal value<br/>Immediate (no cooldown)
    end
    
    Note over Cooldown: Maturity and early exit<br/>require 7-day cooldown<br/>RBT conversion is immediate
```

## 5. Fixed-Term Bonds

```mermaid
graph TB
    subgraph "Bond Types"
        REGULAR_BOND[Regular Bond<br/>USDM/MEGA → RBT<br/>Discounted + Vested]
        LP_BOND[LP Bond<br/>Aligned Bond<br/>LP Token → RBT<br/>LP Locked Forever]
    end
    
    subgraph "Regular Bond Flow"
        USER1[User] -->|Deposit USDM/MEGA| REGULAR_CONTRACT[Bond Contract]
        REGULAR_CONTRACT -->|To Treasury| TREASURY1[Treasury]
        REGULAR_CONTRACT -->|Discounted RBT<br/>with vesting| VESTING1[Linear Vesting<br/>User-selected period]
        VESTING1 -->|Daily unlock| USER1
    end
    
    subgraph "LP Bond Flow"
        USER2[User] -->|1. Create LP| DEX[RBT-USDM Pool]
        DEX -->|2. LP Token| USER2
        USER2 -->|3. Select bond term| TERMS[Bond Terms<br/>Discount + Rewards]
        USER2 -->|4. Deposit LP| LP_CONTRACT[LP Bond Contract]
        LP_CONTRACT -->|LP LOCKED FOREVER| POL[Protocol Owned<br/>Liquidity]
        LP_CONTRACT -->|Discounted RBT<br/>with vesting| VESTING2[Linear Vesting<br/>User-selected period]
        VESTING2 -->|Daily unlock| USER2
        POL -->|Trading fees<br/>0.3% per swap| TREASURY2[Treasury Forever]
    end
    
    subgraph "Early Exit"
        EARLY_EXIT[Early Exit Handler<br/>3.3% fee + forfeit<br/>unvested payouts]
        FORFEIT[Forfeited Amounts<br/>Flow to Treasury]
    end
    
    REGULAR_BOND --> REGULAR_CONTRACT
    LP_BOND --> LP_CONTRACT
    
    VESTING1 -->|If early exit| EARLY_EXIT
    VESTING2 -->|If early exit| EARLY_EXIT
    EARLY_EXIT --> FORFEIT
    FORFEIT --> TREASURY1
```

## 6. BLV (Baseline Value) for HVN

```mermaid
graph TB
    subgraph "BLV Formula"
        BLV_FORMULA[BLV = Locked Reserve / Token Supply<br/>Guaranteed Price Floor]
        EXAMPLE[Example:<br/>Reserve: $100,000<br/>Supply: 100,000 HVN<br/>BLV = $1.00]
    end
    
    subgraph "BLV Growth Mechanism"
        TRADES[Trading Activity]
        RESERVE_GROWTH[Reserve Increases<br/>Portion of each trade<br/>goes to locked reserve]
        BLV_RISE[BLV Rises<br/>BLV = Reserve / Supply]
        FLOOR[Price Floor<br/>Only Goes Up]
    end
    
    subgraph "Buyback Mechanism"
        PRICE_DROP[Price Drops<br/>to BLV level]
        BUYBACK[Protocol Buys<br/>at BLV price]
        BURN[Tokens Burned<br/>Supply decreases]
        NEW_BLV[New BLV Calculated<br/>Higher than before]
    end
    
    subgraph "Safe Borrowing"
        BORROW[Borrow Against HVN<br/>Up to BLV value]
        NO_LIQUIDATION[No Liquidation Risk<br/>BLV can only go up]
        LEVERAGE[Capital-Efficient<br/>Leverage]
    end
    
    BLV_FORMULA --> EXAMPLE
    EXAMPLE --> TRADES
    TRADES --> RESERVE_GROWTH
    RESERVE_GROWTH --> BLV_RISE
    BLV_RISE --> FLOOR
    
    FLOOR --> PRICE_DROP
    PRICE_DROP --> BUYBACK
    BUYBACK --> BURN
    BURN --> NEW_BLV
    NEW_BLV --> FLOOR
    
    FLOOR --> BORROW
    BORROW --> NO_LIQUIDATION
    NO_LIQUIDATION --> LEVERAGE
```

## BLV Price Protection Example

```mermaid
sequenceDiagram
    participant User
    participant Baseline
    participant BLV
    participant Market
    
    Note over User,Market: Day 1 - HVN Launch
    
    User->>Baseline: Buy HVN at $1.00
    Baseline->>BLV: Set initial floor<br/>BLV = $0.90
    BLV-->>User: Protected at $0.90
    
    Note over User,Market: Trading increases BLV
    
    Market->>Baseline: Trading volume increases
    Baseline->>BLV: Reserve grows<br/>BLV = $1.50
    
    Note over User,Market: Price drops near BLV
    
    Market->>User: Price drops to $1.50
    User->>BLV: Check protection
    BLV-->>User: Still protected at $1.50
    
    Note over User,Market: Price hits BLV - Buyback
    
    Market->>User: Price = BLV ($1.50)
    User->>Baseline: Sell at BLV
    Baseline->>BLV: Buy tokens at BLV
    Baseline->>Baseline: Burn purchased tokens
    BLV-->>User: New BLV = $1.55<br/>Floor increased!
    
    Note over User,Market: Safe Borrowing
    
    User->>Baseline: Borrow against HVN
    Baseline->>BLV: Check current BLV
    BLV-->>Baseline: BLV = $1.55
    Baseline-->>User: Borrow up to $1.55 per HVN<br/>NO liquidation risk
```

---

*Complete Blackhaven protocol diagrams based on official documentation*
