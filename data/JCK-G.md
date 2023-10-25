

## Gas Optimizations

| Number | Issue | Instances | Total gas saved |
|--------|-------|-----------|-----------------|
|[G-01]| the value of debtQueue[_debtBlockTimestamp] is already cached use _debtBlock instead of using debtQueue[_debtBlockTimestamp]   |  1  |  |
|[G-02]| Counting down in for statements is more gas efficient   |  1  | 12171 |
|[G-03]| Don’t cache calls that are only used once   | 13  |  |
|[G-04]| public functions not called by the contract should be declared external instead   |  2  |  |
|[G-05]| Multiple accesses of a mapping/array should use a storage pointer   |  3  |  |
|[G-06]| Do not initialize variables with default values   |  4  |  |
|[G-07]| State variables which are not modified within functions should be set as constant or immutable for values set at deployment   |  3  | 30000 |
|[G-08]| Use assembly to perform efficient back-to-back calls   |  2  |  |
|[G-09]| Use assembly to write address storage values   |  2  |  |
|[G-10]| Use hardcoded address instead of address(this)   | 18   |  |
|[G-11]| Cache state variables outside of loop to avoid reading storage on every iteration   |  1  | 100 |
|[G-12]| Use uint256(1)/uint256(2) instead of true/false to save gas for changes   |  2  | 34200 |
|[G-13]| Save loop calls  |  1  |  |


## [G-01] the value of debtQueue[_debtBlockTimestamp] is already cached use _debtBlock instead of using debtQueue[_debtBlockTimestamp]

```solidity
file: src/contracts/AccountingEngine.sol

123     function popDebtFromQueue(uint256 _debtBlockTimestamp) external {
    if (block.timestamp < _debtBlockTimestamp + _params.popDebtDelay) revert AccEng_PopDebtCooldown();

    uint256 _debtBlock = debtQueue[_debtBlockTimestamp];

    if (_debtBlock == 0) revert AccEng_NullAmount();

    totalQueuedDebt = totalQueuedDebt - _debtBlock;
    debtQueue[_debtBlockTimestamp] = 0;

    emit PopDebtFromQueue(_debtBlockTimestamp, _debtBlock);
  }

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L126

## [G‑02] Counting down in for statements is more gas efficient

Counting down is more gas efficient than counting up because neither we are making zero variable to non-zero variable and also we will get gas refund in the last transaction when making non-zero to zero variable.

by changing this logic you can save 12171 gas per one for loop 

Tools used Remix


```solidity
file:  src/contracts/proxies/ODSafeManager.sol

91      for (uint256 _i; _i < _safes.length; _i++) {

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L91

### Test Code

```solidity
contract GasTest is DSTest {
    Contract0 c0;
    Contract1 c1;
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    function testGas() public {
        c0.AddNum();
        c1.AddNum();
    }
}
contract Contract0 {
    uint256 num = 3;
    function AddNum() public {
        uint256 _num = num;
        for(uint i=0;i<=9;i++){
            _num = _num +1;
        }
        num = _num;
    }
}
contract Contract1 {
    uint256 num = 3;
    function AddNum() public {
        uint256 _num = num;
        for(uint i=9;i>=0;i--){
            _num = _num +1;
        }
        num = _num;
    }
}
```

## [G-03] Don’t cache calls that are only used once

```solidity
file: src/contracts/proxies/ODSafeManager.sol

122   address _safeHandler = address(new SAFEHandler(safeEngine));

208   int256 _deltaCollateral = _safeInfo.lockedCollateral.toInt();

209   int256 _deltaDebt = _safeInfo.generatedDebt.toInt();

222   208   int256 _deltaCollateral = _safeInfo.lockedCollateral.toInt();

223   int256 _deltaDebt = _safeInfo.generatedDebt.toInt();

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L122

```solidity
file: src/contracts/proxies/Vault721.sol

110   address _safeManager = address(safeManager);

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L110


```solidity
file: src/contracts/proxies/actions/BasicActions.sol

60   uint256 _coinAmount = ISAFEEngine(_safeEngine).coinBalance(_safeHandler);

78   uint256 _rate = ISAFEEngine(_safeEngine).cData(_cType).accumulatedRate;

79   uint256 _generatedDebt = ISAFEEngine(_safeEngine).safes(_cType, _safeHandler).generatedDebt;

80   uint256 _coinAmount = ISAFEEngine(_safeEngine).coinBalance(_usr);

101  address _safeEngine = ODSafeManager(_manager).safeEngine();

128  address _safeEngine = ODSafeManager(_manager).safeEngine();

179  address _safeEngine = ODSafeManager(_manager).safeEngine();

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#l60

## [G-04] public functions not called by the contract should be declared external instead

when a function is declared as public, it is generated with an internal and an external interface. This means the function can be called both internally (within the contract) and externally (by other contracts or accounts). However, if a public function is never called internally and is only expected to be invoked externally, it is more gas-efficient to explicitly declare it as external.

```solidity
file: src/contracts/proxies/Vault721.sol

140   function tokenURI(uint256 _safeId) public view override returns (string memory uri) {

147   function contractURI() public view returns (string memory uri) {

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L140

## [G-05] Multiple accesses of a mapping/array should use a storage pointer

Caching a mapping’s value in a storage pointer when the value is accessed multiple times saves ~40 gas per access due to not having to perform the same offset calculation every time. Help the Optimizer by saving a storage variable’s reference instead of repeatedly fetching it.

To achieve this, declare a storage pointer for the variable and use it instead of repeatedly fetching the reference in a map or an array. As an example, instead of repeatedly calling _pumps[i], save its reference via a storage pointer: _pumpsInfo storage pumpsInfo = _pumps[i] and use the pointer instead.

### Cache storage pointers for debtQueue[_debtBlockTimestamp];

```solidity
file: src/contracts/AccountingEngine.sol

126   uint256 _debtBlock = debtQueue[_debtBlockTimestamp];

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L126

### Cache storage pointers for _safeData[_safes[_i]];

```solidity
file:  src/contracts/proxies/ODSafeManager.sol

92   _safeHandlers[_i] = _safeData[_safes[_i]].safeHandler;

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L92

```solidity
file:  src/contracts/proxies/Vault721.sol

96    require(_proxyRegistry[_proxy] != address(0), 'V721: non-native proxy');

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L96

## [G-06] Do not initialize variables with default values

```solidity
file:  src/contracts/AccountingEngine.sol

248   totalQueuedDebt = 0;

249   totalOnAuctionDebt = 0;

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L248

```solidity
file: src/contracts/oracles/CamelotRelayer.sol

84  _validity = true;

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L84

```solidity
file:  src/contracts/oracles/UniV3Relayer.sol

90    _validity = true;

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L90

## [G-07] State variables which are not modified within functions should be set as constant or immutable for values set at deployment

Cache such variables and perform operations on them, if operations include modifications to the state variable(s) then remember to equate the state variable to it’s cached counterpart at the end

```solidity
file: src/contracts/oracles/CamelotRelayer.sol

31     string public symbol;

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L31


```solidity
file: src/contracts/oracles/UniV3Relayer.sol
 
30   string public symbol;

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L30


```solidity
file: src/contracts/proxies/Vault721.sol

18   address public governor;

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L18

## [G-08] Use assembly to perform efficient back-to-back calls

If similar external calls are performed back-to-back, we can use assembly to reuse any function signatures and function parameters that stay the same. In addition, we can also reuse the same memory space for each function call (scratch space + free memory pointer), which can potentially allow us to avoid memory expansion costs. In this case, we are also able to efficiently store the function signatures together in memory as one word, saving multiple MLOADs in the process.

Note: In order to do this optimization safely we will cache the free memory pointer value and restore it once we are done with our function calls. We will also set the zero slot back to 0 if neccessary.

### Use for this ISAFEEngine(_safeEngine) back to back external call assembly


```solidity
file:  src/contracts/proxies/actions/BasicActions.sol

78    uint256 _rate = ISAFEEngine(_safeEngine).cData(_cType).accumulatedRate;
      uint256 _generatedDebt = ISAFEEngine(_safeEngine).safes(_cType, _safeHandler).generatedDebt;
      uint256 _coinAmount = ISAFEEngine(_safeEngine).coinBalance(_usr);

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L78-L80

### Use for this ODSafeManager(_manager back to back external call assembly

```solidity
file:  src/contracts/proxies/actions/BasicActions.sol

382    address _safeEngine = ODSafeManager(_manager).safeEngine();
       ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L382-L383

## [G-09] Use assembly to write address storage values

```solidity
file: src/contracts/AccountingEngine.sol    

98   _params = _accEngineParams;

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L98


```solidity
file:  src/contracts/proxies/Vault721.sol

164   _proxyRegistry[_proxy] = _user;

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L164

## [G-10] Use hardcoded address instead of address(this)

Instead of using address(this), it is more gas-efficient to pre-calculate and use the hardcoded address. Foundry’s script.sol and solmate’s LibRlp.sol contracts can help achieve this. Refrences

```solidity
file:  src/contracts/AccountingEngine.sol

105    return _unqueuedUnauctionedDebt(safeEngine.debtBalance(address(this)));

140    _settleDebt(safeEngine.coinBalance(address(this)), safeEngine.debtBalance(address(this)), _rad);

178    uint256 _coinBalance = safeEngine.coinBalance(address(this));

179    uint256 _debtBalance = safeEngine.debtBalance(address(this));

204    uint256 _coinBalance = safeEngine.coinBalance(address(this));

205    uint256 _debtBalance = safeEngine.debtBalance(address(this));

228    _source: address(this),

255    uint256 _debtToSettle = Math.min(safeEngine.coinBalance(address(this)), safeEngine.debtBalance(address(this)));

264    uint256 _coinBalance = safeEngine.coinBalance(address(this));

265    uint256 _debtBalance = safeEngine.debtBalance(address(this));

271    _source: address(this),

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L105


```solidity
file:  src/contracts/proxies/actions/BasicActions.sol

203   _transferInternalCoins(_manager, _safeId, address(this), _deltaWad * RAY);

218   _transferCollateral(_manager, _safeId, address(this), _deltaWad);

297    address(this),

298   _getRepaidDebt(_safeEngine, address(this), _safeInfo.collateralType, _safeInfo.safeHandler)

305   _collateralSource: address(this),

306   _debtDestination: address(this),

337   _safe = _openSAFE(_manager, _cType, address(this));

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L203

## [G-11] Cache state variables outside of loop to avoid reading storage on every iteration

Reading from storage should always try to be avoided within loops. In the following instances, we are able to cache state variables outside of the loop to save a Gwarmaccess (100 gas) per loop iteration.


### Cache _safeData outside of the loop to save 1 SLOAD per iteration

```solidity
file:  src/contracts/proxies/ODSafeManager.sol

91   for (uint256 _i; _i < _safes.length; _i++) {
      _safeHandlers[_i] = _safeData[_safes[_i]].safeHandler;
      _cTypes[_i] = _safeData[_safes[_i]].collateralType;
    }

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L91-L94



## [G-12] Use uint256(1)/uint256(2) instead of true/false to save gas for changes

```solidity
file: src/contracts/oracles/CamelotRelayer.sol

84  _validity = true;

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L84

```solidity
file:  src/contracts/oracles/UniV3Relayer.sol

90    _validity = true;

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L90

## [G-14] Save loop calls

Instead of calling safeData[_safes[_i]] 2 times in each loop for fetching data, it can be saved as a variable.

```solidity
file: src/contracts/proxies/ODSafeManager.sol

91   for (uint256 _i; _i < _safes.length; _i++) {
      _safeHandlers[_i] = _safeData[_safes[_i]].safeHandler;
      _cTypes[_i] = _safeData[_safes[_i]].collateralType;
    }

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L91-L94