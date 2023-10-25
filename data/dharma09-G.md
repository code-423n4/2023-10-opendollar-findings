## [G-1] Optimizing the check order for cost efficient function execution

Checks that involve constants should come before checks that involve state variables, function calls, and calculations. By doing these checks first, the function is able to revert before wasting a Gcoldsload (2100 gas) in a function that may ultimately revert, in the unhappy case.

### [G-1.1] first check if (_dst == _sData.owner) revert AlreadySafeOwner(); to validate parameters before doing any sstores(more than 2200 gas could be saved in case of a revert)

- https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L136C1-L153C1

```diff
File: contracts/gov/ODSafeManager.sol
136: function transferSAFEOwnership(uint256 _safe, address _dst) external {
    require(msg.sender == address(vault721), 'SafeMngr: Only Vault721');

    if (_dst == address(0)) revert ZeroAddress();
    SAFEData memory _sData = _safeData[_safe];
    if (_dst == _sData.owner) revert AlreadySafeOwner(); //@audit

    _usrSafes[_sData.owner].remove(_safe);
    _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);

    _usrSafes[_dst].add(_safe);
    _usrSafesPerCollat[_dst][_sData.collateralType].add(_safe);

    _safeData[_safe].owner = _dst;

    emit TransferSAFEOwnership(msg.sender, _safe, _dst);
  }
```

### [G-1.2] first check  if (_srcData.collateralType != _dstData.collateralType) revert CollateralTypesMismatch(); to validate parameters before doing any sstores(more than 2200 gas could be saved in case of a revert)

- https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L217C2-L227C1

```diff
File: contracts/gov/ODSafeManager.sol
217: function moveSAFE(uint256 _safeSrc, uint256 _safeDst) external safeAllowed(_safeSrc) safeAllowed(_safeDst) {
218:    SAFEData memory _srcData = _safeData[_safeSrc];
219:    SAFEData memory _dstData = _safeData[_safeDst];
220:    if (_srcData.collateralType != _dstData.collateralType) revert CollateralTypesMismatch(); //@audit    ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_srcData.collateralType, _srcData.safeHandler);
221:    int256 _deltaCollateral = _safeInfo.lockedCollateral.toInt();
222:    int256 _deltaDebt = _safeInfo.generatedDebt.toInt();
223:    ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(
224:      _srcData.collateralType, _srcData.safeHandler, _dstData.safeHandler, _deltaCollateral, _deltaDebt
225:    );
```

## [G-2] Use a hardcoded address instead of address(this)

It can be more gas-efficient to use a hardcoded address instead of the address(this) expression, especially if you need to use the same address multiple times in your contract.

The reason for this is that using address(this) requires an additional EXTCODESIZE operation to retrieve the contract’s address from its bytecode, which can increase the gas cost of your contract. By pre-calculating and using a hardcoded address, you can avoid this additional operation and reduce the overall gas cost of your contract.

Here’s an example of how you can use a hardcoded address instead of address(this):

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

In the above example, we have a contract, MyContract, with a public address variable myAddress. Instead of using address(this) to retrieve the contract’s address, we have pre-calculated and hardcoded the address in the variable. This can help to reduce the gas cost of our contract and make our code more efficient.

- https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol

```solidity
File: src/contracts/proxies/actions/BasicActions.sol
203: _transferInternalCoins(_manager, _safeId, address(this), _deltaWad * RAY);
218: _transferCollateral(_manager, _safeId, address(this), _deltaWad);
298: _getRepaidDebt(_safeEngine, address(this), _safeInfo.collateralType, _safeInfo.safeHandler) //@audit
337: _safe = _openSAFE(_manager, _cType, address(this));
```

- https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol

```solidity
File: src/contracts/AccountingEngine.sol
105: return _unqueuedUnauctionedDebt(safeEngine.debtBalance(address(this)));
140: _settleDebt(safeEngine.coinBalance(address(this)), safeEngine.debtBalance(address(this)), _rad);
162: uint256 _coinBalance = safeEngine.coinBalance(address(this));
178: uint256 _coinBalance = safeEngine.coinBalance(address(this)); //@audit external call
179:    uint256 _debtBalance = safeEngine.debtBalance(address(this));
204: uint256 _coinBalance = safeEngine.coinBalance(address(this)); //@audit external call
205:    uint256 _debtBalance = safeEngine.debtBalance(address(this));

```

## [G-3] **A modifier used only once and not being inherited should be inlined to save gas**

- https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODProxy.sol#L21

```solidity
File: src/sontracts/proxies/ODProxy.sol
21: modifier onlyOwner() {
    if (msg.sender != OWNER) revert OnlyOwner();
    _;
  }
```

## [G-4] Use the cached value instead of fetching a storage value

### Vault721.sol.mint(): _proxyRegistry[_proxy] has already been cached and thus the cached value should be used. Saves ~200 gas (2 SLOADS)

- https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L94

```diff
File: src/contracts/proxies/Vault721.sol
94: function mint(address _proxy, uint256 _safeId) external {
95:    require(msg.sender == address(safeManager), 'V721: only safeManager');
-96:    require(_proxyRegistry[_proxy] != address(0), 'V721: non-native proxy'); //@audit Use store value
+96:    require(_user != address(0), 'V721: non-native proxy');
97:    address _user = _proxyRegistry[_proxy];
98:    _safeMint(_user, _safeId);
99:  }
```

## [G-5] With assembly, .call (bool success) transfer can be done gas-optimized

- https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODProxy.sol#L26C1-L36C2

```solidity
File: src/contracts/proxies/ODProxy.sol 
26: function execute(address _target, bytes memory _data) external payable onlyOwner returns (bytes memory _response) {
27:    if (_target == address(0)) revert TargetAddressRequired();
28:
29:    bool _succeeded;
30:    (_succeeded, _response) = _target.delegatecall(_data); //@audit Use assembly
31:
32:    if (!_succeeded) {
33:      revert TargetCallFailed(_response);
34:    }
35:  }
```

## [G-6] Use assembly to perform efficient back-to-back calls

If a similar external call is performed back-to-back, we can use assembly to reuse any function signatures and function parameters that stay the same. In addition, we can also reuse the same memory space for each function call (`scratch space` + `free memory pointer` + `zero slot`), which can potentially allow us to avoid memory expansion costs.

- https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol

```solidity
File: src/contracts/AccountingEngine.sol
178: uint256 _coinBalance = safeEngine.coinBalance(address(this)); //@audit external call
179: uint256 _debtBalance = safeEngine.debtBalance(address(this));
....
204: uint256 _coinBalance = safeEngine.coinBalance(address(this)); //@audit external call
205: uint256 _debtBalance = safeEngine.debtBalance(address(this));
....
255: uint256 _debtToSettle = Math.min(safeEngine.coinBalance(address(this)), safeEngine.debtBalance(address(this)));
```

- https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L63C1-L65C1

```solidity
File: src/contracts/oracles/UniV3Relayer.sol
63: baseAmount = uint128(10 ** IERC20Metadata(_baseToken).decimals()); //@audit back to back external call
64:    multiplier = 18 - IERC20Metadata(_quoteToken).decimals();
```