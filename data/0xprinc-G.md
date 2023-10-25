## 1. Better to not return the state variable directly in `ODSafeManager.sol/openSAFE()` as this will make the return variable `_id` declaration redundant and also eats some more gas using extra SLOAD
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L118-L133

The better version of the function implementation will be 
```Solidity
- 121.      ++_safeId;
+ 121.      _id = ++_safeId;

- 132.      return _safeId;
```