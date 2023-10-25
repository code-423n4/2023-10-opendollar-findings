0. GAS + QA: Vault721.sol::_isNotProxy() - Remove `||` on L155 as its unnecessary and wastes gas.

PoC:

All the possible cases are:
```solidity
return _userRegistry[_user] == address(0) || ODProxy(_userRegistry[_user]).OWNER() != _user;
```
- true || true/false >>> creates new proxy for user
- false || false >>> reverts
- false || true >>> creates new proxy for user >>> should never happen as this would violate protocol invariant

The code above allows for the creation of a new proxy for a user even if a proxy already exists for the user but the user is not the owner(case `false || true`). Should never happen as this would violate protocol invariant, even if the impact is zero.

Additionally, we can see from the above cases that when the left side of `||` is `false`, it either reverts or we have an invalid case which violates an invariant, as described above.

This effectively means that we can remove the one invalid case where `false || true`, but we can also remove the case where `false || false`, because the right side of `||` can only be `false`, never `true`:

This is where it would revert, or not:
```solidity
if (!_isNotProxy(_user)) revert ProxyAlreadyExist();
```
And here is the recommendation for QA as well as GAS:
```solidity
  function _isNotProxy(address _user) internal view returns (bool) {
--  return _userRegistry[_user] == address(0) || ODProxy(_userRegistry[_user]).OWNER() != _user;
++  return _userRegistry[_user] == address(0);
  }
```
