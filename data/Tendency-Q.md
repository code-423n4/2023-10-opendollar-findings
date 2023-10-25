## [L01] Anyone can Add any Safe Among Their Owned Safes

`ODSafeManager::addSAFE` function allows anyone to add any safe amongst their owned safes.

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L235-L239

### RECOMMENDATION

Only a safe owner should be allowed to add a safe to their safe list.
```
    function addSAFE(uint256 _safe) external {
        SAFEData memory _sData = _safeData[_safe];
        require(_sData.owner == msg.sender, 'Not owner');
        _usrSafes[msg.sender].add(_safe);
        _usrSafesPerCollat[msg.sender][_sData.collateralType].add(_safe);
    }

```

