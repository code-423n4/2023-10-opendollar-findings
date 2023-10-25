## (non-critical) `deployCamelotRelayer()` does not check if whether the address is repeated or whether it already exists.

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/factories/CamelotRelayerFactory.sol#L23

`deployCamelotRelayer()` does not check if whether the address is repeated or whether it already exists.It will cause waste of calling. I sugguest add check.

## (non-critical) `openSAFE()` The function allows anyone to call it, which may lead to the proliferation of `vault721`

I suggest adding permission checks to only allow specific controller calls to prevent `vault721` flooding
```solidity
  /// @inheritdoc IODSafeManager
  function openSAFE(bytes32 _cType, address _usr) external returns (uint256 _id) {
    if (_usr == address(0)) revert ZeroAddress();

    ++_safeId;
    address _safeHandler = address(new SAFEHandler(safeEngine));

    _safeData[_safeId] = SAFEData({owner: _usr, safeHandler: _safeHandler, collateralType: _cType});

    _usrSafes[_usr].add(_safeId);
    _usrSafesPerCollat[_usr][_cType].add(_safeId);

    vault721.mint(_usr, _safeId);//@audit

    emit OpenSAFE(msg.sender, _usr, _safeId);
    return _safeId;
  }
```