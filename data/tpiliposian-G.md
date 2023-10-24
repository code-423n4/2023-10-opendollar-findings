## Potential gas loss because of missing checks

### Description

In the `AccountingEngine.sol` contract there is a function `_setSurplusAuctionHouse` that is setting the new surplus auction house:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L305-L311

```solidity
  function _setSurplusAuctionHouse(address _surplusAuctionHouse) internal {
    if (address(surplusAuctionHouse) != address(0)) {
      safeEngine.denySAFEModification(address(surplusAuctionHouse));
    }
    surplusAuctionHouse = ISurplusAuctionHouse(_surplusAuctionHouse);
    safeEngine.approveSAFEModification(_surplusAuctionHouse);
  }
```

Adding the check to ensure that the new `_surplusAuctionHouse` parameter is not the old one can save a lot of gas, as there would not be a reason to call `ISurplusAuctionHouse()` and `approveSAFEModification()` functions.

## Remediation

Add a check to ensure that the new `_surplusAuctionHouse` is not equal to the existing `surplusAuctionHouse`.
