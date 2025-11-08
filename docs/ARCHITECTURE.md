# notanohmfork Architecture

## Table of Contents

1. [System Overview](#system-overview)
2. [Core Components](#core-components)
3. [Mechanisms](#mechanisms)
4. [Smart Contract Architecture](#smart-contract-architecture)
5. [Data Flow](#data-flow)
6. [Security Considerations](#security-considerations)

## System Overview

notanohmfork is a two-token DeFi protocol built on MegaETH that uses a Dynamic Automated Treasury (DAT) with upper-bound Range Bound Stability (RBS) mechanism.

### Design Principles

1. **Capital Efficiency**: 120% collateralization allows sustainable backing
2. **Upside Capture**: RBS only triggers on premium trading
3. **Ecosystem Alignment**: Treasury primarily holds MegaETH ecosystem assets
4. **Yield Generation**: Integration with productive assets (megaNOTE, USDm)
5. **Liquidity Black Hole**: Reduces circulating supply of $MEGA while growing treasury

### Token Model

```
┌─────────────────────────────────────────────────────────────┐
│                      Two-Token System                        │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  RBT (Reserve-Backed Token)                                  │
│  ├─ Primary token                                            │
│  ├─ Backed 1:1.2 by treasury assets                         │
│  ├─ Tradeable on DEX                                         │
│  └─ Can be bonded or staked                                  │
│                                                               │
│  sRBT (Staked RBT)                                           │
│  ├─ Received when staking RBT                               │
│  ├─ Accrues protocol yield                                   │
│  ├─ Non-transferable (locked staking position)              │
│  └─ 1:1 redeemable for RBT + rewards                        │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

## Core Components

### 1. Treasury Contract

The Treasury is the heart of the protocol, managing all assets and backing calculations.

**Responsibilities:**
- Store and manage reserve assets
- Calculate backing ratio in real-time
- Manage asset deposits and withdrawals
- Execute RBS operations
- Generate yield reports

**Key Functions:**
```solidity
interface ITreasury {
    // View Functions
    function getTotalReserves() external view returns (uint256);
    function getBackingPerRBT() external view returns (uint256);
    function getAssetAllocation() external view returns (Asset[] memory);
    
    // Management Functions
    function deposit(address token, uint256 amount) external;
    function withdraw(address token, uint256 amount, address recipient) external;
    function rebalance() external;
    
    // RBS Functions
    function executeRBS(uint256 rbsAmount) external;
}
```

**Asset Management:**

```
Treasury Assets Breakdown:

┌──────────────────────────────────────────────────────────┐
│ Asset Type    │ Allocation │ Purpose                     │
├──────────────────────────────────────────────────────────┤
│ $MEGA         │ 60%        │ Ecosystem alignment         │
│ megaNOTE      │ 20%        │ Stable yield generation     │
│ USDm          │ 10%        │ Liquidity + native stables  │
│ $OHM          │ 10%        │ Diversification + homage    │
└──────────────────────────────────────────────────────────┘
```

### 2. RBT Token Contract

ERC-20 token representing reserve-backed units.

**Key Features:**
- Standard ERC-20 with additional protocol controls
- Minting controlled by Treasury/Bonding contracts
- Burning mechanism for redemptions
- Integration with DEX for price discovery

**Special Mechanisms:**
```solidity
interface IRBT {
    function mint(address to, uint256 amount) external;
    function burn(uint256 amount) external;
    function getCurrentPrice() external view returns (uint256);
    function getIntrinsicValue() external view returns (uint256);
}
```

### 3. Bonding Contract

Allows users to deposit assets to mint RBT at favorable rates.

**Bonding Mechanism:**

```
User Deposits 1.2 $MEGA worth of value
         ↓
┌─────────────────────────────────────┐
│     Bonding Contract                │
│                                     │
│  1. Validate deposit                │
│  2. Calculate RBT to mint           │
│  3. Transfer assets to Treasury     │
│  4. Mint 1.0 RBT to user           │
│  5. Allocate 0.2 to Treasury       │
└─────────────────────────────────────┘
         ↓
User receives 1.0 RBT
Treasury backing strengthens by 0.2
```

**Dynamic Bonding Rates:**
- Base rate: 1.2:1
- Discount based on asset type
- Premium during high demand
- Vesting schedules for large bonds

**Bond Types:**

1. **Instant Bonds**: No vesting, standard rate
2. **Vested Bonds**: 7-day vesting, 5% discount
3. **Strategic Bonds**: 30-day vesting, 15% discount (for large amounts)

### 4. Staking Contract

Enables users to stake RBT for yield share.

**Staking Flow:**

```
         User Stakes RBT
                ↓
┌──────────────────────────────────────────┐
│         Staking Contract                 │
│                                          │
│  1. Lock RBT                            │
│  2. Mint sRBT 1:1                       │
│  3. Start accruing rewards              │
│                                          │
│  Rewards Sources:                       │
│  - RBS profits                          │
│  - megaNOTE yield                       │
│  - Bonding premiums                     │
│  - Protocol revenue                     │
└──────────────────────────────────────────┘
                ↓
         User receives sRBT
      (continuously accruing value)
```

**Unstaking:**
- No lock period (liquid staking)
- Instant unstaking available
- Claims all accrued rewards
- Burns sRBT, returns RBT + rewards

### 5. RBS (Range Bound Stability) Controller

Manages price stability through upper-bound interventions.

**RBS Logic:**

```
Monitor RBT Price on DEX
         ↓
    Is Price > Intrinsic Value + Premium Threshold?
         ↓
      Yes ─────────────────────→ Execute RBS
         │                            ↓
         │                  ┌──────────────────────┐
         │                  │  1. Calculate amount │
         │                  │  2. Mint RBT         │
         │                  │  3. Sell on DEX      │
         │                  │  4. Buy backing      │
         │                  │  5. Increase backing │
         │                  └──────────────────────┘
         │                            ↓
         │                    Price normalizes
         │                    Backing increases
         ↓
      No ──────────→ Continue monitoring
                     (No intervention)
```

**RBS Parameters:**

```solidity
struct RBSConfig {
    uint256 premiumThreshold;      // 5% above intrinsic value
    uint256 maxSellPercent;        // 2% of supply per operation
    uint256 cooldownPeriod;        // 4 hours between operations
    uint256 minLiquidityThreshold; // Minimum DEX liquidity required
}
```

**RBS Decision Tree:**

```
Start: Check Market Conditions
    ↓
[Current Price] vs [Intrinsic Value]
    ↓
    ├─ Price < IV + 5%  → No Action (Hold)
    ↓
    ├─ Price > IV + 5%  → Check Cooldown
    ↓                         ↓
    └─────────────────────────┼─ Last RBS > 4h ago? → NO → Wait
                              ↓
                              YES
                              ↓
                         Check Liquidity
                              ↓
                         DEX TVL > $500k? → NO → Wait
                              ↓
                              YES
                              ↓
                         Execute RBS
                              ↓
                         ┌────────────────────────┐
                         │ 1. Mint RBT (2% max)  │
                         │ 2. Sell for $MEGA     │
                         │ 3. Add to Treasury    │
                         │ 4. Update metrics     │
                         │ 5. Emit event         │
                         └────────────────────────┘
```

### 6. Oracle System

Price feeds for accurate backing calculations.

**Required Oracles:**
- $MEGA / USD
- $ETH / USD
- USDm / USD
- megaNOTE / USD
- $OHM / USD

**Oracle Integration:**
```solidity
interface IPriceOracle {
    function getPrice(address token) external view returns (uint256);
    function getPriceWithAge(address token) external view returns (uint256 price, uint256 timestamp);
    function getTWAP(address token, uint256 period) external view returns (uint256);
}
```

## Mechanisms

### Bonding Mechanism Deep Dive

**Step-by-Step Process:**

1. **User Initiates Bond**
   ```solidity
   function bond(address asset, uint256 amount) external {
       // 1. Validate asset is accepted
       require(acceptedAssets[asset], "Asset not accepted");
       
       // 2. Transfer asset from user
       IERC20(asset).transferFrom(msg.sender, address(this), amount);
       
       // 3. Calculate value in USD
       uint256 valueUSD = oracle.getPrice(asset) * amount;
       
       // 4. Calculate RBT to mint (amount / 1.2)
       uint256 rbtAmount = (valueUSD * 1e18) / BACKING_RATIO; // 1.2
       
       // 5. Transfer to treasury
       IERC20(asset).transfer(address(treasury), amount);
       
       // 6. Mint RBT to user
       rbt.mint(msg.sender, rbtAmount);
       
       // 7. Emit event
       emit Bonded(msg.sender, asset, amount, rbtAmount);
   }
   ```

2. **Treasury Records Backing Increase**
   ```
   Before: 100 RBT backed by 120 assets = 1.20 backing
   User bonds 12 assets, mints 10 RBT
   After: 110 RBT backed by 132 assets = 1.20 backing
   
   Extra 2 assets (0.2 per RBT) strengthen treasury
   ```

3. **Bond Discounts**
   - $MEGA bonds: Base rate (most desired)
   - megaNOTE bonds: 3% discount (stable income)
   - USDm bonds: 5% discount (liquidity provision)
   - $OHM bonds: 7% discount (diversification)

### RBS Mechanism Deep Dive

**Triggering Conditions:**

```solidity
function shouldExecuteRBS() public view returns (bool) {
    uint256 currentPrice = getDEXPrice();
    uint256 intrinsicValue = treasury.getBackingPerRBT();
    uint256 premiumThreshold = intrinsicValue * 105 / 100; // 5% premium
    
    bool isPremium = currentPrice > premiumThreshold;
    bool cooledDown = block.timestamp > lastRBSTime + COOLDOWN_PERIOD;
    bool sufficientLiquidity = getDEXLiquidity() > MIN_LIQUIDITY;
    
    return isPremium && cooledDown && sufficientLiquidity;
}
```

**Execution Process:**

```solidity
function executeRBS() external {
    require(shouldExecuteRBS(), "RBS conditions not met");
    
    // 1. Calculate amount to sell (2% of supply)
    uint256 sellAmount = rbt.totalSupply() * MAX_SELL_PERCENT / 10000;
    
    // 2. Mint RBT
    rbt.mint(address(this), sellAmount);
    
    // 3. Approve DEX
    rbt.approve(address(dex), sellAmount);
    
    // 4. Swap for $MEGA
    uint256 megaReceived = dex.swapExactTokensForTokens(
        sellAmount,
        0, // slippage handled by DEX
        path,
        address(treasury),
        block.timestamp
    );
    
    // 5. Update metrics
    totalRBSProfit += megaReceived;
    lastRBSTime = block.timestamp;
    
    // 6. Distribute profits
    distributeRBSProfit(megaReceived);
    
    emit RBSExecuted(sellAmount, megaReceived, treasury.getBackingPerRBT());
}
```

**Profit Distribution:**

```
RBS Profit = 100 $MEGA
    ↓
├─ 80% → Treasury (compounds backing)
├─ 15% → Stakers (immediate rewards)
└─ 5% → Protocol Operations
```

### Yield Generation

**Sources of Yield:**

1. **megaNOTE Yield** (Primary stable yield)
   - Treasury holds megaNOTE (Avon's yield-bearing token)
   - Automatically accrues yield from USDm deposits
   - Estimated APY: 8-15%

2. **RBS Profits** (Opportunistic)
   - Triggered during premium trading
   - Captures arbitrage opportunities
   - Variable, depends on market conditions

3. **Bonding Premiums** (Volume-dependent)
   - 0.2 extra backing per bond
   - Accumulates over time
   - Grows with protocol adoption

4. **$MEGA Appreciation**
   - 60% of treasury in $MEGA
   - Sequencer rotation yields
   - Proximity market demand

**Yield Distribution Strategy:**

```
Total Protocol Yield (Monthly Example)
    ↓
megaNOTE: 10% APY on 20% of treasury = 1.67% monthly
RBS Profits: Variable, estimate 5% monthly
$MEGA Appreciation: Variable, estimate 10% monthly
Bonding Growth: Variable, estimate 3% monthly
    ↓
Total Estimated Yield: ~20% monthly
    ↓
Distribution:
├─ 70% → Stakers (sRBT holders)
├─ 20% → Treasury (compound growth)
└─ 10% → Protocol Operations & Development
```

## Smart Contract Architecture

### Contract Hierarchy

```
                    ┌──────────────┐
                    │   Governor   │
                    │   (Timelock) │
                    └──────┬───────┘
                           │
          ┌────────────────┼────────────────┐
          │                │                │
    ┌─────▼─────┐   ┌─────▼─────┐   ┌─────▼─────┐
    │ Treasury  │   │    RBT    │   │   Oracle  │
    └─────┬─────┘   └─────┬─────┘   └───────────┘
          │               │
    ┌─────┴─────┬────────┴────┬──────────┐
    │           │             │          │
┌───▼───┐  ┌───▼───┐    ┌────▼────┐  ┌──▼──┐
│Bonding│  │Staking│    │   RBS   │  │ DEX │
└───────┘  └───┬───┘    └─────────┘  └─────┘
               │
           ┌───▼───┐
           │  sRBT │
           └───────┘
```

### Core Contracts

#### 1. Treasury.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

contract Treasury is Ownable, ReentrancyGuard {
    // State Variables
    mapping(address => uint256) public reserves;
    address[] public supportedAssets;
    
    IRBT public rbt;
    IPriceOracle public oracle;
    
    // Events
    event Deposit(address indexed token, uint256 amount);
    event Withdrawal(address indexed token, uint256 amount, address indexed recipient);
    event Rebalanced(uint256 timestamp);
    
    // View Functions
    function getTotalReservesUSD() public view returns (uint256) {
        uint256 total = 0;
        for (uint i = 0; i < supportedAssets.length; i++) {
            address asset = supportedAssets[i];
            uint256 amount = reserves[asset];
            uint256 price = oracle.getPrice(asset);
            total += amount * price / 1e18;
        }
        return total;
    }
    
    function getBackingPerRBT() public view returns (uint256) {
        uint256 totalReserves = getTotalReservesUSD();
        uint256 totalSupply = rbt.totalSupply();
        if (totalSupply == 0) return 0;
        return totalReserves * 1e18 / totalSupply;
    }
    
    // Management Functions
    function deposit(address token, uint256 amount) external onlyAuthorized {
        IERC20(token).transferFrom(msg.sender, address(this), amount);
        reserves[token] += amount;
        emit Deposit(token, amount);
    }
    
    function rebalance() external onlyAuthorized {
        // Implementation for rebalancing logic
        emit Rebalanced(block.timestamp);
    }
}
```

#### 2. RBT.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/AccessControl.sol";

contract RBT is ERC20, AccessControl {
    bytes32 public constant MINTER_ROLE = keccak256("MINTER_ROLE");
    bytes32 public constant BURNER_ROLE = keccak256("BURNER_ROLE");
    
    ITreasury public treasury;
    
    constructor() ERC20("Reserve Backed Token", "RBT") {
        _grantRole(DEFAULT_ADMIN_ROLE, msg.sender);
    }
    
    function mint(address to, uint256 amount) external onlyRole(MINTER_ROLE) {
        _mint(to, amount);
    }
    
    function burn(uint256 amount) external {
        _burn(msg.sender, amount);
    }
    
    function getIntrinsicValue() external view returns (uint256) {
        return treasury.getBackingPerRBT();
    }
}
```

#### 3. BondingContract.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract BondingContract is ReentrancyGuard, Ownable {
    ITreasury public treasury;
    IRBT public rbt;
    IPriceOracle public oracle;
    
    uint256 public constant BACKING_RATIO = 1.2e18; // 1.2:1
    
    mapping(address => bool) public acceptedAssets;
    mapping(address => uint256) public bondDiscounts; // in basis points
    
    event Bonded(address indexed user, address indexed asset, uint256 assetAmount, uint256 rbtMinted);
    
    function bond(address asset, uint256 amount) external nonReentrant {
        require(acceptedAssets[asset], "Asset not accepted");
        
        // Transfer asset from user
        IERC20(asset).transferFrom(msg.sender, address(this), amount);
        
        // Get asset value in USD
        uint256 valueUSD = oracle.getPrice(asset) * amount / 1e18;
        
        // Apply discount if applicable
        uint256 discount = bondDiscounts[asset];
        valueUSD = valueUSD * (10000 + discount) / 10000;
        
        // Calculate RBT to mint
        uint256 rbtAmount = valueUSD * 1e18 / BACKING_RATIO;
        
        // Transfer to treasury
        IERC20(asset).transfer(address(treasury), amount);
        treasury.recordDeposit(asset, amount);
        
        // Mint RBT to user
        rbt.mint(msg.sender, rbtAmount);
        
        emit Bonded(msg.sender, asset, amount, rbtAmount);
    }
}
```

#### 4. StakingContract.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract StakingContract is ReentrancyGuard {
    IRBT public rbt;
    IsRBT public sRBT;
    
    uint256 public rewardIndex;
    mapping(address => uint256) public stakerRewardIndex;
    mapping(address => uint256) public stakedBalance;
    
    event Staked(address indexed user, uint256 amount);
    event Unstaked(address indexed user, uint256 amount, uint256 rewards);
    event RewardsDistributed(uint256 amount, uint256 newIndex);
    
    function stake(uint256 amount) external nonReentrant {
        require(amount > 0, "Cannot stake 0");
        
        // Claim pending rewards first
        _claimRewards(msg.sender);
        
        // Transfer RBT from user
        rbt.transferFrom(msg.sender, address(this), amount);
        
        // Mint sRBT 1:1
        sRBT.mint(msg.sender, amount);
        
        // Update state
        stakedBalance[msg.sender] += amount;
        stakerRewardIndex[msg.sender] = rewardIndex;
        
        emit Staked(msg.sender, amount);
    }
    
    function unstake(uint256 amount) external nonReentrant {
        require(stakedBalance[msg.sender] >= amount, "Insufficient balance");
        
        // Claim all rewards
        uint256 rewards = _claimRewards(msg.sender);
        
        // Burn sRBT
        sRBT.burn(msg.sender, amount);
        
        // Return RBT + rewards
        rbt.transfer(msg.sender, amount + rewards);
        
        // Update state
        stakedBalance[msg.sender] -= amount;
        
        emit Unstaked(msg.sender, amount, rewards);
    }
    
    function distributeRewards(uint256 rewardAmount) external {
        uint256 totalStaked = sRBT.totalSupply();
        require(totalStaked > 0, "No stakers");
        
        // Update reward index
        rewardIndex += rewardAmount * 1e18 / totalStaked;
        
        emit RewardsDistributed(rewardAmount, rewardIndex);
    }
    
    function _claimRewards(address user) internal returns (uint256) {
        uint256 earned = getPendingRewards(user);
        if (earned > 0) {
            stakerRewardIndex[user] = rewardIndex;
        }
        return earned;
    }
    
    function getPendingRewards(address user) public view returns (uint256) {
        uint256 stakerIndex = stakerRewardIndex[user];
        uint256 indexDelta = rewardIndex - stakerIndex;
        return stakedBalance[user] * indexDelta / 1e18;
    }
}
```

#### 5. RBSController.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract RBSController is Ownable {
    IRBT public rbt;
    ITreasury public treasury;
    IDEXRouter public dex;
    IPriceOracle public oracle;
    
    uint256 public constant PREMIUM_THRESHOLD = 500; // 5% in basis points
    uint256 public constant MAX_SELL_PERCENT = 200; // 2% in basis points
    uint256 public constant COOLDOWN_PERIOD = 4 hours;
    uint256 public constant MIN_LIQUIDITY = 500_000e18; // $500k
    
    uint256 public lastRBSTime;
    uint256 public totalRBSProfit;
    
    event RBSExecuted(uint256 rbtSold, uint256 assetsAcquired, uint256 newBacking);
    
    function shouldExecuteRBS() public view returns (bool) {
        // Get current market price
        uint256 currentPrice = _getDEXPrice();
        
        // Get intrinsic value
        uint256 intrinsicValue = treasury.getBackingPerRBT();
        
        // Calculate premium threshold
        uint256 premiumPrice = intrinsicValue * (10000 + PREMIUM_THRESHOLD) / 10000;
        
        // Check conditions
        bool isPremium = currentPrice > premiumPrice;
        bool cooledDown = block.timestamp > lastRBSTime + COOLDOWN_PERIOD;
        bool sufficientLiquidity = _getDEXLiquidity() > MIN_LIQUIDITY;
        
        return isPremium && cooledDown && sufficientLiquidity;
    }
    
    function executeRBS() external onlyAuthorized {
        require(shouldExecuteRBS(), "RBS conditions not met");
        
        // Calculate sell amount
        uint256 sellAmount = rbt.totalSupply() * MAX_SELL_PERCENT / 10000;
        
        // Mint RBT to this contract
        rbt.mint(address(this), sellAmount);
        
        // Approve DEX
        rbt.approve(address(dex), sellAmount);
        
        // Swap path: RBT -> MEGA
        address[] memory path = new address[](2);
        path[0] = address(rbt);
        path[1] = address(megaToken);
        
        // Execute swap
        uint256[] memory amounts = dex.swapExactTokensForTokens(
            sellAmount,
            0, // Accept any amount (price already validated)
            path,
            address(treasury),
            block.timestamp
        );
        
        uint256 megaReceived = amounts[1];
        
        // Update metrics
        totalRBSProfit += megaReceived;
        lastRBSTime = block.timestamp;
        
        // Record in treasury
        treasury.recordRBSProfit(megaReceived);
        
        emit RBSExecuted(sellAmount, megaReceived, treasury.getBackingPerRBT());
    }
    
    function _getDEXPrice() internal view returns (uint256) {
        // Implementation depends on DEX
        // Returns price of RBT in terms of backing asset
    }
    
    function _getDEXLiquidity() internal view returns (uint256) {
        // Returns total liquidity in RBT/MEGA pair
    }
}
```

## Data Flow

### Bonding Flow

```
User
  ↓ [Approve $MEGA]
  ↓ [Call bond(MEGA, amount)]
  ↓
BondingContract
  ↓ [Validate asset]
  ↓ [Calculate RBT amount]
  ↓ [transferFrom user]
  ↓
Treasury
  ↓ [Record deposit]
  ↓ [Update reserves]
  ↓
BondingContract
  ↓ [Call mint on RBT]
  ↓
RBT Contract
  ↓ [Mint RBT to user]
  ↓
User receives RBT
```

### Staking Flow

```
User
  ↓ [Approve RBT]
  ↓ [Call stake(amount)]
  ↓
StakingContract
  ↓ [transferFrom user]
  ↓ [Call mint on sRBT]
  ↓
sRBT Contract
  ↓ [Mint sRBT to user]
  ↓
StakingContract
  ↓ [Update staker index]
  ↓
User receives sRBT
(starts accruing rewards)
```

### RBS Flow

```
Price Oracle
  ↓ [Current Price > Intrinsic + 5%]
  ↓
RBSController
  ↓ [Check conditions]
  ↓ [shouldExecuteRBS() = true]
  ↓ [executeRBS()]
  ↓
RBT Contract
  ↓ [Mint 2% of supply]
  ↓
DEX
  ↓ [Swap RBT for $MEGA]
  ↓
Treasury
  ↓ [Receive $MEGA]
  ↓ [Update backing ratio]
  ↓
StakingContract
  ↓ [Distribute 15% to stakers]
  ↓
sRBT holders
  ↓ [Accrue rewards]
```

### Yield Distribution Flow

```
Multiple Sources:
  ├─ megaNOTE accrual
  ├─ RBS profits
  ├─ Bonding premiums
  └─ $MEGA appreciation
       ↓
Treasury
  ↓ [Aggregate all yields]
  ↓ [Calculate distribution]
  ↓
Distribution:
  ├─ 70% → StakingContract.distributeRewards()
  │         ↓
  │      sRBT holders (proportional)
  │
  ├─ 20% → Treasury (compound)
  │         ↓
  │      Increase backing ratio
  │
  └─ 10% → Protocol multisig
            ↓
         Operations & Development
```

## Security Considerations

### Access Control

```solidity
// Role-based access control hierarchy

Governor (Timelock)
  ├─ Can upgrade contracts
  ├─ Can change critical parameters
  └─ Can emergency pause

Treasury Manager
  ├─ Can rebalance assets
  ├─ Can execute approved withdrawals
  └─ Cannot mint tokens

RBS Operator
  ├─ Can execute RBS when conditions met
  ├─ Cannot change parameters
  └─ Cannot access treasury directly

Emergency Guardian
  ├─ Can pause contracts
  ├─ Cannot upgrade or change parameters
  └─ 24h timelock on actions
```

### Critical Parameters

All critical parameters must be behind timelock:

```solidity
struct ProtocolParameters {
    uint256 backingRatio;          // 1.2e18 (48h timelock)
    uint256 rbsPremiumThreshold;   // 500 bp (24h timelock)
    uint256 rbsMaxSell;            // 200 bp (24h timelock)
    uint256 rbsCooldown;           // 4 hours (12h timelock)
    uint256 stakingRewardSplit;    // 70% (24h timelock)
}
```

### Oracle Security

- Use Chainlink or Pyth for price feeds
- Implement TWAP (Time-Weighted Average Price) for RBS decisions
- Have fallback oracles
- Circuit breakers for extreme price movements

```solidity
function getPriceWithValidation(address token) internal view returns (uint256) {
    uint256 primaryPrice = primaryOracle.getPrice(token);
    uint256 secondaryPrice = secondaryOracle.getPrice(token);
    
    // Prices must be within 2% of each other
    uint256 deviation = abs(primaryPrice - secondaryPrice) * 10000 / primaryPrice;
    require(deviation < 200, "Oracle price deviation too high");
    
    return primaryPrice;
}
```

### Reentrancy Protection

All external calls must use ReentrancyGuard:

```solidity
function bond(address asset, uint256 amount) external nonReentrant {
    // Implementation
}

function unstake(uint256 amount) external nonReentrant {
    // Implementation
}
```

### Emergency Mechanisms

```solidity
// Pausable functionality
bool public paused;

modifier whenNotPaused() {
    require(!paused, "Contract is paused");
    _;
}

function emergencyPause() external onlyGuardian {
    paused = true;
    emit EmergencyPause(block.timestamp);
}

// Timelocked unpause (24h minimum)
function scheduleUnpause() external onlyGovernor {
    unpauseTime = block.timestamp + 24 hours;
}

function executeUnpause() external {
    require(block.timestamp >= unpauseTime, "Timelock not expired");
    paused = false;
}
```

### Audit Recommendations

1. **Pre-launch Audits:**
   - At least 2 independent security audits
   - Formal verification of critical functions
   - Economic modeling and simulation

2. **Continuous Security:**
   - Bug bounty program (up to $1M)
   - Real-time monitoring and alerts
   - Regular security reviews

3. **Testnet Deployment:**
   - 30+ days on MegaETH testnet
   - Stress testing with simulated attacks
   - Community testing program

## Performance Optimization

### Gas Optimization

Leveraging MegaETH's performance:

```solidity
// Batch operations
function batchBond(
    address[] calldata assets,
    uint256[] calldata amounts
) external nonReentrant {
    for (uint i = 0; i < assets.length; i++) {
        _bondInternal(assets[i], amounts[i]);
    }
}

// View function caching
uint256 private cachedBacking;
uint256 private backingCacheTime;
uint256 private constant CACHE_DURATION = 10; // 10 seconds on MegaETH

function getBackingPerRBT() public view returns (uint256) {
    if (block.timestamp < backingCacheTime + CACHE_DURATION) {
        return cachedBacking;
    }
    return _calculateBacking();
}
```

### Event Optimization

```solidity
// Indexed events for efficient querying
event Bonded(
    address indexed user,
    address indexed asset,
    uint256 assetAmount,
    uint256 rbtMinted,
    uint256 indexed timestamp
);

event RBSExecuted(
    uint256 indexed timestamp,
    uint256 rbtSold,
    uint256 assetsAcquired,
    uint256 newBacking
);
```

## Upgradeability

Using TransparentProxy pattern:

```
┌──────────────┐
│    Users     │
└──────┬───────┘
       │
┌──────▼───────────────────┐
│  TransparentProxyAdmin   │
│  (Controlled by Governor)│
└──────┬───────────────────┘
       │
┌──────▼───────┐
│  Proxy       │
└──────┬───────┘
       │
┌──────▼───────┐
│  Logic V1    │  ──→  Can upgrade to V2
└──────────────┘
```

Only non-token contracts are upgradeable:
- ✅ Treasury (upgradeable)
- ✅ Bonding (upgradeable)
- ✅ Staking (upgradeable)
- ✅ RBS Controller (upgradeable)
- ❌ RBT (immutable)
- ❌ sRBT (immutable)

## Conclusion

notanohmfork's architecture is designed to be:
- **Secure**: Multi-layer security, audited, time-locked
- **Efficient**: Optimized for MegaETH's performance
- **Sustainable**: Self-reinforcing yield generation
- **Transparent**: All operations verifiable on-chain
- **Adaptable**: Upgradeable non-token contracts

The system creates a positive feedback loop:
```
More users → More bonding → Stronger backing → Higher confidence → More users
     ↓           ↓              ↓                  ↓
 RBS profits → Staker rewards → Increased APY → More staking → Higher TVL
```

This architecture positions notanohmfork as the liquidity backbone of the MegaETH ecosystem.

