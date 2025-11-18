# Blackhaven Complete Mechanics Diagram

## Complete System Overview (Olympus-Style)

```mermaid
graph TB
    subgraph "Staking & Unstaking"
        STAKE[STAKE<br/>HVN]
        UNSTAKE[UNSTAKE<br/>HVN]
        HVN_TOKEN[HVN Token<br/>Governance]
        SHVN_GRID[Staked HVN Grid<br/>sHVN Positions]
    end
    
    subgraph "Treasury Assets"
        TREASURY[Treasury<br/>Reserve Assets]
        
        subgraph "Treasury Composition"
            USDM[USDM<br/>Stablecoin]
            USDMY[USDMy<br/>Stablecoin]
            MEGA[MEGA<br/>100% Staked in Sequencer]
            RBT_USDM[RBT-USDM LP<br/>POL]
            DEFI_YIELD[DeFi Yields<br/>From Strategies]
            MEGA_POINTS[MEGA Points<br/>Converted to MEGA]
        end
    end
    
    subgraph "Entry Products"
        subgraph "HPN - Haven Protected Notes"
            HPN_USDM[HPN USDM<br/>ERC-721 NFT]
            HPN_USDMY[HPN USDMy<br/>ERC-721 NFT]
        end
        
        subgraph "Regular Bonds"
            BOND_USDM[Bond USDM<br/>→ Discounted RBT]
            BOND_MEGA[Bond MEGA<br/>→ Discounted RBT]
        end
        
        subgraph "LP Bonds - Aligned Bonds"
            LP_BOND[LP Bond<br/>RBT-USDM LP<br/>→ Discounted RBT<br/>LP Locked Forever]
        end
    end
    
    subgraph "RBT System"
        RBT_TOKEN[RBT Token<br/>Reserve-Backed]
        ARBT_TOKEN[aRBT Token<br/>Non-transferable<br/>Locked RBT]
        BACKING[Backing Calculator<br/>Treasury Value / RBT Supply]
    end
    
    subgraph "Protocol Owned Liquidity"
        DEX[RBT-USDM Pool<br/>On MegaETH DEX]
        POL_REGISTRY[POL Registry<br/>LP Tokens Locked Forever]
        TRADING_FEES[Trading Fees<br/>0.3% per swap<br/>→ Treasury]
    end
    
    subgraph "MegaETH Integration"
        SEQUENCER[MEGA Sequencer<br/>Staking<br/>100% of all MEGA]
        DEFI_STRATEGIES[Whitelisted<br/>MegaETH DeFi<br/>USDM/USDMy]
        PROXIMITY[Proximity Markets<br/>HVN Governance Controls]
    end
    
    %% Staking Flow
    HVN_TOKEN -->|Stake| STAKE
    STAKE --> SHVN_GRID
    SHVN_GRID -->|Unstake| UNSTAKE
    UNSTAKE --> HVN_TOKEN
    
    %% Treasury Backing
    SHVN_GRID -.->|Earns rewards from| TREASURY
    RBT_TOKEN -.->|Backed by| TREASURY
    
    %% Entry Products to Treasury
    HPN_USDM -->|Deposits| TREASURY
    HPN_USDMY -->|Deposits| TREASURY
    BOND_USDM -->|Proceeds| TREASURY
    BOND_MEGA -->|Proceeds| TREASURY
    LP_BOND -->|LP Tokens| POL_REGISTRY
    
    %% Treasury Composition
    TREASURY --> USDM
    TREASURY --> USDMY
    TREASURY --> MEGA
    TREASURY --> RBT_USDM
    TREASURY --> DEFI_YIELD
    TREASURY --> MEGA_POINTS
    
    %% MEGA Allocation
    MEGA -->|100%| SEQUENCER
    SEQUENCER -->|Rewards| TREASURY
    
    %% DeFi Strategies
    USDM --> DEFI_STRATEGIES
    USDMY --> DEFI_STRATEGIES
    DEFI_STRATEGIES -->|Yields| TREASURY
    
    %% POL Management
    POL_REGISTRY --> DEX
    DEX --> TRADING_FEES
    TRADING_FEES --> TREASURY
    
    %% RBT Minting
    TREASURY --> BACKING
    BACKING -->|Proportional Minting| RBT_TOKEN
    RBT_TOKEN -->|Lock| ARBT_TOKEN
    
    %% HPN to RBT Conversion
    HPN_USDM -.->|Can convert to| RBT_TOKEN
    HPN_USDMY -.->|Can convert to| RBT_TOKEN
    
    %% Bonds to RBT
    BOND_USDM -->|Mints discounted| RBT_TOKEN
    BOND_MEGA -->|Mints discounted| RBT_TOKEN
    LP_BOND -->|Mints discounted| RBT_TOKEN
    
    %% RBT Usage
    RBT_TOKEN --> DEX
    RBT_TOKEN --> DEFI_STRATEGIES
    
    %% Proximity Markets
    HVN_TOKEN -->|Governance controls| PROXIMITY
    PROXIMITY -.->|Yield opportunities<br/>Network utility| TREASURY
    
    %% Styling
    style TREASURY fill:#fff,stroke:#000,stroke-width:3px
    style RBT_TOKEN fill:#fff,stroke:#000,stroke-width:2px
    style POL_REGISTRY fill:#fff,stroke:#000,stroke-width:3px
    style SEQUENCER fill:#fff,stroke:#533483,stroke-width:2px
    style SHVN_GRID fill:#f9f9f9,stroke:#000,stroke-width:2px
```

## Detailed Annotations

### Staking & Unstaking
- **Stake HVN**: Lock HVN to receive sHVN (staked HVN)
- **Unstake**: Instant unstaking (no cooldown for HVN/sHVN)
- **sHVN Rewards**: Earn rewards from treasury growth, configured via HVN governance
- **Note**: sHVN does NOT control governance, only receives rewards

### Treasury Composition
- **USDM/USDMy**: Stablecoin deposits from HPN and Bonds
- **MEGA**: 100% of all MEGA tokens are staked in MegaETH Sequencer
- **RBT-USDM LP**: Protocol Owned Liquidity, locked forever
- **DeFi Yields**: Returns from whitelisted MegaETH DeFi strategies
- **MEGA Points**: Converted to MEGA and staked in Sequencer

### Entry Products
- **HPN (Haven Protected Notes)**: 
  - ERC-721 NFTs
  - Principal-protected positions
  - USDM/USDMy deposits only
  - 7-day cooldown on maturity/early exit
  - Can convert to RBT (immediate, no cooldown)
  
- **Regular Bonds**:
  - USDM/MEGA → Discounted RBT
  - Linear vesting
  - 3.3% early exit fee
  
- **LP Bonds (Aligned Bonds)**:
  - RBT-USDM LP tokens → Discounted RBT
  - LP tokens locked forever (POL)
  - Trading fees go to treasury

### RBT System
- **RBT**: Reserve-Backed Token, proportional minting preserves reserve ratio
- **aRBT**: Non-transferable token from locking RBT (formula: RBT_locked × T_lock / T_max, T_max = 2 years)
- **Backing**: Treasury Value / RBT Supply

### Protocol Owned Liquidity
- **POL**: LP tokens from LP Bonds are locked forever
- **Trading Fees**: 0.3% per swap goes to treasury
- **Liquidity Depth**: Provides price stability for RBT

### MegaETH Integration
- **Sequencer Staking**: 100% of MEGA staked, rewards compound to treasury
- **DeFi Strategies**: Whitelisted strategies for USDM/USDMy
- **Proximity Markets**: HVN governance controls how staked MEGA is used for yield opportunities and network utility

---

*Complete mechanics diagram showing all Blackhaven protocol components and their interactions*

