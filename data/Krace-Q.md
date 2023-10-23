### [Low Risk](#low-risk-1)

| Total Low Risk Issues | 2 |
|:--:|:--:|

| Count | Title | Instances |
|:--:|:-------| :--: |
| [L-01](#l-01-precision-loss-in-auctionsurplus) | Precision loss in auctionSurplus | 1 |
| [L-02](#l-02-frontrun-attack-in-vault721) | Frontrun attack in Vault721 | 2 |


### [Non-Critical](#non-critical-1)

| Total Non-Critical Issues | 2 |
|:--:|:--:|

| Count | Title | Instances |
|:--:|:-------| :--: |
| [NC-01](#nc-01-redundant-checks-in-auctionsurplus) | Redundant checks in auctionSurplus | 1 |
| [NC-02](#nc-02-useless-check-in-auctionsurplus) | Useless check in auctionSurplus | 1 |

## Low Risk

### [L-01] Precision loss in auctionSurplus

[`auctionSurplus()`](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L230) will calculate the `_rad` by: `surplusAmount` * `surplusTransferPercentage` / `WAD`. But there is no checks to ensure that the result of multiplication is bigger than `WAD`, so precision loss may occur when the owner wrongly sets the `_params`.

```
    if (_params.surplusTransferPercentage > 0) {
      if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver();

      safeEngine.transferInternalCoins({
        _source: address(this),
        _destination: extraSurplusReceiver,
        _rad: _params.surplusAmount.wmul(_params.surplusTransferPercentage)  // @audit if surplusTransferPercentage * surplusAmount is less than WAD, precisionl will loss
      });

      lastSurplusTime = block.timestamp;
      emit TransferSurplus(extraSurplusReceiver, _params.surplusAmount.wmul(_params.surplusTransferPercentage));
    }
```

```
  function wmul(uint256 _x, uint256 _y) internal pure returns (uint256 _wmul) {
    return (_x * _y) / WAD;
  }
```


### [L-02] Frontrun attack in Vault721

[`initializeManager`](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L56) and [`initializeRenderer`](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L72) can be invoked by anyone when the address is not set.

Attack could frontrun this function to set the corresponding address.

```
  /**
   * @dev initializes SafeManager contract
   */
  function initializeManager() external {
    if (address(safeManager) == address(0)) _setSafeManager(msg.sender);
  }
```
```
  /**
   * @dev initializes NFTRenderer contract
   */
  function initializeRenderer() external {
    if (address(nftRenderer) == address(0)) _setNftRenderer(msg.sender);
  }
```


## Non-critical

### [NC-01] Redundant checks in auctionSurplus

[`auctionSurplus()`](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L198) has a redundant checks at [line 201](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L201) and [line 225](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L225). And the second check is useless.

```
  function auctionSurplus() external returns (uint256 _id) {
    if(_params.surplusTransferPercentage > WAD) revert AccEng_surplusTransferPercentOverLimit();
    if (_params.surplusAmount == 0) revert AccEng_NullAmount();
    if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver(); //@audit redundant
    [....]
    // transfer surplus percentage
    if (_params.surplusTransferPercentage > 0) {
      if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver(); //@audit redundant
```



### [NC-02] Useless check in auctionSurplus

This [check](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L213) is useless because there is a more strict [check](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L199) at the begin of function `auctionSurplus`.

```
  function auctionSurplus() external returns (uint256 _id) {
    if(_params.surplusTransferPercentage > WAD) revert AccEng_surplusTransferPercentOverLimit();  //@audit this requires that surplusTransferPercentage <= WAD
    if (_params.surplusAmount == 0) revert AccEng_NullAmount();
    if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver();
    if (block.timestamp < lastSurplusTime + _params.surplusDelay) revert AccEng_SurplusCooldown();

    uint256 _coinBalance = safeEngine.coinBalance(address(this));
    uint256 _debtBalance = safeEngine.debtBalance(address(this));
    (_coinBalance, _debtBalance) = _settleDebt(_coinBalance, _debtBalance, _unqueuedUnauctionedDebt(_debtBalance));

    if (_coinBalance < _debtBalance + _params.surplusAmount + _params.surplusBuffer) {
      revert AccEng_InsufficientSurplus();
    }

    // auction surplus percentage
    if (_params.surplusTransferPercentage < ONE_HUNDRED_WAD) {
    //@audit this is useless because surplusTransferPercentage is strictly less than WAD, ONE_HUNDRED_WAD is 100*WAD
      _id = surplusAuctionHouse.startAuction({
        _amountToSell: _params.surplusAmount.wmul(ONE_HUNDRED_WAD - _params.surplusTransferPercentage),
        _initialBid: 0
      });

      lastSurplusTime = block.timestamp;
      emit AuctionSurplus(_id, 0, _params.surplusAmount.wmul(ONE_HUNDRED_WAD - _params.surplusTransferPercentage));
    }
```