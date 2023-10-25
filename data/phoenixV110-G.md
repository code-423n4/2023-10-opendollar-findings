# [G-01] require() statements can be replaced with revert() and custom error.

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L95-#L96
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L188

## Existing
```
require(msg.sender == address(safeManager), 'V721: only safeManager');
require(_proxyRegistry[_proxy] != address(0), 'V721: non-native proxy');
```
```
require(to != address(0), 'V721: no burn');
```

## Recommendation
```
if (msg.sender == address(safeManager)) {
	revert('V721: only safeManager');
}
if (_proxyRegistry[_proxy] != address(0)) {
	revert('V721: non-native proxy');
}
```
```
if (to != address(0)) {
	revert('V721: no burn');
}
```

# [G-02] Unchecked for loop increment will save gas

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L91-#L94

## Existing
```
for (uint256 _i; _i < _safes.length; _i++) {
    _safeHandlers[_i] = _safeData[_safes[_i]].safeHandler;
    _cTypes[_i] = _safeData[_safes[_i]].collateralType;
}
```

## Recommendation
```
for (uint256 _i; _i < _safes.length; ) {
    _safeHandlers[_i] = _safeData[_safes[_i]].safeHandler;
    _cTypes[_i] = _safeData[_safes[_i]].collateralType;
    unchecked {
    	++i;
    }
}
```

# [G-03] _safes.length should be cached to save gas when it is being used at multiple places

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L89-#L94

## Existing
```
_safes = _usrSafes[_usr].values();
_safeHandlers = new address[](_safes.length);
_cTypes = new bytes32[](_safes.length);
for (uint256 _i; _i < _safes.length; _i++) {
  _safeHandlers[_i] = _safeData[_safes[_i]].safeHandler;
  _cTypes[_i] = _safeData[_safes[_i]].collateralType;
}
```

## Recommendation
```
_safes = _usrSafes[_usr].values();
uint256 _safelength = _safes.length;
_safeHandlers = new address[](_safelength);
_cTypes = new bytes32[](_safelength);
for (uint256 _i; _i < _safelength; _i++) {
  _safeHandlers[_i] = _safeData[_safes[_i]].safeHandler;
  _cTypes[_i] = _safeData[_safes[_i]].collateralType;
}
```