## Gas Optimizations

| Number        | Issue                                                                                          | Instances |
| ------------- | :--------------------------------------------------------------------------------------------- | :-------: |
| [G-01](#g-01) | Use hardcode address instead of address(this)                                                  |    20     |
| [G-02](#g-02) | State variables that are used multiple times in a function should be cached in stack variables |     9     |
| [G-03](#g-03) | Avoid contract existence checks by using low level calls                                       |    45     |
| [G-04](#g-04) | Use assembly for math (add, sub, mul, div)                                                     |     2     |
| [G-05](#g-05) | Do not calculate constants                                                                     |     1     |

<a name='g-01'></a>

## [G-01] Use hardcode address instead of address(this)

It can be more gas-efficient to use a hardcoded address instead of the address(this) expression.

The reason for this is that using address(this) requires an additional EXTCODESIZE operation to retrieve the contract's address from its bytecode, which can increase the gas cost of your contract. By pre-calculating and using a hardcoded address, you can avoid this additional operation and reduce the overall gas cost of your contract.

Here's an example of how you can use a hardcoded address instead of address(this):

```
contract MyContract {
    address public myAddress = 0x1234567890123456789012345678901234567890;

    function doSomething() public {
        // Use myAddress instead of address(this)
        require(msg.sender == myAddress, "Caller is not authorized");

        // Do something
    }
}
```

Total Instances: `20`

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L105C5-L105C76
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L140
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L162
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L169
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L178
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L179
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L204
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L205C5-L205C66
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L228
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L255C5-L255C116
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L264
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L265
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L271C8-L271C32

```solidity
File: src/contracts/AccountingEngine.sol

105: return _unqueuedUnauctionedDebt(safeEngine.debtBalance(address(this)));

140:  _settleDebt(safeEngine.coinBalance(address(this)), safeEngine.debtBalance(address(this)), _rad);

162: uint256 _coinBalance = safeEngine.coinBalance(address(this));

169: emit CancelDebt(_rad, _coinBalance - _rad, safeEngine.debtBalance(address(this)));

178:  uint256 _coinBalance = safeEngine.coinBalance(address(this));

179: uint256 _debtBalance = safeEngine.debtBalance(address(this));

228: _source: address(this),

255: uint256 _debtToSettle = Math.min(safeEngine.coinBalance(address(this)), safeEngine.debtBalance(address(this)));

204: uint256 _coinBalance = safeEngine.coinBalance(address(this));

205: uint256 _debtBalance = safeEngine.debtBalance(address(this));

264: uint256 _coinBalance = safeEngine.coinBalance(address(this));

265: uint256 _debtBalance = safeEngine.debtBalance(address(this));

271:  _source: address(this),

```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L203
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L218
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L297
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L298
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L305
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L306
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L337C4-L337C56

```solidity
File: src/contracts/proxies/actions/BasicActions.sol

203:  _transferInternalCoins(_manager, _safeId, address(this), _deltaWad * RAY);

218:  _transferCollateral(_manager, _safeId, address(this), _deltaWad);

297: address(this),

298:      _getRepaidDebt(_safeEngine, address(this), _safeInfo.collateralType, _safeInfo.safeHandler)

305:   _collateralSource: address(this),

306:      _debtDestination: address(this),

337:   _safe = _openSAFE(_manager, _cType, address(this));

```

<a name='g-02'></a>

## [G-02] State variables that are used multiple times in a function should be cached in stack variables

_The following instances are missed in the automated report_

When performing multiple operations on a state variable in a function, it is recommended to cache it first. Either multiple reads or multiple writes to a state variable can save gas by caching it on the stack. Caching of a state variable replaces each Gwarmaccess `100 gas` with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses. Saves `100 gas` per instance.

Total Instances: `9`

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L140C5-L140C101
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L167
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L169
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L192
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L234
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L276
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L309

```solidity
File: src/contracts/AccountingEngine.sol

//@audit safeEngine
140: _settleDebt(safeEngine.coinBalance(address(this)), safeEngine.debtBalance(address(this)), _rad);

//@audit totalOnAuctionDebt at line 160
167: totalOnAuctionDebt -= _rad;

//@audit safeEngine at line 162,166
169:     emit CancelDebt(_rad, _coinBalance - _rad, safeEngine.debtBalance(address(this)));

//@audit _params.debtAuctionBidSize at line 176,181,184,189
//@audit _params.debtAuctionMintedTokens at line 188
192:     emit AuctionDebt(_id, _params.debtAuctionMintedTokens, _params.debtAuctionBidSize);

//@audit _params.surplusTransferPercentage at line 199,213,215,220,224,230
//@audit extraSurplusReceiver at line 201,229
234:       emit TransferSurplus(extraSurplusReceiver, _params.surplusAmount.wmul(_params.surplusTransferPercentage));

//@audit postSettlementSurplusDrain at line 261,272
276:   emit TransferSurplus(postSettlementSurplusDrain, _coinBalance);

//@audit _surplusAuctionHouse at line 306,307
309:     surplusAuctionHouse = ISurplusAuctionHouse(_surplusAuctionHouse);

```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L155

```solidity
File: src/contracts/proxies/Vault721.sol

//@audit _userRegistry[_user]
155:     return _userRegistry[_user] == address(0) || ODProxy(_userRegistry[_user]).OWNER() != _user;

```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L93

```solidity
File: src/contracts/proxies/ODSafeManager.sol

//@audit _safeData[_safes[_i]] at line 92
93:  _cTypes[_i] = _safeData[_safes[_i]].collateralType;

```

<a name='g-03'></a>

## [G-03] Avoid contract existence checks by using low level calls

Prior to 0.8.10 the compiler inserted extra code, including EXTCODESIZE (100 gas), to check for contract existence for external function calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value. Similar behavior can be achieved in earlier versions by using low-level calls, since low level calls never check for contract existence.

Total Instances: `45`

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L92
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L94
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L298
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L309

```solidity
File: src/contracts/AccountingEngine.sol

92:  safeEngine = ISAFEEngine(_safeEngine.assertNonNull());

94: debtAuctionHouse = IDebtAuctionHouse(_debtAuctionHouse);

298:     else if (_param == 'debtAuctionHouse') debtAuctionHouse = IDebtAuctionHouse(_address);

309: surplusAuctionHouse = ISurplusAuctionHouse(_surplusAuctionHouse);

```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L42
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L45
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L46
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L57
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L58C5-L58C62
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L61

```solidity
File: src/contracts/oracles/CamelotRelayer.sol

42: camelotPair = IAlgebraFactory(_CAMELOT_FACTORY).poolByPair(_baseToken, _quoteToken);

45: address _token0 = ICamelotPair(camelotPair).token0();

46:    address _token1 = ICamelotPair(camelotPair).token1();

57:  baseAmount = uint128(10 ** IERC20Metadata(_baseToken).decimals());

58: multiplier = 18 - IERC20Metadata(_quoteToken).decimals();

61: symbol = string(abi.encodePacked(IERC20Metadata(_baseToken).symbol(), ' / ', IERC20Metadata(_quoteToken).symbol()));

```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L48
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L63C5-L63C71
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L64
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L67

```solidity
File: src/contracts/oracles/UniV3Relayer.sol

48:   uniV3Pool = IUniswapV3Factory(_UNI_V3_FACTORY).getPool(_baseToken, _quoteToken, _feeTier);

63: baseAmount = uint128(10 ** IERC20Metadata(_baseToken).decimals());

64:    multiplier = 18 - IERC20Metadata(_quoteToken).decimals();

67: symbol = string(abi.encodePacked(IERC20Metadata(_baseToken).symbol(), ' / ', IERC20Metadata(_quoteToken).symbol()));

```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L173
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L197C7-L197C87

```solidity
File: src/contracts/proxies/Vault721.sol

173: safeManager = IODSafeManager(_safeManager);

197: IODSafeManager(safeManager).transferSAFEOwnership(firstTokenId, address(proxy));

```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L66
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L161
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L170
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L177
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L184
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L191
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L194C5-L194C59
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L207
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L210C5-L210C59
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L221
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L224
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L251

````solidity
File: src/contracts/proxies/ODSafeManager.sol

66:     vault721 = IVault721(_vault721);

161:     ISAFEEngine(safeEngine).modifySAFECollateralization(

170:     ISAFEEngine(safeEngine).transferCollateral(_sData.collateralType, _sData.safeHandler, _dst, _wad);

177:     ISAFEEngine(safeEngine).transferCollateral(_cType, _sData.safeHandler, _dst, _wad);

184:     ISAFEEngine(safeEngine).transferInternalCoins(_sData.safeHandler, _dst, _rad);

191:     ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_sData.collateralType, _sData.safeHandler);

194:     ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(

207:     ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_sData.collateralType, _sData.safeHandler);

210:     ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(

221:     ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_srcData.collateralType, _srcData.safeHandler);

224:     ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(

251:     ILiquidationEngine(_liquidationEngine).protectSAFE(_sData.collateralType, _sData.safeHandler, _saviour);

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L37C4-L37C76
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L38
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L58
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L59
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L60
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L78C5-L78C76
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L79
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L80
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L103
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L130
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L181
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L290
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L292C5-L292C121
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L302C5-L302C59
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L356
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L386
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L384

```solidity
File: src/contracts/proxies/actions/BasicActions.sol

37: uint256 _rate = ISAFEEngine(_safeEngine).cData(_cType).accumulatedRate;

38:    uint256 _coinAmount = ISAFEEngine(_safeEngine).coinBalance(_safeHandler);

58: uint256 _rate = ISAFEEngine(_safeEngine).cData(_cType).accumulatedRate;

59:    uint256 _generatedDebt = ISAFEEngine(_safeEngine).safes(_cType, _safeHandler).generatedDebt;

60:    uint256 _coinAmount = ISAFEEngine(_safeEngine).coinBalance(_safeHandler);

78:   uint256 _rate = ISAFEEngine(_safeEngine).cData(_cType).accumulatedRate;

79:    uint256 _generatedDebt = ISAFEEngine(_safeEngine).safes(_cType, _safeHandler).generatedDebt;

80:    uint256 _coinAmount = ISAFEEngine(_safeEngine).coinBalance(_usr);

103:     ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

130:    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

181:  ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

290:      ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

292: ISAFEEngine.SAFE memory _safeData = ISAFEEngine(_safeEngine).safes(_safeInfo.collateralType, _safeInfo.safeHandler);

302: ISAFEEngine(_safeEngine).modifySAFECollateralization({

356:     ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

384: ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

386: ISAFEEngine.SAFE memory _safeData = ISAFEEngine(_safeEngine).safes(_safeInfo.collateralType, _safeInfo.safeHandler);

````

<a name='g-04'></a>

## [G-04] Use assembly for math (add, sub, mul, div)

Use assembly for math instead of Solidity. You can check for overflow/underflow in assembly to ensure safety.

Addition:

```solidity
//addition in Solidity
function addTest(uint256 a, uint256 b) public pure {
    uint256 c = a + b;
}
```

Gas: 303

```solidity
//addition in assembly
function addAssemblyTest(uint256 a, uint256 b) public pure {
    assembly {
        let c := add(a, b)
        if lt(c, a) {
            mstore(0x00, "overflow")
            revert(0x00, 0x20)
        }
    }
}

```

Gas: 263

Subtraction

```solidity

//subtraction in Solidity
function subTest(uint256 a, uint256 b) public pure {
  uint256 c = a - b;
}
```

Gas: 300

```solidity
//subtraction in assembly
function subAssemblyTest(uint256 a, uint256 b) public pure {
    assembly {
        let c := sub(a, b)
        if gt(c, a) {
            mstore(0x00, "underflow")
            revert(0x00, 0x20)
        }
    }
}
```

Gas: 263

Multiplication

```solidity
//multiplication in Solidity
function mulTest(uint256 a, uint256 b) public pure {
    uint256 c = a * b;
}
```

Gas: 325

```solidity
//multiplication in assembly
function mulAssemblyTest(uint256 a, uint256 b) public pure {
    assembly {
        let c := mul(a, b)
        if lt(c, a) {
            mstore(0x00, "overflow")
            revert(0x00, 0x20)
        }
    }
}
```

Gas: 265

Division

```solidity
//division in Solidity
function divTest(uint256 a, uint256 b) public pure {
    uint256 c = a * b;
}
```

Gas: 325

```solidity
//division in assembly
function divAssemblyTest(uint256 a, uint256 b) public pure {
    assembly {
        let c := div(a, b)
        if gt(c, a) {
            mstore(0x00, "underflow")
            revert(0x00, 0x20)
        }
    }
}
```

Gas: 265

Total Instances: `2`

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L152C4-L152C43
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L153

```solidity
File: src/contracts/AccountingEngine.sol

152: _newCoinBalance = _coinBalance - _rad;

153:    _newDebtBalance = _debtBalance - _rad;

```

<a name='g-05'></a>

## [G-05] Do not calculate constants

Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas.

Total Instances: `1`

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L28

```solidity
File: src/contracts/AccountingEngine.sol

28:   uint256 internal constant ONE_HUNDRED_WAD = 100 * WAD;

```
