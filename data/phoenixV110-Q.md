# [L-01] Misleading comments while checking _baseToken, _quoteToken in CamelotRelayer & UniV3Relayer oracles 

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L48-#L55
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L54-#L61

## Description
In the constructor _token0 and _token1 is set to baseToken and quoteToken global variables but the validation comment says both token0 and token1 are validated is untrue. The check can be refactored as suggested below.

### Existing
```
if (_token0 == _baseToken) {
	baseToken = _token0;
	quoteToken = _token1;
} else {
	baseToken = _token1;
	quoteToken = _token0;
}
```

### Suggestion
```
if (_token0 == _baseToken) {
	if(_token1 == _quoteToken) {
		baseToken = _token0;
		quoteToken = _token1;
	} else {
		revert CustomError;
	}
} else if (_token1 == _baseToken){
	if(_token0 == _quoteToken) {
		baseToken = _token1;
		quoteToken = _token0;
	} else {
		revert CustomError;
	}
} else {
	revert CustomError;
}
```

# [L-02] Handling missing for case where ERC20 token has decimal > 18 in CamelotRelayer & UniV3Relayer oracles 

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L64
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L58

## Description
In the constructor token decimals of an ERC20 is assumed to be <= 18 which can be wrong for some tokens. There should be a handling to avoid unexpected execution failures.

### Existing
```
multiplier = 18 - IERC20Metadata(_quoteToken).decimals();
```

### Suggestion
```
uint256 quoteTokenDecimals = IERC20Metadata(_quoteToken).decimals();

if(quoteTokenDecimals > 18) {
	revert CustomError();
}
multiplier = 18 - quoteTokenDecimals;
```

# [L-03] Validation check missing for _quotePeriod and no method to update it after contract deployment

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L59
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L65

## Description
_quotePeriod can be sent as 0 by mistake and there is no method available to update it.

### Suggestion
add a non zero check in the constructor and add a method to update the quotePeriod if necessary.

# [L-04] No validation on addSAFE() method when adding a safe to _usrSafes mapping

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L235-#L239

## Description
Any user can send any arbitrary number to addSAFE() method and it will be added to _usrSafes mapping of the user.
```
function addSAFE(uint256 _safe) external {
    SAFEData memory _sData = _safeData[_safe];
    _usrSafes[msg.sender].add(_safe);
    _usrSafesPerCollat[msg.sender][_sData.collateralType].add(_safe);
}
```

### Suggestion
Add a check if the safe exists and if the msg.sender is the owner of the safe or not. Only then allow it to add to the mapping.
```
function addSAFE(uint256 _safe) external {
    SAFEData memory _sData = _safeData[_safe];
    if(_sData.owner == msg.sender) {
    	_usrSafes[msg.sender].add(_safe);
    	_usrSafesPerCollat[msg.sender][_sData.collateralType].add(_safe);
    }
}
```

# [L-05] Unreachable line of code while checking _isNotProxy() method

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L155

## Description
Check ODProxy(_userRegistry[_user]).OWNER() != _user will always be false as the owner is always set _user when the proxy is deployed and it is not modifier anywhere in the code.
```
function _isNotProxy(address _user) internal view returns (bool) {
    return _userRegistry[_user] == address(0) || ODProxy(_userRegistry[_user]).OWNER() != _user;
}
```

### Suggestion
Redundant check ODProxy(_userRegistry[_user]).OWNER() can be removed.

# [L-06] Addess(0) check missing and no method to update the governor address

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L34

## Description
There is no address(0) check while setting up the governor in constructor(). There isn't any method to update the governor.
```
constructor(address _governor) ERC721('OpenDollar Vault', 'ODV') {
    governor = _governor;
}
```

### Suggestion
Add a check if the _governor == address(0) the revert() the transaction.

# [NC-01] uint8 can be used for multiplier instead of uint256

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L36
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L35

## Description
For multiplier uint256 is used while it's value will be <=18.
```
uint256 public multiplier;
```

### Suggestion
uint8 can be used for multiplier varaible.

# [NC-02] The order of the functions is not as per suggested

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L37-#L39
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L61-L#63

## Description
The above mentioned methods should be put after the constructor definition. 

