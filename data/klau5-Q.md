# Allowed user have too much priviledge

## Links to affected code

[https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L105](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L105)

## Impact

Allowed user can revoke approval of other allowed address.

## Proof of Concept

The allowed user can allow other user. If allowed address is compromised, it can cancel approval of other allowed user.

```solidity
modifier safeAllowed(uint256 _safe) {
  address _owner = _safeData[_safe].owner;
  if (msg.sender != _owner && safeCan[_owner][_safe][msg.sender] == 0) revert SafeNotAllowed();
  _;
}

function allowSAFE(uint256 _safe, address _usr, uint256 _ok) external safeAllowed(_safe) {
    address _owner = _safeData[_safe].owner;
    safeCan[_owner][_safe][_usr] = _ok;
    emit AllowSAFE(msg.sender, _safe, _usr, _ok);
}
```

## Tools Used

Manual Review

## Recommended Mitigation Steps

Restrict only the owner to `allowSAFE`.

```diff
- function allowSAFE(uint256 _safe, address _usr, uint256 _ok) external safeAllowed(_safe) {
+ function allowSAFE(uint256 _safe, address _usr, uint256 _ok) external {
    address _owner = _safeData[_safe].owner;
+   require(msg.sender == _owner, "not owner");
    safeCan[_owner][_safe][_usr] = _ok;
    emit AllowSAFE(msg.sender, _safe, _usr, _ok);
  }
```

# Clear safeCan in transferSAFEOwnership

## Links to affected code

[https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L136](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L136)

## Impact

Old approval remains even if user gets SAFE again.

## Proof of Concept

There is no removal `safeCan` at `transferSAFEOwnership` . When the user gets SAFE again, the old approval is still remained.

```solidity
modifier safeAllowed(uint256 _safe) {
  address _owner = _safeData[_safe].owner;
  if (msg.sender != _owner && safeCan[_owner][_safe][msg.sender] == 0) revert SafeNotAllowed();
  _;
}

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

## Tools Used

Manual Review

## Recommended Mitigation Steps

Delete `safeCan` at `transferSAFEOwnership` . You might use Set instead of mapping.