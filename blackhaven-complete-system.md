# Blackhaven Complete System Diagram

## Full Protocol Architecture - Olympus DAO Style

```mermaid
graph TB
    subgraph "TOP - GOVERNANCE"
        HVN[HVN]
        STAKE[STAKE]
        UNSTAKE[UNSTAKE]
        SHVN[Staked HVN<br/>sHVN holders<br/>earn treasury rewards]
        
        HVN -->|stake| STAKE
        STAKE --> SHVN
        SHVN -->|unstake| UNSTAKE
        UNSTAKE --> HVN
    end
    
    subgraph "LEFT - USER DEPOSITS"
        USER_HPN[User:<br/>USDM/USDMy]
        USER_BOND_USDM[User:<br/>USDM]
        USER_BOND_MEGA[User:<br/>MEGA]
        USER_LP[User:<br/>RBT-USDM LP]
        
        HPN_PRODUCT[HPN<br/>ERC-721 NFT<br/>Principal protected]
        BOND_USDM[USDM Bond<br/>Discounted RBT<br/>Vesting]
        BOND_MEGA[MEGA Bond<br/>Discounted RBT<br/>Vesting]
        LP_BOND[LP Bond<br/>Discounted RBT<br/>LP locked forever]
        
        USER_HPN --> HPN_PRODUCT
        USER_BOND_USDM --> BOND_USDM
        USER_BOND_MEGA --> BOND_MEGA
        USER_LP --> LP_BOND
    end
    
    subgraph "CENTER - TREASURY"
        TREASURY[TREASURY<br/>━━━━━━━━━━━━━━━<br/>USDM Reserve<br/>MEGA Reserve<br/>POL RBT-USDM<br/>━━━━━━━━━━━━━━━<br/>Backing RBT]
    end
    
    subgraph "RIGHT - VALUE GENERATION"
        MEGAETH_DEFI[MegaETH DeFi<br/>Whitelisted strategies<br/>generates yield]
        
        POL_FEES[POL generates<br/>trading fees]
        
        MEGA_POINTS[MEGA Points<br/>from TVL]
    end
    
    subgraph "BOTTOM - RBT SYSTEM"
        RBT[RBT Token<br/>Reserve-Backed]
        
        ARBT[aRBT<br/>Locked RBT<br/>Revenue sharing]
        
        RBT -->|lock| ARBT
    end
    
    subgraph "BOTTOM RIGHT - LIQUIDITY"
        RBT_USDM_POOL[RBT-USDM<br/>Liquidity Pool]
        
        POL_VAULT[POL<br/>from LP Bonds<br/>locked forever]
    end
    
    HPN_PRODUCT -->|deposits flow in| TREASURY
    BOND_USDM -->|deposits flow in| TREASURY
    BOND_MEGA -->|deposits flow in| TREASURY
    LP_BOND -->|deposits flow in| TREASURY
    
    LP_BOND -->|LP locked| POL_VAULT
    
    TREASURY -->|deploys USDM| MEGAETH_DEFI
    MEGAETH_DEFI -->|yields back| TREASURY
    
    POL_VAULT -->|generates| POL_FEES
    POL_FEES -->|fees to| TREASURY
    
    MEGA_POINTS -->|accrue to| TREASURY
    
    TREASURY -->|mints proportionally| RBT
    
    RBT -->|trades in| RBT_USDM_POOL
    
    HPN_PRODUCT -->|can convert to| RBT
    BOND_USDM -->|vests to| RBT
    BOND_MEGA -->|vests to| RBT
    LP_BOND -->|vests to| RBT
    
    TREASURY -->|distributes rewards| SHVN
    
    HVN -.->|governs| TREASURY
    HVN -.->|governs| MEGAETH_DEFI
    
    RBT -->|stronger backing| TREASURY
    SHVN -->|attracts more| USER_HPN
    RBT -->|attracts more| USER_BOND_USDM
```

---

*Blackhaven Protocol - Complete System with User Flows and Integrations*

