# Blackhaven Protocol - Frontend User Flows

## 1. HVN Token - User Journey

```mermaid
graph TB
    subgraph "Acquiring HVN"
        BUY_HVN[Buy HVN<br/>on Baseline Markets]
        HVN_WALLET[HVN in Your Wallet]
        BLV_PROTECTED[Protected by BLV Floor<br/>Price can't go below BLV]
    end
    
    subgraph "What You Can Do"
        STAKE_ACTION[Stake HVN → Get sHVN]
        VOTE_ACTION[Vote on Governance<br/>Treasury strategy<br/>DeFi whitelisting<br/>MEGA allocation]
        HOLD_ACTION[Hold & Wait<br/>BLV floor rises]
        BORROW_ACTION[Borrow Against HVN<br/>Up to BLV value<br/>No liquidation]
    end
    
    subgraph "Staking Benefits"
        SHVN_BALANCE[sHVN Balance]
        EARN_REWARDS[Earn Treasury Rewards<br/>Configured via governance]
        LONG_TERM[Long-term alignment]
    end
    
    BUY_HVN --> HVN_WALLET
    HVN_WALLET --> BLV_PROTECTED
    
    HVN_WALLET --> STAKE_ACTION
    HVN_WALLET --> VOTE_ACTION
    HVN_WALLET --> HOLD_ACTION
    HVN_WALLET --> BORROW_ACTION
    
    STAKE_ACTION --> SHVN_BALANCE
    SHVN_BALANCE --> EARN_REWARDS
    EARN_REWARDS --> LONG_TERM
```

## 2. Haven Protected Notes (HPN) - User Flow

```mermaid
graph TB
    subgraph "Step 1: Deposit"
        USER_START[You Have USDM or USDMy]
        CHOOSE_TERM[Choose Lock Period]
        DEPOSIT_BTN[Click Deposit]
        RECEIVE_NFT[Receive HPN NFT<br/>Shows your position]
    end
    
    subgraph "Step 2: Earning"
        ACCRUING[Your Position Accrues:<br/>Yield<br/>MEGA Points<br/>Amplified rewards]
        DASHBOARD[Dashboard Shows:<br/>Principal amount<br/>Accrued rewards<br/>Days remaining]
    end
    
    subgraph "Step 3: Exit Options"
        OPTION_A[Option A: Wait to Maturity<br/>Get: Principal + All Rewards<br/>Wait: 7 days after maturity]
        OPTION_B[Option B: Exit Early<br/>Get: Principal - 2-3% fee<br/>Lose: All rewards<br/>Wait: 7 days]
        OPTION_C[Option C: Convert to RBT<br/>Get: RBT instantly<br/>Lose: All rewards<br/>Wait: None]
    end
    
    USER_START --> CHOOSE_TERM
    CHOOSE_TERM --> DEPOSIT_BTN
    DEPOSIT_BTN --> RECEIVE_NFT
    
    RECEIVE_NFT --> ACCRUING
    ACCRUING --> DASHBOARD
    
    DASHBOARD --> OPTION_A
    DASHBOARD --> OPTION_B
    DASHBOARD --> OPTION_C
    
    OPTION_A -->|After 7 days| CLAIM_A[Claim Button<br/>Principal + Rewards]
    OPTION_B -->|After 7 days| CLAIM_B[Claim Button<br/>Principal - Fee]
    OPTION_C -->|Instant| CLAIM_C[RBT in Wallet]
```

## 3. Fixed-Term Bonds - User Flow

```mermaid
graph TB
    subgraph "Step 1: Choose Bond Type"
        START[Visit Bonds Page]
        REGULAR_OPTION[Regular Bond<br/>Pay with USDM or MEGA]
        LP_OPTION[LP Bond<br/>Pay with RBT-USDM LP]
    end
    
    subgraph "Step 2: Bond Details"
        SEE_DISCOUNT[See Discount %<br/>vs Market Price]
        SEE_TERM[Select Bond Term<br/>1 week, 1 month, 3 months, etc.]
        SEE_REWARDS[See Additional Rewards<br/>MEGA incentives]
    end
    
    subgraph "Step 3: Purchase"
        APPROVE_TOKEN[Approve Token]
        CLICK_BOND[Click Bond]
        VESTING_STARTS[Vesting Starts<br/>RBT unlocks daily]
    end
    
    subgraph "Step 4: Track & Claim"
        DASHBOARD_BONDS[Bonds Dashboard Shows:<br/>Total vested RBT<br/>Daily unlock rate<br/>Days remaining]
        CLAIM_VESTED[Claim Vested RBT<br/>Available anytime]
        EARLY_EXIT_BOND[Early Exit Option<br/>Pay 3.3% fee<br/>Lose unvested RBT]
    end
    
    START --> REGULAR_OPTION
    START --> LP_OPTION
    
    REGULAR_OPTION --> SEE_DISCOUNT
    LP_OPTION --> SEE_DISCOUNT
    
    SEE_DISCOUNT --> SEE_TERM
    SEE_TERM --> SEE_REWARDS
    
    SEE_REWARDS --> APPROVE_TOKEN
    APPROVE_TOKEN --> CLICK_BOND
    CLICK_BOND --> VESTING_STARTS
    
    VESTING_STARTS --> DASHBOARD_BONDS
    DASHBOARD_BONDS --> CLAIM_VESTED
    DASHBOARD_BONDS --> EARLY_EXIT_BOND
    
    CLAIM_VESTED --> RBT_WALLET[RBT in Your Wallet]
```

## 4. RBT & aRBT - User Journey

```mermaid
graph TB
    subgraph "Getting RBT"
        SOURCE_HPN[From HPN Conversion]
        SOURCE_BOND[From Bonds Vesting]
        SOURCE_MARKET[Buy on Market]
        RBT_BALANCE[RBT in Your Wallet]
    end
    
    subgraph "Using RBT"
        USE_DEFI[Use in MegaETH DeFi<br/>Lending, LP, etc.]
        HOLD_RBT[Hold RBT<br/>Treasury-backed]
        LOCK_RBT[Lock RBT<br/>Get aRBT benefits]
    end
    
    subgraph "Locking for aRBT"
        CHOOSE_DURATION[Choose Lock Duration<br/>Up to 2 years]
        SEE_ARBT[See aRBT Amount<br/>aRBT = RBT × Time / 2 years]
        CONFIRM_LOCK[Confirm Lock]
        ARBT_BENEFITS[Get aRBT Benefits:<br/>Revenue sharing<br/>Protocol fees<br/>Enhanced trust]
    end
    
    SOURCE_HPN --> RBT_BALANCE
    SOURCE_BOND --> RBT_BALANCE
    SOURCE_MARKET --> RBT_BALANCE
    
    RBT_BALANCE --> USE_DEFI
    RBT_BALANCE --> HOLD_RBT
    RBT_BALANCE --> LOCK_RBT
    
    LOCK_RBT --> CHOOSE_DURATION
    CHOOSE_DURATION --> SEE_ARBT
    SEE_ARBT --> CONFIRM_LOCK
    CONFIRM_LOCK --> ARBT_BENEFITS
```

## 5. LP Bonds - Detailed User Flow

```mermaid
graph TB
    subgraph "Step 1: Create LP"
        HAVE_RBT[You Have RBT]
        HAVE_USDM[You Have USDM]
        ADD_LIQUIDITY[Add Liquidity<br/>to RBT-USDM Pool]
        GET_LP[Receive LP Token]
    end
    
    subgraph "Step 2: Bond LP Token"
        LP_BONDS_PAGE[Go to LP Bonds Page]
        SEE_TERMS[See Available Terms:<br/>Discount %<br/>Vesting period<br/>MEGA rewards]
        SELECT_TERM[Select Term]
        APPROVE_LP[Approve LP Token]
    end
    
    subgraph "Step 3: Confirm"
        REVIEW[Review:<br/>LP value<br/>RBT you'll receive<br/>Vesting schedule]
        UNDERSTAND[Understand:<br/>LP locked forever in treasury<br/>Trading fees go to protocol]
        CONFIRM[Click Confirm Bond]
    end
    
    subgraph "Step 4: Receive & Track"
        VESTING[Vesting Starts<br/>RBT unlocks daily]
        TRACK[Track on Dashboard:<br/>Vested amount<br/>Claimable RBT<br/>Days remaining]
        CLAIM[Claim Vested RBT<br/>Anytime]
    end
    
    HAVE_RBT --> ADD_LIQUIDITY
    HAVE_USDM --> ADD_LIQUIDITY
    ADD_LIQUIDITY --> GET_LP
    
    GET_LP --> LP_BONDS_PAGE
    LP_BONDS_PAGE --> SEE_TERMS
    SEE_TERMS --> SELECT_TERM
    SELECT_TERM --> APPROVE_LP
    
    APPROVE_LP --> REVIEW
    REVIEW --> UNDERSTAND
    UNDERSTAND --> CONFIRM
    
    CONFIRM --> VESTING
    VESTING --> TRACK
    TRACK --> CLAIM
```

## 6. Complete User Dashboard View

```mermaid
graph TB
    subgraph "Dashboard Overview"
        HEADER[Your Blackhaven Portfolio]
        TOTAL_VALUE[Total Portfolio Value]
    end
    
    subgraph "HVN Section"
        HVN_BAL[HVN Balance]
        SHVN_BAL[sHVN Balance]
        REWARDS_EARNED[Rewards Earned]
        STAKE_BTN[Stake/Unstake Button]
        VOTE_BTN[Vote on Proposals]
    end
    
    subgraph "HPN Section"
        ACTIVE_HPNS[Active HPNs]
        HPN_LIST[List of Your NFTs:<br/>Principal<br/>Accrued rewards<br/>Days left]
        HPN_ACTIONS[Actions:<br/>View details<br/>Exit early<br/>Convert to RBT<br/>Claim at maturity]
    end
    
    subgraph "Bonds Section"
        ACTIVE_BONDS[Active Bonds]
        BOND_LIST[List of Your Bonds:<br/>Total RBT<br/>Vested amount<br/>Claimable now]
        BOND_ACTIONS[Actions:<br/>Claim vested<br/>View schedule<br/>Exit early]
    end
    
    subgraph "RBT Section"
        RBT_BAL[RBT Balance]
        ARBT_BAL[aRBT Balance]
        RBT_ACTIONS[Actions:<br/>Lock for aRBT<br/>Use in DeFi<br/>Trade]
    end
    
    subgraph "Quick Actions"
        NEW_HPN[Deposit New HPN]
        NEW_BOND[Purchase Bond]
        NEW_LP_BOND[Create LP Bond]
    end
    
    HEADER --> TOTAL_VALUE
    TOTAL_VALUE --> HVN_BAL
    TOTAL_VALUE --> ACTIVE_HPNS
    TOTAL_VALUE --> ACTIVE_BONDS
    TOTAL_VALUE --> RBT_BAL
    
    HVN_BAL --> SHVN_BAL
    SHVN_BAL --> REWARDS_EARNED
    HVN_BAL --> STAKE_BTN
    HVN_BAL --> VOTE_BTN
    
    ACTIVE_HPNS --> HPN_LIST
    HPN_LIST --> HPN_ACTIONS
    
    ACTIVE_BONDS --> BOND_LIST
    BOND_LIST --> BOND_ACTIONS
    
    RBT_BAL --> ARBT_BAL
    RBT_BAL --> RBT_ACTIONS
    
    HEADER --> NEW_HPN
    HEADER --> NEW_BOND
    HEADER --> NEW_LP_BOND
```

## 7. Baseline Markets HVN Launch - User Experience

```mermaid
graph TB
    subgraph "Before Launch"
        PREPARE[Prepare USDM/ETH]
        LEARN_BLV[Learn About BLV Floor<br/>Guaranteed minimum price]
        WAIT[Wait for Launch]
    end
    
    subgraph "Launch Day"
        LAUNCH[HVN Launch on Baseline]
        SEE_PRICE[See Current Price]
        SEE_BLV[See BLV Floor<br/>Your downside protection]
        BUY_DECISION[Decide Amount to Buy]
    end
    
    subgraph "Post-Purchase"
        HVN_RECEIVED[HVN in Your Wallet]
        PROTECTED[Protected by BLV<br/>Price can't go below]
        WATCH_BLV[Watch BLV Grow<br/>As trading increases]
    end
    
    subgraph "Advanced Features"
        BORROW[Borrow Against HVN<br/>Up to BLV value]
        NO_LIQ[No Liquidation Risk<br/>BLV only goes up]
        LEVERAGE[Access Leverage<br/>Capital efficient]
    end
    
    PREPARE --> LEARN_BLV
    LEARN_BLV --> WAIT
    WAIT --> LAUNCH
    
    LAUNCH --> SEE_PRICE
    SEE_PRICE --> SEE_BLV
    SEE_BLV --> BUY_DECISION
    
    BUY_DECISION --> HVN_RECEIVED
    HVN_RECEIVED --> PROTECTED
    PROTECTED --> WATCH_BLV
    
    HVN_RECEIVED --> BORROW
    BORROW --> NO_LIQ
    NO_LIQ --> LEVERAGE
```

---

*Frontend-focused user flows for Blackhaven Protocol*

