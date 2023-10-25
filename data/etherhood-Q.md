- Check return values for EnumerableSet's add and remove function. It is used in a lot of places, without checking returned value if the operation was success or not.
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol
