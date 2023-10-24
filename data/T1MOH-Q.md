## 1. Low. Consider disabling ability to deploy proxy for proxy
https://github.com/open-dollar/od-contracts/blob/67e5917e7dc0c16324aff3fde0298cd218a15152/src/contracts/proxies/Vault721.sol#L154-L156

Perform additional check to ensure that user is not registered proxy
```solidity
  function _isNotProxy(address _user) internal view returns (bool) {
-   return _userRegistry[_user] == address(0) || ODProxy(_userRegistry[_user]).OWNER() != _user;
+   return (_userRegistry[_user] == address(0) || ODProxy(_userRegistry[_user]).OWNER() != _user) && 
+     _proxyRegistry[_user] == address(0);
  }
```