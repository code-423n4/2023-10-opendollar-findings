# GAS OPTIMIZATION

|      |  issue  |  instance  |
|------|---------|------------|
|[G-01]| Use hardcode address instead address(this)|18|
|[G-01]|Uncheck arithmetics operations that can’t underflow/overflow|4|
|[G-03]|Use calldata instead of memory for function arguments that do not get mutated|1|
|[G-04]| Use assembly to perform efficient back-to-back calls|8|
|[G-05]|Do not calculate constants|1|
|[G-06]|Use assembly to write address storage values|8|
|[G-07]|Use ERC721A instead ERC721|1|
|[G-08]|Use nested if statements instead of &&|2|
|[G-09]|Make 3 event parameters indexed when possible|1|




## [G-01] Use hardcode address instead address(this)

The reason for this is that using address(this) requires an additional EXTCODESIZE operation to retrieve the contract's address from its bytecode, which can increase the gas cost of your contract. By pre-calculating and using a hardcoded address, you can avoid this additional operation and reduce the overall gas cost of your contract.




```solidity
File: src/contracts/AccountingEngine.sol
105   return _unqueuedUnauctionedDebt(safeEngine.debtBalance(address(this)));

140  _settleDebt(safeEngine.coinBalance(address(this)), safeEngine.debtBalance(address(this)), _rad);

162  uint256 _coinBalance = safeEngine.coinBalance(address(this));

169  emit CancelDebt(_rad, _coinBalance - _rad, safeEngine.debtBalance(address(this)));

178  uint256 _coinBalance = safeEngine.coinBalance(address(this));
179  uint256 _debtBalance = safeEngine.debtBalance(address(this));

187 _incomeReceiver: address(this),


204    uint256 _coinBalance = safeEngine.coinBalance(address(this));
205    uint256 _debtBalance = safeEngine.debtBalance(address(this));


264    uint256 _coinBalance = safeEngine.coinBalance(address(this));
265    uint256 _debtBalance = safeEngine.debtBalance(address(this));

271    _source: address(this),
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L105


```solidity
File: src/contracts/AccountingEngine.sol
203   _transferInternalCoins(_manager, _safeId, address(this), _deltaWad * RAY);

218    _transferCollateral(_manager, _safeId, address(this), _deltaWad);

297     address(this),

298    _getRepaidDebt(_safeEngine, address(this), _safeInfo.collateralType, _safeInfo.safeHandler)

305      _collateralSource: address(this),
306      _debtDestination: address(this),

337    _safe = _openSAFE(_manager, _cType, address(this));
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L203



## [G-02] Uncheck arithmetics operations that can’t underflow/overflow

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block

```solidity
File: src/contracts/AccountingEngine.sol
130    totalQueuedDebt = totalQueuedDebt - _debtBlock;

152    _newCoinBalance = _coinBalance - _rad;
153    _newDebtBalance = _debtBalance - _rad;

167    totalOnAuctionDebt -= _rad;
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L130
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L152
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L153
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L167



```solidity
File: src/contracts/proxies/actions/BasicActions.sol
85    _deltaWad = _rad / RAY;
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L85




## [G‑03] Use calldata instead of memory for function arguments that do not get mutated

 This makes it so that the data is not automatically loaded into memory. If the data passed into the function does not need to be changed (like updating values in an array), it can be passed in as calldata. The one exception to this is if the argument must later be passed into another function that takes an argument that specifies memory storage.


```solidity
26   function execute(address _target, bytes memory _data) external payable onlyOwner returns (bytes memory _response) {    
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODProxy.sol#L26


## [G-04] Use assembly to perform efficient back-to-back calls

If a similar external call is performed back-to-back, we can use assembly to reuse any function signatures and function parameters that stay the same. In addition, we can also reuse the same memory space for each function call (scratch space + free memory pointer + zero slot), which can potentially allow us to avoid memory e


```solidity
File: src/contracts/oracles/CamelotRelayer.sol
45    address _token0 = ICamelotPair(camelotPair).token0();
46    address _token1 = ICamelotPair(camelotPair).token1();
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L45-L45
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L45-L46


```solidity
File: src/contracts/oracles/UniV3Relayer.sol
51    address _token0 = IUniswapV3Pool(uniV3Pool).token0();
52    address _token1 = IUniswapV3Pool(uniV3Pool).token1();

63    baseAmount = uint128(10 ** IERC20Metadata(_baseToken).decimals());
64    multiplier = 18 - IERC20Metadata(_quoteToken).decimals();
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L51
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L52
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L63
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L64



```solidity
File: src/contracts/proxies/actions/BasicActions.sol
58    uint256 _rate = ISAFEEngine(_safeEngine).cData(_cType).accumulatedRate;
59    uint256 _generatedDebt = ISAFEEngine(_safeEngine).safes(_cType, _safeHandler).generatedDebt;
60    uint256 _coinAmount = ISAFEEngine(_safeEngine).coinBalance(_safeHandler);


78    uint256 _rate = ISAFEEngine(_safeEngine).cData(_cType).accumulatedRate;
79    uint256 _generatedDebt = ISAFEEngine(_safeEngine).safes(_cType, _safeHandler).generatedDebt;
80    uint256 _coinAmount = ISAFEEngine(_safeEngine).coinBalance(_usr);
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L58-L60


```solidity
File: src/contracts/AccountingEngine.sol
178       uint256 _coinBalance = safeEngine.coinBalance(address(this));
179       uint256 _debtBalance = safeEngine.debtBalance(address(this));

204       uint256 _coinBalance = safeEngine.coinBalance(address(this));
205       uint256 _debtBalance = safeEngine.debtBalance(address(this));

264       uint256 _coinBalance = safeEngine.coinBalance(address(this));
265       uint256 _debtBalance = safeEngine.debtBalance(address(this));
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L178-L179




## [G-05] Do not calculate constants

Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas

```solidity
File: src/contracts/AccountingEngine.sol
28     uint256 internal constant ONE_HUNDRED_WAD = 100 * WAD;
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L28

## [G-06] Use assembly to write address storage values

By using assembly to write to address storage values, you can bypass some of these operations and lower the gas cost of writing to storage.

```solidity
File: src/contracts/AccountingEngine.sol
299    else if (_param == 'postSettlementSurplusDrain') postSettlementSurplusDrain = _address;
300    else if (_param == 'extraSurplusReceiver') extraSurplusReceiver = _address;
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L299-L300


```solidity
File: src/contracts/oracles/UniV3Relayer.sol
56      baseToken = _token0;
57      quoteToken = _token1;

59      baseToken = _token1;
60      quoteToken = _token0;
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L56-L57


```solidity
File: src/contracts/proxies/ODSafeManager.sol
65    safeEngine = _safeEngine.assertNonNull();
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L65


```solidity
File: src/contracts/proxies/Vault721.sol
34   governor = _governor;
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L34



## [G-07] Use ERC721A instead ERC721

 Compared with ERC721, ERC721A is a more gas-efficient standard to mint a lot of of NFTs simultaneously. It allows developers to mint multiple NFTs at the same gas price. This has been a great improvement due to Ethereum's sky-rocketing gas fee.

```solidity
File: src/contracts/proxies/Vault721.sol
4   import {ERC721} from '@openzeppelin/token/ERC721/ERC721.sol';
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L4






## [G-08] Use nested if statements instead of &&

If the if statement has a logical AND and is not followed by an else statement, it can be replaced with 2 if statements.

```solidity
File: src/contracts/proxies/ODSafeManager.sol
51   if (msg.sender != _owner && safeCan[_owner][_safe][msg.sender] == 0) revert SafeNotAllowed();

60   if (msg.sender != _handler && handlerCan[_handler][msg.sender] == 0) revert HandlerNotAllowed();
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L51
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L60



## [G-09] Make 3 event parameters indexed when possible

It is the most gas efficient to make up to 3 event parameters indexed. 

```solidity
File: src/contracts/proxies/Vault721.sol
28   event CreateProxy(address indexed _user, address _proxy);
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L28
