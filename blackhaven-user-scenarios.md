# Blackhaven User Scenarios & Interactive Flows

## 🚀 Scenario 1: The DeFi Veteran

```mermaid
journey
    title DeFi Veteran Journey - From OHM Fork Skeptic to Blackhaven Believer
    
    section Research Phase
      Reads about Blackhaven: 3: Veteran
      Compares to OHM/Baseline: 4: Veteran
      Notices MegaETH Speed: 5: Veteran
      Checks Audits: 5: Veteran
    
    section Entry Strategy
      Buys HVN on Baseline: 5: Veteran
      Stakes 80% to sHVN: 5: Veteran
      Provides RBT-USDM LP: 4: Veteran
      Bonds LP for 90 days: 5: Veteran
    
    section Active Management
      Votes on Governance: 5: Veteran
      Monitors Treasury Growth: 5: Veteran
      Compounds sHVN Rewards: 5: Veteran
      Influences Proximity Strategy: 5: Veteran
    
    section Results (6 months)
      HVN Price +300%: 5: Veteran
      sHVN Rewards Earned: 5: Veteran
      LP Bond Vested: 5: Veteran
      Becomes Community Leader: 5: Veteran
```

### Veteran's Strategy Breakdown:
```
Initial Capital: $100,000
├── $40,000 → HVN (80% staked to sHVN)
├── $30,000 → RBT-USDM LP → 90-day Bond
└── $30,000 → HPN 12-month (safety allocation)

After 6 Months:
├── HVN: $160,000 (4x due to BLV + growth)
├── sHVN Rewards: $8,000 (20% APR on treasury growth)
├── LP Bond: $33,000 RBT (10% discount vested)
├── HPN: $30,000 + $1,800 yield + MEGA points
└── Total: $232,800 (+132.8%)
```

## 🎮 Scenario 2: The MegaETH Gamer

```mermaid
sequenceDiagram
    participant Gamer
    participant PumpParty as Pump.Party
    participant Blackhaven
    participant Treasury
    participant Rewards
    
    Note over Gamer: Loves MegaETH games
    Gamer->>PumpParty: Wins MEGA in game
    PumpParty->>Gamer: 1000 MEGA prize
    
    Gamer->>Blackhaven: Discover HPN
    Note over Gamer: "Principal protected? Based!"
    
    Gamer->>Blackhaven: Deposit 1000 MEGA
    Blackhaven->>Treasury: Convert to USDM value
    Treasury->>Gamer: Mint HPN NFT #420
    
    loop Every Day for 30 days
        Treasury->>Rewards: Generate yield
        Rewards->>Gamer: Accrue MEGA points
    end
    
    Gamer->>Blackhaven: Convert HPN to RBT
    Note over Gamer: "Need liquidity for new game"
    Blackhaven->>Gamer: RBT = Principal value
    Blackhaven->>Treasury: Keep all accrued rewards
    
    Gamer->>PumpParty: Use RBT in game
    Note over Gamer: Still protected, still gaming!
```

## 💰 Scenario 3: The Yield Farmer

```mermaid
graph TD
    subgraph "Month 1: Research"
        RESEARCH["📚 Study Yields<br/>HPN: 8-12% APY<br/>sHVN: Variable<br/>LP Bonds: 10% discount"]
        COMPARE["⚖️ Compare Risk<br/>HPN: No risk<br/>sHVN: Gov token risk<br/>Bonds: Time risk"]
    end
    
    subgraph "Month 2: Diversified Entry"
        SPLIT["💵 Split $200k"]
        HPN_ALLOC["🛡️ $100k → HPN 6mo<br/>Safety first"]
        BOND_ALLOC["💎 $50k → 30-day bonds<br/>Quick returns"]
        HVN_ALLOC["⚡ $50k → HVN + stake<br/>Upside exposure"]
    end
    
    subgraph "Month 6: Optimization"
        REINVEST["♻️ Reinvest Returns"]
        HPN_ROLL["Roll HPN → 12mo term<br/>Better yield"]
        BOND_LADDER["Create bond ladder<br/>Weekly maturities"]
        GOV_ACTIVE["Vote for yield strategies<br/>Influence returns"]
    end
    
    subgraph "Year 1 Results"
        TOTAL["💰 Total Value: $276,000<br/>+38% with safety"]
        PASSIVE["😎 Mostly Passive<br/>10min/week management"]
        NETWORK["🌐 Network Effects<br/>Part of MegaMafia"]
    end
    
    RESEARCH --> COMPARE
    COMPARE --> SPLIT
    SPLIT --> HPN_ALLOC
    SPLIT --> BOND_ALLOC
    SPLIT --> HVN_ALLOC
    
    HPN_ALLOC --> REINVEST
    BOND_ALLOC --> REINVEST
    HVN_ALLOC --> REINVEST
    
    REINVEST --> HPN_ROLL
    REINVEST --> BOND_LADDER
    REINVEST --> GOV_ACTIVE
    
    HPN_ROLL --> TOTAL
    BOND_LADDER --> PASSIVE
    GOV_ACTIVE --> NETWORK
```

## 🏦 Scenario 4: The Institution

```mermaid
graph TB
    subgraph "Due Diligence Phase"
        DD["📋 Due Diligence<br/>• Audit reports ✓<br/>• On-chain verification ✓<br/>• Team background ✓"]
        LEGAL["⚖️ Legal Review<br/>• DAO structure<br/>• Token classification<br/>• Compliance check"]
        RISK["📊 Risk Assessment<br/>• Smart contract risk<br/>• Economic model<br/>• Liquidity analysis"]
    end
    
    subgraph "Allocation Strategy"
        COMMITTEE["🏛️ Investment Committee<br/>Approves $10M allocation"]
        
        STRUCTURE["📁 Structure Position<br/>• $7M → HPN 12-month<br/>• $2M → POL via LP Bonds<br/>• $1M → HVN governance"]
    end
    
    subgraph "Execution"
        CUSTODY["🔐 Custody Setup<br/>Multi-sig controls"]
        DEPLOY["💼 Deploy Capital<br/>Staged over 1 week"]
        MONITOR["📡 Monitor 24/7<br/>Real-time dashboards"]
    end
    
    subgraph "Reporting"
        MONTHLY["📊 Monthly Reports<br/>• Yield: 9.5% APY<br/>• Risk: Low<br/>• Liquidity: High"]
        BOARD["👔 Board Updates<br/>Outperforming TradFi"]
        INCREASE["📈 Increase Allocation<br/>Additional $20M approved"]
    end
    
    DD --> LEGAL
    LEGAL --> RISK
    RISK --> COMMITTEE
    COMMITTEE --> STRUCTURE
    STRUCTURE --> CUSTODY
    CUSTODY --> DEPLOY
    DEPLOY --> MONITOR
    MONITOR --> MONTHLY
    MONTHLY --> BOARD
    BOARD --> INCREASE
```

## 🎯 Scenario 5: The Proximity Maximizer

```mermaid
sequenceDiagram
    participant Trader as HFT Trader
    participant Blackhaven
    participant HVN as HVN Governance
    participant Proximity as Proximity Markets
    participant MEV as MEV Opportunities
    
    Note over Trader: Former Citadel quant
    Trader->>Blackhaven: Research 10ms advantage
    Note over Trader: "Holy shit, real HFT on-chain"
    
    Trader->>HVN: Buy 100k HVN tokens
    Trader->>HVN: Stake all to sHVN
    Note over Trader: "Need governance power"
    
    Trader->>HVN: Propose Proximity Strategy
    HVN->>HVN: Community votes YES
    
    loop Each Trading Window
        HVN->>Proximity: Allocate MEGA to slots
        Proximity->>Yield: Generate opportunities
        Yield-->>Blackhaven: Yield to treasury
        Blackhaven-->>Trader: sHVN rewards share
    end
    
    Note over Trader: Making bank while<br/>benefiting all holders
```

## 🌍 Scenario 6: The Global Macro Investor

```mermaid
graph LR
    subgraph "Macro Thesis"
        THESIS["🌍 Thesis<br/>• Crypto adoption accelerating<br/>• DeFi > TradFi yields<br/>• ETH ecosystem winning"]
    end
    
    subgraph "Blackhaven Fit"
        FIT1["✓ MegaETH = Next Gen"]
        FIT2["✓ Real yield model"]
        FIT3["✓ Downside protection"]
        FIT4["✓ Governance influence"]
    end
    
    subgraph "Portfolio Construction"
        ALLOC["Portfolio<br/>• 5% Crypto allocation<br/>• 40% of crypto → Blackhaven<br/>• = 2% total portfolio"]
        
        MIX["Blackhaven Mix<br/>• 60% HPN (safety)<br/>• 30% HVN (upside)<br/>• 10% LP Bonds (yield)"]
    end
    
    subgraph "Risk Management"
        HEDGE["Hedges<br/>• HPN = principal protection<br/>• HVN BLV = downside limit<br/>• Geographic diversification"]
    end
    
    THESIS --> FIT1
    THESIS --> FIT2
    THESIS --> FIT3
    THESIS --> FIT4
    
    FIT1 --> ALLOC
    FIT2 --> ALLOC
    FIT3 --> ALLOC
    FIT4 --> ALLOC
    
    ALLOC --> MIX
    MIX --> HEDGE
```

## 🚀 Scenario 7: The MegaMafia Builder

```mermaid
journey
    title MegaMafia Builder - Integrating Blackhaven
    
    section Building Phase
      Develops on MegaETH: 5: Builder
      Needs treasury for protocol: 4: Builder
      Discovers Blackhaven: 5: Builder
    
    section Integration
      Integrates RBT as collateral: 5: Builder
      Accepts HPN NFTs: 5: Builder
      Builds on 10ms speed: 5: Builder
      Leverages POL liquidity: 5: Builder
    
    section Growth
      Protocol TVL grows: 5: Builder
      Earns MEGA points: 5: Builder
      Treasury compounds: 5: Builder
      Joins Blackhaven governance: 5: Builder
    
    section Success
      Protocol + Blackhaven synergy: 5: Builder
      Featured in MegaMafia: 5: Builder
      Attracts more users: 5: Builder
      Everyone wins: 5: Builder, User
```

## 📊 Comparative User Outcomes

```mermaid
graph TD
    subgraph "Traditional DeFi User"
        OLD_DEPOSIT["Deposit $10k"]
        OLD_FARM["Farm governance tokens"]
        OLD_DUMP["Tokens dump -80%"]
        OLD_LOSS["End with $6k"]
        OLD_SAD["😢 Rekt"]
        
        OLD_DEPOSIT --> OLD_FARM
        OLD_FARM --> OLD_DUMP
        OLD_DUMP --> OLD_LOSS
        OLD_LOSS --> OLD_SAD
    end
    
    subgraph "Blackhaven User"
        NEW_DEPOSIT["Deposit $10k"]
        NEW_CHOICE["Choose strategy"]
        NEW_EARN["Earn real yield"]
        NEW_COMPOUND["Compound returns"]
        NEW_HAPPY["😊 Making it"]
        
        NEW_DEPOSIT --> NEW_CHOICE
        NEW_CHOICE --> NEW_EARN
        NEW_EARN --> NEW_COMPOUND
        NEW_COMPOUND --> NEW_HAPPY
    end
    
    subgraph "Results After 1 Year"
        OLD_RESULT["Traditional: -40%<br/>Farmed and dumped"]
        NEW_RESULT["Blackhaven: +25%<br/>Sustainable growth"]
    end
    
    OLD_SAD --> OLD_RESULT
    NEW_HAPPY --> NEW_RESULT
    
    style OLD_SAD fill:#f44336
    style NEW_HAPPY fill:#4caf50
```

## 🎭 The Personas Summary

| Persona | Entry Strategy | Risk Profile | Expected Return | Time Horizon |
|---------|---------------|--------------|-----------------|--------------|
| DeFi Veteran | HVN + sHVN + LP Bonds | High | 100%+ APY | Long-term |
| MegaETH Gamer | HPN + RBT utility | Low | 10-15% APY | Flexible |
| Yield Farmer | Diversified mix | Medium | 25-40% APY | 6-12 months |
| Institution | HPN heavy + governance | Low | 9-12% APY | Long-term |
| HFT Trader | HVN for proximity control | High | Variable/High | Active |
| Macro Investor | Balanced allocation | Medium | 20-30% APY | 2-5 years |
| MegaMafia Builder | Integration + treasury | Medium | Ecosystem growth | Permanent |

---

## 🎯 Key Takeaways

1. **Multiple Valid Strategies** - From safe HPN to aggressive HVN governance plays
2. **Real Yield Focus** - No ponzi mechanics, actual revenue generation
3. **MegaETH Native Benefits** - 10ms execution enables strategies impossible elsewhere
4. **Community Aligned** - Everyone benefits from protocol growth
5. **Risk Spectrum Coverage** - Products for every risk tolerance

---

*Your strategy, your timeline, your success. Welcome to Blackhaven.*
