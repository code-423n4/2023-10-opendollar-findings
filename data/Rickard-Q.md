# [L-01] Missing `whenDisabled` modifier in `_onContractDisable()`
[https://github.com/open-dollar/od-contracts/blob/dev/src/contracts/AccountingEngine.sol#L240-L257](https://github.com/open-dollar/od-contracts/blob/dev/src/contracts/AccountingEngine.sol#L240-L257)

The absence of the `whenDisabled` modifier in the _onContractDisable function could lead to unexpected behaviour when the contract is disabled. [As per the provided comment](https://github.com/open-dollar/od-contracts/blob/dev/src/contracts/AccountingEngine.sol#L241), this function is intended to be executed only when the contract is in a disabled state, usually triggered by a Global Settlement. Without the proper modifier, there is a risk that this function could be called in situations where it is not intended to be.
## Recommended Mitigation Steps
````diff
-  function _onContractDisable() internal override {
+  function _onContractDisable() internal override whenDisabled {
      totalQueuedDebt = 0;
      totalOnAuctionDebt = 0;
      disableTimestamp = block.timestamp;
  
      surplusAuctionHouse.disableContract();
      debtAuctionHouse.disableContract();
  
      uint256 _debtToSettle = Math.min(safeEngine.coinBalance(address(this)), safeEngine.debtBalance(address(this)));
      safeEngine.settleDebt(_debtToSettle);
    }
````