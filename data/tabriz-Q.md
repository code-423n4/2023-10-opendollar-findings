## [L-01] Divide before multiply
Solidity's integer division truncates. Thus, performing division before multiplication can lead to precision loss.

```
   _deltaDebt = ((_deltaWad * RAY - _coinAmount) / _rate).toInt();
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L43C4-L43C70

## [L-02] Stop Using Solidity's transfer() Now
**Proof Of Concept**
https://consensys.io/diligence/blog/2019/09/stop-using-soliditys-transfer-now/

```
    safeEngine.transferInternalCoins({
        _source: address(this),
        _destination: extraSurplusReceiver,
        _rad: _params.surplusAmount.wmul(_params.surplusTransferPercentage)
      });
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L227C4-L231C10

```
       emit TransferSurplus(extraSurplusReceiver, _params.surplusAmount.wmul(_params.surplusTransferPercentage));
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L234C4-L234C113

```
  function transferPostSettlementSurplus() external whenDisabled {
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L260C1-L260C67

```
   safeEngine.transferInternalCoins({
        _source: address(this),
        _destination: postSettlementSurplusDrain,
        _rad: _coinBalance
      });

      emit TransferSurplus(postSettlementSurplusDrain, _coinBalance);
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L270C4-L276C70

```
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
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L187C2-L197C87

# Non- Critical 

## [N-01] Empty blocks should be removed or Emit something
Code contains empty block

```
 // --- Init ---
  constructor() Authorizable(msg.sender) {}
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/factories/CamelotRelayerFactory.sol#L19C2-L20C44