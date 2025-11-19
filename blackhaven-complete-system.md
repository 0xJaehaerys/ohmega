# Blackhaven Complete System Diagram

## Full Protocol Architecture - Olympus DAO Style

```mermaid
graph TB
    subgraph "TOP - RBT LOCKING"
        RBT_TOP[RBT]
        LOCK_BTN[LOCK]
        UNLOCK_BTN[UNLOCK]
        ARBT_LOCKED[aRBT<br/>━━━━━━━━━━━<br/>aRBT = RBT × T_lock / T_max<br/>T_max = 2 years<br/>━━━━━━━━━━━<br/>Non-transferable<br/>Earns protocol fees<br/>Revenue sharing]
        
        RBT_TOP -->|choose lock duration<br/>0 months to 2 years| LOCK_BTN
        LOCK_BTN -->|receive aRBT<br/>start earning fees| ARBT_LOCKED
        ARBT_LOCKED -->|unlock after period ends<br/>wait for lock expiry| UNLOCK_BTN
        UNLOCK_BTN -->|get locked RBT back| RBT_TOP
    end
    
    subgraph "LEFT TOP - HPN"
        USER_HPN[User<br/>USDM/USDMy]
        HPN[HPN<br/>━━━━━━━━━━━<br/>ERC-721 NFT<br/>Principal Protected<br/>Earns Yield<br/>Earns MEGA Points]
        
        USER_HPN -->|deposit stablecoins<br/>select lock period| HPN
    end
    
    subgraph "LEFT BOTTOM - BONDS"
        USER_BONDS[User<br/>USDM or MEGA]
        
        BONDS[Fixed-Term Bonds<br/>━━━━━━━━━━━<br/>Discounted RBT<br/>Linear vesting<br/>Daily unlock]
        
        USER_BONDS -->|deposit for<br/>discounted RBT| BONDS
    end
    
    subgraph "CENTER - TREASURY"
        TREASURY[TREASURY<br/>═══════════════════<br/>USDM Reserve → DeFi<br/>MEGA Reserve → Sequencer<br/>POL: RBT-USDM Pool<br/>═══════════════════<br/>Backs all RBT<br/>proportionally]
    end
    
    subgraph "RIGHT - LP BONDS"
        USER_LP[User<br/>RBT-USDM LP token]
        
        LP_BONDS[LP Bond Aligned<br/>━━━━━━━━━━━<br/>LP locked FOREVER<br/>Becomes POL<br/>Extra rewards]
        
        USER_LP -->|deposit LP token<br/>for discounted RBT| LP_BONDS
    end
    
    subgraph "BOTTOM LEFT - USER DEPOSITS TO TREASURY"
        HPN_DEPOSIT[HPN Deposits<br/>━━━━━━━━━━━<br/>USDM/USDMy]
        BOND_DEPOSIT[Bond Deposits<br/>━━━━━━━━━━━<br/>USDM/MEGA]
        LP_DEPOSIT[LP Bond<br/>━━━━━━━━━━━<br/>RBT-USDM LP]
    end
    
    subgraph "BOTTOM CENTER - PROTOCOL REVENUE"
        YIELD_INCOME[MegaETH DeFi<br/>━━━━━━━━━━━<br/>Lending yields<br/>Strategy returns]
        POL_FEES[POL Trading Fees<br/>━━━━━━━━━━━<br/>RBT-USDM Pool<br/>LP locked forever<br/>Swap fees]
        MEGA_REWARDS[MEGA System<br/>━━━━━━━━━━━<br/>MEGA Points from TVL<br/>Sequencer staking<br/>Proximity Markets]
        PCB[Premium Capture<br/>Band<br/>━━━━━━━━━━━<br/>Coming Soon<br/>RBT price premiums]
    end
    
    subgraph "BOTTOM RIGHT - FEES & PENALTIES"
        EXIT_FEES[Exit Fees<br/>━━━━━━━━━━━<br/>HPN early: 2-3 percent<br/>Bonds early: 3.3 percent]
        FORFEIT[Forfeited Rewards<br/>━━━━━━━━━━━<br/>Early exits<br/>Lose all rewards<br/>Goes to treasury]
    end
    
    subgraph "RIGHT BOTTOM - OUTPUTS"
        RBT_OUT[RBT Token<br/>━━━━━━━━━━━<br/>Reserve-Backed<br/>Tradeable<br/>Used in MegaETH DeFi]
        
        USER_GETS[Users receive RBT<br/>━━━━━━━━━━━<br/>Bonds: daily unlock<br/>HPN: instant convert<br/>At maturity: full amount]
        
        RBT_OUT -->|users claim| USER_GETS
    end
    
    HPN -->|forwards deposits| HPN_DEPOSIT
    BONDS -->|forwards deposits| BOND_DEPOSIT
    LP_BONDS -->|forwards LP<br/>locked forever| LP_DEPOSIT
    
    HPN_DEPOSIT -->|USDM/USDMy| TREASURY
    BOND_DEPOSIT -->|USDM/MEGA| TREASURY
    LP_DEPOSIT -->|RBT-USDM LP<br/>permanent POL| TREASURY
    
    YIELD_INCOME -->|lending returns| TREASURY
    POL_FEES -->|swap fees from<br/>RBT-USDM trades| TREASURY
    MEGA_REWARDS -->|converted MEGA| TREASURY
    EXIT_FEES -->|penalty fees| TREASURY
    FORFEIT -->|forfeited yield<br/>and points| TREASURY
    PCB -.->|future revenue<br/>price premiums| TREASURY
    
    TREASURY -->|mints proportionally<br/>maintains reserve ratio| RBT_OUT
    
    TREASURY -.->|deploys USDM| YIELD_INCOME
    TREASURY -.->|deploys MEGA| MEGA_REWARDS
    
    ARBT_LOCKED -.->|earns protocol fees<br/>revenue sharing| TREASURY
```

---

## HPN (Haven Protected Notes) - Complete Flow

```mermaid
graph LR
    subgraph "USER ENTRY"
        USER[User<br/>USDM or USDMy]
    end
    
    subgraph "HPN NFT"
        NFT[HPN<br/>━━━━━━━━━━━━━━━<br/>ERC-721 NFT<br/>Principal Protected<br/>━━━━━━━━━━━━━━━<br/>Deposit: USDM/USDMy<br/>Lock: User-selected duration<br/>━━━━━━━━━━━━━━━<br/>Tracks:<br/>• Principal amount<br/>• Lock end date<br/>• Accrued yield<br/>• Accrued MEGA Points<br/>━━━━━━━━━━━━━━━<br/>Not market-priced]
    end
    
    subgraph "TREASURY DEPLOYMENT"
        TREASURY[Treasury<br/>Deploys to<br/>MegaETH DeFi]
    end
    
    subgraph "EXIT OPTIONS"
        OPT_A[Option A: Maturity<br/>━━━━━━━━━━━━━━━<br/>Wait until lock ends<br/>7-day cooldown<br/>━━━━━━━━━━━━━━━<br/>Receive:<br/>✓ Full principal<br/>✓ ALL yield<br/>✓ ALL MEGA Points]
        
        OPT_B[Option B: Early Exit<br/>━━━━━━━━━━━━━━━<br/>Exit anytime<br/>Fee: 2-3 percent<br/>7-day cooldown<br/>━━━━━━━━━━━━━━━<br/>Receive:<br/>✓ Principal minus fee<br/>✗ Lose ALL rewards]
        
        OPT_C[Option C: Convert RBT<br/>━━━━━━━━━━━━━━━<br/>Anytime<br/>NO cooldown<br/>NFT burns<br/>━━━━━━━━━━━━━━━<br/>Receive:<br/>✓ RBT instantly<br/>✗ Lose ALL rewards]
    end
    
    subgraph "USER RECEIVES"
        USER_OUT[User Receives]
    end
    
    USER -->|deposit USDM/USDMy<br/>select lock period| NFT
    NFT -->|mint NFT to user| USER
    NFT -->|forward deposits| TREASURY
    TREASURY -->|deploy to strategies<br/>earn yield + MEGA Points| NFT
    
    NFT --> OPT_A
    NFT --> OPT_B
    NFT --> OPT_C
    
    OPT_A -->|after 7 days| USER_OUT
    OPT_B -->|after 7 days| USER_OUT
    OPT_B -.->|forfeited rewards<br/>to treasury| TREASURY
    OPT_C -->|instant| USER_OUT
    OPT_C -.->|forfeited rewards<br/>to treasury| TREASURY
```

---

## Fixed-Term Bonds - Complete Flow

```mermaid
graph TB
    subgraph "USER ENTRY - REGULAR BONDS"
        USER_REG[User<br/>USDM or MEGA]
    end
    
    subgraph "REGULAR BONDS"
        BOND[Fixed-Term Bond<br/>━━━━━━━━━━━━━━━<br/>Quote: USDM or MEGA<br/>━━━━━━━━━━━━━━━<br/>Payout: RBT at DISCOUNT<br/>Below market price<br/>More appealing than market buy<br/>━━━━━━━━━━━━━━━<br/>Additional rewards<br/>━━━━━━━━━━━━━━━<br/>Vesting: User-selected period<br/>Linear daily unlock<br/>━━━━━━━━━━━━━━━<br/>Maturity: Full RBT + rewards<br/>Early exit: 3.3 percent fee]
    end
    
    subgraph "USER ENTRY - LP BONDS"
        USER_LP_START[User creates<br/>RBT-USDM LP]
        LP_TOKEN[LP Token]
    end
    
    subgraph "LP BONDS ALIGNED"
        LP_BOND[LP Bond Aligned<br/>━━━━━━━━━━━━━━━<br/>Deposit: RBT-USDM LP token<br/>LP locked FOREVER<br/>Becomes POL<br/>━━━━━━━━━━━━━━━<br/>Payout: RBT at DISCOUNT<br/>Below market price<br/>━━━━━━━━━━━━━━━<br/>Bonus: Extra MEGA rewards<br/>Boosts MEGA rewards inflow<br/>━━━━━━━━━━━━━━━<br/>Vesting: User-selected period<br/>Linear daily unlock<br/>━━━━━━━━━━━━━━━<br/>LP stays locked forever<br/>Even after full RBT claim]
    end
    
    subgraph "TREASURY VAULT"
        TREASURY_BONDS[Treasury<br/>Receives deposits<br/>Backs RBT]
    end
    
    subgraph "POL SYSTEM"
        POL[POL Vault<br/>━━━━━━━━━━━━━━━<br/>RBT-USDM Pool<br/>LP locked FOREVER<br/>━━━━━━━━━━━━━━━<br/>Generates trading fees<br/>No sell pressure]
    end
    
    subgraph "VESTING POSITIONS"
        VESTING_REG[Regular Bond Vesting<br/>━━━━━━━━━━━━━━━<br/>Total RBT owed<br/>Vested: claimable now<br/>Unvested: locked]
        
        VESTING_LP[LP Bond Vesting<br/>━━━━━━━━━━━━━━━<br/>Total RBT owed<br/>Vested: claimable now<br/>Unvested: locked]
    end
    
    subgraph "EXIT OPTIONS BONDS"
        MAT_BOND[Hold to Maturity<br/>━━━━━━━━━━━━━━━<br/>Wait full vesting period<br/>NO cooldown<br/>━━━━━━━━━━━━━━━<br/>Claim all RBT<br/>+ All rewards]
        
        EARLY_BOND[Early Exit<br/>━━━━━━━━━━━━━━━<br/>Exit anytime<br/>Fee: 3.3 percent of bond<br/>NO cooldown<br/>━━━━━━━━━━━━━━━<br/>Receive:<br/>✓ Vested RBT + rewards<br/>✓ After paying fee<br/>━━━━━━━━━━━━━━━<br/>Forfeit to treasury:<br/>✗ Unvested RBT<br/>✗ Unvested rewards<br/>locked portion only]
    end
    
    subgraph "USER RECEIVES BONDS"
        USER_OUT_BOND[User Receives<br/>RBT]
    end
    
    USER_REG -->|deposit USDM or MEGA<br/>select vesting period| BOND
    BOND -->|forward to reserves| TREASURY_BONDS
    BOND -->|create vesting position<br/>linear daily unlock| VESTING_REG
    
    USER_LP_START -->|provide liquidity| LP_TOKEN
    LP_TOKEN -->|deposit LP token<br/>select vesting period| LP_BOND
    LP_BOND -->|LP locked FOREVER| POL
    LP_BOND -->|create vesting position<br/>linear daily unlock| VESTING_LP
    
    POL -->|trading fees| TREASURY_BONDS
    
    VESTING_REG --> MAT_BOND
    VESTING_REG --> EARLY_BOND
    VESTING_LP --> MAT_BOND
    VESTING_LP --> EARLY_BOND
    
    MAT_BOND -->|full RBT| USER_OUT_BOND
    EARLY_BOND -->|vested RBT + vested rewards<br/>minus 3.3 percent fee| USER_OUT_BOND
    EARLY_BOND -.->|unvested RBT +<br/>unvested rewards<br/>forfeited to treasury| TREASURY_BONDS
```

---

## HVN Governance - How Users Control Treasury

```mermaid
graph TB
    subgraph "TOP - HVN HOLDERS"
        USER1[User 1: 1000 HVN]
        USER2[User 2: 5000 HVN]
        USER3[User 3: 500 HVN]
        
        SHVN_OPTION[Can stake → sHVN<br/>Earn treasury rewards<br/>NO governance rights]
    end
    
    subgraph "MIDDLE - GOVERNANCE PROPOSALS"
        BIP1[BIP-1: Treasury Strategy<br/>━━━━━━━━━━━━━━━<br/>Adjust DeFi allocations<br/>Set reserve ratios<br/>Change deployment]
        
        BIP2[BIP-2: Whitelist DeFi<br/>━━━━━━━━━━━━━━━<br/>Add MegaETH protocol<br/>Remove risky strategy<br/>Update limits]
        
        BIP3[BIP-3: MEGA Allocation<br/>━━━━━━━━━━━━━━━<br/>Proximity Market bidding<br/>Sequencer participation<br/>Yield optimization]
        
        BIP4[BIP-4: sHVN Rewards<br/>━━━━━━━━━━━━━━━<br/>Reward distribution rates<br/>Emission schedules<br/>Treasury share]
    end
    
    subgraph "BOTTOM - TREASURY CONTROLLED"
        TREASURY[TREASURY<br/>═══════════════════<br/>USDM Reserve<br/>MEGA Reserve<br/>POL RBT-USDM<br/>═══════════════════<br/>Controlled by governance]
        
        CONTROL1[Controls DeFi deployment<br/>Whitelisted strategies<br/>Risk parameters]
        
        CONTROL2[Controls MEGA usage<br/>Proximity Markets<br/>Sequencer staking]
        
        CONTROL3[Controls sHVN rewards<br/>Distribution from treasury<br/>Reward pool config]
        
        TREASURY --> CONTROL1
        TREASURY --> CONTROL2
        TREASURY --> CONTROL3
    end
    
    USER1 -.->|1000 votes| BIP1
    USER1 -.->|1000 votes| BIP2
    USER1 -.->|1000 votes| BIP3
    USER1 -.->|1000 votes| BIP4
    
    USER2 -.->|5000 votes| BIP1
    USER2 -.->|5000 votes| BIP2
    USER2 -.->|5000 votes| BIP3
    USER2 -.->|5000 votes| BIP4
    
    USER3 -.->|500 votes| BIP1
    USER3 -.->|500 votes| BIP2
    USER3 -.->|500 votes| BIP3
    USER3 -.->|500 votes| BIP4
    
    BIP1 -->|if approved<br/>governs| CONTROL1
    BIP2 -->|if approved<br/>governs| CONTROL1
    BIP3 -->|if approved<br/>governs| CONTROL2
    BIP4 -->|if approved<br/>governs| CONTROL3
    
    CONTROL3 -->|rewards flow| SHVN_OPTION
```

---

## BLV (Baseline Value) - HVN Price Floor

```mermaid
graph TB
    subgraph "HVN ON BASELINE MARKETS"
        HVN_LAUNCH[HVN Launch<br/>on Baseline Markets]
        INITIAL_BLV[Initial BLV Set<br/>BLV = Locked Reserve / Supply]
        RESERVES[Locked Reserves<br/>Back every HVN token]
    end
    
    subgraph "BLV MECHANISM"
        BLV_FLOOR[BLV Price Floor<br/>━━━━━━━━━━━━━━━<br/>Guaranteed minimum price<br/>Can NEVER go down<br/>Only rises<br/>━━━━━━━━━━━━━━━<br/>Formula: BLV = Reserve / Supply<br/>━━━━━━━━━━━━━━━<br/>Every trade increases reserve<br/>Buyback & burn when price = BLV]
        
        TRADING[Trading Activity<br/>Increases reserves<br/>BLV rises]
        
        BUYBACK[Price hits BLV<br/>━━━━━━━━━━━━━━━<br/>Protocol buys HVN<br/>Burns tokens<br/>Supply decreases<br/>BLV increases]
    end
    
    subgraph "USER BENEFITS"
        PROTECTION[Price Protection<br/>━━━━━━━━━━━━━━━<br/>Know worst case<br/>Price can't go below BLV<br/>Clear downside]
        
        BORROW[Safe Borrowing<br/>━━━━━━━━━━━━━━━<br/>Borrow up to BLV value<br/>NO liquidation risk<br/>BLV only goes up]
        
        LEVERAGE[Capital-Efficient<br/>━━━━━━━━━━━━━━━<br/>Leverage without liquidation<br/>Scales with demand<br/>Autonomous liquidity]
    end
    
    subgraph "LIQUIDITY SYSTEM"
        AUTO_LIQ[Autonomous Liquidity<br/>━━━━━━━━━━━━━━━<br/>System-managed<br/>No market makers needed<br/>Can't be pulled<br/>Permanent protection]
    end
    
    HVN_LAUNCH --> INITIAL_BLV
    INITIAL_BLV --> RESERVES
    RESERVES --> BLV_FLOOR
    
    BLV_FLOOR --> TRADING
    TRADING -->|reserve grows| BLV_FLOOR
    
    BLV_FLOOR --> BUYBACK
    BUYBACK -->|BLV increases| BLV_FLOOR
    
    BLV_FLOOR --> PROTECTION
    BLV_FLOOR --> BORROW
    BLV_FLOOR --> LEVERAGE
    
    RESERVES --> AUTO_LIQ
    AUTO_LIQ --> PROTECTION
```

---

## MegaETH Integration - Proximity Markets & Sequencer

```mermaid
graph TB
    subgraph "BLACKHAVEN TREASURY"
        TREASURY_MEGA[Treasury<br/>━━━━━━━━━━━━━━━<br/>MEGA Reserve<br/>100 percent to Sequencer<br/>━━━━━━━━━━━━━━━<br/>Controlled by HVN governance]
        
        MEGA_SOURCES[MEGA Sources:<br/>• Bond deposits<br/>• MEGA Points from TVL<br/>• Forfeits & penalties]
    end
    
    subgraph "HVN GOVERNANCE CONTROL"
        HVN_GOV[HVN Governance<br/>━━━━━━━━━━━━━━━<br/>Decides MEGA allocation<br/>Controls bidding strategy<br/>Optimizes yield]
        
        VOTE_PROXIMITY[Vote on:<br/>• Proximity slot selection<br/>• Bidding amounts<br/>• Optimization strategy]
    end
    
    subgraph "MEGAETH SEQUENCER"
        SEQUENCER[MegaETH Sequencer<br/>━━━━━━━━━━━━━━━<br/>100 percent MEGA staked<br/>━━━━━━━━━━━━━━━<br/>10ms blocks<br/>100k TPS<br/>Real-time execution]
        
        SEQ_REWARDS[Sequencer Rewards<br/>Staking returns]
    end
    
    subgraph "PROXIMITY MARKETS"
        PROXIMITY[Proximity Markets<br/>━━━━━━━━━━━━━━━<br/>Sequencer-adjacent<br/>floorspace bidding<br/>━━━━━━━━━━━━━━━<br/>Better execution speed<br/>Liquidity efficiency<br/>Priority access]
        
        PROXIMITY_BENEFITS[Benefits:<br/>• Improved execution<br/>• Yield opportunities<br/>• Network utility<br/>• Performance gains]
    end
    
    subgraph "VALUE BACK TO TREASURY"
        RETURNS[Returns to Treasury:<br/>━━━━━━━━━━━━━━━<br/>• Sequencer staking rewards<br/>• Yield from proximity<br/>• Network performance gains<br/>━━━━━━━━━━━━━━━<br/>Strengthens RBT backing]
    end
    
    MEGA_SOURCES --> TREASURY_MEGA
    
    HVN_GOV -->|controls allocation| VOTE_PROXIMITY
    VOTE_PROXIMITY -->|decides usage| TREASURY_MEGA
    
    TREASURY_MEGA -->|stakes 100 percent MEGA| SEQUENCER
    TREASURY_MEGA -->|allocates per governance| PROXIMITY
    
    SEQUENCER --> SEQ_REWARDS
    PROXIMITY --> PROXIMITY_BENEFITS
    
    SEQ_REWARDS -->|rewards back| RETURNS
    PROXIMITY_BENEFITS -->|yield & utility| RETURNS
    
    RETURNS --> TREASURY_MEGA
```

---

*Blackhaven Protocol - Complete System with User Flows and Integrations*

