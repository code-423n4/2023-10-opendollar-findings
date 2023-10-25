# Lack of ACCES CONTROL on `addSAFE(uint256 _safe)` in `ODSafeManager` letting anyone call this function with arbitrary value

## Impact
LOW as it doesn't really impact the protocol

# addSAFE function
```js
// @audit arbitrary call to add a safe to yourself ? 
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

`addSAFE(uint256 _safe)` doesn't have the `safeAllowed(_safe)` modifier as `removeSAFE(uint256 _safe)` have.

In this case Anyone can add an arbitrary safe to his `_usrSafes[]` list.

## Recommended Mitigation Steps

Add the modifier `safeAllowed(_safe)` to the `addSAFE(uint256 _safe)`.