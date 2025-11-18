# Blackhaven Complete System Diagram

## Full Protocol Architecture - Olympus DAO Style

```mermaid
graph TB
    subgraph "TOP - HPN"
        USER_HPN[User deposits<br/>USDM or USDMy]
        HPN[HPN<br/>━━━━━━━━━━━━━━<br/>ERC-721 NFT<br/>Principal Protected<br/>Earns Yield<br/>Earns MEGA Points]
        USER_HPN_OUT[User receives:<br/>NFT<br/>Yield<br/>MEGA Points<br/>━━━━━━━━━━━━━━<br/>OR instant RBT]
        
        USER_HPN --> HPN
        HPN --> USER_HPN_OUT
    end
    
    subgraph "LEFT - BONDS"
        USER_BONDS[User deposits<br/>━━━━━━━━━━━━━━<br/>USDM or MEGA]
        
        BONDS[Fixed-Term Bonds<br/>━━━━━━━━━━━━━━<br/>Discounted RBT<br/>Linear vesting<br/>Daily unlock]
        
        USER_BONDS_OUT[User receives:<br/>━━━━━━━━━━━━━━<br/>Discounted RBT<br/>Unlocks daily<br/>Full at maturity]
        
        USER_BONDS --> BONDS
        BONDS --> USER_BONDS_OUT
    end
    
    subgraph "CENTER - TREASURY"
        TREASURY[TREASURY<br/>═══════════════════<br/>USDM Reserve<br/>MEGA Reserve<br/>POL: RBT-USDM LP<br/>═══════════════════<br/>Backs all RBT<br/>═══════════════════<br/>Deploys to DeFi<br/>Stakes MEGA<br/>Owns POL forever]
    end
    
    subgraph "RIGHT - LP BONDS"
        USER_LP[User deposits<br/>━━━━━━━━━━━━━━<br/>RBT-USDM LP token]
        
        LP_BONDS[LP Bond Aligned<br/>━━━━━━━━━━━━━━<br/>LP locked FOREVER<br/>Becomes POL<br/>Earns rewards]
        
        USER_LP_OUT[User receives:<br/>━━━━━━━━━━━━━━<br/>Discounted RBT<br/>Extra rewards<br/>Unlocks daily]
        
        USER_LP --> LP_BONDS
        LP_BONDS --> USER_LP_OUT
    end
    
    subgraph "BOTTOM LEFT - RBT SYSTEM"
        RBT_CIRCLE[RBT Token<br/>━━━━━━━━━━━━━━<br/>Backed by Treasury<br/>Tradeable<br/>Used in MegaETH DeFi]
        
        ARBT_CIRCLE[aRBT<br/>━━━━━━━━━━━━━━<br/>Lock RBT 0-2 years<br/>Get protocol fees<br/>Revenue sharing]
        
        RBT_CIRCLE --> ARBT_CIRCLE
    end
    
    subgraph "BOTTOM RIGHT - VALUE GENERATION"
        POL_GEN[POL<br/>━━━━━━━━━━━━━━<br/>RBT-USDM Pool<br/>LP locked forever<br/>Trading fees]
        
        DEFI_GEN[MegaETH DeFi<br/>━━━━━━━━━━━━━━<br/>Lending<br/>Yield strategies<br/>Returns]
        
        MEGA_GEN[MEGA<br/>━━━━━━━━━━━━━━<br/>Sequencer staking<br/>MEGA Points<br/>Proximity Markets]
    end
    
    HPN -->|deposits| TREASURY
    BONDS -->|deposits| TREASURY
    LP_BONDS -->|deposits| TREASURY
    LP_BONDS -->|LP locked forever| POL_GEN
    
    TREASURY -->|mints| RBT_CIRCLE
    
    POL_GEN -->|fees| TREASURY
    DEFI_GEN -->|yields| TREASURY
    MEGA_GEN -->|rewards| TREASURY
    
    TREASURY -.->|deploys| DEFI_GEN
    TREASURY -.->|stakes| MEGA_GEN
```

---

*Blackhaven Protocol - Complete System with User Flows and Integrations*

