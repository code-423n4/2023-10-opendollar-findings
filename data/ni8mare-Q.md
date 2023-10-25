## Low risk issues

1. The `safeAllowed` modifier in `ODSafeManager` is defined as follows:

```
  /**
   * @notice Checks if the sender is the owner of the safe or the safe has permissions to call the function
   * @param  _safe Id of the safe to check if msg.sender has permissions for
   */
  modifier safeAllowed(uint256 _safe) {
    address _owner = _safeData[_safe].owner;
    if (msg.sender != _owner && safeCan[_owner][_safe][msg.sender] == 0) revert SafeNotAllowed();
    _;
  }
```

The comments suggest that in the conditional, there should be an `OR` operator instead of `AND`. The use of `OR` operator also makes sense. Cause if `AND` is used then it does not make sense for the user to use the `allowSAFE` function to allow anyone else, because the current logic of safeAllowed modifier only allows the owner to make changes. It also forces the owner to allow himself to make changes, which does not make sense.

2. It's not clear which token the OD token will be paired with in order to determine the price in the uniV3Relayer contract. Then the following lines are problematic: 

```
    baseAmount = uint128(10 ** IERC20Metadata(_baseToken).decimals());
    multiplier = 18 - IERC20Metadata(_quoteToken).decimals();
    quotePeriod = _quotePeriod;
```

Decimals of the token can be greater than 18. Hence, the calculation for the `multiplier` will overflow and revert if it is greater than 18. Necessary checks to be implemented to make sure it does not happen