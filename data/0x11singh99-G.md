## Gas Optimizations

| Number                                                                                        | Issue                                                                             | Instances |
| --------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------- | :-------: |
| [[G-01](#g-01-state-variables-only-set-in-the-constructor-should-be-declared-immutable)]      | State variables only set in the constructor should be declared `immutable`        |     7     |
| [[G-02](#g-02-state-variables-can-be-cached-instead-of-re-reading-them-from-storage)]         | `State` variables can be cached instead of re-reading them from storage           |    19     |
| [[G-03](#g-03-structs-can-be-packed-into-fewer-storage-slots)]                                | Structs can be packed into fewer storage slots                                    |     4     |
| [[G-04](#g-04-state-variables-can-be-packed-into-fewer-storage-slots)]                        | State variables can be packed into fewer storage slots                            |     2     |
| [[G-05](#g-05-missing-zero-address-check-in-constructor)]                                     | Missing `zero-address` check in `constructor`                                     |     1     |
| [[G-06](#g-06-custom-errors-cost-less-than-requireassert)]                                    | Custom `error`s cost less than `require`/`assert`                                 |     1     |
| [[G-07](#g-07-revert-transaction-as-soon-as-possible)]                                        | Revert Transaction as soon as possible                                            |     3     |
| [[G-08](#g-08-cache-calculated-value-instead-of-recalculate)]                                 | Cache `calculated` value instead of recalculate                                   |     7     |
| [[G-09](#g-09-remove-unused-import-variable)]                                                 | Remove unused `import` variable                                                   |     5     |
| [[G-10](#g-10-remove-unused-function-parameter-to-save-gas)]                                  | Remove unused function `parameter` to save gas                                    |     1     |
| [[G-11](#g-11-use-calldata-instead-of-memory-for-function-arguments-that-do-not-get-mutated)] | Use `calldata` instead of `memory` for function arguments that do not get mutated |     1     |
| [[G-12](#g-12-dont-cache-value-if-it-is-only-used-once)]                                      | Don’t `cache` value if it is only used once                                       |     6     |

## [G-01] State variables only set in the constructor should be declared `immutable`

Accessing state variables within a function involves an SLOAD operation, but `immutable` variables can be accessed directly without the need of it, thus reducing gas costs. As these state variables are assigned only in the constructor, consider declaring them `immutable`.
Avoids a Gsset(** 20000 gas**) in the constructor, and replaces the first access in each transaction(Gcoldsload - ** 2100 gas **) and each access thereafter(Gwarmacces - ** 100 gas ) with aPUSH32( 3 gas **).

**Note: These instances missed by bot-report**

_7 instance in 2 files_

```solidity
File : /src/contracts/oracles/UniV3Relayer.sol

23:  address public baseToken;

25:  address public quoteToken;

30:  string public symbol;

33:  uint128 public baseAmount;

35:  uint256 public multiplier;

37:  uint32 public quotePeriod;

```

[23-37](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L23C3-L37C29)

```solidity
File : src/contracts/proxies/ODSafeManager.sol

26: address public safeEngine;

```

[26](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L26)

## [G-02] `State` variables can be cached instead of re-reading them from storage

Caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read.

**Note: These instances missed by bot-report**

_19 Instances in 2 Files_

<details>
<summary>see instances</summary>

```solidity

File : contracts/proxies/Vault721.sol

  function mint(address _proxy, uint256 _safeId) external {
    require(msg.sender == address(safeManager), 'V721: only safeManager');
    require(_proxyRegistry[_proxy] != address(0), 'V721: non-native proxy');
    address _user = _proxyRegistry[_proxy];
    _safeMint(_user, _safeId);
  }

```

[94-99](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L94C1-L99C4)

```diff
File : contracts/proxies/Vault721.sol

   function mint(address _proxy, uint256 _safeId) external {
      require(msg.sender == address(safeManager), 'V721: only safeManager');
+     address _user = _proxyRegistry[_proxy];
-     require(_proxyRegistry[_proxy] != address(0), 'V721: non-native proxy');
-     address _user = _proxyRegistry[_proxy];
+     require(_user != address(0), 'V721: non-native proxy');
      _safeMint(_user, _safeId);
   }

```

```solidity

File : contracts/proxies/Vault721.sol

   function _isNotProxy(address _user) internal view returns (bool) {
       return _userRegistry[_user] == address(0) || ODProxy(_userRegistry[_user]).OWNER() != _user;
   }

```

[154-156](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L154C3-L156C4)

```diff
File : contracts/proxies/Vault721.sol

   function _isNotProxy(address _user) internal view returns (bool) {
+      address userRegistry = _userRegistry[_user];
-       return _userRegistry[_user] == address(0) || ODProxy(_userRegistry[_user]).OWNER() != _user;
+       return userRegistry == address(0) || ODProxy(userRegistry).OWNER() != _user;
   }

```

```solidity

File : contracts/AccountingEngine.sol

  function cancelAuctionedDebtWithSurplus(uint256 _rad) external {
    if (_rad > totalOnAuctionDebt) revert AccEng_InsufficientDebt();

    uint256 _coinBalance = safeEngine.coinBalance(address(this));

    if (_rad > _coinBalance) revert AccEng_InsufficientSurplus();

    safeEngine.settleDebt(_rad);
    totalOnAuctionDebt -= _rad;

    emit CancelDebt(_rad, _coinBalance - _rad, safeEngine.debtBalance(address(this)));
  }

```

[159-170](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L159C1-L170C4)

```diff
File : contracts/AccountingEngine.sol

    function cancelAuctionedDebtWithSurplus(uint256 _rad) external {
+       uint256 _totalOnAuctionDebt = totalOnAuctionDebt;
-        if (_rad > totalOnAuctionDebt) revert AccEng_InsufficientDebt();
+        if (_rad > _totalOnAuctionDebt) revert AccEng_InsufficientDebt();

        uint256 _coinBalance = safeEngine.coinBalance(address(this));

        if (_rad > _coinBalance) revert AccEng_InsufficientSurplus();

        safeEngine.settleDebt(_rad);
-        totalOnAuctionDebt -= _rad;
+        totalOnAuctionDebt = _totalOnAuctionDebt -_rad;

    emit CancelDebt(_rad, _coinBalance - _rad, safeEngine.debtBalance(address(this)));
  }

```

```solidity

File : contracts/AccountingEngine.sol

  function auctionDebt() external returns (uint256 _id) {
    if (_params.debtAuctionBidSize == 0) revert AccEng_DebtAuctionDisabled();

    uint256 _coinBalance = safeEngine.coinBalance(address(this));
    uint256 _debtBalance = safeEngine.debtBalance(address(this));

    if (_params.debtAuctionBidSize > _unqueuedUnauctionedDebt(_debtBalance)) revert AccEng_InsufficientDebt();

    (_coinBalance, _debtBalance) = _settleDebt(_coinBalance, _debtBalance, _coinBalance);
    totalOnAuctionDebt += _params.debtAuctionBidSize;

    _id = debtAuctionHouse.startAuction({
      _incomeReceiver: address(this),
      _amountToSell: _params.debtAuctionMintedTokens,
      _initialBid: _params.debtAuctionBidSize
    });

    emit AuctionDebt(_id, _params.debtAuctionMintedTokens, _params.debtAuctionBidSize);
  }

```

[175-193](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L175C1-L193C4)

```diff
File : contracts/AccountingEngine.sol

  function auctionDebt() external returns (uint256 _id) {
+
+    uint256 _debtAuctionBidSize = _params.debtAuctionBidSize;
-    if (_params.debtAuctionBidSize == 0) revert AccEng_DebtAuctionDisabled();
+    if (_debtAuctionBidSize == 0) revert AccEng_DebtAuctionDisabled();

    uint256 _coinBalance = safeEngine.coinBalance(address(this));
    uint256 _debtBalance = safeEngine.debtBalance(address(this));

-    if (_params.debtAuctionBidSize > _unqueuedUnauctionedDebt(_debtBalance)) revert AccEng_InsufficientDebt();
+    if (_debtAuctionBidSize > _unqueuedUnauctionedDebt(_debtBalance)) revert AccEng_InsufficientDebt();

    (_coinBalance, _debtBalance) = _settleDebt(_coinBalance, _debtBalance, _coinBalance);
-    totalOnAuctionDebt += _params.debtAuctionBidSize;
+    totalOnAuctionDebt += _debtAuctionBidSize;

+    uint256 _debtAuctionMintedTokens = _params.debtAuctionMintedTokens;
    _id = debtAuctionHouse.startAuction({
      _incomeReceiver: address(this),
-      _amountToSell: _params.debtAuctionMintedTokens,
+      _amountToSell: _debtAuctionMintedTokens,
-      _initialBid: _params.debtAuctionBidSize
+      _initialBid: _debtAuctionBidSize
    });

-    emit AuctionDebt(_id, _params.debtAuctionMintedTokens, _params.debtAuctionBidSize);
+    emit AuctionDebt(_id, _debtAuctionMintedTokens, _debtAuctionBidSize);
  }

```

```solidity

File : contracts/AccountingEngine.sol

 function transferPostSettlementSurplus() external whenDisabled {
    if (address(postSettlementSurplusDrain) == address(0)) revert AccEng_NullSurplusReceiver();
    if (block.timestamp < disableTimestamp + _params.disableCooldown) revert AccEng_PostSettlementCooldown();

    uint256 _coinBalance = safeEngine.coinBalance(address(this));
    uint256 _debtBalance = safeEngine.debtBalance(address(this));
    uint256 _debtToSettle = Math.min(_coinBalance, _debtBalance);
    (_coinBalance,) = _settleDebt(_coinBalance, _debtBalance, _debtToSettle);

    if (_coinBalance > 0) {
      safeEngine.transferInternalCoins({
        _source: address(this),
        _destination: postSettlementSurplusDrain,
        _rad: _coinBalance
      });

      emit TransferSurplus(postSettlementSurplusDrain, _coinBalance);
    }
  }

```

[260-278](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L260C2-L278C4)

```diff
File : contracts/AccountingEngine.sol

 function transferPostSettlementSurplus() external whenDisabled {
    if (address(postSettlementSurplusDrain) == address(0)) revert AccEng_NullSurplusReceiver();
    if (block.timestamp < disableTimestamp + _params.disableCooldown) revert AccEng_PostSettlementCooldown();

    uint256 _coinBalance = safeEngine.coinBalance(address(this));
    uint256 _debtBalance = safeEngine.debtBalance(address(this));
    uint256 _debtToSettle = Math.min(_coinBalance, _debtBalance);
    (_coinBalance,) = _settleDebt(_coinBalance, _debtBalance, _debtToSettle);

    if (_coinBalance > 0) {
+     address _postSettlementSurplusDrain = postSettlementSurplusDrain;
      safeEngine.transferInternalCoins({
        _source: address(this),
-        _destination: postSettlementSurplusDrain,
+        _destination: _postSettlementSurplusDrain,
        _rad: _coinBalance
      });

-      emit TransferSurplus(postSettlementSurplusDrain, _coinBalance);
+      emit TransferSurplus(_postSettlementSurplusDrain, _coinBalance);
    }
  }

```

</details>

## [G-03] Structs can be packed into fewer storage slots

The EVM works with 32 byte words. Variables less than 32 bytes can be declared next to each other in storage and this will pack the values together into a single 32 byte storage slot (if values combined are <= 32 bytes). If the variables packed together are retrieved together in functions (more likely with structs), we will effectively save ~2000 gas with every subsequent SLOAD for that storage slot. This is due to us incurring a Gwarmaccess (100 gas) versus a Gcoldsload (2100 gas).

_4 Instances in 1 File_

### Note : `AccountingEngineParams` struct defined in interface `IAccountingEngine.sol` which is not in scope but it is meant to used here taking space in scope contract `AccountingEngine.sol` storage slots so we will consider`AccountingEngineParams` struct definition in scope, reduce the size of this to save `space` in scoped contract `AccountingEngine.sol`by truncating the size of `AccountingEngineParams` struct variables.

```solidity
File : src/contracts/AccountingEngine.sol

58: AccountingEngineParams public _params;

```

[58](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L58)

### Reduce uint type for `surplusTransferPercentage`, `surplusDelay`, `popDebtDelay` and `disableCooldown` from `uint256` to `uint48` to save 3 SLOT (~6000 Gas)

`surplusDelay`, `popDebtDelay` and `disableCooldown` are holding time in seconds so `uint48` is more than sufficient to hold it. `surplusTransferPercentage` is holding _%_ from 1 to 100 so `uint48` will also be enough to hold it even including precision if applied. By doing this we can save 3 storage slots.

```solidity
File : src/interfaces/IAccountingEngine.sol

struct AccountingEngineParams {
    // percent of the Surplus the system transfers instead of auctioning [0/100]
    uint256 surplusTransferPercentage;
    // Delay between surplus actions
    uint256 surplusDelay;
    // Delay after which debt can be popped from debtQueue
    uint256 popDebtDelay;
    // Time to wait (post settlement) until any remaining surplus can be transferred to the settlement auctioneer
    uint256 disableCooldown;
    // Amount of surplus stability fees transferred or sold in one surplus auction
    uint256 surplusAmount;
    // Amount of stability fees that need to accrue in this contract before any surplus auction can start
    uint256 surplusBuffer;
    // Amount of protocol tokens to be minted post-auction
    uint256 debtAuctionMintedTokens;
    // Amount of debt sold in one debt auction (initial coin bid for debtAuctionMintedTokens protocol tokens)
    uint256 debtAuctionBidSize;
  }

```

[95-112](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/interfaces/IAccountingEngine.sol#L95C3-L112C4)

```diff
File : src/interfaces/IAccountingEngine.sol

struct AccountingEngineParams {
-    // percent of the Surplus the system transfers instead of auctioning [0/100]
-    uint256 surplusTransferPercentage;
-    // Delay between surplus actions
-    uint256 surplusDelay;
-    // Delay after which debt can be popped from debtQueue
-    uint256 popDebtDelay;
-    // Time to wait (post settlement) until any remaining surplus can be transferred to the settlement auctioneer
-    uint256 disableCooldown;
+    // percent of the Surplus the system transfers instead of auctioning [0/100]
+    uint48 surplusTransferPercentage;
+    // Delay between surplus actions
+    uint48 surplusDelay;
+    // Delay after which debt can be popped from debtQueue
+    uint48 popDebtDelay;
+    // Time to wait (post settlement) until any remaining surplus can be transferred to the settlement auctioneer
+    uint48 disableCooldown;
    // Amount of surplus stability fees transferred or sold in one surplus auction
    uint256 surplusAmount;
    // Amount of stability fees that need to accrue in this contract before any surplus auction can start
    uint256 surplusBuffer;
    // Amount of protocol tokens to be minted post-auction
    uint256 debtAuctionMintedTokens;
    // Amount of debt sold in one debt auction (initial coin bid for debtAuctionMintedTokens protocol tokens)
    uint256 debtAuctionBidSize;
  }

```

## [G-04] State variables can be packed into fewer storage slots.

The EVM works with 32 byte words. Variables less than 32 bytes can be declared next to eachother in storage and this will pack the values together into a single 32 byte storage slot (if the values combined are <= 32 bytes). If the variables packed together are retrieved together in functions, we will effectively save ~2000 gas with every subsequent SLOAD for that storage slot. This is due to us incurring a Gwarmaccess (100 gas) versus a Gcoldsload (2100 gas).

If variables occupying the same slot are both written the same function or by the constructor, avoids a separate Gsset (20000 gas). Reads of the variables can also be cheaper

_2 Instances in 1 File_

**Note : In bot-report only `lastSurplusTime` and `disableTimestamp` is truncated and packed into 1 SLOT which saves only 1 SLOT. But by further re-arranging them and pack both of them with `address public extraSurplusReceiver` which can save another storage SLOT. This re-arrangement of these variable missed by bot report. So I am including this in my report by showing another extra SLOT saving**

### Reduce uint type for `lastSurplusTime` and `disableTimestamp` by truncating timestamp from `uint256` to `uint48` and pack with `address public extraSurplusReceiver` to save 2 SLOT.

```solidity
File : src/contracts/AccountingEngine.sol

50:  address public postSettlementSurplusDrain;

52:  address public extraSurplusReceiver;

  ...

74:  uint256 public lastSurplusTime;

76:  uint256 public disableTimestamp;


```

[50-76](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L50C3-L77C1)

```diff
File : src/contracts/AccountingEngine.sol

  address public postSettlementSurplusDrain;
  /// @inheritdoc IAccountingEngine
  address public extraSurplusReceiver;
+ uint48 public lastSurplusTime;
+ uint48 public disableTimestamp;

  // --- Params ---

  /// @inheritdoc IAccountingEngine
  // solhint-disable-next-line private-vars-leading-underscore
  AccountingEngineParams public _params;

  /// @inheritdoc IAccountingEngine
  function params() external view returns (AccountingEngineParams memory _accEngineParams) {
    return _params;
  }

  // --- Data ---

  /// @inheritdoc IAccountingEngine
  mapping(uint256 _timestamp => uint256 _rad) public debtQueue;
  /// @inheritdoc IAccountingEngine
  uint256 public /* RAD */ totalOnAuctionDebt;
  /// @inheritdoc IAccountingEngine
  uint256 public /* RAD */ totalQueuedDebt;
  /// @inheritdoc IAccountingEngine
- uint256 public lastSurplusTime;
  /// @inheritdoc IAccountingEngine
- uint256 public disableTimestamp;


```

## [G-05] Missing `zero-address` check in `constructor`

Missing checks for zero-addresses may lead to infunctional protocol, if the variable addresses are updated incorrectly. It also waste gas as it requires the redeployment of the contract.

_1 Instances in 1 File_

### Check `_vault721` for address 0

```solidity
File : src/contracts/proxies/ODSafeManager.sol

64:  constructor(address _safeEngine, address _vault721) {
65:   safeEngine = _safeEngine.assertNonNull();
66:   vault721 = IVault721(_vault721);
67:   vault721.initializeManager();
68:   }

```

[64-68](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L64C3-L68C4)

## [G-06] Custom `error`s cost less than `require`/`assert`

Consider the use of a custom `error`, as it leads to a cheaper deploy cost and run time cost. The run time cost is only relevant when the revert condition is met.

**Note: These instances missed by bot-report**

_1 Instance in 1 File_

```solidity

File : contracts/proxies/Vault721.sol

   require(_safeManager != address(0));//@audit use custom error

```

[111](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L111C4-L111C41)

## [G-07] Revert Transaction as soon as possible

Always try reverting transactions as early as possible when using require/if statements. In case a transaction revert occurs, the user will pay the gas up until the revert was executed

_3 Instances in 2 Files_

```diff
File : contracts/proxies/ODSafeManager.sol

 function transferSAFEOwnership(uint256 _safe, address _dst) external {
+   if (_dst == address(0)) revert ZeroAddress();
    require(msg.sender == address(vault721), 'SafeMngr: Only Vault721');

-   if (_dst == address(0)) revert ZeroAddress();
    SAFEData memory _sData = _safeData[_safe];
    if (_dst == _sData.owner) revert AlreadySafeOwner();

    _usrSafes[_sData.owner].remove(_safe);
    _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);

    _usrSafes[_dst].add(_safe);
    _usrSafesPerCollat[_dst][_sData.collateralType].add(_safe);

    _safeData[_safe].owner = _dst;

    emit TransferSAFEOwnership(msg.sender, _safe, _dst);
  }

```

[136-152](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L136C2-L152C4)

```diff
File : contracts/AccountingEngine.sol

 function auctionDebt() external returns (uint256 _id) {
    if (_params.debtAuctionBidSize == 0) revert AccEng_DebtAuctionDisabled();
+
+    uint256 _debtBalance = safeEngine.debtBalance(address(this));
+
+    if (_params.debtAuctionBidSize > _unqueuedUnauctionedDebt(_debtBalance)) revert AccEng_InsufficientDebt();
+
     uint256 _coinBalance = safeEngine.coinBalance(address(this));
-    uint256 _debtBalance = safeEngine.debtBalance(address(this));

-    if (_params.debtAuctionBidSize > _unqueuedUnauctionedDebt(_debtBalance)) revert AccEng_InsufficientDebt();

    (_coinBalance, _debtBalance) = _settleDebt(_coinBalance, _debtBalance, _coinBalance);
    totalOnAuctionDebt += _params.debtAuctionBidSize;

    _id = debtAuctionHouse.startAuction({
      _incomeReceiver: address(this),
      _amountToSell: _params.debtAuctionMintedTokens,
      _initialBid: _params.debtAuctionBidSize
    });

    emit AuctionDebt(_id, _params.debtAuctionMintedTokens, _params.debtAuctionBidSize);
  }

```

[175-193](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L175C1-L193C4)

## [G-08] Cache `calculated` value instead of recalculate

_7 Instances in 1 File_

<details>
<summary>see instances</summary>

```solidity

File : contracts/proxies/actions/BasicActions.sol

    // If there was already enough COIN in the safeEngine balance, just exits it without adding more debt
    if (_coinAmount < _deltaWad * RAY) {
      // Calculates the needed deltaDebt so together with the existing coins in the safeEngine is enough to exit wad amount of COIN tokens
      _deltaDebt = ((_deltaWad * RAY - _coinAmount) / _rate).toInt();
      // This is neeeded due lack of precision. It might need to sum an extra deltaDebt wei (for the given COIN wad amount)
      _deltaDebt = uint256(_deltaDebt) * _rate < _deltaWad * RAY ? _deltaDebt + 1 : _deltaDebt;
    }
  }

```

[39-47](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L39C1-L47C4)

```diff
File : contracts/proxies/actions/BasicActions.sol

+    uint256 _delta = _deltaWad * RAY;
    // If there was already enough COIN in the safeEngine balance, just exits it without adding more debt
-    if (_coinAmount < _deltaWad * RAY) {
+    if (_coinAmount < _delta) {
      // Calculates the needed deltaDebt so together with the existing coins in the safeEngine is enough to exit wad amount of COIN tokens
-      _deltaDebt = ((_deltaWad * RAY - _coinAmount) / _rate).toInt();
+      _deltaDebt = ((_delta - _coinAmount) / _rate).toInt();
      // This is neeeded due lack of precision. It might need to sum an extra deltaDebt wei (for the given COIN wad amount)
-      _deltaDebt = uint256(_deltaDebt) * _rate < _deltaWad * RAY ? _deltaDebt + 1 : _deltaDebt;
+      _deltaDebt = uint256(_deltaDebt) * _rate < _delta ? _deltaDebt + 1 : _deltaDebt;
    }
  }

```

```solidity

File : contracts/proxies/actions/BasicActions.sol

   function _collectAndExitCoins(address _manager, address _coinJoin, uint256 _safeId, uint256 _deltaWad) internal {
      // Moves the COIN amount to proxy's address
      _transferInternalCoins(_manager, _safeId, address(this), _deltaWad * RAY);
      // Exits the COIN amount to the user's address
      _exitSystemCoins(_coinJoin, _deltaWad * RAY);
  }

```

[201-206](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L201C3-L206C4)

```diff
File : contracts/proxies/actions/BasicActions.sol

   function _collectAndExitCoins(address _manager, address _coinJoin, uint256 _safeId, uint256 _deltaWad) internal {
+      uint256 _delta =  _deltaWad * RAY;
      // Moves the COIN amount to proxy's address
-      _transferInternalCoins(_manager, _safeId, address(this), _deltaWad * RAY);
+      _transferInternalCoins(_manager, _safeId, address(this), _delta);
      // Exits the COIN amount to the user's address
-      _exitSystemCoins(_coinJoin, _deltaWad * RAY);
+      _exitSystemCoins(_coinJoin, _delta);
  }

```

</details>

## [G-09] Remove unused `import` variable

_5 Instances in 1 File_

<details>
<summary>see instances</summary>

```solidity

File : contracts/proxies/actions/BasicActions.sol

05:     import {ODProxy} from '@contracts/proxies/ODProxy.sol';//@audit remove ODProxy

        import {ISAFEEngine} from '@interfaces/ISAFEEngine.sol';
08:     import {ICoinJoin} from '@interfaces/utils/ICoinJoin.sol';//@audit remove ICoinJoin
        import {ITaxCollector} from '@interfaces/ITaxCollector.sol';
10:     import {ICollateralJoin} from '@interfaces/utils/ICollateralJoin.sol';//@audit remove ICollateralJoin
11:     import {IERC20Metadata} from '@openzeppelin/token/ERC20/extensions/IERC20Metadata.sol';//@audit remove IERC20Metadata
        import {IBasicActions} from '@interfaces/proxies/actions/IBasicActions.sol';

14:     import {Math, WAD, RAY, RAD} from '@libraries/Math.sol';//@audit remove RAD

```

[05-14](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L5C1-L14C57)

</details>

## [G-10] Remove unused function `parameter` to save gas

_1 Instance in 1 File_

```solidity

File : contracts/proxies/Vault721.sol

//@audit remove `batchSize`
  function _afterTokenTransfer(address from, address to, uint256 firstTokenId, uint256 batchSize) internal override {
      require(to != address(0), 'V721: no burn');
      if (from != address(0)) {
       address payable proxy;

       if (_isNotProxy(to)) {
        proxy = _build(to);
      } else {
        proxy = payable(_userRegistry[to]);
      }
      IODSafeManager(safeManager).transferSAFEOwnership(firstTokenId, address(proxy));
     }
   }

```

[187-199](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L187C3-L199C4)

## [G-11] Use `calldata` instead of `memory` for function arguments that do not get mutated

When you specify a data location as memory, that value will be copied into memory. When you specify the location as calldata, the value will stay static within calldata. If the value is a large, complex type, using memory may result in extra memory expansion costs.

_1 Instance in 1 File_

```diff
File : contracts/proxies/ODProxy.sol

-    function execute(address _target, bytes memory _data) external payable onlyOwner returns (bytes memory _response) {
+    function execute(address _target, bytes calldata _data) external payable onlyOwner returns (bytes memory _response) {

```

[26](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODProxy.sol#L26C2-L26C118)

## [G-12] Don’t `cache` value if it is only used once

If a value is only intended to be used once then it should not be cached. Caching the value will result in unnecessary stack manipulation.

_6 Instances in 2 Files_

<details>
<summary>see instances</summary>

```solidity

File : contracts/proxies/actions/BasicActions.sol

    address _safeEngine = ODSafeManager(_manager).safeEngine();//@audit don't cache _safeEngine
    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

    // Generates debt in the SAFE
    _modifySAFECollateralization(
      _manager,
      _safeId,
      0,
      _getGeneratedDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
    );

    // Moves the COIN amount to user's address
    _collectAndExitCoins(_manager, _coinJoin, _safeId, _deltaWad);
  }

```

[101-115](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L101C1-L115C4)

```solidity

File : contracts/proxies/actions/BasicActions.sol

 address _safeEngine = ODSafeManager(_manager).safeEngine();//@audit don't cache _safeEngine
    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

    // Joins COIN amount into the safeEngine
    _joinSystemCoins(_coinJoin, _safeInfo.safeHandler, _deltaWad);

    // Paybacks debt to the SAFE
    _modifySAFECollateralization(
      _manager, _safeId, 0, _getRepaidDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler)
    );
  }

```

[128-139](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L128C4-L139C4)

```solidity

File : contracts/proxies/actions/BasicActions.sol

 address _safeEngine = ODSafeManager(_manager).safeEngine();//@audit don't cache _safeEngine
    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

    // Takes token amount from user's wallet and joins into the safeEngine
    _joinCollateral(_collateralJoin, _safeInfo.safeHandler, _collateralAmount);

    // Locks token amount into the SAFE and generates debt
    _modifySAFECollateralization(
      _manager,
      _safeId,
      _collateralAmount.toInt(),
      _getGeneratedDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
    );

    // Exits and transfers COIN amount to the user's address
    _collectAndExitCoins(_manager, _coinJoin, _safeId, _deltaWad);
  }

```

[179-196](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L179C4-L196C4)

```solidity

File : contracts/proxies/actions/BasicActions.sol

    address _safeEngine = ODSafeManager(_manager).safeEngine();//@audit don't cache _safeEngine
    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

    // Joins COIN amount into the safeEngine
    _joinSystemCoins(_coinJoin, _safeInfo.safeHandler, _debtWad);

    // Paybacks debt to the SAFE and unlocks token amount from it
    _modifySAFECollateralization(
      _manager,
      _safeId,
      -_collateralWad.toInt(),
      _getRepaidDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler)
    );

    // Transfers token amount to the user's address
    _collectAndExitCollateral(_manager, _collateralJoin, _safeId, _collateralWad);
  }

```

[354-371](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L354C1-L371C4)

```solidity

File : contracts/proxies/ODSafeManager.sol

    int256 _deltaCollateral = _safeInfo.lockedCollateral.toInt();//@audit don't cache _deltaCollateral
    int256 _deltaDebt = _safeInfo.generatedDebt.toInt();//@audit don't cache _deltaDebt
    ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(
      _srcData.collateralType, _srcData.safeHandler, _dstData.safeHandler, _deltaCollateral, _deltaDebt
    );

    // Remove safe from owner's list (notice it doesn't erase safe ownership)
    _usrSafes[_srcData.owner].remove(_safeSrc);
    _usrSafesPerCollat[_srcData.owner][_srcData.collateralType].remove(_safeSrc);
    emit MoveSAFE(msg.sender, _safeSrc, _safeDst);
  }

```

[222-232](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L222C4-L232C4)

</details>
