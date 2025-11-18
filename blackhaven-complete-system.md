# Blackhaven Complete System Diagram

## Full Protocol Architecture - Olympus DAO Style

```mermaid
graph TB
    subgraph "USER PRODUCTS"
        HPN[HPN<br/>━━━━━━━━━━━━━━━<br/>Deposit: USDM/USDMy<br/>ERC-721 NFT<br/>Principal protected<br/>━━━━━━━━━━━━━━━<br/>Exit: Maturity 7d cooldown<br/>Exit: Early 2-3 percent fee<br/>Exit: Convert to RBT instant]
        
        BOND_USDM[USDM Bond<br/>━━━━━━━━━━━━━━━<br/>Discounted RBT<br/>Linear vesting<br/>Early exit: 3.3 percent fee]
        
        BOND_MEGA[MEGA Bond<br/>━━━━━━━━━━━━━━━<br/>Discounted RBT<br/>Linear vesting<br/>Early exit: 3.3 percent fee]
        
        LP_BOND[LP Bond<br/>━━━━━━━━━━━━━━━<br/>Deposit: RBT-USDM LP<br/>LP locked FOREVER<br/>Becomes POL<br/>Discounted RBT + rewards]
    end
    
    subgraph "TREASURY CORE"
        TREASURY[TREASURY<br/>━━━━━━━━━━━━━━━<br/>USDM Reserve → DeFi<br/>MEGA Reserve → Sequencer<br/>POL → RBT-USDM forever<br/>━━━━━━━━━━━━━━━<br/>Backs RBT proportionally]
    end
    
    subgraph "VALUE GENERATION"
        MEGAETH_DEFI[MegaETH DeFi<br/>━━━━━━━━━━━━━━━<br/>Whitelisted strategies<br/>Lending & Yield<br/>Generates returns]
        
        POL_VAULT[POL Vault<br/>━━━━━━━━━━━━━━━<br/>RBT-USDM Pool<br/>LP locked FOREVER<br/>Trading fees to treasury<br/>No sell pressure]
        
        MEGA_POINTS[MEGA Points<br/>━━━━━━━━━━━━━━━<br/>From TVL contributions<br/>Amplified via HPN<br/>Converted to MEGA]
    end
    
    subgraph "RBT SYSTEM"
        RBT[RBT Token<br/>━━━━━━━━━━━━━━━<br/>Reserve-Backed<br/>Transferable<br/>Used in MegaETH DeFi]
        
        ARBT[aRBT Token<br/>━━━━━━━━━━━━━━━<br/>Non-transferable<br/>aRBT = RBT × T_lock / T_max<br/>T_max = 2 years<br/>Revenue sharing<br/>Protocol fees distribution]
        
        RBT --> ARBT
    end
    
    HPN --> TREASURY
    BOND_USDM --> TREASURY
    BOND_MEGA --> TREASURY
    LP_BOND --> TREASURY
    
    LP_BOND --> POL_VAULT
    
    TREASURY --> MEGAETH_DEFI
    MEGAETH_DEFI --> TREASURY
    
    POL_VAULT --> TREASURY
    MEGA_POINTS --> TREASURY
    
    TREASURY --> RBT
    
    HPN --> RBT
    BOND_USDM --> RBT
    BOND_MEGA --> RBT
    LP_BOND --> RBT
```

---

*Blackhaven Protocol - Complete System with User Flows and Integrations*

