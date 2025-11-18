# Blackhaven Protocol - User Flows

## 1. HVN Token Flow

```mermaid
sequenceDiagram
    actor User
    participant Baseline as Baseline Markets
    participant Wallet as User's Wallet
    participant Staking as Staking Contract
    participant Governance as Governance Portal
    
    Note over User,Baseline: Step 1: Acquire HVN
    User->>Baseline: 1. Buy HVN tokens
    Baseline-->>User: HVN received + BLV protection
    Note over User: You are protected by BLV floor<br/>Price cannot go below BLV
    
    Note over User,Staking: Step 2: Stake for sHVN (Optional)
    User->>Staking: 2. Stake HVN
    Staking-->>User: Receive sHVN (non-transferable)
    Note over User: Earn treasury growth rewards<br/>Configured via governance
    
    Note over User,Governance: Step 3: Participate in Governance
    User->>Governance: 3. Vote with HVN (1 HVN = 1 vote)
    Note over Governance: Vote on:<br/>- Treasury strategy<br/>- DeFi whitelisting<br/>- MEGA allocation<br/>- Reward rates
    
    Note over User: Step 4: Additional Options
    User->>Baseline: 4a. Borrow against HVN (up to BLV)
    Note over Baseline: No liquidation risk<br/>BLV only goes up
    User->>Wallet: 4b. Hold and watch BLV grow
```

## 2. Haven Protected Notes (HPN) Flow

```mermaid
sequenceDiagram
    actor User
    participant HPN as HPN dApp
    participant NFT as Your HPN NFT
    participant Treasury as Treasury
    participant Dashboard as Your Dashboard
    
    Note over User,HPN: Step 1: Deposit
    User->>HPN: 1. Connect wallet with USDM/USDMy
    User->>HPN: 2. Select lock period (custom duration)
    User->>HPN: 3. Enter deposit amount
    User->>HPN: 4. Click "Deposit"
    HPN-->>User: Mint HPN NFT to your wallet
    HPN->>Treasury: Forward USDM/USDMy to treasury
    Note over User: NFT represents your position<br/>with principal + terms
    
    Note over User,Dashboard: Step 2: Track Earnings
    loop Daily
        Treasury->>NFT: Accrue yield
        Treasury->>NFT: Accrue MEGA Points (amplified)
    end
    User->>Dashboard: Check dashboard anytime
    Dashboard-->>User: Shows: Principal, Accrued Rewards, Days Left
    
    Note over User,Dashboard: Step 3: Exit (Choose 1 of 3 options)
    
    alt Option A: Hold to Maturity
        User->>HPN: Click "Claim at Maturity"
        HPN->>HPN: Initiate 7-day cooldown
        Note over User: Wait 7 days
        User->>HPN: Click "Claim" after cooldown
        HPN-->>User: Principal + ALL Rewards
    else Option B: Early Exit
        User->>HPN: Click "Early Exit"
        Note over HPN: Charge 2-3% fee<br/>Forfeit ALL rewards
        HPN->>HPN: Initiate 7-day cooldown
        Note over User: Wait 7 days
        User->>HPN: Click "Claim" after cooldown
        HPN-->>User: Principal - Fee
    else Option C: Convert to RBT
        User->>HPN: Click "Convert to RBT"
        Note over HPN: Burn NFT<br/>Forfeit ALL rewards
        HPN-->>User: RBT INSTANTLY (no cooldown)
    end
```

## 3. Regular Bonds Flow (USDM/MEGA)

```mermaid
sequenceDiagram
    actor User
    participant Bonds as Bonds dApp
    participant Treasury as Treasury
    participant Vesting as Vesting Contract
    participant Dashboard as Your Dashboard
    
    Note over User,Bonds: Step 1: View Bond Options
    User->>Bonds: 1. Visit Bonds page
    Bonds-->>User: Show available bonds:<br/>- USDM Bond<br/>- MEGA Bond
    Bonds-->>User: Display for each:<br/>• Discount % vs market<br/>• Vesting periods available<br/>• MEGA rewards multiplier
    
    Note over User,Bonds: Step 2: Select & Purchase
    User->>Bonds: 2. Select bond type (USDM or MEGA)
    User->>Bonds: 3. Choose vesting period<br/>(1 week, 1 month, 3 months, etc.)
    Bonds-->>User: Calculate: You will receive X RBT<br/>Vesting: Y RBT per day
    User->>Bonds: 4. Approve token spending
    User->>Bonds: 5. Click "Purchase Bond"
    Bonds->>Treasury: Send USDM/MEGA to treasury
    Bonds->>Vesting: Create vesting position
    Note over User: Bond purchase complete<br/>Vesting starts immediately
    
    Note over User,Dashboard: Step 3: Track & Claim
    loop Daily
        Vesting->>Vesting: Unlock RBT portion
    end
    User->>Dashboard: Check vesting dashboard
    Dashboard-->>User: Shows:<br/>• Total RBT bonded<br/>• Vested (claimable now)<br/>• Unvested (locked)<br/>• Days remaining
    User->>Vesting: Click "Claim Vested RBT" (anytime)
    Vesting-->>User: Transfer vested RBT to wallet
    
    Note over User,Vesting: Optional: Early Exit
    User->>Vesting: Click "Early Exit"
    Note over Vesting: Charge 3.3% fee<br/>Forfeit ALL unvested RBT
    Vesting-->>User: Return vested RBT - 3.3% fee
```

## 4. LP Bonds Flow (Aligned Bonds)

```mermaid
sequenceDiagram
    actor User
    participant DEX as RBT-USDM DEX Pool
    participant LPBonds as LP Bonds dApp
    participant Treasury as Treasury (POL)
    participant Vesting as Vesting Contract
    participant Dashboard as Your Dashboard
    
    Note over User,DEX: Step 1: Create LP Token
    User->>DEX: 1. Go to RBT-USDM pool
    User->>DEX: 2. Add liquidity:<br/>- Deposit RBT<br/>- Deposit USDM
    DEX-->>User: Receive LP token
    Note over User: You now have RBT-USDM LP token
    
    Note over User,LPBonds: Step 2: Bond LP Token
    User->>LPBonds: 3. Visit LP Bonds page
    LPBonds-->>User: Show available terms:<br/>• Discount %<br/>• Vesting periods<br/>• MEGA rewards
    User->>LPBonds: 4. Select vesting period
    LPBonds-->>User: Calculate: You will receive X RBT<br/>Your LP locked FOREVER in treasury
    User->>LPBonds: 5. Approve LP token
    User->>LPBonds: 6. Click "Bond LP"
    LPBonds->>Treasury: Lock LP token PERMANENTLY (POL)
    LPBonds->>Vesting: Create vesting position
    Note over Treasury: LP generates trading fees<br/>0.3% per swap → treasury forever
    
    Note over User,Dashboard: Step 3: Track & Claim
    loop Daily
        Vesting->>Vesting: Unlock RBT portion
    end
    User->>Dashboard: Check vesting dashboard
    Dashboard-->>User: Shows:<br/>• Total RBT bonded<br/>• Vested (claimable now)<br/>• Unvested (locked)<br/>• Days remaining
    User->>Vesting: Click "Claim Vested RBT" (anytime)
    Vesting-->>User: Transfer vested RBT to wallet
    
    Note over User,Vesting: Optional: Early Exit
    User->>Vesting: Click "Early Exit"
    Note over Vesting: Charge 3.3% fee<br/>Forfeit ALL unvested RBT<br/>LP stays locked forever
    Vesting-->>User: Return vested RBT - 3.3% fee
```

## 5. RBT & aRBT Flow

```mermaid
sequenceDiagram
    actor User
    participant Wallet as Your Wallet
    participant aRBT as aRBT Contract
    participant Dashboard as Your Dashboard
    
    Note over User,Wallet: Step 1: Acquire RBT (3 ways)
    Note over User: Option 1: From HPN conversion
    Note over User: Option 2: From claiming vested bonds
    Note over User: Option 3: Buy on market
    User->>Wallet: RBT now in wallet
    Note over User: RBT is treasury-backed<br/>Can use in MegaETH DeFi
    
    Note over User,aRBT: Step 2: Lock RBT for aRBT (Optional)
    User->>aRBT: 1. Go to aRBT locking page
    User->>aRBT: 2. Enter RBT amount to lock
    User->>aRBT: 3. Select lock duration (up to 2 years)
    aRBT-->>User: Calculate aRBT you'll receive:<br/>aRBT = RBT × T_lock / T_max<br/>(T_max = 2 years)
    Note over User: Example:<br/>Lock 100 RBT for 1 year<br/>= 50 aRBT
    User->>aRBT: 4. Click "Lock RBT"
    aRBT-->>User: Receive aRBT (non-transferable)
    
    Note over User,Dashboard: Step 3: aRBT Benefits
    User->>Dashboard: View aRBT dashboard
    Dashboard-->>User: Shows:<br/>• aRBT balance<br/>• Protocol fees earned<br/>• Revenue share from treasury<br/>• Lock expiration date
    Note over User: Benefits:<br/>• Revenue sharing<br/>• Protocol fees<br/>• Enhanced ecosystem trust<br/>• RBT stability support
```

## 6. Complete User Dashboard

```mermaid
graph TB
    DASHBOARD[BLACKHAVEN DASHBOARD]
    
    subgraph "Portfolio Summary"
        TOTAL[Total Portfolio Value: $X]
        HVN_VALUE[HVN Holdings: $Y]
        HPN_VALUE[Active HPNs: $Z]
        BONDS_VALUE[Active Bonds: $W]
        RBT_VALUE[RBT Balance: $V]
    end
    
    subgraph "HVN & sHVN"
        HVN_BAL[HVN: 1000 tokens]
        SHVN_BAL[sHVN: 500 tokens]
        REWARDS[Claimable Rewards: 50 tokens]
        BLV_INFO[BLV Floor: $0.85]
        HVN_BTN1[Button: Stake HVN]
        HVN_BTN2[Button: Vote on Proposals]
        HVN_BTN3[Button: Claim Rewards]
    end
    
    subgraph "Haven Protected Notes"
        HPN_TITLE[Your HPNs: 2 active]
        HPN1[HPN #1234<br/>Principal: 1000 USDM<br/>Accrued: 50 USDM + 100 Points<br/>Days left: 15]
        HPN2[HPN #5678<br/>Principal: 5000 USDMy<br/>Accrued: 200 USDMy + 500 Points<br/>Matured - Ready to claim]
        HPN_BTN1[Button: Convert to RBT]
        HPN_BTN2[Button: Early Exit]
        HPN_BTN3[Button: Claim at Maturity]
    end
    
    subgraph "Bonds"
        BONDS_TITLE[Your Bonds: 3 active]
        BOND1[USDM Bond<br/>Total: 1000 RBT<br/>Vested: 400 RBT<br/>Unvested: 600 RBT<br/>Days left: 20]
        BOND2[LP Bond<br/>Total: 5000 RBT<br/>Vested: 2000 RBT<br/>Unvested: 3000 RBT<br/>Days left: 45]
        BONDS_BTN1[Button: Claim Vested RBT]
        BONDS_BTN2[Button: Early Exit]
    end
    
    subgraph "RBT & aRBT"
        RBT_BAL[RBT Balance: 500 tokens]
        ARBT_BAL[aRBT Balance: 250 tokens<br/>Locked until: Jan 2026<br/>Revenue earned: 25 tokens]
        RBT_BTN1[Button: Lock for aRBT]
        RBT_BTN2[Button: Use in DeFi]
    end
    
    subgraph "Quick Actions"
        ACTION1[+ New HPN Deposit]
        ACTION2[+ Purchase Bond]
        ACTION3[+ Create LP Bond]
    end
    
    DASHBOARD --> TOTAL
    TOTAL --> HVN_VALUE
    TOTAL --> HPN_VALUE
    TOTAL --> BONDS_VALUE
    TOTAL --> RBT_VALUE
    
    DASHBOARD --> HVN_BAL
    HVN_BAL --> SHVN_BAL
    HVN_BAL --> REWARDS
    HVN_BAL --> BLV_INFO
    HVN_BAL --> HVN_BTN1
    HVN_BAL --> HVN_BTN2
    HVN_BAL --> HVN_BTN3
    
    DASHBOARD --> HPN_TITLE
    HPN_TITLE --> HPN1
    HPN_TITLE --> HPN2
    HPN1 --> HPN_BTN1
    HPN2 --> HPN_BTN3
    
    DASHBOARD --> BONDS_TITLE
    BONDS_TITLE --> BOND1
    BONDS_TITLE --> BOND2
    BOND1 --> BONDS_BTN1
    BOND2 --> BONDS_BTN1
    
    DASHBOARD --> RBT_BAL
    RBT_BAL --> ARBT_BAL
    RBT_BAL --> RBT_BTN1
    RBT_BAL --> RBT_BTN2
    
    DASHBOARD --> ACTION1
    DASHBOARD --> ACTION2
    DASHBOARD --> ACTION3
```

## 7. Baseline Markets - HVN Launch Experience

```mermaid
sequenceDiagram
    actor User
    participant Baseline as Baseline Markets
    participant HVN as HVN Token
    participant BLV as BLV Mechanism
    
    Note over User,Baseline: Before Launch
    User->>User: 1. Prepare USDM/ETH in wallet
    User->>Baseline: 2. Visit Baseline Markets
    Baseline-->>User: Learn about BLV:<br/>• Guaranteed price floor<br/>• BLV only goes up<br/>• No liquidation borrowing
    Note over User: Wait for HVN launch announcement
    
    Note over User,Baseline: Launch Day
    User->>Baseline: 3. Navigate to HVN launch page
    Baseline-->>User: Display:<br/>• Current HVN price: $X<br/>• BLV floor: $Y<br/>• Your protection: Price can't go below $Y
    User->>Baseline: 4. Enter purchase amount
    Baseline-->>User: Calculate:<br/>• HVN you'll receive<br/>• Your BLV protection level
    User->>Baseline: 5. Click "Buy HVN"
    Baseline-->>User: HVN tokens received
    Note over User: You are now protected by BLV floor
    
    Note over User,BLV: Post-Purchase Tracking
    User->>Baseline: 6. Monitor HVN dashboard
    Baseline-->>User: Shows:<br/>• HVN balance<br/>• Current price<br/>• BLV floor (rising)<br/>• Borrowing capacity
    loop As Trading Increases
        BLV->>BLV: BLV floor rises
        Note over BLV: More trading = Higher BLV<br/>Your protection increases
    end
    
    Note over User,Baseline: Advanced Options
    User->>Baseline: 7a. Borrow against HVN
    Baseline-->>User: Borrow up to BLV value<br/>NO liquidation risk
    Note over User: If price drops to BLV,<br/>protocol buys & burns tokens<br/>→ BLV increases again
    
    User->>User: 7b. Stake HVN → Get sHVN
    User->>User: 7c. Vote on governance
    Note over User: All actions transparent<br/>on-chain
```

---

*User-focused sequence diagrams for Blackhaven Protocol frontend*

