# Blackhaven Architecture Diagrams

## Complete System Architecture

```mermaid
graph LR
    subgraph "User Entry Points"
        USER1[User<br/>HPN Depositor]
        USER2[User<br/>Bond Buyer]
        USER3[User<br/>LP Provider]
        USER4[User<br/>RBT Holder]
    end
    
    subgraph "Product Layer"
        HPN[HPN Contract<br/>ERC-721 NFTs<br/>USDM/USDMy only]
        REGULAR_BONDS[Bond Depository<br/>USDM/MEGA -> RBT]
        LP_BONDS[LP Bond Contract<br/>LP Token -> RBT<br/>Aligned Bonds]
        MARKET[Secondary Market<br/>RBT Trading]
    end
    
    subgraph "Treasury Core"
        TREASURY[Treasury Contract]
        RESERVE[Reserve Assets]
        MEGA_STAKING[100% MEGA<br/>Sequencer Staking]
        DEFI_STRATEGIES[Whitelisted<br/>MegaETH DeFi]
    end
    
    subgraph "RBT System"
        RBT_CONTRACT[RBT Contract<br/>Reserve-Backed Token]
        ARBT_CONTRACT[aRBT Contract<br/>Non-transferable<br/>Lock RBT -> aRBT]
        BACKING[Backing Calculator<br/>Treasury Value / RBT Supply]
        SUPPLY[Supply Controller<br/>Proportional Minting]
    end
    
    subgraph "POL Management"
        POL_REGISTRY[POL Registry<br/>LP Tokens Locked Forever]
        LP_LOCKER[LP Token Locker<br/>Permanent Lock]
        FEE_COLLECTOR[Trading Fee Collector<br/>From POL]
    end
    
    subgraph "Value Accrual"
        YIELD[Yield Generation<br/>From Strategies]
        FEES[Protocol Fees<br/>HPN: 2-3% early exit<br/>Bonds: 3.3% early exit]
        FORFEIT[Forfeited Rewards<br/>From early exits]
        MEGA_POINTS[MEGA Points<br/>Converted to MEGA]
        PRM[Premium Range Mechanism<br/>Coming Soon]
    end
    
    USER1 -->|deposits USDM/USDMy| HPN
    USER2 -->|deposits USDM/MEGA| REGULAR_BONDS
    USER3 -->|1. creates LP| DEX[RBT-USDM Pool]
    DEX -->|2. LP Token| USER3
    USER3 -->|3. deposits LP| LP_BONDS
    USER4 -->|locks RBT| ARBT_CONTRACT
    
    HPN -->|forwards deposits| TREASURY
    REGULAR_BONDS -->|forwards deposits| TREASURY
    LP_BONDS -->|LP tokens| LP_LOCKER
    LP_LOCKER -->|locked forever| POL_REGISTRY
    
    TREASURY -->|100% MEGA| MEGA_STAKING
    TREASURY -->|other assets| DEFI_STRATEGIES
    
    HPN -->|can convert to| RBT_CONTRACT
    REGULAR_BONDS -->|mints discounted RBT| RBT_CONTRACT
    LP_BONDS -->|mints discounted RBT| RBT_CONTRACT
    
    DEFI_STRATEGIES -->|generates| YIELD
    MEGA_STAKING -->|sequencer rewards| YIELD
    HPN -->|early exit fees| FEES
    REGULAR_BONDS -->|early exit fees| FEES
    LP_BONDS -->|early exit fees| FEES
    HPN -->|unclaimed rewards| FORFEIT
    REGULAR_BONDS -->|unvested payouts| FORFEIT
    LP_BONDS -->|unvested payouts| FORFEIT
    
    POL_REGISTRY -->|trading fees| FEE_COLLECTOR
    FEE_COLLECTOR --> RESERVE
    
    YIELD --> RESERVE
    FEES --> RESERVE
    FORFEIT --> RESERVE
    MEGA_POINTS -->|converted to MEGA| RESERVE
    PRM -.->|future| RESERVE
    
    RESERVE -->|determines| BACKING
    BACKING -->|informs| SUPPLY
    SUPPLY -->|controls| RBT_CONTRACT
    
    RBT_CONTRACT -->|trades on| MARKET
    RBT_CONTRACT -->|lock to get| ARBT_CONTRACT
    
```

## Treasury Operations Architecture

```mermaid
graph TD
    subgraph "Inflow Sources"
        HPN_DEPOSITS[HPN Deposits<br/>USDM/USDMy]
        REGULAR_BOND_PROCEEDS[Regular Bond Proceeds<br/>USDM/MEGA]
        LP_BOND_PROCEEDS[LP Bond Proceeds<br/>LP Tokens]
        YIELD_GEN[Generated Yields<br/>From DeFi Strategies]
        SEQUENCER_REWARDS[Sequencer Rewards<br/>From MEGA Staking]
        MEGA_POINTS[MEGA Points<br/>Converted to MEGA]
        EXIT_FEES[Exit Fees<br/>HPN: 2-3%<br/>Bonds: 3.3%]
        FORFEITED_REWARDS[Forfeited Rewards<br/>From early exits]
        POL_FEES[POL Trading Fees<br/>From RBT-USDM Pool]
        PRM_FUTURE[Premium Range Mechanism<br/>Coming Soon]
    end
    
    subgraph "Treasury Contract"
        CONTROLLER[Treasury Controller]
        MEGA_HANDLER[MEGA Handler<br/>100% to Sequencer]
        ALLOCATOR[Strategy Allocator<br/>Whitelisted DeFi]
        ACCOUNTING[Accounting Module]
    end
    
    subgraph "Deployment Targets"
        MEGA_STAKING[MEGA Sequencer<br/>Staking<br/>100% of all MEGA]
        DEFI_STRATEGIES[Whitelisted<br/>MegaETH DeFi<br/>USDM/USDMy only]
    end
    
    subgraph "POL Management"
        POL_REGISTRY[POL Registry<br/>LP Tokens Locked Forever]
        FEE_COLLECTOR[Fee Collector<br/>Trading Fees]
    end
    
    subgraph "Output Distribution"
        RBT_BACKING[RBT Backing Growth<br/>Proportional Minting]
        SHVN_REWARDS[sHVN Reward Pool<br/>From treasury growth<br/>Configured via governance]
    end
    
    HPN_DEPOSITS --> CONTROLLER
    REGULAR_BOND_PROCEEDS --> CONTROLLER
    LP_BOND_PROCEEDS --> CONTROLLER
    YIELD_GEN --> CONTROLLER
    SEQUENCER_REWARDS --> CONTROLLER
    MEGA_POINTS --> CONTROLLER
    EXIT_FEES --> CONTROLLER
    FORFEITED_REWARDS --> CONTROLLER
    POL_FEES --> CONTROLLER
    PRM_FUTURE -.-> CONTROLLER
    
    CONTROLLER --> MEGA_HANDLER
    CONTROLLER --> ALLOCATOR
    CONTROLLER --> ACCOUNTING
    
    MEGA_HANDLER -->|ALL MEGA| MEGA_STAKING
    ALLOCATOR -->|USDM/USDMy| DEFI_STRATEGIES
    
    LP_BOND_PROCEEDS -->|LP tokens locked| POL_REGISTRY
    POL_REGISTRY --> FEE_COLLECTOR
    FEE_COLLECTOR --> ACCOUNTING
    
    MEGA_STAKING -->|staking rewards| ACCOUNTING
    DEFI_STRATEGIES -->|lending/yield| ACCOUNTING
    
    ACCOUNTING -->|Distribution configured<br/>via HVN governance| RBT_BACKING
    ACCOUNTING -->|Distribution configured<br/>via HVN governance| SHVN_REWARDS
```

## LP Bonds (Aligned Bonds) Detailed Flow

```mermaid
graph TB
    subgraph "Step 1: LP Creation"
        USER[User]
        DEX_POOL[RBT-USDM Pool<br/>On MegaETH DEX]
        LP_TOKEN[LP Token<br/>RBT + USDM]
    end
    
    subgraph "Step 2: LP Bond Deposit"
        LP_BOND_CONTRACT[LP Bond Contract<br/>Aligned Bonds]
        BOND_TERMS[Bond Terms<br/>Select duration<br/>Discount + Rewards]
    end
    
    subgraph "Step 3: POL Lock"
        LP_LOCKER[LP Token Locker<br/>PERMANENT LOCK]
        POL_REGISTRY[POL Registry<br/>Protocol Owned Liquidity]
    end
    
    subgraph "Step 4: RBT Vesting"
        VESTING_CONTROLLER[Vesting Controller<br/>Linear Vesting]
        VESTED_RBT[Vested RBT<br/>Discounted Price]
        UNVESTED_RBT[Unvested RBT<br/>Subject to forfeit]
    end
    
    subgraph "Step 5: POL Benefits"
        TRADING_FEES[Trading Fees<br/>0.3% per swap<br/>To Treasury Forever]
        LIQUIDITY_DEPTH[Liquidity Depth<br/>Price Stability]
    end
    
    USER -->|1. Provide liquidity| DEX_POOL
    DEX_POOL -->|2. Receive| LP_TOKEN
    USER -->|3. Select bond term| BOND_TERMS
    USER -->|4. Deposit LP token| LP_BOND_CONTRACT
    BOND_TERMS --> LP_BOND_CONTRACT
    
    LP_BOND_CONTRACT -->|5. Lock forever| LP_LOCKER
    LP_LOCKER -->|6. Register| POL_REGISTRY
    
    LP_BOND_CONTRACT -->|7. Create vesting| VESTING_CONTROLLER
    VESTING_CONTROLLER -->|8. Daily unlock| VESTED_RBT
    VESTING_CONTROLLER -->|9. Unvested| UNVESTED_RBT
    
    VESTED_RBT -->|10. Claim| USER
    UNVESTED_RBT -->|11. If early exit| FORFEIT[Forfeited to Treasury<br/>3.3% fee]
    
    POL_REGISTRY -->|12. Generates| TRADING_FEES
    POL_REGISTRY -->|13. Provides| LIQUIDITY_DEPTH
    TRADING_FEES --> TREASURY[Treasury Forever]
```

## RBT and aRBT System

```mermaid
graph LR
    subgraph "RBT System"
        RBT_CONTRACT[RBT Contract<br/>Reserve-Backed Token<br/>Transferable]
        BACKING_CALC[Backing Calculator<br/>Backing = Treasury Value / RBT Supply]
        PROPORTIONAL_MINT[Proportional Minting<br/>Preserves Reserve Ratio]
    end
    
    subgraph "aRBT System"
        LOCK_MECHANISM[Lock Mechanism<br/>User locks RBT<br/>Selects T_lock duration]
        FORMULA[aRBT Formula<br/>aRBT_balance = RBT_locked × T_lock / T_max<br/><br/>Where:<br/>RBT_locked = amount committed<br/>T_lock = selected lock duration<br/>T_max = 2 years maximum]
        ARBT_CONTRACT[aRBT Contract<br/>Aligned RBT<br/>Non-transferable<br/>Pure utility token]
    end
    
    subgraph "Value Chain"
        STEP1[Lock RBT]
        STEP2[creates]
        STEP3[aRBT/Stability]
        STEP4[leads to]
        STEP5[Demand]
        STEP6[for]
        STEP7[MegaETH Use]
    end
    
    subgraph "Benefits"
        STABILITY_BENEFIT[RBT Stability<br/>Locked RBT strengthens<br/>token liquidity]
        REVENUE_SHARE[Revenue Sharing<br/>Protocol fees distributed<br/>to aRBT holders]
        ECOSYSTEM_UTILITY[Ecosystem Utility<br/>Enhanced trust facilitates<br/>RBT acceptance in MegaETH DeFi]
    end
    
    subgraph "User Actions"
        USER_RBT[RBT Holder]
        USER_ARBT[aRBT Holder]
    end
    
    USER_RBT -->|locks RBT| LOCK_MECHANISM
    LOCK_MECHANISM --> FORMULA
    FORMULA --> ARBT_CONTRACT
    
    STEP1 --> STEP2
    STEP2 --> STEP3
    STEP3 --> STEP4
    STEP4 --> STEP5
    STEP5 --> STEP6
    STEP6 --> STEP7
    
    ARBT_CONTRACT --> STEP3
    STEP3 --> STABILITY_BENEFIT
    STEP5 --> ECOSYSTEM_UTILITY
    STEP7 --> ECOSYSTEM_UTILITY
    
    ARBT_CONTRACT --> REVENUE_SHARE
    
    BACKING_CALC --> PROPORTIONAL_MINT
    PROPORTIONAL_MINT --> RBT_CONTRACT
    
    LOCK_MECHANISM --> USER_ARBT
    
    RBT_CONTRACT -->|used in| DEFI[MegaETH DeFi Ecosystem]
    ECOSYSTEM_UTILITY --> DEFI
```

## HPN (Haven Protected Notes) Architecture

```mermaid
graph LR
    subgraph "User Interface"
        USER[User]
        NFT[HPN NFT<br/>ERC-721<br/>Unique Position]
    end
    
    subgraph "HPN Contract"
        DEPOSIT_HANDLER[Deposit Handler<br/>USDM/USDMy only]
        POSITION_TRACKER[Position Tracker<br/>Principal + Terms]
        REWARD_ACCRUAL[Reward Accrual<br/>Yield + MEGA Points]
        EXIT_HANDLER[Exit Handler<br/>3 Options]
    end
    
    subgraph "State Management"
        POSITIONS[(Position Storage<br/>Principal, Terms)]
        REWARDS[(Reward Storage<br/>Yield, Points)]
        COOLDOWNS[(Cooldown Storage<br/>7-day period)]
    end
    
    subgraph "Integration Layer"
        TREASURY_INT[Treasury Interface<br/>Forward deposits]
        RBT_INT[RBT Interface<br/>Convert to RBT]
        ORACLE[Price Oracle<br/>For conversions]
    end
    
    USER -->|1. Deposit USDM/USDMy| DEPOSIT_HANDLER
    DEPOSIT_HANDLER -->|2. Mint NFT| NFT
    DEPOSIT_HANDLER -->|3. Record position| POSITION_TRACKER
    POSITION_TRACKER --> POSITIONS
    
    DEPOSIT_HANDLER -->|4. Forward funds| TREASURY_INT
    
    POSITION_TRACKER -->|5. Track rewards| REWARD_ACCRUAL
    REWARD_ACCRUAL --> REWARDS
    
    USER -->|Option A: Hold to maturity| EXIT_HANDLER
    USER -->|Option B: Early exit 2-3% fee| EXIT_HANDLER
    USER -->|Option C: Convert to RBT| EXIT_HANDLER
    
    EXIT_HANDLER -->|check position| POSITIONS
    EXIT_HANDLER -->|check rewards| REWARDS
    EXIT_HANDLER -->|initiate cooldown| COOLDOWNS
    
    EXIT_HANDLER -->|if converting| RBT_INT
    EXIT_HANDLER -->|get pricing| ORACLE
    
    COOLDOWNS -->|after 7 days| USER
```

## Fixed-Term Bonds Architecture (Olympus DAO Style)

```mermaid
graph TB
    subgraph "User Entry - Regular Bonds"
        USER_USDM[User with USDM]
        USER_MEGA[User with MEGA]
        BOND_USDM[USDM Bond<br/>Deposit USDM]
        BOND_MEGA[MEGA Bond<br/>Deposit MEGA]
    end
    
    subgraph "User Entry - LP Bonds"
        USER_LP_CREATOR[User Creates LP]
        RBT_USDM_POOL[RBT-USDM Pool<br/>DEX]
        LP_TOKEN_HOLDER[LP Token Holder]
        BOND_LP[LP Bond<br/>Deposit LP Token]
    end
    
    subgraph "Bond Depository"
        BOND_CONTROLLER[Bond Controller<br/>Pricing & Vesting]
        DISCOUNT_CALC[Discount Calculator<br/>Market Price vs Bond Price]
        VESTING_SCHEDULE[Vesting Schedule<br/>Linear Release<br/>User-selected period]
    end
    
    subgraph "Treasury Core"
        TREASURY_VAULT[Treasury Vault]
        USDM_RESERVE[USDM Reserve]
        MEGA_RESERVE[MEGA Reserve<br/>100% to Sequencer]
        POL_VAULT[POL Vault<br/>LP Locked Forever]
    end
    
    subgraph "RBT Minting Engine"
        RBT_MINTER[RBT Minter<br/>Proportional to Treasury]
        BACKING_RATIO[Backing Ratio<br/>Treasury Value / RBT Supply]
        DISCOUNT_RBT[Discounted RBT<br/>Below Market Price]
    end
    
    subgraph "Vesting & Distribution"
        VESTING_VAULT[Vesting Vault<br/>User Positions]
        DAILY_UNLOCK[Daily Unlock<br/>Linear Vesting]
        CLAIMABLE_RBT[Claimable RBT<br/>Vested Amount]
        UNVESTED_RBT[Unvested RBT<br/>Subject to Forfeit]
    end
    
    subgraph "Exit Mechanisms"
        MATURITY_CLAIM[Maturity Claim<br/>Full RBT + Rewards]
        EARLY_EXIT_HANDLER[Early Exit<br/>3.3% Fee<br/>Forfeit Unvested]
        FORFEIT_TO_TREASURY[Forfeited RBT<br/>Back to Treasury]
    end
    
    subgraph "Protocol Owned Liquidity"
        POL_REGISTRY[POL Registry<br/>Permanent LP Lock]
        TRADING_FEE_COLLECTOR[Trading Fees<br/>0.3% per swap]
        POL_TO_TREASURY[Fees to Treasury<br/>Forever]
    end
    
    subgraph "Value Accrual Loop"
        TREASURY_GROWTH[Treasury Growth]
        RBT_BACKING_INCREASE[RBT Backing Increase]
        DEEPER_LIQUIDITY[Deeper RBT Liquidity]
        MORE_BONDS[More Bond Demand]
    end
    
    USER_USDM --> BOND_USDM
    USER_MEGA --> BOND_MEGA
    
    USER_LP_CREATOR --> RBT_USDM_POOL
    RBT_USDM_POOL --> LP_TOKEN_HOLDER
    LP_TOKEN_HOLDER --> BOND_LP
    
    BOND_USDM --> BOND_CONTROLLER
    BOND_MEGA --> BOND_CONTROLLER
    BOND_LP --> BOND_CONTROLLER
    
    BOND_CONTROLLER --> DISCOUNT_CALC
    BOND_CONTROLLER --> VESTING_SCHEDULE
    
    BOND_USDM -->|deposits| USDM_RESERVE
    BOND_MEGA -->|deposits| MEGA_RESERVE
    BOND_LP -->|locks forever| POL_VAULT
    
    USDM_RESERVE --> TREASURY_VAULT
    MEGA_RESERVE --> TREASURY_VAULT
    POL_VAULT --> POL_REGISTRY
    
    TREASURY_VAULT --> BACKING_RATIO
    BACKING_RATIO --> RBT_MINTER
    DISCOUNT_CALC --> DISCOUNT_RBT
    RBT_MINTER --> DISCOUNT_RBT
    
    DISCOUNT_RBT --> VESTING_VAULT
    VESTING_SCHEDULE --> VESTING_VAULT
    
    VESTING_VAULT --> DAILY_UNLOCK
    DAILY_UNLOCK --> CLAIMABLE_RBT
    DAILY_UNLOCK --> UNVESTED_RBT
    
    CLAIMABLE_RBT --> MATURITY_CLAIM
    UNVESTED_RBT --> EARLY_EXIT_HANDLER
    EARLY_EXIT_HANDLER --> FORFEIT_TO_TREASURY
    FORFEIT_TO_TREASURY --> TREASURY_VAULT
    
    MATURITY_CLAIM -->|full vested RBT| USER_USDM
    MATURITY_CLAIM -->|full vested RBT| USER_MEGA
    MATURITY_CLAIM -->|full vested RBT| LP_TOKEN_HOLDER
    
    POL_REGISTRY --> TRADING_FEE_COLLECTOR
    TRADING_FEE_COLLECTOR --> POL_TO_TREASURY
    POL_TO_TREASURY --> TREASURY_VAULT
    
    TREASURY_VAULT --> TREASURY_GROWTH
    TREASURY_GROWTH --> RBT_BACKING_INCREASE
    RBT_BACKING_INCREASE --> DEEPER_LIQUIDITY
    DEEPER_LIQUIDITY --> MORE_BONDS
    MORE_BONDS --> BOND_CONTROLLER
```

## Complete System Integration

```mermaid
graph LR
    subgraph "MegaETH Layer"
        MEGAETH[MegaETH<br/>10ms blocks<br/>100k TPS]
        SEQUENCER[Sequencer<br/>Rotation]
        PROXIMITY[Proximity Markets<br/>HVN Governance Controls]
    end
    
    subgraph "Blackhaven Core"
        TREASURY_CORE[Treasury<br/>Reserve Management]
        RBT_SYSTEM[RBT System<br/>Reserve-Backed Token]
        ARBT_SYSTEM[aRBT System<br/>Locked RBT]
        HVN_GOVERNANCE[HVN<br/>Governance Token<br/>100M Supply]
        SHVN_STAKING[sHVN<br/>Staked HVN<br/>Treasury Rewards]
    end
    
    subgraph "Products"
        HPN_PRODUCT[HPN<br/>ERC-721 NFTs]
        REGULAR_BONDS_PRODUCT[Regular Bonds<br/>USDM/MEGA]
        LP_BONDS_PRODUCT[LP Bonds<br/>Aligned Bonds]
    end
    
    subgraph "External Integrations"
        BASELINE[Baseline Markets<br/>HVN Launch<br/>BLV Floor]
        DEFI_PROTOCOLS[MegaETH DeFi<br/>Whitelisted Strategies]
        ORACLES[Price Oracles<br/>Backing Calculation]
    end
    
    MEGAETH --> TREASURY_CORE
    TREASURY_CORE -->|stakes 100% MEGA| SEQUENCER
    HVN_GOVERNANCE -->|controls MEGA allocation| PROXIMITY
    
    HPN_PRODUCT --> TREASURY_CORE
    REGULAR_BONDS_PRODUCT --> TREASURY_CORE
    LP_BONDS_PRODUCT --> TREASURY_CORE
    TREASURY_CORE --> RBT_SYSTEM
    
    HVN_GOVERNANCE -->|launched on| BASELINE
    TREASURY_CORE -->|deploys to| DEFI_PROTOCOLS
    ORACLES -->|price feeds| TREASURY_CORE
    
    HVN_GOVERNANCE -->|stake to get| SHVN_STAKING
    RBT_SYSTEM -->|used in| DEFI_PROTOCOLS
    ARBT_SYSTEM -->|strengthens| RBT_SYSTEM
    
    SEQUENCER -->|rewards| TREASURY_CORE
    PROXIMITY -->|Yield opportunities<br/>Network utility| TREASURY_CORE
    DEFI_PROTOCOLS -->|yield| TREASURY_CORE
    
    TREASURY_CORE -->|growth rewards| SHVN_STAKING
```

---

*Complete architecture diagrams based on official Blackhaven documentation*
