1. The `ODSafeManager` contract has functions for adding and removing a safe from the user's list of safes. One of these functions uses the `safeAllowed` modifier, while the other does not. This means that users can add any safe to their list of safes, but they can only delete safes that they own. It seems that both functions should either have the modifier or neither of them should.

```solidity
/// @inheritdoc IODSafeManager
function addSAFE(uint256 _safe) external {
  SAFEData memory _sData = _safeData[_safe];
  _usrSafes[msg.sender].add(_safe);
  _usrSafesPerCollat[msg.sender][_sData.collateralType].add(_safe);
}

/// @inheritdoc IODSafeManager
function removeSAFE(uint256 _safe) external safeAllowed(_safe) {
  SAFEData memory _sData = _safeData[_safe];
  _usrSafes[_sData.owner].remove(_safe);
  _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);
}
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L234-L246

2. The `setSafeManager` function in the `Vault721` contract does not update the `NFTRenderer`'s implementation of `safeManager`, resulting in incorrect data source for rendering token URIs.

```solidity
function setSafeManager(address _safeManager) external onlyGovernor {
  _setSafeManager(_safeManager);
}

function _setSafeManager(address _safeManager) internal nonZero(_safeManager) {
  safeManager = IODSafeManager(_safeManager);
}
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L126-L128