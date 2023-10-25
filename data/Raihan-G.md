# gas
note: last 18,19 missed from bots
# summary
|      | issue | instance |
|------|-------|----------|
|[G‑01]|Avoid contract existence checks by using low-level calls|56|
|[G‑02]|Refactor external/internal function to avoid unnecessary External Calls|2|
|[G‑03]|Use nested if statements instead of &&|2|
|[G‑04]|Timestamps and block numbers in storage do not need to be uint256|3|
|[G‑05]|Make 3 event parameters indexed when possible|1|
|[G‑06]|Use assembly to perform efficient back-to-back calls|6|
|[G‑07]|Do not calculate constants|1|
|[G‑08]|Use assembly to write address storage values|8|
|[G‑09]|Use hardcode address instead address(this)|18|
|[G‑10]|Use ERC721A instead ERC721|1|
|[G‑11]|Uncheck arithmetics operations that can’t underflow/overflow|5|
|[G‑12]|Refactor a modifier to call a local function instead of directly having the code in the modifier, saving bytecode size and thereby deployment cost|2|
|[G‑13]|Using assembly to revert with an error message|4|
|[G‑14]|Calling functions via interface incurs memory expansion costs, so use assembly to re-use data already in memory|50|
|[G‑15]|Write gas-optimal for-loops|1|
|[G‑16]|Don’t make variables public unless it is necessary to do so|~|
|[G‑17]|Common math operations, like min and max have gas efficient alternatives|~|
|[G‑18]|Use calldata instead of memory for function arguments that do not get mutated|1|
|[G‑19]|State variables should be cached in stack variables rather than re-reading them from storage|20|


## [G‑01] Avoid contract existence checks by using low-level calls
Prior to 0.8.10 the compiler inserted extra code, including EXTCODESIZE (100 gas), to check for contract existence for external function calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value. Similar behavior can be achieved in earlier versions by using low-level calls, since low-level calls never check for contract existence

There are 56 instances of this issue: total save 5600 
```solidity
File: src/contracts/oracles/CamelotRelayer.sol
42    camelotPair = IAlgebraFactory(_CAMELOT_FACTORY).poolByPair(_baseToken, _quoteToken);

45    address _token0 = ICamelotPair(camelotPair).token0();
46    address _token1 = ICamelotPair(camelotPair).token1();

57    baseAmount = uint128(10 ** IERC20Metadata(_baseToken).decimals());
58    multiplier = 18 - IERC20Metadata(_quoteToken).decimals();

61    symbol = string(abi.encodePacked(IERC20Metadata(_baseToken).symbol(), ' / ', IERC20Metadata(_quoteToken).symbol()));
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L42


```solidity
File: src/contracts/oracles/UniV3Relayer.sol
48     uniV3Pool = IUniswapV3Factory(_UNI_V3_FACTORY).getPool(_baseToken, _quoteToken, _feeTier);

51    address _token0 = IUniswapV3Pool(uniV3Pool).token0();
52    address _token1 = IUniswapV3Pool(uniV3Pool).token1();

63    baseAmount = uint128(10 ** IERC20Metadata(_baseToken).decimals());
64    multiplier = 18 - IERC20Metadata(_quoteToken).decimals();

67    symbol = string(abi.encodePacked(IERC20Metadata(_baseToken).symbol(), ' / ', IERC20Metadata(_quoteToken).symbol()));
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L48


```solidity
File: src/contracts/proxies/actions/BasicActions.sol
58    uint256 _rate = ISAFEEngine(_safeEngine).cData(_cType).accumulatedRate;
59    uint256 _generatedDebt = ISAFEEngine(_safeEngine).safes(_cType, _safeHandler).generatedDebt;
60    uint256 _coinAmount = ISAFEEngine(_safeEngine).coinBalance(_safeHandler);


78    uint256 _rate = ISAFEEngine(_safeEngine).cData(_cType).accumulatedRate;
79    uint256 _generatedDebt = ISAFEEngine(_safeEngine).safes(_cType, _safeHandler).generatedDebt;
80    uint256 _coinAmount = ISAFEEngine(_safeEngine).coinBalance(_usr);

101    address _safeEngine = ODSafeManager(_manager).safeEngine();
102    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
103    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

128    address _safeEngine = ODSafeManager(_manager).safeEngine();
129    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
130    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);


143    _safeId = ODSafeManager(_manager).openSAFE(_cType, _usr);

149    ODSafeManager(_manager).transferCollateral(_safeId, _dst, _deltaWad);

154    ODSafeManager(_manager).transferInternalCoins(_safeId, _dst, _rad);

164    ODSafeManager(_manager).modifySAFECollateralization(_safeId, _deltaCollateral, _deltaDebt);


179    address _safeEngine = ODSafeManager(_manager).safeEngine();
180    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
181    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);


259    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);


288    address _safeEngine = ODSafeManager(_manager).safeEngine();
289    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
290    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

292    ISAFEEngine.SAFE memory _safeData = ISAFEEngine(_safeEngine).safes(_safeInfo.collateralType, _safeInfo.safeHandler);

302    ISAFEEngine(_safeEngine).modifySAFECollateralization({


354    address _safeEngine = ODSafeManager(_manager).safeEngine();
355    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
356    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);  

382    address _safeEngine = ODSafeManager(_manager).safeEngine();
383    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
384    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

386    ISAFEEngine.SAFE memory _safeData = ISAFEEngine(_safeEngine).safes(_safeInfo.collateralType, _safeInfo.safeHandler);
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L58-L60


```solidity
File: src/contracts/proxies/ODSafeManager.sol
161   ISAFEEngine(safeEngine).modifySAFECollateralization(

170   ISAFEEngine(safeEngine).transferCollateral(_sData.collateralType, _sData.safeHandler, _dst, _wad);

177   ISAFEEngine(safeEngine).transferCollateral(_cType, _sData.safeHandler, _dst, _wad);

184   ISAFEEngine(safeEngine).transferInternalCoins(_sData.safeHandler, _dst, _rad);

191   ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_sData.collateralType, _sData.safeHandler);

194   ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(

207   ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_sData.collateralType, _sData.safeHandler);

210   ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(

221   ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_srcData.collateralType, _srcData.safeHandler);

224   ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(

251   ILiquidationEngine(_liquidationEngine).protectSAFE(_sData.collateralType, _sData.safeHandler, _saviour);    
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L161


```solidity
File: src/contracts/proxies/Vault721.sol
197   IODSafeManager(safeManager).transferSAFEOwnership(firstTokenId, address(proxy));
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L197



## [G-02] Refactor external/internal function to avoid unnecessary External Calls
The functions below perform external calls that are previously performed in the functions that invoke them. We can refactor the external/internal functions so we could pass the cached external calls into them and avoid the extra external calls that would otherwise take place in the internal functions.

There are 2 instances of this issue:
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

`Fix code:`
```diff
58 function _getRepaidDeltaDebt(
    address _safeEngine,
    bytes32 _cType,
    address _safeHandler
  ) internal view returns (int256 _deltaDebt) {
+   ISAFEEngine safeEngine = ISAFEEngine(_safeEngine);
-   uint256 _rate = ISAFEEngine(_safeEngine).cData(_cType).accumulatedRate;
-   uint256 _generatedDebt = ISAFEEngine(_safeEngine).safes(_cType, _safeHandler).generatedDebt;
-   uint256 _coinAmount = ISAFEEngine(_safeEngine).coinBalance(_safeHandler);
+   uint256 _rate = safeEngine.cData(_cType).accumulatedRate;
+   uint256 _generatedDebt = safeEngine.safes(_cType, _safeHandler).generatedDebt;
+   uint256 _coinAmount = safeEngine.coinBalance(_safeHandler);
    // Uses the whole coin balance in the safeEngine to reduce the debt
    _deltaDebt = (_coinAmount / _rate).toInt();
    // Checks the calculated deltaDebt is not higher than safe.generatedDebt (total debt), otherwise uses its value
    _deltaDebt = uint256(_deltaDebt) <= _generatedDebt ? -_deltaDebt : -_generatedDebt.toInt();
  }


78  function _getRepaidDebt(
    address _safeEngine,
    address _usr,
    bytes32 _cType,
    address _safeHandler
  ) internal view returns (uint256 _deltaWad) {
+   ISAFEEngine safeEngine = ISAFEEngine(_safeEngine);
-   uint256 _rate = ISAFEEngine(_safeEngine).cData(_cType).accumulatedRate;
-   uint256 _generatedDebt = ISAFEEngine(_safeEngine).safes(_cType, _safeHandler).generatedDebt;
-   uint256 _coinAmount = ISAFEEngine(_safeEngine).coinBalance(_usr);
+   uint256 _rate = safeEngine.cData(_cType).accumulatedRate;
+   uint256 _generatedDebt = safeEngine.safes(_cType, _safeHandler).generatedDebt;
+   uint256 _coinAmount = safeEngine.coinBalance(_usr);
    // Uses the whole coin balance in the safeEngine to reduce the debt
    uint256 _rad = _generatedDebt * _rate - _coinAmount;
    // Calculates the equivalent COIN amount
    _deltaWad = _rad / RAY;
    // If the rad precision has some dust, it will need to request for 1 extra wad wei
    _deltaWad = _deltaWad * RAY < _rad ? _deltaWad + 1 : _deltaWad;
  }
   
```


## [G-03] Use nested if statements instead of &&
If the if statement has a logical AND and is not followed by an else statement, it can be replaced with 2 if statements.

There are 2 instances of this issue:
```solidity
File: src/contracts/proxies/ODSafeManager.sol
51   if (msg.sender != _owner && safeCan[_owner][_safe][msg.sender] == 0) revert SafeNotAllowed();

60   if (msg.sender != _handler && handlerCan[_handler][msg.sender] == 0) revert HandlerNotAllowed();
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L51


## [G-04] Timestamps and block numbers in storage do not need to be uint256
A timestamp of size uint48 will work for millions of years into the future. A block number increments once every 12 seconds. This should give you a sense of the size of numbers that are sensible.

```solidity
File: src/contracts/AccountingEngine.sol
74    uint256 public lastSurplusTime;
75    /// @inheritdoc IAccountingEngine
76    uint256 public disableTimestamp;
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L74-L76

## [G-05] Make 3 event parameters indexed when possible
It is the most gas efficient to make up to 3 event parameters indexed. If there are less than 3 parameters, you need to make all parameters indexed.

There are 1 instance of this issue:
```solidity
File: src/contracts/proxies/Vault721.sol
28   event CreateProxy(address indexed _user, address _proxy);
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L28

## [G-06] Use assembly to perform efficient back-to-back calls
If a similar external call is performed back-to-back, we can use assembly to reuse any function signatures and function parameters that stay the same. In addition, we can also reuse the same memory space for each function call (scratch space + free memory pointer + zero slot), which can potentially allow us to avoid memory expansion costs.
Note: In order to do this optimization safely we will cache the free memory pointer value and restore it once we are done with our function calls. We will also set the zero slot back to 0 if neccessary.
[Reffrence](https://code4rena.com/reports/2023-05-ajna#g-10-use-assembly-to-perform-efficient-back-to-back-calls)

There are 8 instances of this issue:
```solidity
File: src/contracts/oracles/CamelotRelayer.sol
45    address _token0 = ICamelotPair(camelotPair).token0();
46    address _token1 = ICamelotPair(camelotPair).token1();
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L45-L46

```solidity
File: src/contracts/oracles/UniV3Relayer.sol
51    address _token0 = IUniswapV3Pool(uniV3Pool).token0();
52    address _token1 = IUniswapV3Pool(uniV3Pool).token1();

63    baseAmount = uint128(10 ** IERC20Metadata(_baseToken).decimals());
64    multiplier = 18 - IERC20Metadata(_quoteToken).decimals();
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L51-L52

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

## [G-07] Do not calculate constants
Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas
[Reffrence](https://code4rena.com/reports/2022-07-golom#g-24-do-not-calculate-constants)

There are 1 instance of this issue:
```solidity
File: src/contracts/AccountingEngine.sol
28     uint256 internal constant ONE_HUNDRED_WAD = 100 * WAD;
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L28

## [G-08] Use assembly to write address storage values
By using assembly to write to address storage values, you can bypass some of these operations and lower the gas cost of writing to storage. Assembly code allows you to directly access the Ethereum Virtual Machine (EVM) and perform low-level operations that are not possible in Solidity.


There are 8 instances of this issue:
```solidity
File: src/contracts/AccountingEngine.sol
// @audit postSettlementSurplusDrain is address storage variable
299    else if (_param == 'postSettlementSurplusDrain') postSettlementSurplusDrain = _address;
300    else if (_param == 'extraSurplusReceiver') extraSurplusReceiver = _address;
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L299-L300


```solidity
File: src/contracts/oracles/UniV3Relayer.sol
// @audit baseToken and quoteToken are address storage variables
56      baseToken = _token0;
57      quoteToken = _token1;

59      baseToken = _token1;
60      quoteToken = _token0;
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L56-L57


```solidity
File: src/contracts/proxies/ODSafeManager.sol
// @audit safeEngine is address storage variable
65    safeEngine = _safeEngine.assertNonNull();
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L65


```solidity
File: src/contracts/proxies/Vault721.sol
// @audit governor is address storage variable
34   governor = _governor;
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L34


## [G-09] Use hardcode address instead address(this)
it can be more gas-efficient to use a hardcoded address instead of the address(this) expression, especially if you need to use the same address multiple times in your contract.

The reason for this is that using address(this) requires an additional EXTCODESIZE operation to retrieve the contract's address from its bytecode, which can increase the gas cost of your contract. By pre-calculating and using a hardcoded address, you can avoid this additional operation and reduce the overall gas cost of your contract.

Here's an example of how you can use a hardcoded address instead of address(this):

```
contract MyContract {
    address public myAddress = 0x1234567890123456789012345678901234567890;
    #L
    function doSomething() public {
        // Use myAddress instead of address(this)
        require(msg.sender == myAddress, "Caller is not authorized");
        
        // Do something
    }
}
```
In the above example, we have a contract MyContract with a public address variable myAddress. Instead of using address(this) to retrieve the contract's address, we have pre-calculated and hardcoded the address in the variable. This can help to reduce the gas cost of our contract and make our code more efficient.

[References](https://book.getfoundry.sh/reference/forge-std/compute-create-address)

There are 18 instances of this issue:
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


## [G-10] Use ERC721A instead ERC721
ERC721A standard, ERC721A is an improvement standard for ERC721 tokens. It was proposed by the Azuki team and used for developing their NFT collection. Compared with ERC721, ERC721A is a more gas-efficient standard to mint a lot of of NFTs simultaneously. It allows developers to mint multiple NFTs at the same gas price. This has been a great improvement due to Ethereum's sky-rocketing gas fee.

[Reffrence](https://nextrope.com/erc721-vs-erc721a-2/)
There are 1 instance of this issue:
```solidity
File: src/contracts/proxies/Vault721.sol
4   import {ERC721} from '@openzeppelin/token/ERC721/ERC721.sol';
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L4

## [G-11] Uncheck arithmetics operations that can’t underflow/overflow
Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block

There are 5 instances of this issue:
```solidity
File: src/contracts/AccountingEngine.sol
130    totalQueuedDebt = totalQueuedDebt - _debtBlock;

152    _newCoinBalance = _coinBalance - _rad;
153    _newDebtBalance = _debtBalance - _rad;

167    totalOnAuctionDebt -= _rad;
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L167


```solidity
File: src/contracts/proxies/actions/BasicActions.sol
85    _deltaWad = _rad / RAY;
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L85






## [G-12] Refactor a modifier to call a local function instead of directly having the code in the modifier, saving bytecode size and thereby deployment cost [Ref](https://0xmacro.com/blog/solidity-gas-optimizations-cheat-sheet/#:~:text=Refactor%20a%20modifier%20to%20call%20a%20local%20function%20instead%20of%20directly%20having%20the%20code%20in%20the%20modifier%2C%20saving%20bytecode%20size%20and%20thereby%20deployment%20cost)
Modifiers code is copied in all instances where it's used, increasing bytecode size. By doing a refractor to the internal function, one can reduce bytecode size significantly at the cost of one JUMP. Consider doing this only if you are constrained by bytecode size.

```solidity
File: src/contracts/proxies/ODSafeManager.sol
49  modifier safeAllowed(uint256 _safe) {

59  modifier handlerAllowed(address _handler) {
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L49

## [G-13] Using assembly to revert with an error message
When reverting in solidity code, it is common practice to use a require or revert statement to revert execution with an error message. This can in most cases be further optimized by using assembly to revert with the error message.


```solidity
File: src/contracts/proxies/ODSafeManager.sol
137    require(msg.sender == address(vault721), 'SafeMngr: Only Vault721');
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L137


```solidity
File: src/contracts/proxies/Vault721.sol
95    require(msg.sender == address(safeManager), 'V721: only safeManager');
96    require(_proxyRegistry[_proxy] != address(0), 'V721: non-native proxy');

188   require(to != address(0), 'V721: no burn');
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L95-L96






## [G-14] Calling functions via interface incurs memory expansion costs, so use assembly to re-use data already in memory
When calling a function on a contract B from another contract A, it’s most convenient to use the interface, create an instance of B with an address and call the function we wish to call. This works very well, but due to how solidity compiles our code, it stores the data to send to contract B in a new memory location thereby expanding memory, sometimes unnecessarily.


With inline assembly, we can optimize our code better and save some gas by using previously used memory locations that we don’t need again or (if the calldata contract B  expects is less than 64 bytes) in the scratch space to store our calldata.


Here’s an example comparing the two;

```
/// 30570
contract Sol {
    function set(address addr, uint256 num) external {
        Callme(addr).setNum(num);
    }
}

/// 30350
contract Assembly {
    function set(address addr, uint256 num) external {
        assembly {
            mstore(0x00, hex"cd16ecbf")
            mstore(0x04, num)

            if iszero(extcodesize(addr)) {
                revert(0x00, 0x00) // revert if address has no code deployed to it
            }

            let success := call(gas(), addr, 0x00, 0x00, 0x24, 0x00, 0x00)
            
            if iszero(success) {
                revert(0x00, 0x00)
            }
        }
    }
}


contract Callme {
    uint256 num = 1;

    function setNum(uint256 a) external {
        num = a;
    }
}

```
We can see that calling set(uint256) on Assembly costs 220 gas less than it would cost if we used solidity.

Do note that when using inline assembly to make external calls, it’s important to check if the address we are calling has code deployed to it using extcodesize(addr) and revert if this returns 0. This is important because calling an address that has no code deployed to it always returns true which can be devastating for our contract logic in most scenarios.


```solidity
File: src/contracts/oracles/CamelotRelayer.sol
42    camelotPair = IAlgebraFactory(_CAMELOT_FACTORY).poolByPair(_baseToken, _quoteToken);

45    address _token0 = ICamelotPair(camelotPair).token0();
46    address _token1 = ICamelotPair(camelotPair).token1();

57    baseAmount = uint128(10 ** IERC20Metadata(_baseToken).decimals());
58    multiplier = 18 - IERC20Metadata(_quoteToken).decimals();

61    symbol = string(abi.encodePacked(IERC20Metadata(_baseToken).symbol(), ' / ', IERC20Metadata(_quoteToken).symbol()));
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L42


```solidity
File: src/contracts/oracles/UniV3Relayer.sol
48     uniV3Pool = IUniswapV3Factory(_UNI_V3_FACTORY).getPool(_baseToken, _quoteToken, _feeTier);

51    address _token0 = IUniswapV3Pool(uniV3Pool).token0();
52    address _token1 = IUniswapV3Pool(uniV3Pool).token1();

63    baseAmount = uint128(10 ** IERC20Metadata(_baseToken).decimals());
64    multiplier = 18 - IERC20Metadata(_quoteToken).decimals();

67    symbol = string(abi.encodePacked(IERC20Metadata(_baseToken).symbol(), ' / ', IERC20Metadata(_quoteToken).symbol()));
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L48


```solidity
File: src/contracts/proxies/actions/BasicActions.sol
58    uint256 _rate = ISAFEEngine(_safeEngine).cData(_cType).accumulatedRate;
59    uint256 _generatedDebt = ISAFEEngine(_safeEngine).safes(_cType, _safeHandler).generatedDebt;
60    uint256 _coinAmount = ISAFEEngine(_safeEngine).coinBalance(_safeHandler);


78    uint256 _rate = ISAFEEngine(_safeEngine).cData(_cType).accumulatedRate;
79    uint256 _generatedDebt = ISAFEEngine(_safeEngine).safes(_cType, _safeHandler).generatedDebt;
80    uint256 _coinAmount = ISAFEEngine(_safeEngine).coinBalance(_usr);

101    address _safeEngine = ODSafeManager(_manager).safeEngine();
102    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
103    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

128    address _safeEngine = ODSafeManager(_manager).safeEngine();
129    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
130    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);


143    _safeId = ODSafeManager(_manager).openSAFE(_cType, _usr);

149    ODSafeManager(_manager).transferCollateral(_safeId, _dst, _deltaWad);

154    ODSafeManager(_manager).transferInternalCoins(_safeId, _dst, _rad);

164    ODSafeManager(_manager).modifySAFECollateralization(_safeId, _deltaCollateral, _deltaDebt);


179    address _safeEngine = ODSafeManager(_manager).safeEngine();
180    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
181    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);


259    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);


288    address _safeEngine = ODSafeManager(_manager).safeEngine();
289    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
290    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

292    ISAFEEngine.SAFE memory _safeData = ISAFEEngine(_safeEngine).safes(_safeInfo.collateralType, _safeInfo.safeHandler);

302    ISAFEEngine(_safeEngine).modifySAFECollateralization({


354    address _safeEngine = ODSafeManager(_manager).safeEngine();
355    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
356    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);  

382    address _safeEngine = ODSafeManager(_manager).safeEngine();
383    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
384    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

386    ISAFEEngine.SAFE memory _safeData = ISAFEEngine(_safeEngine).safes(_safeInfo.collateralType, _safeInfo.safeHandler);
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L58-L60


```solidity
File: src/contracts/proxies/ODSafeManager.sol
170   ISAFEEngine(safeEngine).transferCollateral(_sData.collateralType, _sData.safeHandler, _dst, _wad);

177   ISAFEEngine(safeEngine).transferCollateral(_cType, _sData.safeHandler, _dst, _wad);

184   ISAFEEngine(safeEngine).transferInternalCoins(_sData.safeHandler, _dst, _rad);

191   ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_sData.collateralType, _sData.safeHandler);


207   ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_sData.collateralType, _sData.safeHandler);

221   ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_srcData.collateralType, _srcData.safeHandler);

251   ILiquidationEngine(_liquidationEngine).protectSAFE(_sData.collateralType, _sData.safeHandler, _saviour);    
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L161


```solidity
File: src/contracts/proxies/Vault721.sol
197   IODSafeManager(safeManager).transferSAFEOwnership(firstTokenId, address(proxy));
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L197

## [G-15] Write gas-optimal for-loops
This is what a gas-optimal for loop looks like, if you combine the two tricks above:

for (uint256 i; i < limit; ) {
    
    // inside the loop
    
    unchecked {
        ++i;
    }
}

The two differences here from a conventional for loop is that i++ becomes ++i (as noted above), and it is unchecked because the limit variable ensures it won’t overflow.


```solidity
File: src/contracts/proxies/ODSafeManager.sol
91   for (uint256 _i; _i < _safes.length; _i++) {
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L91

## [G-16] Don’t make variables public unless it is necessary to do so
A public storage variable has an implicit public function of the same name. A public function increases the size of the jump table and adds bytecode to read the variable in question. That makes the contract larger.

Remember, private variables aren’t private, it’s not difficult to extract the variable value using web3.js.

This is especially true for constants which are meant to be read by humans rather than smart contracts.

## [G-17] Common math operations, like min and max have gas efficient alternatives
Unoptimized
```
function max(uint256 x, uint256 y) public pure returns (uint256 z) {
	z = x > y ? x : y;
}
```
Optimized
```
function max(uint256 x, uint256 y) public pure returns (uint256 z) {
    /// @solidity memory-safe-assembly
    assembly {
        z := xor(x, mul(xor(x, y), gt(y, x)))
    }
}
```
The code above is taken from the math section of the Solady Library, more math operations can be found there. It is worth exploring the library to see what gas efficient operations are available to you.


The reason the above example is more gas efficient is because the ternary operator (and in general, code with conditionals in it) contain conditional jumps in the opcodes, which are more costly.

## [G‑18] Use calldata instead of memory for function arguments that do not get mutated
Mark data types as calldata instead of memory where possible. This makes it so that the data is not automatically loaded into memory. If the data passed into the function does not need to be changed (like updating values in an array), it can be passed in as calldata. The one exception to this is if the argument must later be passed into another function that takes an argument that specifies memory storage.

There are 1 instances of this issue:

```solidity
// @audit _data is memory
26   function execute(address _target, bytes memory _data) external payable onlyOwner returns (bytes memory _response) {    
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODProxy.sol#L26

## [G‑19] State variables should be cached in stack variables rather than re-reading them from storage
The instances below point to the second+ access of a state variable within a function. Caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

Saves 100 gas per instance: total save gas 2000
```solidity
File: src/contracts/AccountingEngine.sol
// @audit _params is state variable
  function auctionDebt() external returns (uint256 _id) {
    if (_params.debtAuctionBidSize == 0) revert AccEng_DebtAuctionDisabled();

    uint256 _coinBalance = safeEngine.coinBalance(address(this));
    uint256 _debtBalance = safeEngine.debtBalance(address(this));

    if (_params.debtAuctionBidSize > _unqueuedUnauctionedDebt(_debtBalance)) revert AccEng_InsufficientDebt();

    (_coinBalance, _debtBalance) = _settleDebt(_coinBalance, _debtBalance, _coinBalance);
    totalOnAuctionDebt += _params.debtAuctionBidSize;

    _id = debtAuctionHouse.startAuction({
      _incomeReceiver: address(this),
      _amountToSell: _params.debtAuctionMintedTokens,
      _initialBid: _params.debtAuctionBidSize
    });

    emit AuctionDebt(_id, _params.debtAuctionMintedTokens, _params.debtAuctionBidSize);
  }

// @audit this issue also in auctionSurplus function _params variable used 15
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L175-L194

`fix code:`

```solidity
function auctionDebt() external returns (uint256 _id) {
  AccountingEngineParams memory params = _params; // Cache _params in local variable

  if (params.debtAuctionBidSize == 0) revert AccEng_DebtAuctionDisabled();

  uint256 _coinBalance = safeEngine.coinBalance(address(this));
  uint256 _debtBalance = safeEngine.debtBalance(address(this));

  if (params.debtAuctionBidSize > _unqueuedUnauctionedDebt(_debtBalance)) revert AccEng_InsufficientDebt();

  (_coinBalance, _debtBalance) = _settleDebt(_coinBalance, _debtBalance, _coinBalance);
  totalOnAuctionDebt += params.debtAuctionBidSize;

  _id = debtAuctionHouse.startAuction({
    _incomeReceiver: address(this),
    _amountToSell: params.debtAuctionMintedTokens,
    _initialBid: params.debtAuctionBidSize
  });

  emit AuctionDebt(_id, params.debtAuctionMintedTokens, params.debtAuctionBidSize);
}

```