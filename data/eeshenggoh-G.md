OWNER is set at the start and is immutable so this condition will never be true.

## POC
```
  function _isNotProxy(address _user) internal view returns (bool) {
    return _userRegistry[_user] == address(0) || ODProxy(_userRegistry[_user]).OWNER() != _user;
  }

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L154C1-L156C4
```

## Recommendation
Remove `ODProxy(_userRegistry[_user]).OWNER() != _user`.