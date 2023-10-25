## [L-01] `transferSAFEOwnership` does not reset users allowed to modify a safe on behalf of an owner

`transferSAFEOwnership` is not deleting potential allowed users in `safeCan` upon transferring a safe ownership. This could very much be an intended design (keeping in "memory" the allowed users (delegatees) for a given owner), in case an owner wants to send an NFT-Vault and receive it back, the allowed users would remain the same. But it would also mean that if a NFT-Vault owner allows Bob to execute actions on his safe, then send his NFT to someone and wait 1 year before getting it back, he will still allow Bob automatically after receiving the NFT which could be detrimental if the owner does not trust Bob anymore.

[ODSafeManager.sol#L136-L152](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L136-L152)
```solidity
  function transferSAFEOwnership(uint256 _safe, address _dst) external {
    require(msg.sender == address(vault721), 'SafeMngr: Only Vault721');


    if (_dst == address(0)) revert ZeroAddress();
    SAFEData memory _sData = _safeData[_safe];
    if (_dst == _sData.owner) revert AlreadySafeOwner();


    _usrSafes[_sData.owner].remove(_safe);
    _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);


    _usrSafes[_dst].add(_safe);
    _usrSafesPerCollat[_dst][_sData.collateralType].add(_safe);


    _safeData[_safe].owner = _dst;


    emit TransferSAFEOwnership(msg.sender, _safe, _dst);
  }
```


## [L-02 ] `ODGovernor` contract does not allow for cancelling proposals

`ODGovernor` does not expose `_cancel` externally nor calls it internally, therefore it will not be possible to cancel proposals in the current implementation. However, I would advise great care if modifications are made to allow canceling proposals in the future as it could lead to [this issue](https://www.coinspect.com/openzeppelin-governor-dos/).

[ODGovernor.sol#L117-L124](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/gov/ODGovernor.sol#L117-L124)
```solidity
  function _cancel(
    address[] memory targets,
    uint256[] memory values,
    bytes[] memory calldatas,
    bytes32 descriptionHash
  ) internal override(Governor, GovernorTimelockControl) returns (uint256) {
    return super._cancel(targets, values, calldatas, descriptionHash);
  }
```

## [N-01] Redundant check in `auctionSurplus`

The following check in `auctionSurplus` is redundant:
[AccountingEngine.sol#L225](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L225)
```solidity
      if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver();
```

It is already done at the beginning of the function:

[AccountingEngine.sol#L198-L202](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L198-L202)
```solidity
  function auctionSurplus() external returns (uint256 _id) {
    if(_params.surplusTransferPercentage > WAD) revert AccEng_surplusTransferPercentOverLimit();
    if (_params.surplusAmount == 0) revert AccEng_NullAmount();
    if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver();
    if (block.timestamp < lastSurplusTime + _params.surplusDelay) revert AccEng_SurplusCooldown();
```

## [N-02] Unnecessary check

This condition `ODProxy(_userRegistry[_user]).OWNER() != _user` in the following code will never happen because the only way to fill the `_userRegistry[_user]` is via `_build` and in this function `ODProxy` is correctly initialized with `_user`.
[Vault721.sol#L154-L156](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L154-L156)
```solidity
  function _isNotProxy(address _user) internal view returns (bool) {
    return _userRegistry[_user] == address(0) || ODProxy(_userRegistry[_user]).OWNER() != _user;
  }
```