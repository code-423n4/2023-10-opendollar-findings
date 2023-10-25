# [L1-01] Misleading comments while checking _baseToken, _quoteToken in CamelotRelayer & UniV3Relayer oracles 

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L48-#L55
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L54-#L61

## Description
In the constructor _token0 and _token1 is set to baseToken and quoteToken global variables but the validation comment says both token0 and token1 are validated is untrue. The check can be refactored as suggested below.

Existing
```
if (_token0 == _baseToken) {
	baseToken = _token0;
	quoteToken = _token1;
} else {
	baseToken = _token1;
	quoteToken = _token0;
}
```

Suggested
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