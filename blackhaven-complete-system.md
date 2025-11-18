# Blackhaven Complete System Diagram

## Full Protocol Architecture - Olympus DAO Style

```mermaid
graph TB
    subgraph "GOVERNANCE"
        HVN[HVN]
        SHVN[sHVN<br/>Staked HVN]
        
        HVN <-->|stake/unstake| SHVN
    end
    
    subgraph "USER PRODUCTS"
        HPN[HPN<br/>USDM/USDMy<br/>NFT]
        BOND_USDM[USDM Bond]
        BOND_MEGA[MEGA Bond]
        LP_BOND[LP Bond<br/>RBT-USDM]
    end
    
    subgraph "TREASURY"
        TREASURY[TREASURY<br/>━━━━━━━━━━━━━━━<br/>USDM Reserve<br/>MEGA Reserve<br/>POL<br/>━━━━━━━━━━━━━━━]
    end
    
    subgraph "VALUE GENERATION"
        MEGAETH_DEFI[MegaETH DeFi<br/>generates yield]
        POL_FEES[POL<br/>generates fees]
    end
    
    subgraph "RBT SYSTEM"
        RBT[RBT<br/>Reserve-Backed]
        ARBT[aRBT<br/>Non-transferable<br/>aRBT = RBT × T/2yr<br/>Revenue sharing]
        
        RBT -->|lock up to 2 years| ARBT
    end
    
    HPN --> TREASURY
    BOND_USDM --> TREASURY
    BOND_MEGA --> TREASURY
    LP_BOND --> TREASURY
    
    TREASURY --> MEGAETH_DEFI
    MEGAETH_DEFI --> TREASURY
    
    LP_BOND --> POL_FEES
    POL_FEES --> TREASURY
    
    TREASURY --> RBT
    
    HPN --> RBT
    BOND_USDM --> RBT
    BOND_MEGA --> RBT
    LP_BOND --> RBT
    
    TREASURY --> SHVN
    
    HVN -.-> TREASURY
```

---

*Blackhaven Protocol - Complete System with User Flows and Integrations*

