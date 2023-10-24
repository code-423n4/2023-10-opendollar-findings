# QA Report

## Summary

| id | title |
| --- | --- |
| [L-01](#l-01-someone-can-add-a-safe-to-their-list-that-cannot-be-removed) | Someone can add a safe to their list that cannot be removed |
| [L-02](#l-02-initial-values-for-governorsettings-are-very-low) | Initial values for `GovernorSettings` are very low |
| [L-03](#l-03-no-way-to-change-governor-of-vault721) | No way to change governor of `Vault721` |
| [R-01](#r-01-_isnotproxy-is-confusingly-named) | `_isNotProxy` is confusingly named |
| [R-02](#r-02-avoid-negations-especially-double-negations) | Avoid negations, especially double negations |
| [R-03](#r-03-calculation-is-repeated-in-same-code-block) | Calculation is repeated in same code block |
| [R-04](#r-04-unnecessary-owner-check) | Unnecessary `OWNER` check |
| [NC-01](#nc-01-confusing-documentation) | Confusing documentation |
| [NC-02](#nc-02-removed-whitspace-unaligns-code) | Removed whitspace unaligns code |

## Low

### L-01 Someone can add a safe to their list that cannot be removed

`ODSafeManager::addSAFE` and `removeSAFE` behave a bit weird.

[`ODSafeManager::addSAFE`](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L235-L239):
```solidity
File: src/contracts/proxies/ODSafeManager.sol

235:  function addSAFE(uint256 _safe) external {
236:    SAFEData memory _sData = _safeData[_safe];
237:    _usrSafes[msg.sender].add(_safe);
238:    _usrSafesPerCollat[msg.sender][_sData.collateralType].add(_safe);
239:  }
```

Here, a user can add any safe (created or not) to their list.

However in [`removeSAFE`](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L242-L246):
```solidity
File: src/contracts/proxies/ODSafeManager.sol

242:  function removeSAFE(uint256 _safe) external safeAllowed(_safe) {
243:    SAFEData memory _sData = _safeData[_safe];
244:    _usrSafes[_sData.owner].remove(_safe);
245:    _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);
246:  }
```

Since it is `safeAllowed(_safe)` a user can only remove a safe from their list when they are the owner of it.

Hence a user could accidentally add a safe to their list which they are unable to remove.

These lists are only informational and have no effect on the logic of the protocol but they could cause UIs or other off-chain functions to look weird.

#### Recommendation
Consider changing so that instead of having `removeSafe` remove the safe from `owner`. Remove it from `msg.sender` and remove the check that `msg.sender` is owner.

### L-02 Initial values for `GovernorSettings` are very low

`ODGovernor` is a OZ `Governor` with some plugins. It sets up its parameters in the constructor:

[`ODGovernor::constructor`](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/gov/ODGovernor.sol#L41):
```solidity
File: src/contracts/gov/ODGovernor.sol

41:    GovernorSettings(1, 15, 0)
```

These are in turn, `initialVotingDelay=1`, `initialVotingPeriod=15` and `initialProposalThreshold=0`.

These values are very low, and `initialProposalThreshold=0` can invite to spam. `initialVotingPeriod=15` is very short as optimism uses L1 blocks with are just 12 seconds.

I assume these are test chain values, similar to the factories in uniswap and camelot relayers. 

#### Recommendations
Do not forget to change these values before going live.

### L-03 No way to change governor of `Vault721`

`Vault721` is owned by `governor`. This address has the ability to change [`safeManager`](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L126-L128) and [`nftRenderer`](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L133-L135).

There is however no way for `governor` to change its own address. If governance for whatever reason needs to migrate to a new address this makes that move problematic as that would lock those two values in `Vault721`

#### Recommendation
Consider adding a way to change the `governor` address of `Vault721`.

## Recommendations

### R-01 `_isNotProxy` is confusingly named

[`Vault721::_isNotProxy`](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L154-L156)
```solidity
File: src/contracts/proxies/Vault721.sol

154:  function _isNotProxy(address _user) internal view returns (bool) {
155:    return _userRegistry[_user] == address(0) || ODProxy(_userRegistry[_user]).OWNER() != _user;
156:  }
```
This tests if `_user` has a registered proxy. However, the name `_isNotProxy` implies that `_user` should _be_ a proxy.

Consider changing the name of `_isNotProxy` to `_hasNoProxy`. This better conveys the relationship between the user and the proxy.

### R-02 Avoid negations, especially double negations

[`Vault721::_isNotProxy`](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L154-L156)
```solidity
File: src/contracts/proxies/Vault721.sol

154:  function _isNotProxy(address _user) internal view returns (bool) {
155:    return _userRegistry[_user] == address(0) || ODProxy(_userRegistry[_user]).OWNER() != _user;
156:  }
```
this is used in these three locations:

```solidity
File: src/contracts/proxies/Vault721.sol

78:     if (!_isNotProxy(msg.sender)) revert ProxyAlreadyExist();

86:     if (!_isNotProxy(_user)) revert ProxyAlreadyExist();

192:      if (_isNotProxy(to)) {
193:        proxy = _build(to);
194:      } else {
195:        proxy = payable(_userRegistry[to]);
196:      }
```

The double negation `!_isNotProxy` is hard to grasp.

Consider switching the function to check if there is a proxy and rename it to `_isProxy` (see above issue about naming though). Then turn around the if-statement in `_afterTokenTransfer`.

### R-03 Calculation is repeated in same code block

`_params.surplusAmount.wmul(ONE_HUNDRED_WAD - _params.surplusTransferPercentage)` is done on both line `215` and `220` in [`AccountingEngine::auctionSurplus`](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L213-L221)
```solidity
File: src/contracts/AccountingEngine.sol

213:    if (_params.surplusTransferPercentage < ONE_HUNDRED_WAD) {
214:      _id = surplusAuctionHouse.startAuction({
215:        _amountToSell: _params.surplusAmount.wmul(ONE_HUNDRED_WAD - _params.surplusTransferPercentage),
216:        _initialBid: 0
217:      });
218:
219:      lastSurplusTime = block.timestamp;
220:      emit AuctionSurplus(_id, 0, _params.surplusAmount.wmul(ONE_HUNDRED_WAD - _params.surplusTransferPercentage));
221:    }
```

Consider rewriting it to avoid repeating calculations:
```diff
    if (_params.surplusTransferPercentage < ONE_HUNDRED_WAD) {
+     uint256 _surplusToSell = _params.surplusAmount.wmul(ONE_HUNDRED_WAD - _params.surplusTransferPercentage);
      _id = surplusAuctionHouse.startAuction({
+       _amountToSell: _surplusToSell,
-       _amountToSell: _params.surplusAmount.wmul(ONE_HUNDRED_WAD - _params.surplusTransferPercentage),
        _initialBid: 0
      });

      lastSurplusTime = block.timestamp;
+     emit AuctionSurplus(_id, 0, _surplusToSell);
-     emit AuctionSurplus(_id, 0, _params.surplusAmount.wmul(ONE_HUNDRED_WAD - _params.surplusTransferPercentage));
    }
```

### R-04 Unnecessary `OWNER` check

[`Vault721::_isNotProxy`](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L154-L156)
```solidity
File: src/contracts/proxies/Vault721.sol

154:  function _isNotProxy(address _user) internal view returns (bool) {
155:    return _userRegistry[_user] == address(0) || ODProxy(_userRegistry[_user]).OWNER() != _user;
156:  }
```

When [creating a proxy](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L162-L167), `_userRegistry[_user]` is written with the created proxy. In `ODProxy`, `OWNER` is [`immutable`](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODProxy.sol#L12) hence cannot be changed.

The check `ODProxy(_userRegistry[_user]).OWNER() != _user` is therefore unnecessary since if there is a proxy in `_userRegistry` the `_user` _must_ be owner of it.

Consider removing the extra, unnecessary, check as it is confusing and can fool you to believe `OWNER` is mutable.

## Informational / Non-critical

### NC-01 Confusing documentation

The documentation for `ODSafeManager::openSAFE` says:

[`IODSafeManager::openSAFE`](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/interfaces/proxies/IODSafeManager.sol#L122-L128)
```solidity
File: src/interfaces/proxies/IODSafeManager.sol

122:  /**
123:   * @notice Open a new safe for a user address
124:   * @param  _cType Bytes32 representation of the collateral type
125:   * @param  _usr Address of the user to open the safe for
126:   * @return _id Id of the new SAFE
127:   */
128:  function openSAFE(bytes32 _cType, address _usr) external returns (uint256 _id);
```

[`ODSafeManager::openSAFE`](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L117-L133):
```solidity
File: src/contracts/proxies/ODSafeManager.sol

117:  /// @inheritdoc IODSafeManager
118:  function openSAFE(bytes32 _cType, address _usr) external returns (uint256 _id) {
119:    if (_usr == address(0)) revert ZeroAddress();
...
129:    vault721.mint(_usr, _safeId);
130:
131:    emit OpenSAFE(msg.sender, _usr, _safeId);
132:    return _safeId;
133:  }
```

And the parameter for who the safe is for is `_usr` throughout the function. However, the parameter `_usr` from `ODSafeManager` is required to be `_proxy` in [`Vault721::mint`](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L90-L99):
```solidity
File: src/contracts/proxies/Vault721.sol

90:   /**
91:    * @dev mint can only be called by the SafeManager
92:    * enforces that only ODProxies call `openSafe` function by checking _proxyRegistry
93:    */
94:   function mint(address _proxy, uint256 _safeId) external {
95:     require(msg.sender == address(safeManager), 'V721: only safeManager');
        // @audit what is passed as `_usr` from safe manager is referred to as `_proxy` here
96:     require(_proxyRegistry[_proxy] != address(0), 'V721: non-native proxy');
97:     address _user = _proxyRegistry[_proxy];
98:     _safeMint(_user, _safeId);
99:   }
```

This is confusing, consider correcting the documentation for `openSAFE` to reflect that the user needs to open a proxy with `Vault721` before and then call `openSAFE` from that proxy.

### NC-02 Removed whitspace unaligns code

[`AccountingEngine::auctionSurplus`](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L199-L200)
```solidity
File: src/contracts/AccountingEngine.sol

        // missing whitespace
199:    if(_params.surplusTransferPercentage > WAD) revert AccEng_surplusTransferPercentOverLimit();
200:    if (_params.surplusAmount == 0) revert AccEng_NullAmount();
```

A whitespace between `if` and `(` is missing, unaliging the code with the rest of the if-statements