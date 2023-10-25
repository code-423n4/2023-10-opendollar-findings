Only owner of safe should be allowed to addSafe in ODSafeManager.sol.
```
  function addSAFE(uint256 _safe) external {
    SAFEData memory _sData = _safeData[_safe];
    _usrSafes[msg.sender].add(_safe);
    _usrSafesPerCollat[msg.sender][_sData.collateralType].add(_safe);
  }
```
This will prevent unwanted safes to be added against an owners consent.