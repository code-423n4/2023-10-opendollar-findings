### [Gas-0] Use `delete` instead of setting variables to their default value
```diff
  function popDebtFromQueue(uint256 _debtBlockTimestamp) external {
    if (block.timestamp < _debtBlockTimestamp + _params.popDebtDelay) revert AccEng_PopDebtCooldown();

    uint256 _debtBlock = debtQueue[_debtBlockTimestamp];

    if (_debtBlock == 0) revert AccEng_NullAmount();

    totalQueuedDebt = totalQueuedDebt - _debtBlock;
-   debtQueue[_debtBlockTimestamp] = 0;

+   delete debtQueue[_debtBlockTimestamp];
```
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L131
```
```diff
  function _onContractDisable() internal override { // @audit-ok
    totalQueuedDebt = 0; // @audit
    totalOnAuctionDebt = 0;
```
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L247-L249
```

### [Gas-0] Store state variable in stack memory and use it further [Here i mention those instances which are missing in Bot Report]
```diff
  function auctionDebt() external returns (uint256 _id) {
+   uint256 debtAuctionBidSize = _params.debtAuctionBidSize;
+   uint256 debtAuctionBidSize = _params.debtAuctionBidSize;

-   if (_params.debtAuctionBidSize == 0) revert AccEng_DebtAuctionDisabled();
+   if (debtAuctionBidSize == 0) revert AccEng_DebtAuctionDisabled();

    uint256 _coinBalance = safeEngine.coinBalance(address(this));
    uint256 _debtBalance = safeEngine.debtBalance(address(this));

-   if (_params.debtAuctionBidSize > _unqueuedUnauctionedDebt(_debtBalance)) revert AccEng_InsufficientDebt();
+   if (debtAuctionBidSize > _unqueuedUnauctionedDebt(_debtBalance)) revert AccEng_InsufficientDebt();

    (_coinBalance, _debtBalance) = _settleDebt(_coinBalance, _debtBalance, _coinBalance);
-   totalOnAuctionDebt += _params.debtAuctionBidSize;
+   totalOnAuctionDebt += debtAuctionBidSize;

    _id = debtAuctionHouse.startAuction({
      _incomeReceiver: address(this),
-     _amountToSell: _params.debtAuctionMintedTokens,
-     _initialBid: _params.debtAuctionBidSize // @audit
+     _amountToSell: debtAuctionMintedTokens,
+     _initialBid: debtAuctionBidSize 
    });

-   emit AuctionDebt(_id, _params.debtAuctionMintedTokens, _params.debtAuctionBidSize); // @audit
+   emit AuctionDebt(_id, debtAuctionMintedTokens, debtAuctionBidSize);
  }
```
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L192
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L188-L189
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L181
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L176
```
```diff
function auctionSurplus() external returns (uint256 _id) {
+   AccountingEngineParams memory params = _params;

-   if(_params.surplusTransferPercentage > WAD) revert AccEng_surplusTransferPercentOverLimit(); 
-   if (_params.surplusAmount == 0) revert AccEng_NullAmount();
+   if(params.surplusTransferPercentage > WAD) revert AccEng_surplusTransferPercentOverLimit(); 
+   if (params.surplusAmount == 0) revert AccEng_NullAmount();
    if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver();
-   if (block.timestamp < lastSurplusTime + _params.surplusDelay) revert AccEng_SurplusCooldown();
+   if (block.timestamp < lastSurplusTime + params.surplusDelay) revert AccEng_SurplusCooldown();

    uint256 _coinBalance = safeEngine.coinBalance(address(this));
    uint256 _debtBalance = safeEngine.debtBalance(address(this));
    (_coinBalance, _debtBalance) = _settleDebt(_coinBalance, _debtBalance, _unqueuedUnauctionedDebt(_debtBalance));

-   if (_coinBalance < _debtBalance + _params.surplusAmount + _params.surplusBuffer) {
+   if (_coinBalance < _debtBalance + _params.surplusAmount + params.surplusBuffer) {
      revert AccEng_InsufficientSurplus();
    }

    // auction surplus percentage
    if (_params.surplusTransferPercentage < ONE_HUNDRED_WAD) {
      _id = surplusAuctionHouse.startAuction({
-       _amountToSell: _params.surplusAmount.wmul(ONE_HUNDRED_WAD - _params.surplusTransferPercentage),
+       _amountToSell: params.surplusAmount.wmul(ONE_HUNDRED_WAD - params.surplusTransferPercentage),
        _initialBid: 0
      });

      lastSurplusTime = block.timestamp;
-     emit AuctionSurplus(_id, 0, _params.surplusAmount.wmul(ONE_HUNDRED_WAD - _params.surplusTransferPercentage));
+     emit AuctionSurplus(_id, 0, params.surplusAmount.wmul(ONE_HUNDRED_WAD - params.surplusTransferPercentage));
    }

    // transfer surplus percentage
-   if (_params.surplusTransferPercentage > 0) {
+   if (params.surplusTransferPercentage > 0) {
      if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver();

      safeEngine.transferInternalCoins({
        _source: address(this),
        _destination: extraSurplusReceiver,
-       _rad: _params.surplusAmount.wmul(_params.surplusTransferPercentage)
+       _rad: _params.surplusAmount.wmul(params.surplusTransferPercentage)
      });

      lastSurplusTime = block.timestamp;
-     emit TransferSurplus(extraSurplusReceiver, _params.surplusAmount.wmul(_params.surplusTransferPercentage));
+     emit TransferSurplus(extraSurplusReceiver, params.surplusAmount.wmul(params.surplusTransferPercentage));
    }
  }

```
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L199
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L200
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L202
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L208
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L220
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L230
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L234
```

### [Gas-0] Use `switch` case instead of `if-else` condition which is more gas efficient
```diff
function _modifyParameters(bytes32 _param, bytes memory _data) internal override {
    uint256 _uint256 = _data.toUint256();
    address _address = _data.toAddress();

    // params
    if (_param == 'surplusTransferPercentage') _params.surplusTransferPercentage = _uint256; // @audit use switch case
    else if (_param == 'surplusDelay') _params.surplusDelay = _uint256;
    else if (_param == 'popDebtDelay') _params.popDebtDelay = _uint256;
    else if (_param == 'disableCooldown') _params.disableCooldown = _uint256;
    else if (_param == 'surplusAmount') _params.surplusAmount = _uint256;
    else if (_param == 'debtAuctionBidSize') _params.debtAuctionBidSize = _uint256;
    else if (_param == 'debtAuctionMintedTokens') _params.debtAuctionMintedTokens = _uint256;
    else if (_param == 'surplusBuffer') _params.surplusBuffer = _uint256;
    // registry
    else if (_param == 'surplusAuctionHouse') _setSurplusAuctionHouse(_address);
    else if (_param == 'debtAuctionHouse') debtAuctionHouse = IDebtAuctionHouse(_address);
    else if (_param == 'postSettlementSurplusDrain') postSettlementSurplusDrain = _address;
    else if (_param == 'extraSurplusReceiver') extraSurplusReceiver = _address;
    else revert UnrecognizedParam();
  }
```
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L288-L301
```



### [Gas-0] Store result of arithmatic operation in memory variable and then use further it instaed of calculating same expression again and again
```diff
  function _getGeneratedDeltaDebt(
    address _safeEngine,
    bytes32 _cType,
    address _safeHandler,
    uint256 _deltaWad
  ) internal view returns (int256 _deltaDebt) {
    uint256 _rate = ISAFEEngine(_safeEngine).cData(_cType).accumulatedRate;
    uint256 _coinAmount = ISAFEEngine(_safeEngine).coinBalance(_safeHandler);

    // If there was already enough COIN in the safeEngine balance, just exits it without adding more debt
+   uint256 res = _deltaWad * RAY;
-   if (_coinAmount < _deltaWad * RAY) { // @audit cache _deltaWad * RAY
+   if (_coinAmount < res) {
      // Calculates the needed deltaDebt so together with the existing coins in the safeEngine is enough to exit wad amount of COIN tokens
-     _deltaDebt = ((_deltaWad * RAY - _coinAmount) / _rate).toInt();
+     _deltaDebt = ((res - _coinAmount) / _rate).toInt();
      // This is neeeded due lack of precision. It might need to sum an extra deltaDebt wei (for the given COIN wad amount)
      _deltaDebt = uint256(_deltaDebt) * _rate < _deltaWad * RAY ? _deltaDebt + 1 : _deltaDebt;
    }
  }
```
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L41
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L43
```
```diff
  function _collectAndExitCoins(address _manager, address _coinJoin, uint256 _safeId, uint256 _deltaWad) internal {
    // Moves the COIN amount to proxy's address
+   uint256 res = _deltaWad * RAY;

-   _transferInternalCoins(_manager, _safeId, address(this), _deltaWad * RAY); 
+   _transferInternalCoins(_manager, _safeId, address(this), res);
    // Exits the COIN amount to the user's address
-   _exitSystemCoins(_coinJoin, _deltaWad * RAY);
+   _exitSystemCoins(_coinJoin, res);
  }
```
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L203-L205
```

### [Gas-0] Instead of using `var + 1`, use `var++` which is more gas efficient
```diff
-    _deltaWad = _deltaWad * RAY < _rad ? _deltaWad + 1 : _deltaWad;
+    _deltaWad = _deltaWad * RAY < _rad ? _deltaWad++ : _deltaWad;
```
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L87
```

### [Gas-0] No need to cache function call result when it only used once
```diff
 internal {
-   address _safeEngine = ODSafeManager(_manager).safeEngine(); // @audit no need to cache
    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

    // Takes token amount from user's wallet and joins into the safeEngine
    _joinCollateral(_collateralJoin, _safeInfo.safeHandler, _collateralAmount);

    // Locks token amount into the SAFE and generates debt
    _modifySAFECollateralization(
      _manager,
      _safeId,
      _collateralAmount.toInt(),
-     _getGeneratedDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
+     _getGeneratedDeltaDebt(ODSafeManager(_manager).safeEngine(), _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
    );

    // Exits and transfers COIN amount to the user's address
    _collectAndExitCoins(_manager, _coinJoin, _safeId, _deltaWad);
  }
```
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L179
```
```diff
  function repayDebtAndFreeTokenCollateral(
    address _manager,
    address _taxCollector,
    address _collateralJoin,
    address _coinJoin,
    uint256 _safeId,
    uint256 _collateralWad,
    uint256 _debtWad
  ) external delegateCall {
-   address _safeEngine = ODSafeManager(_manager).safeEngine(); // @audit no chahe
    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

    // Joins COIN amount into the safeEngine
    _joinSystemCoins(_coinJoin, _safeInfo.safeHandler, _debtWad);

    // Paybacks debt to the SAFE and unlocks token amount from it
    _modifySAFECollateralization(
      _manager,
      _safeId,
      -_collateralWad.toInt(),
-     _getRepaidDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler)
+     _getRepaidDeltaDebt(ODSafeManager(_manager).safeEngine(), _safeInfo.collateralType, _safeInfo.safeHandler)
    );

    // Transfers token amount to the user's address
    _collectAndExitCollateral(_manager, _collateralJoin, _safeId, _collateralWad);
  }
```
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L354
```

### [Gas-0] `_safeData[_safes[_i]]` could be cached inside for loop
```diff
function getSafesData(address _usr)
    external
    view
    returns (uint256[] memory _safes, address[] memory _safeHandlers, bytes32[] memory _cTypes)
  {
    _safes = _usrSafes[_usr].values();
    _safeHandlers = new address[](_safes.length);
    _cTypes = new bytes32[](_safes.length);
    for (uint256 _i; _i < _safes.length; _i++) {
+     SAFEData memory data = _safeData[_safes[_i]]; 
-     _safeHandlers[_i] = _safeData[_safes[_i]].safeHandler; 
-     _cTypes[_i] = _safeData[_safes[_i]].collateralType;

+     _safeHandlers[_i] = data.safeHandler; 
+     _cTypes[_i] = data.collateralType;
    }
```
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L92-L93
```

### [Gas-0] Uncheck those operations which will not overflow/underflow
```diff
 function openSAFE(bytes32 _cType, address _usr) external returns (uint256 _id) {
    if (_usr == address(0)) revert ZeroAddress();

-   ++_safeId;
+   unchecked{ ++_safeId; }
```
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L121
```

### [Gas-0]
```diff
```

### [Gas-0]
```diff
```

### [Gas-0]
```diff
```

### [Gas-0]
```diff
```

### [Gas-0]
```diff
```

### [Gas-0]
```diff
```