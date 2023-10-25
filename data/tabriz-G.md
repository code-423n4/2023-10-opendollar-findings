## [G-01] Make for loop unchecked
The risk of for loops getting overflowed is extremely low. Because it always increments by 1 and is
limited to the arrays length. Even if the arrays are extremely long, it will take a massive amount of time
and gas to let the for loop overflow.

```
 for (uint256 _i; _i < _safes.length; _i++) {
      _safeHandlers[_i] = _safeData[_safes[_i]].safeHandler;
      _cTypes[_i] = _safeData[_safes[_i]].collateralType;
    }
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L91C4-L94C6

## [G-02] Rearranging order of state variable declarations to pack them will
save storage slots and gas
put them in follwing order
uint64
bool
address

```
contract AccountingEngine is Authorizable, Modifiable, Disableable, IAccountingEngine {
  using Encoding for bytes;
  using Assertions for address;
  using Math for uint256;

  uint256 internal constant ONE_HUNDRED_WAD = 100 * WAD;
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L23C1-L28C57

```
contract CamelotRelayer is IBaseOracle, ICamelotRelayer {
  // --- Registry ---
  address internal constant _CAMELOT_FACTORY = GOERLI_CAMELOT_V3_FACTORY;

  /// @inheritdoc ICamelotRelayer
  address public camelotPair;
  /// @inheritdoc ICamelotRelayer
  address public baseToken;
  /// @inheritdoc ICamelotRelayer
  address public quoteToken;

  // --- Data ---
  /// @inheritdoc IBaseOracle
  string public symbol;

  /// @inheritdoc ICamelotRelayer
  uint128 public baseAmount;
  /// @inheritdoc ICamelotRelayer
  uint256 public multiplier;
  /// @inheritdoc ICamelotRelayer
  uint32 public quotePeriod;
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L18C1-L38C29

```
contract UniV3Relayer is IBaseOracle, IUniV3Relayer {
  // --- Registry ---
  address internal constant _UNI_V3_FACTORY = GOERLI_UNISWAP_V3_FACTORY;

  /// @inheritdoc IUniV3Relayer
  address public uniV3Pool;
  /// @inheritdoc IUniV3Relayer
  address public baseToken;
  /// @inheritdoc IUniV3Relayer
  address public quoteToken;

  // --- Data ---

  /// @inheritdoc IBaseOracle
  string public symbol;

  /// @inheritdoc IUniV3Relayer
  uint128 public baseAmount;
  /// @inheritdoc IUniV3Relayer
  uint256 public multiplier;
  /// @inheritdoc IUniV3Relayer
  uint32 public quotePeriod;

```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L16C1-L38C1

## [G-03] Caching global variables is expensive than using the variable itself.

```
  lastSurplusTime = block.timestamp;
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L219C5-L219C41

```
 lastSurplusTime = block.timestamp;
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L233C6-L233C41

```
  disableTimestamp = block.timestamp;

```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L250C3-L251C1

## [G-04] Use hardcode address instead address(this)
Instead of using address(this), it is more gas-efficient to pre-calculate and use the hardcoded address. Foundry’s script.sol and solmate’s LibRlp.sol contracts can help achieve this.

```
 return _unqueuedUnauctionedDebt(safeEngine.debtBalance(address(this)));
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L105C4-L105C76

```
 function settleDebt(uint256 _rad) external {
    _settleDebt(safeEngine.coinBalance(address(this)), safeEngine.debtBalance(address(this)), _rad);
  }

```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L139C2-L142C1

```
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
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L178C3-L190C8

```
uint256 _coinBalance = safeEngine.coinBalance(address(this));
    uint256 _debtBalance = safeEngine.debtBalance(address(this));
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L204C5-L205C66

```
  _source: address(this),
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L228C7-L228C32

```
  uint256 _debtToSettle = Math.min(safeEngine.coinBalance(address(this)), safeEngine.debtBalance(address(this)));
    safeEngine.settleDebt(_debtToSettle);
  }

  /// @inheritdoc IAccountingEngine
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
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L255C3-L274C10

```
 _transferInternalCoins(_manager, _safeId, address(this), _deltaWad * RAY);
    // Exits the COIN amount to the user's address
    _exitSystemCoins(_coinJoin, _deltaWad * RAY);
  }

  /**
   * @notice Transfers an amount of collateral to the proxy address and exits collateral tokens to the user
   */
  function _collectAndExitCollateral(
    address _manager,
    address _collateralJoin,
    uint256 _safeId,
    uint256 _deltaWad
  ) internal {
    // Moves the amount from the SAFE handler to proxy's address
    _transferCollateral(_manager, _safeId, address(this), _deltaWad);
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L203C4-L218C70

```
   address(this),
      _getRepaidDebt(_safeEngine, address(this), _safeInfo.collateralType, _safeInfo.safeHandler)
    );

```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L297C4-L300C1


```
  _collateralSource: address(this),
      _debtDestination: address(this),
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L305C5-L306C39

```
 _safe = _openSAFE(_manager, _cType, address(this));
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L337C4-L337C56

 ## [G-05] For uint use != 0 instead of > 0
 
```
 if (_params.surplusTransferPercentage > 0) {
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L224C4-L224C49

```
  if (_coinBalance > 0) {
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L269C3-L269C28

## [G-06] Avoid unnecessary read of array length in for loops can save gas.

```
 for (uint256 _i; _i < _safes.length; _i++) {
      _safeHandlers[_i] = _safeData[_safes[_i]].safeHandler;
      _cTypes[_i] = _safeData[_safes[_i]].collateralType;
    }
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L91C4-L94C6

## [G-07]  `10 ** multiplier` can be changed to `1emultiplier` and save some gas.

```
 return _quoteResult * 10 ** multiplier;
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L104C2-L104C44

```
 return _quoteResult * 10 ** multiplier;
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L111C4-L111C44

```
 baseAmount = uint128(10 ** IERC20Metadata(_baseToken).decimals());
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L57C4-L57C71

## [G-08] Make fewer external calls.

Every call to an external contract costs a decent amount of gas. For optimization of gas usage, It’s better to call one function and have it return all the data you need rather than calling a separate function for every piece of data. This might go against the best coding practices for other languages, but solidity is special.

## [G-9] `variable == false` instead of `!variable`.

a bit cheapier when you replace:

```
  if (!_succeeded) {
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODProxy.sol#L32C3-L32C23

```
 if (!_isNotProxy(msg.sender)) revert 
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L78

```
    if (!_isNotProxy(_user)) revert 
  
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L86

```
 if (msg.sender != _handler && handlerCan[_handler][msg.sender] == 0) revert HandlerNotAllowed();
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L60C4-L60C101

## [G-10] Use assembly for math (add, sub, mul, div)
Use assembly for math instead of Solidity. You can check for overflow/underflow in assembly to ensure safety. If using Solidity versions < 0.8.0 and you are using Safemath, you can gain significant gas savings by using assembly to calculate values and checking for overflow/underflow.

```
 return _quoteResult * 10 ** multiplier;
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L104C4-L104C44

```
 if (_coinAmount < _deltaWad * RAY) {
      // Calculates the needed deltaDebt so together with the existing coins in the safeEngine is enough to exit wad amount of COIN tokens
      _deltaDebt = ((_deltaWad * RAY - _coinAmount) / _rate).toInt();
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L41C4-L43C70

```
  _deltaWad = _deltaWad * RAY < _rad ? _deltaWad + 1 : _deltaWad;
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L87C3-L87C68