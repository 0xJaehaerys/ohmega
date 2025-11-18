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

## 2A. HPN Deposit Flow

```mermaid
graph LR
    USER[User]
    HPN[HPN Contract]
    NFT[HPN NFT]
    TREASURY[Treasury]
    
    USER -->|deposit USDM or USDMy<br/>select lock period| HPN
    HPN -->|mint NFT<br/>tracks principal + rewards| USER
    HPN -->|forward stablecoins| TREASURY
    TREASURY -->|deploy to MegaETH DeFi<br/>earn yield + MEGA Points| NFT
```

## 2B. HPN Exit Options

```mermaid
graph TB
    NFT[HPN NFT]
    
    MATURITY[Hold to Maturity]
    EARLY[Early Exit]
    CONVERT[Convert to RBT]
    
    USER_MAT[User]
    USER_EARLY[User]
    USER_CONVERT[User]
    
    NFT -->|wait until lock ends<br/>7-day cooldown| MATURITY
    NFT -->|exit anytime<br/>2-3 percent fee<br/>7-day cooldown| EARLY
    NFT -->|instant conversion<br/>NO cooldown| CONVERT
    
    MATURITY -->|receive principal<br/>+ ALL rewards<br/>+ ALL MEGA Points| USER_MAT
    EARLY -->|receive principal<br/>- fee<br/>lose all rewards| USER_EARLY
    CONVERT -->|receive RBT<br/>lose all rewards<br/>NFT burns| USER_CONVERT
```

## 3A. Regular Bonds (USDM/MEGA)

```mermaid
graph LR
    USER[User]
    BOND[Bond Contract]
    TREASURY[Treasury]
    VESTING[Vesting Position]
    RBT[RBT]
    
    USER -->|deposit USDM or MEGA<br/>select vesting period<br/>receive discount| BOND
    BOND -->|forward to reserves<br/>MEGA: 100 percent to Sequencer| TREASURY
    BOND -->|create vested position<br/>linear daily unlock| VESTING
    VESTING -->|wait to maturity<br/>claim all vested RBT| RBT
    VESTING -->|early exit<br/>3.3 percent fee<br/>forfeit unvested| RBT
    RBT -->|receive RBT| USER
```

## 3B. LP Bonds (Aligned Bonds)

```mermaid
graph LR
    USER[User]
    LP_POOL[RBT-USDM Pool]
    LP_TOKEN[LP Token]
    LP_BOND[LP Bond Contract]
    POL[POL Vault]
    VESTING[Vesting Position]
    RBT[RBT]
    
    USER -->|provide liquidity<br/>RBT + USDM| LP_POOL
    LP_POOL -->|receive LP token| USER
    USER -->|deposit LP token<br/>select vesting period<br/>receive discount + rewards| LP_BOND
    LP_BOND -->|LP locked FOREVER<br/>becomes POL| POL
    POL -->|generates 0.3 percent trading fees<br/>to treasury| LP_BOND
    LP_BOND -->|create vested position<br/>linear daily unlock| VESTING
    VESTING -->|claim vested RBT<br/>+ rewards| RBT
    RBT -->|receive RBT| USER
```

## 4A. How to Get RBT

```mermaid
graph TB
    HPN[HPN NFT]
    BONDS[Bonds Vesting]
    MARKET[Market Buy]
    
    RBT[RBT Token]
    
    HPN -->|convert anytime<br/>instant NO cooldown<br/>at principal value<br/>forfeit rewards| RBT
    BONDS -->|wait for vesting<br/>linear daily unlock<br/>or early exit 3.3 percent fee| RBT
    MARKET -->|buy on DEX/CEX<br/>at market price| RBT
```

## 4B. RBT to aRBT (Locking)

```mermaid
graph LR
    RBT[RBT Token]
    LOCK[Lock RBT]
    ARBT[aRBT Token]
    
    USER_BENEFITS[User Benefits]
    
    RBT -->|lock for chosen period<br/>max 2 years| LOCK
    LOCK -->|aRBT = RBT × T_lock / T_max<br/>non-transferable<br/>pure utility| ARBT
    ARBT -->|revenue sharing<br/>protocol fees<br/>+ RBT stability<br/>+ ecosystem trust| USER_BENEFITS
    USER_BENEFITS -->|locked RBT strengthens| RBT
```

## 5A. HVN Launch on Baseline Markets

```mermaid
graph LR
    BASELINE[Baseline Markets]
    HVN[HVN Token]
    BLV[BLV Floor]
    FEATURES[Features]
    
    BASELINE -->|launch platform<br/>100M supply<br/>1 HVN = 1 vote| HVN
    HVN -->|guaranteed price floor<br/>rises with trading<br/>backed by reserves| BLV
    BLV -->|autonomous liquidity<br/>no liquidation borrowing<br/>capital-efficient leverage| FEATURES
```

## 5B. HVN Staking & Governance

```mermaid
graph TB
    HVN[HVN Token]
    STAKE[Stake]
    SHVN[sHVN]
    
    GOVERNANCE[Governance Votes]
    TREASURY[Treasury]
    REWARDS[Rewards]
    
    HVN -->|stake HVN<br/>governance-linked| STAKE
    STAKE -->|receive sHVN<br/>non-transferable| SHVN
    SHVN -->|earn treasury rewards<br/>configured via governance| REWARDS
    
    HVN -->|1 HVN = 1 vote<br/>propose and vote| GOVERNANCE
    GOVERNANCE -->|treasury strategy<br/>DeFi whitelisting<br/>MEGA allocation Proximity Markets<br/>reward distribution| TREASURY
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
