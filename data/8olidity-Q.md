## Unused Return Value 

#### Description
Either the return value of an external call is not stored in a local or state variable, or the return value is declared but never used in the function body.


```solidity

src\contracts\SAFEEngine.sol:


  156      _modifySAFECollateralization(_cType, _safe, _deltaCollateral, _deltaDebt);
  157      _modifySAFECollateralization(_cType, _safe, _deltaCollateral, _deltaDebt);
  158:     __cData.debtAmount = __cData.debtAmount.add(_deltaDebt);
  159:     __cData.lockedAmount = __cData.lockedAmount.add(_deltaCollateral);

  159  
  160  

  258      _modifySAFECollateralization(_cType, _safe, _deltaCollateral, _deltaDebt);
  259      _modifySAFECollateralization(_cType, _safe, _deltaCollateral, _deltaDebt);
  260:     __cData.debtAmount = __cData.debtAmount.add(_deltaDebt);
  261:     __cData.lockedAmount = __cData.lockedAmount.add(_deltaCollateral);

  261  
  262  

  299      SAFEEngineCollateralData storage __cData = _cData[_cType];
  300      SAFEEngineCollateralData storage __cData = _cData[_cType];
  301:     __cData.accumulatedRate = __cData.accumulatedRate.add(_rateMultiplier);

  301      int256 _deltaSurplus = __cData.debtAmount.mul(_rateMultiplier);
  302      int256 _deltaSurplus = __cData.debtAmount.mul(_rateMultiplier);

  372    function _modifyCollateralBalance(bytes32 _cType, address _account, int256 _wad) internal {
  373    function _modifyCollateralBalance(bytes32 _cType, address _account, int256 _wad) internal {
  374:     tokenCollateral[_cType][_account] = tokenCollateral[_cType][_account].add(_wad);

  374      _emitTransferCollateral(_cType, address(0), _account, _wad);
  375      _emitTransferCollateral(_cType, address(0), _account, _wad);

  377    function _modifyInternalCoins(address _dst, int256 _rad) internal {
  378    function _modifyInternalCoins(address _dst, int256 _rad) internal {
  379:     coinBalance[_dst] = coinBalance[_dst].add(_rad);
  380:     globalDebt = globalDebt.add(_rad);

  380      if (_rad > 0) emit TransferInternalCoins(address(0), _dst, uint256(_rad));
  381      if (_rad > 0) emit TransferInternalCoins(address(0), _dst, uint256(_rad));

  390      SAFE storage _safeData = _safes[_cType][_safe];
  391      SAFE storage _safeData = _safes[_cType][_safe];
  392:     _safeData.lockedCollateral = _safeData.lockedCollateral.add(_deltaCollateral);
  393:     _safeData.generatedDebt = _safeData.generatedDebt.add(_deltaDebt);
  393  
  394  

src\contracts\factories\CamelotRelayerFactory.sol:
  27      _camelotRelayer = new CamelotRelayerChild(_baseToken, _quoteToken, _quotePeriod);
  28      _camelotRelayer = new CamelotRelayerChild(_baseToken, _quoteToken, _quotePeriod);
  29:     _camelotRelayers.add(address(_camelotRelayer));
  29      emit NewCamelotRelayer(address(_camelotRelayer), _baseToken, _quoteToken, _quotePeriod);
  30      emit NewCamelotRelayer(address(_camelotRelayer), _baseToken, _quoteToken, _quotePeriod);


src\contracts\proxies\ODSafeManager.sol:
  124  
  125  
  126:     _usrSafes[_usr].add(_safeId);
  127:     _usrSafesPerCollat[_usr][_cType].add(_safeId);

  145  
  146:     _usrSafes[_dst].add(_safe);
  147:     _usrSafesPerCollat[_dst][_sData.collateralType].add(_safe);

  235      SAFEData memory _sData = _safeData[_safe];
  236      SAFEData memory _sData = _safeData[_safe];
  237:     _usrSafes[msg.sender].add(_safe);
  238:     _usrSafesPerCollat[msg.sender][_sData.collateralType].add(_safe);
  238    }
  239    }


src\contracts\proxies\ODSafeManager.sol:
  141  
  142  
  143:     _usrSafes[_sData.owner].remove(_safe);
  144:     _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);

  144  
  145  

  197      // Remove safe from owner's list (notice it doesn't erase safe ownership)
  198      // Remove safe from owner's list (notice it doesn't erase safe ownership)
  199:     _usrSafes[_sData.owner].remove(_safe);
  200:     _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);

  200      emit QuitSystem(msg.sender, _safe, _dst);
  201      emit QuitSystem(msg.sender, _safe, _dst);

  227      // Remove safe from owner's list (notice it doesn't erase safe ownership)
  228      // Remove safe from owner's list (notice it doesn't erase safe ownership)
  229:     _usrSafes[_srcData.owner].remove(_safeSrc);
  230:     _usrSafesPerCollat[_srcData.owner][_srcData.collateralType].remove(_safeSrc);

  230      emit MoveSAFE(msg.sender, _safeSrc, _safeDst);
  231      emit MoveSAFE(msg.sender, _safeSrc, _safeDst);

  242      SAFEData memory _sData = _safeData[_safe];
  243      SAFEData memory _sData = _safeData[_safe];
  244:     _usrSafes[_sData.owner].remove(_safe);
  245:     _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);
  245    }
  246    }


```



#### Recommendation
Ensure the return value of external function calls is used. Remove or comment out the unused return function parameters. 