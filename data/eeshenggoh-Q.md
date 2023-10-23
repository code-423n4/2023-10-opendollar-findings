## Summary

There is no transfer ownership event emitted. 

## PoC
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
    }
  }
```
## Recommendation
Should emit event when transferring ownership.