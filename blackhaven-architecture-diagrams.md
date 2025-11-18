# Blackhaven Protocol Architecture

## 1. Complete Blackhaven System Architecture

```mermaid
graph TB
    subgraph "User Entry Points"
        USER1[User with<br/>USDM/USDMy]
        USER2[User with<br/>MEGA]
        USER3[User with<br/>RBT-USDM LP]
    end
    
    subgraph "Product Layer"
        HPN[Haven Protected<br/>Notes<br/>ERC-721 NFTs]
        REGULAR_BONDS[Regular Bonds<br/>USDM/MEGA → RBT]
        LP_BONDS[LP Bonds<br/>Aligned Bonds]
    end
    
    subgraph "Treasury Core"
        TREASURY[Treasury<br/>Contract]
        USDM_RESERVE[USDM<br/>Reserve]
        MEGA_RESERVE[MEGA<br/>Reserve]
        POL[Protocol Owned<br/>Liquidity<br/>LP Locked Forever]
    end
    
    subgraph "Value Generation"
        SEQUENCER[MegaETH<br/>Sequencer<br/>100 percent MEGA staked]
        DEFI_STRAT[Whitelisted<br/>MegaETH DeFi<br/>USDM/USDMy only]
        POL_FEES[Trading Fees<br/>0.3 percent per swap]
    end
    
    subgraph "RBT Minting"
        RBT_MINTER[RBT Minter<br/>Proportional to<br/>Treasury Value]
        BACKING[Backing Ratio<br/>Treasury Value /<br/>RBT Supply]
    end
    
    subgraph "Token System"
        RBT[RBT Token<br/>Reserve-Backed<br/>Transferable]
        ARBT[aRBT Token<br/>Non-transferable<br/>Locked RBT]
    end
    
    subgraph "Governance"
        HVN[HVN Token<br/>100M Supply<br/>Governance]
        SHVN[sHVN<br/>Staked HVN<br/>Treasury Rewards]
        BASELINE[Baseline Markets<br/>BLV Floor<br/>No Liquidation]
    end
    
    USER1 -->|deposits USDM/USDMy<br/>select lock period| HPN
    USER2 -->|deposits USDM/MEGA<br/>select vesting period| REGULAR_BONDS
    USER3 -->|deposits LP token<br/>locked forever| LP_BONDS
    
    HPN -->|forwards deposits| TREASURY
    REGULAR_BONDS -->|forwards deposits| TREASURY
    LP_BONDS -->|locks LP permanently| POL
    
    TREASURY --> USDM_RESERVE
    TREASURY --> MEGA_RESERVE
    
    USDM_RESERVE -->|deploys capital| DEFI_STRAT
    MEGA_RESERVE -->|stakes ALL MEGA| SEQUENCER
    POL -->|generates fees| POL_FEES
    
    SEQUENCER -->|sequencer rewards| TREASURY
    DEFI_STRAT -->|lending/yield| TREASURY
    POL_FEES -->|trading fees| TREASURY
    
    TREASURY -->|calculates backing| BACKING
    BACKING -->|mints proportionally| RBT_MINTER
    RBT_MINTER --> RBT
    
    RBT -->|users lock RBT<br/>aRBT = RBT × T/2yrs| ARBT
    
    HPN -->|can convert to RBT<br/>instant, forfeit rewards| RBT
    REGULAR_BONDS -->|vests to RBT<br/>daily unlock| RBT
    LP_BONDS -->|vests to RBT<br/>daily unlock| RBT
    
    HVN -->|launched on| BASELINE
    HVN -->|stake to receive| SHVN
    SHVN -->|earns rewards from| TREASURY
    
    HVN -->|governs| TREASURY
    HVN -->|governs| DEFI_STRAT
```

## 2. HPN (Haven Protected Notes) Architecture

```mermaid
graph TB
    subgraph "User Actions"
        USER[User]
        WALLET[User Wallet<br/>USDM/USDMy]
    end
    
    subgraph "HPN Contract"
        DEPOSIT_HANDLER[Deposit Handler<br/>Mints ERC-721 NFT]
        NFT_STORAGE[NFT Storage<br/>Principal + Terms<br/>+ Accrued Rewards]
        EXIT_HANDLER[Exit Handler<br/>3 Options]
    end
    
    subgraph "Exit Options"
        MATURITY[Hold to Maturity<br/>7-day cooldown<br/>Principal + ALL Rewards]
        EARLY_EXIT[Early Exit<br/>7-day cooldown<br/>Principal - 2-3 percent fee<br/>Forfeit rewards]
        RBT_CONVERT[Convert to RBT<br/>NO cooldown<br/>Instant conversion<br/>Forfeit rewards]
    end
    
    subgraph "Treasury Integration"
        TREASURY[Treasury]
        DEFI[MegaETH DeFi<br/>Strategies]
        YIELD[Yield Accrual<br/>MEGA Points<br/>Amplified]
    end
    
    subgraph "RBT System"
        RBT[RBT Token]
        BURN[Burn NFT<br/>on conversion]
    end
    
    USER -->|1. Connect wallet| WALLET
    WALLET -->|2. Deposit USDM/USDMy<br/>select lock period| DEPOSIT_HANDLER
    DEPOSIT_HANDLER -->|3. Mint NFT| NFT_STORAGE
    DEPOSIT_HANDLER -->|4. Forward funds| TREASURY
    
    TREASURY -->|5. Deploy capital| DEFI
    DEFI -->|6. Generate yield + points| YIELD
    YIELD -->|7. Accrue to NFT| NFT_STORAGE
    
    NFT_STORAGE -->|8a. At maturity| MATURITY
    NFT_STORAGE -->|8b. Before maturity| EARLY_EXIT
    NFT_STORAGE -->|8c. Anytime| RBT_CONVERT
    
    MATURITY -->|wait 7 days| USER
    EARLY_EXIT -->|charge fee, wait 7 days| USER
    RBT_CONVERT -->|immediate| BURN
    BURN -->|instant RBT| RBT
    RBT --> USER
```

## 3. Fixed-Term Bonds Architecture

```mermaid
graph TB
    subgraph "Bond Entry"
        USER_USDM[User with USDM]
        USER_MEGA[User with MEGA]
        USER_LP[User with<br/>RBT-USDM LP]
    end
    
    subgraph "Bond Depository"
        USDM_BOND[USDM Bond]
        MEGA_BOND[MEGA Bond]
        LP_BOND[LP Bond<br/>Aligned]
        PRICING[Bond Pricing<br/>Discount Calculator]
        TERMS[Vesting Terms<br/>User-selected period]
    end
    
    subgraph "Treasury"
        TREASURY[Treasury Vault]
        USDM_RES[USDM Reserve]
        MEGA_RES[MEGA Reserve<br/>100 percent to Sequencer]
        POL_VAULT[POL Vault<br/>LP Locked FOREVER]
    end
    
    subgraph "Vesting System"
        VESTING[Vesting Contract<br/>Linear Release]
        DAILY_UNLOCK[Daily RBT Unlock]
        CLAIMABLE[Claimable RBT<br/>User Dashboard]
    end
    
    subgraph "RBT Minting"
        BACKING[Treasury Value /<br/>RBT Supply]
        MINTER[RBT Minter<br/>Discounted RBT]
    end
    
    subgraph "Exit Mechanisms"
        MATURITY[Hold to Maturity<br/>Claim all vested RBT]
        EARLY_EXIT[Early Exit<br/>3.3 percent fee<br/>Forfeit unvested]
    end
    
    USER_USDM -->|deposit USDM| USDM_BOND
    USER_MEGA -->|deposit MEGA| MEGA_BOND
    USER_LP -->|deposit LP token| LP_BOND
    
    USDM_BOND --> PRICING
    MEGA_BOND --> PRICING
    LP_BOND --> PRICING
    
    PRICING -->|calculate discount<br/>vs market price| TERMS
    
    USDM_BOND -->|to treasury| USDM_RES
    MEGA_BOND -->|to treasury| MEGA_RES
    LP_BOND -->|LOCKED FOREVER| POL_VAULT
    
    USDM_RES --> TREASURY
    MEGA_RES --> TREASURY
    POL_VAULT -->|trading fees 0.3 percent| TREASURY
    
    TREASURY --> BACKING
    BACKING --> MINTER
    TERMS --> MINTER
    
    MINTER -->|create position| VESTING
    VESTING -->|unlock daily| DAILY_UNLOCK
    DAILY_UNLOCK --> CLAIMABLE
    
    CLAIMABLE -->|wait to maturity| MATURITY
    CLAIMABLE -->|exit early| EARLY_EXIT
    
    MATURITY -->|full vested RBT| USER_USDM
    EARLY_EXIT -->|vested - 3.3 percent fee<br/>forfeit unvested| USER_USDM
```

## 4. RBT & aRBT System

```mermaid
graph LR
    subgraph "RBT Sources"
        HPN_CONVERT[HPN Conversion<br/>Instant<br/>at principal value]
        BOND_VEST[Bond Vesting<br/>Daily unlock]
        MARKET_BUY[Market Purchase<br/>DEX/CEX]
    end
    
    subgraph "RBT Token"
        RBT[RBT Token<br/>Reserve-Backed<br/>Transferable]
        BACKING[Backing Ratio<br/>Treasury Value /<br/>RBT Supply]
    end
    
    subgraph "aRBT Locking"
        LOCK_UI[Lock Interface<br/>Select duration<br/>up to 2 years]
        FORMULA[aRBT Formula<br/>aRBT = RBT × T_lock / T_max<br/>T_max = 2 years]
        ARBT[aRBT Token<br/>Non-transferable<br/>Pure utility]
    end
    
    subgraph "aRBT Benefits"
        REVENUE[Revenue Sharing<br/>Protocol fees]
        STABILITY[RBT Stability<br/>Strengthens liquidity]
        TRUST[Ecosystem Trust<br/>MegaETH DeFi integration]
    end
    
    subgraph "Treasury"
        TREASURY[Treasury]
        RESERVE[Reserve Assets]
    end
    
    HPN_CONVERT --> RBT
    BOND_VEST --> RBT
    MARKET_BUY --> RBT
    
    TREASURY --> BACKING
    RESERVE --> BACKING
    BACKING -->|proportional minting| RBT
    
    RBT -->|user locks RBT| LOCK_UI
    LOCK_UI --> FORMULA
    FORMULA --> ARBT
    
    ARBT --> REVENUE
    ARBT --> STABILITY
    ARBT --> TRUST
    
    STABILITY -->|locked RBT<br/>creates stability| RBT
    TRUST -->|facilitates acceptance| RBT
```

## 5. HVN & sHVN Governance System

```mermaid
graph TB
    subgraph "Baseline Markets Launch"
        BASELINE[Baseline Markets<br/>HVN Launch Platform]
        BLV[BLV Floor<br/>Guaranteed price floor<br/>Only goes up]
        LIQUIDITY[Autonomous Liquidity<br/>No liquidation borrowing]
    end
    
    subgraph "HVN Token"
        HVN[HVN Token<br/>100M Total Supply<br/>1 HVN = 1 vote]
        ALLOCATION[Token Allocation<br/>22 percent Public no vesting<br/>12.5 percent Core 1yr+2yr<br/>7.5 percent Private 1yr+3mo<br/>18 percent Ecosystem 1yr<br/>40 percent Future Emissions]
    end
    
    subgraph "Staking"
        STAKE[Stake HVN]
        SHVN[sHVN<br/>Non-transferable<br/>Staked HVN]
        REWARDS[Treasury Rewards<br/>Configured via governance]
    end
    
    subgraph "Governance Actions"
        TREASURY_GOV[Treasury Strategy<br/>Deployment params<br/>Reserve ratios]
        DEFI_GOV[DeFi Whitelisting<br/>Strategy selection]
        MEGA_GOV[MEGA Allocation<br/>Proximity Market usage]
        REWARDS_GOV[Reward Distribution<br/>Emission schedules]
    end
    
    subgraph "Proximity Markets"
        PROXIMITY[Proximity Markets<br/>Sequencer-adjacent<br/>floorspace bidding]
        MEGA_STAKING[MEGA Staking<br/>100 percent of treasury MEGA]
    end
    
    subgraph "Treasury"
        TREASURY[Treasury Contract]
        GROWTH[Treasury Growth<br/>Yields + Fees + Forfeits]
    end
    
    BASELINE -->|launch platform| HVN
    HVN --> BLV
    BLV --> LIQUIDITY
    
    HVN --> ALLOCATION
    
    HVN -->|users stake| STAKE
    STAKE --> SHVN
    
    HVN -->|vote on| TREASURY_GOV
    HVN -->|vote on| DEFI_GOV
    HVN -->|vote on| MEGA_GOV
    HVN -->|vote on| REWARDS_GOV
    
    TREASURY_GOV --> TREASURY
    DEFI_GOV --> TREASURY
    REWARDS_GOV --> REWARDS
    
    MEGA_GOV -->|controls allocation| PROXIMITY
    PROXIMITY --> MEGA_STAKING
    MEGA_STAKING --> TREASURY
    
    TREASURY --> GROWTH
    GROWTH -->|distributes rewards| SHVN
    REWARDS -->|distributes rewards| SHVN
```

## 6. Treasury Flywheel & Value Accrual

```mermaid
graph TB
    subgraph "User Deposits"
        HPN_DEP[HPN Deposits<br/>USDM/USDMy]
        BOND_DEP[Bond Deposits<br/>USDM/MEGA/LP]
    end
    
    subgraph "Treasury Inflows"
        DEPOSITS[User Deposits]
        YIELD[Strategy Yields]
        SEQ_REWARDS[Sequencer Rewards<br/>MEGA staking]
        MEGA_POINTS[MEGA Points<br/>from TVL]
        EXIT_FEES[Exit Fees<br/>HPN: 2-3 percent<br/>Bonds: 3.3 percent]
        FORFEITS[Forfeited Rewards<br/>Early exits]
        POL_FEES[POL Trading Fees<br/>0.3 percent per swap]
        PRM[Premium Range<br/>Mechanism<br/>Coming Soon]
    end
    
    subgraph "Treasury Core"
        TREASURY[Treasury Contract<br/>Reserve Management]
        RESERVES[Reserve Assets<br/>Growing continuously]
    end
    
    subgraph "Deployment"
        MEGA_SEQ[MEGA Sequencer<br/>100 percent of MEGA]
        DEFI_STRAT[MegaETH DeFi<br/>Whitelisted strategies<br/>USDM/USDMy only]
        POL_MGMT[POL Management<br/>LP locked forever]
    end
    
    subgraph "Value Distribution"
        RBT_BACKING[RBT Backing Growth<br/>Stronger reserve ratio]
        SHVN_REWARDS[sHVN Rewards<br/>Treasury growth distribution]
        LIQUIDITY[Deeper Liquidity<br/>RBT-USDM pool]
    end
    
    subgraph "Flywheel Loop"
        MORE_USERS[Attracts More Users<br/>Stronger backing]
        MORE_DEPOSITS[More Deposits<br/>Cycle continues]
    end
    
    HPN_DEP --> DEPOSITS
    BOND_DEP --> DEPOSITS
    
    DEPOSITS --> TREASURY
    YIELD --> TREASURY
    SEQ_REWARDS --> TREASURY
    MEGA_POINTS --> TREASURY
    EXIT_FEES --> TREASURY
    FORFEITS --> TREASURY
    POL_FEES --> TREASURY
    PRM -.->|future| TREASURY
    
    TREASURY --> RESERVES
    
    RESERVES --> MEGA_SEQ
    RESERVES --> DEFI_STRAT
    RESERVES --> POL_MGMT
    
    MEGA_SEQ -->|sequencer rewards| YIELD
    DEFI_STRAT -->|lending/yield| YIELD
    POL_MGMT -->|trading fees| POL_FEES
    
    RESERVES --> RBT_BACKING
    RESERVES --> SHVN_REWARDS
    RESERVES --> LIQUIDITY
    
    RBT_BACKING --> MORE_USERS
    SHVN_REWARDS --> MORE_USERS
    LIQUIDITY --> MORE_USERS
    
    MORE_USERS --> MORE_DEPOSITS
    MORE_DEPOSITS --> HPN_DEP
    MORE_DEPOSITS --> BOND_DEP
```

---

*Blackhaven Protocol Architecture - System Component Diagrams*
