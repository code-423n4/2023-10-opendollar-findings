# [QA-1] Lack of checking of `address(0)` in constructor in `SAFEHandler.sol`

[File: SAFEHandler.sol](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/SAFEHandler.sol#L17)
```
ISAFEEngine(_safeEngine).approveSAFEModification(msg.sender);
```

There's no check if `_safeEngine` is `address(0)`.

# [QA-2] Missing check for contract existence in `ODProxy.sol`

[File: ODProxy.sol](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODProxy.sol#L30)
```
(_succeeded, _response) = _target.delegatecall(_data);
```

If there's no contract at `_target`, the `_target.delegatecall(_data)` will return true.
Check for the contract existence before delegate calling to the `_target`.


# [QA-3] `OWNER` is immutable in `ODProxy.sol`, which means it won't be possible to change it in the future
When, for any reason - there would be need to change the owner of `ODProxy.sol` - it won't be possible, since `OWNER` is set as immutable variable.

[File: ODProxy.sol](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODProxy.sol#L12)
```
address public immutable OWNER;
```

# [QA-4] Unify order of parameters in `ODProxy.sol`

[File: ODProxy.sol](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODProxy.sol#L189-205)
```
function quitSystem(uint256 _safe, address _dst) external safeAllowed(_safe) handlerAllowed(_dst) {

(...)

function enterSystem(address _src, uint256 _safe) external handlerAllowed(_src) safeAllowed(_safe) {
```

Functions `quitSystem()` and `enterSystem()` - are related to the similar funcionality. First one allows to quit system, while the second one - enter it.
The parameters order should be unified for them. In current implementation: for `quitSystem()` `_safe` is the 1st parameter, while `_dst` the 2nd one.
For `enterSystem()`, `_safe` is the 2nd parameter, while `_src` is the 1st one. Since both `quitSystem()` and `enterSystem()` operates on `_safe` - consider using it as 1st parameter in both functions:

```
quitSystem(uint256 _safe, address _dst)
enterSystem(uint256 _safe, address _src)
```

# [QA-5] Unify the order of modifiers in `OdProxy.sol`


[File: ODProxy.sol](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODProxy.sol#L189-205)
```
function quitSystem(uint256 _safe, address _dst) external safeAllowed(_safe) handlerAllowed(_dst) {

(...)

function enterSystem(address _src, uint256 _safe) external handlerAllowed(_src) safeAllowed(_safe) {
```

Consider unifying the order of modifiers to be the same in functions, which share related functionality.
Firstly, use `safeAllowed(_safe)`, then `handlerAllowed(_dst)`:

```
function quitSystem(uint256 _safe, address _dst) external safeAllowed(_safe) handlerAllowed(_dst) {

(...)

function enterSystem(address _src, uint256 _safe) external safeAllowed(_safe) handlerAllowed(_src)  {
```


# [QA-6] Lack of checking of `address(0)` in constructor in `ODSafeManager.sol`

[File: ODProxy.sol](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODProxy.sol#L66)
```
vault721 = IVault721(_vault721)
```

While `constructor` verifies that `_safeEngine` is not address 0, it does not verify if `_vault721` is not address 0.

# [QA-7] Lack of checking of `address(0)` in functions in `ODSafeManager.sol`

* function `allowHandler()`
[File: ODProxy.sol](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODProxy.sol#L107)
```
handlerCan[msg.sender][_usr] = _ok;
```

There's no check if `_usr` is not `address(0)`.


# [QA-8] Lack of event emission in `ODSafeManager.sol`

Functions `addSAFE()` and `removeSAFE()` do not emit events when safe is added/removed.

[File: ODProxy.sol](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODProxy.sol#L235-246)
```
  function addSAFE(uint256 _safe) external {
    SAFEData memory _sData = _safeData[_safe];
    _usrSafes[msg.sender].add(_safe);
    _usrSafesPerCollat[msg.sender][_sData.collateralType].add(_safe);
  }

  /// @inheritdoc IODSafeManager
  function removeSAFE(uint256 _safe) external safeAllowed(_safe) {
    SAFEData memory _sData = _safeData[_safe];
    _usrSafes[_sData.owner].remove(_safe);
    _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);
  }
```

# [QA-9] Loop over unbounded array in `ODSafeManager.sol`

New safe can be added to `_usrSafes` by calling `addSAFE()`:

[File: ODSafeManager.sol](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L235-L240)
```
  function addSAFE(uint256 _safe) external {
    SAFEData memory _sData = _safeData[_safe];
    _usrSafes[msg.sender].add(_safe);
    _usrSafesPerCollat[msg.sender][_sData.collateralType].add(_safe);
  }
```

This means, that there's no limit of number of safes which can be added. User can add `addSAFE()` with new safe as many times he/she wants. This means, that `_usrSafes` and `_usrSafesPerCollat` can contain a lot of elements.

In [File: ODSafeManager.sol](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L-83-95)
```
function getSafesData(address _usr)
    external
    view
    returns (uint256[] memory _safes, address[] memory _safeHandlers, bytes32[] memory _cTypes)
  {
    _safes = _usrSafes[_usr].values();
    _safeHandlers = new address[](_safes.length);
    _cTypes = new bytes32[](_safes.length);
    for (uint256 _i; _i < _safes.length; _i++) {
      _safeHandlers[_i] = _safeData[_safes[_i]].safeHandler;
      _cTypes[_i] = _safeData[_safes[_i]].collateralType;
    }
  }
```

we are looping over `_usrSafes[_usr]`. This means, that if there are a lot of added safes, function will revert  with out of gas error. It should be a way for `getSafesData()` to display  limited set of safes, to avoid that revert. E.g. re-implement a function in a way, which will allow to choose how many safes we want to return (and the starting point, e.g. we want to get `limit` set starting at index `offset`).
One way of solving this problem would be re-implementing `getSafesData()` by adding `offset` and `limit` parameters to it.

```
function getSafesData(address _usr, uint _offset, uint _limit)
    external
    view
    returns (uint256[] memory _safes, address[] memory _safeHandlers, bytes32[] memory _cTypes)
  {
    _safes = _usrSafes[_usr].values();
    _safeHandlers = new address[](_safes.length);
    _cTypes = new bytes32[](_safes.length);
    require(_safes.length >= _offset + _limit, "Out of bounds error!");
    for (uint256 _i = _offset; _i < _limit; _i++) {
      _safeHandlers[_i] = _safeData[_safes[_i]].safeHandler;
      _cTypes[_i] = _safeData[_safes[_i]].collateralType;
    }
  }
```

# [QA-10] Lack of event emission in `Vault721.sol`

Function which set/change important protocol's parameters should emit events. Below, there's a list of functions from `Vault721.sol` which does not emit events:

* `updateNftRenderer`
* `updateContractURI`
* `setSafeManager`
* `setNftRenderer`


# [N-1] Invalid comment in `ODSafeManager.sol`

[File: ODSafeManager.sol](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L135-L136)
```
  // Give the safe ownership to a dst address.
  function transferSAFEOwnership(uint256 _safe, address _dst) external {
```

`Give the safe ownership to a dst address.` should be changed to `Give the safe ownership to a _dst address.`, since function `transferSAFEOwnership()` has `_dst` (not `dst`) parameter.


# [N-2] Make comments in `UniV3Relayer.sol` more specific

[File: UniV3Relayer.sol](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/oracles/UniV3Relayer.sol#L71)
```
 Method will return invalid if the pool doesn't have enough history
```

There's no explanation what "invalid" means. It turns out, that when pool doesn't have enough history, function will return: `(0, false)`.
Whenever commenting a function behavior, be as specific as possible. Word "invalid" is too ambiguous in this context.


# [N-3] Abide to Checks Effects Interactions pattern in `AccountingEngine.sol`

[File: AccountingEngine.sol](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L166-L167)
```
    safeEngine.settleDebt(_rad);
    totalOnAuctionDebt -= _rad;
```

Reduce the `totalOnAuctionDebt` before calling `safeEngine.settleDebt()`.


This does not pose any security risk, especially, that `safeEngine.settleDebt()` does not use `totalOnAuctionDebt` variable. Nonetheless, coding with keeping  Checks Effects Interactions pattern in mind is good security practice.

