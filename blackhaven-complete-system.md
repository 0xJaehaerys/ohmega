# Blackhaven Complete System Diagram

## Full Protocol Architecture - Olympus DAO Style

```mermaid
graph TB
    subgraph "TOP - RBT LOCKING"
        RBT_TOP[RBT]
        LOCK_BTN[LOCK]
        UNLOCK_BTN[UNLOCK]
        ARBT_LOCKED[aRBT<br/>locked RBT<br/>0-2 years]
        
        RBT_TOP --> LOCK_BTN
        LOCK_BTN --> ARBT_LOCKED
        ARBT_LOCKED --> UNLOCK_BTN
        UNLOCK_BTN --> RBT_TOP
    end
    
    subgraph "LEFT TOP - HPN"
        USER_HPN[User<br/>USDM/USDMy]
        HPN[HPN<br/>━━━━━━━━━━━<br/>NFT<br/>Principal Protected]
        
        USER_HPN --> HPN
    end
    
    subgraph "LEFT BOTTOM - BONDS"
        USER_BONDS[User<br/>USDM or MEGA]
        
        BONDS[Fixed-Term<br/>Bonds<br/>━━━━━━━━━━━<br/>Discounted RBT<br/>Vesting]
        
        USER_BONDS --> BONDS
    end
    
    subgraph "CENTER - TREASURY"
        TREASURY[TREASURY<br/>═══════════════════<br/>USDM Reserve<br/>MEGA Reserve<br/>POL RBT-USDM<br/>═══════════════════<br/>RBT is backed<br/>by treasury]
    end
    
    subgraph "RIGHT - LP BONDS"
        USER_LP[User<br/>RBT-USDM LP]
        
        LP_BONDS[LP Bond<br/>━━━━━━━━━━━<br/>LP locked FOREVER<br/>POL]
        
        USER_LP --> LP_BONDS
    end
    
    subgraph "BOTTOM - TREASURY INFLOWS"
        HPN_DEPOSIT[HPN Deposits<br/>USDM/USDMy]
        BOND_DEPOSIT[Bond Deposits<br/>USDM/MEGA]
        LP_DEPOSIT[LP Bond<br/>RBT-USDM LP]
        
        YIELD_INCOME[Yield from<br/>MegaETH DeFi]
        POL_FEES[Trading Fees<br/>from POL]
        MEGA_REWARDS[MEGA Points<br/>Sequencer rewards]
        EXIT_FEES[Exit Fees<br/>2-3 percent HPN<br/>3.3 percent Bonds]
        FORFEIT[Forfeited Rewards<br/>Early exits]
    end
    
    subgraph "RIGHT BOTTOM - OUTPUTS"
        RBT_OUT[RBT<br/>━━━━━━━━━━━<br/>Reserve-Backed<br/>Minted by Treasury]
        
        USER_GETS[Users receive<br/>━━━━━━━━━━━<br/>RBT daily unlock<br/>or instant convert]
        
        RBT_OUT --> USER_GETS
    end
    
    HPN --> HPN_DEPOSIT
    BONDS --> BOND_DEPOSIT
    LP_BONDS --> LP_DEPOSIT
    
    HPN_DEPOSIT --> TREASURY
    BOND_DEPOSIT --> TREASURY
    LP_DEPOSIT --> TREASURY
    
    YIELD_INCOME --> TREASURY
    POL_FEES --> TREASURY
    MEGA_REWARDS --> TREASURY
    EXIT_FEES --> TREASURY
    FORFEIT --> TREASURY
    
    TREASURY -->|mints proportionally| RBT_OUT
    
    ARBT_LOCKED -.->|receives protocol fees| TREASURY
```

---

*Blackhaven Protocol - Complete System with User Flows and Integrations*

