# OlympusDAO (OHM) - Глубокий Анализ

## Обзор

OlympusDAO - это протокол резервной валюты, который создает децентрализованную, обеспеченную казначейством валюту через механизмы бондинга, стейкинга и управления резервами.

## Ключевые Механизмы (Изучено из Кода)

### 1. Treasury Management

**Из `Treasury.sol` (строки 17-496):**

```solidity
contract OlympusTreasury {
    uint256 public totalReserves;
    uint256 public totalDebt;
    
    mapping(address => uint256) public reserves;
    mapping(STATUS => mapping(address => bool)) public permissions;
}
```

**Ключевые Функции:**

#### deposit() - Депозит активов для минтинга OHM
```solidity
function deposit(uint256 _amount, address _token, uint256 _profit) 
    external returns (uint256 send_)
{
    IERC20(_token).safeTransferFrom(msg.sender, address(this), _amount);
    uint256 value = tokenValue(_token, _amount);
    
    // mint OHM needed and store amount of rewards for distribution
    send_ = value.sub(_profit);
    OHM.mint(msg.sender, send_);
    
    totalReserves = totalReserves.add(value);
}
```

**Механика:**
- Принимает резервный токен (DAI, FRAX, LP tokens)
- Конвертирует в OHM value
- Минтит OHM за вычетом прибыли (profit остается в treasury)
- Увеличивает totalReserves

#### excessReserves() - Избыточные резервы
```solidity
function excessReserves() public view returns (uint256) {
    return totalReserves.sub(OHM.totalSupply().sub(totalDebt));
}
```

**Логика:**
- Total Reserves - (OHM Supply - Debt) = Excess
- Избыток может быть использован для наград стейкерам
- Backing ratio = totalReserves / (OHM Supply - Debt)

### 2. Bonding Mechanism

**Из `BondDepository.sol` (строки 15-569):**

#### Структура Market
```solidity
struct Market {
    IERC20 quoteToken;          // Token to accept as payment
    bool capacityInQuote;        // capacity limit is in payment token
    uint256 capacity;            // capacity remaining
    uint64 totalDebt;            // total debt from market
    uint64 maxPayout;            // max payout in one tx
    uint256 purchased;           // quote tokens exchanged
    uint64 sold;                 // base tokens out
}

struct Terms {
    bool fixedTerm;              // fixed term or fixed expiration
    uint64 controlVariable;      // scaling variable for price
    uint48 vesting;              // length of vesting for fixed-term, timestamp for fixed-expiration
    uint48 conclusion;           // timestamp when market no longer offered
    uint64 maxDebt;              // max debt market can take on
}
```

#### deposit() - Покупка бонда
```solidity
function deposit(
    uint256 _id,
    uint256 _amount,
    uint256 _maxPrice,
    address _user,
    address _referral
) external returns (uint256 payout_, uint256 expiry_, uint256 index_) {
    Market storage market = markets[_id];
    
    // Debt and control variable decay over time
    _decay(_id, currentTime);
    
    // Get current price
    uint256 price = _marketPrice(_id);
    require(price <= _maxPrice, "Depository: more than max price");
    
    // Calculate payout: payout = amount / price
    payout_ = ((_amount * 1e18) / price) / (10**metadata[_id].quoteDecimals);
    
    // Update market state
    market.capacity -= market.capacityInQuote ? _amount : payout_;
    market.totalDebt += uint64(payout_);
    
    // Transfer payment to treasury
    market.quoteToken.safeTransferFrom(msg.sender, address(treasury), _amount);
}
```

**Механика цены:**
```
price = controlVariable * debtRatio
debtRatio = totalDebt / baseSupply
```

#### _tune() - Автоматическая настройка цены
```solidity
function _tune(uint256 _id, uint48 _time) internal {
    // Calculate ideal total debt to satisfy capacity in remaining time
    uint256 targetDebt = (capacity * meta.length) / timeRemaining;
    
    // Derive new control variable from target debt
    uint64 newControlVariable = uint64((price * treasury.baseSupply()) / targetDebt);
    
    // If lowering price, spread change over tune interval
    if (newControlVariable < terms[_id].controlVariable) {
        uint64 change = terms[_id].controlVariable - newControlVariable;
        adjustments[_id] = Adjustment(change, _time, meta.tuneInterval, true);
    }
}
```

**Умная механика:**
- Автоматически подстраивает цену бондов
- Целевой долг = capacity * length / timeRemaining
- Постепенное снижение цены (не резкое)
- Повышение цены немедленное

### 3. Staking Mechanism

**Из `Staking.sol` (строки 14-309):**

#### stake() - Стейкинг OHM
```solidity
function stake(
    address _to,
    uint256 _amount,
    bool _rebasing,
    bool _claim
) external returns (uint256) {
    OHM.safeTransferFrom(msg.sender, address(this), _amount);
    _amount = _amount.add(rebase()); // add bounty if rebase occurred
    
    if (_claim && warmupPeriod == 0) {
        return _send(_to, _amount, _rebasing);
    } else {
        // Enter warmup period
        warmupInfo[_to] = Claim({
            deposit: info.deposit.add(_amount),
            gons: info.gons.add(sOHM.gonsForBalance(_amount)),
            expiry: epoch.number.add(warmupPeriod),
            lock: info.lock
        });
    }
}
```

#### rebase() - Ребейз механизм
```solidity
function rebase() public returns (uint256) {
    if (epoch.end <= block.timestamp) {
        // Trigger rebase on sOHM
        sOHM.rebase(epoch.distribute, epoch.number);
        
        epoch.end = epoch.end.add(epoch.length);
        epoch.number++;
        
        if (address(distributor) != address(0)) {
            distributor.distribute();
        }
        
        // Calculate new distribution amount
        uint256 balance = OHM.balanceOf(address(this));
        uint256 staked = sOHM.circulatingSupply();
        epoch.distribute = balance.sub(staked);
    }
}
```

**sOHM механика:**
- **gons** (governance units) - внутренние единицы
- Balance = gons * index
- Rebase увеличивает index
- Количество gons не меняется, но balance растет

**Пример:**
```
Initial: 1 sOHM = 1 gon, index = 1.0
User stakes 100 OHM → получает 100 sOHM (100 gons)

After rebase with 10% rewards:
index = 1.1
100 gons * 1.1 = 110 sOHM

User теперь имеет 110 sOHM от 100 gons!
```

### 4. Token Model

**Из `OlympusERC20.sol`:**

```solidity
contract OlympusERC20Token is ERC20Permit, IOHM {
    function mint(address account_, uint256 amount_) external override onlyVault {
        _mint(account_, amount_);
    }
    
    function burn(uint256 amount) external override {
        _burn(msg.sender, amount);
    }
}
```

**Контроль эмиссии:**
- Только Treasury (vault) может минтить
- Любой может сжечь свои токены
- 9 decimals (оптимизация для расчетов)

## Range Bound Stability (RBS)

**Современный механизм (v3):**

RBS - это обновленная система, которая:
1. **Upper Wall (Ceiling)**: Когда цена OHM поднимается выше целевой, протокол продает OHM из treasury
2. **Lower Wall (Floor)**: Когда цена падает ниже целевой, протокол покупает OHM обратно
3. **Cushion**: Мягкая зона вокруг целевой цены, где протокол постепенно интервенирует

**Механика (концептуально):**

```
Price Zones:

Upper Wall (e.g. $12)
    ↑ Protocol sells OHM aggressively
Upper Cushion ($11)
    ↑ Protocol slowly sells OHM
Target Price ($10)
Lower Cushion ($9)
    ↓ Protocol slowly buys OHM
Lower Wall (e.g. $8)
    ↓ Protocol buys OHM aggressively
```

**Цели RBS:**
- Стабилизация цены в определенном диапазоне
- Накопление резервов при высокой цене
- Защита backing при низкой цене
- Постепенное, не шоковое воздействие на рынок

## Ключевые Уроки для notanohmfork

### ✅ Что Взять из OHM

1. **Treasury Management**
   - Diversified reserves (stable + volatile)
   - Permission-based access control
   - Audit functions для проверки резервов
   - Debt tracking для займов

2. **Bonding**
   - Control variable для динамической цены
   - Auto-tuning механизм
   - Vesting periods для предотвращения dumping
   - Circuit breakers (max debt)

3. **Staking**
   - gons-based accounting (эффективно)
   - Rebase mechanism для распределения наград
   - Warmup period для защиты от flash loan атак

4. **Price Discovery**
   - Dynamic pricing based on debt ratio
   - Gradual price adjustments
   - Market-driven capacity

### ⚠️ Что НЕ Брать / Модифицировать

1. **NO Rebase**
   - Rebase механизм сложен для интеграции с DeFi
   - Проблемы с wallet tracking
   - Путаница для пользователей
   - **notanohmfork использует простой staking с rewards**

2. **NO Lower Bound RBS**
   - Покупка OHM обратно истощает treasury
   - В bearish рынке может быстро опустошить резервы
   - **notanohmfork использует только UPPER bound RBS**

3. **NO Fixed-term bonds**
   - Сложность для casual пользователей
   - Требует vesting tracking
   - **notanohmfork использует instant bonds с опциональным vesting**

4. **NO Excessive Governance**
   - OHM имеет многоуровневую систему разрешений
   - Timelock на все изменения
   - **notanohmfork упрощает через role-based control**

## Comparison Table

| Feature | OlympusDAO | notanohmfork |
|---------|-----------|--------------|
| **Token Model** | OHM + sOHM (rebasing) | RBT + sRBT (non-rebasing) |
| **Backing** | ~1:1 dynamic | 1.2:1 fixed |
| **Bonding** | Variable price, vesting | Variable price, instant option |
| **Staking** | Rebase (gons) | Direct rewards |
| **RBS** | Upper + Lower | Upper only |
| **Target** | Reserve currency | Liquidity engine |
| **Yield Source** | Bonding + RBS | Bonding + RBS + megaNOTE |
| **Governance** | Complex timelock | Simplified roles |

## Код Metrics

**Treasury.sol**: 496 lines
- ~200 lines основная логика
- ~150 lines permission system
- ~100 lines timelock system
- ~50 lines view functions

**BondDepository.sol**: 569 lines
- ~170 lines core bonding logic
- ~150 lines pricing & decay
- ~100 lines market creation
- ~80 lines tuning mechanism
- ~70 lines view functions

**Staking.sol**: 309 lines
- ~100 lines staking/unstaking
- ~50 lines rebase logic
- ~80 lines warmup period
- ~50 lines wrap/unwrap (gOHM)
- ~30 lines view functions

## Заключение

OlympusDAO предоставляет проверенную архитектуру для:
✅ Treasury management
✅ Dynamic bonding
✅ Efficient staking
✅ Automated price discovery

notanohmfork берет лучшее из OHM и адаптирует для MegaETH:
- Упрощенная token model (без rebase)
- Upper-only RBS (фокус на capture upside)
- Интеграция с MegaETH ecosystem (megaNOTE, USDm)
- Real-time performance (MegaETH speed)

**Код изучен ✅**
**Механизмы поняты ✅**
**Готово для адаптации ✅**

