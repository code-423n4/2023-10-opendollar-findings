# Gas report

## Table Of Contents

- [Gas report](#gas-report)
  - [Table Of Contents](#table-of-contents)
  - [Summary](#summary)
  - [Using immutable on variables that are only set in the constructor and never after (Save 14.7K Gas)](#using-immutable-on-variables-that-are-only-set-in-the-constructor-and-never-after-save-147k-gas)
  - [Avoid making external calls if we don't have to](#avoid-making-external-calls-if-we-dont-have-to)
  - [Avoid Making any state reads if we can revert early(Save 2100 Gas )](#avoid-making-any-state-reads-if-we-can-revert-earlysave-2100-gas-)
  - [Variable already cached but in the wrong order - refactor to save 1 SLOAD(100 Gas)](#variable-already-cached-but-in-the-wrong-order---refactor-to-save-1-sload100-gas)
  - [Modifier execution order can help save an entire SLOAD(Save 2100 Gas)](#modifier-execution-order-can-help-save-an-entire-sloadsave-2100-gas)
  - [Using storage instead of memory for structs/arrays saves gas(**Not found by the bot**)](#using-storage-instead-of-memory-for-structsarrays-saves-gasnot-found-by-the-bot)
    - [ODSafeManager.sol.transferSAFEOwnership(): Use storage for `_sData`](#odsafemanagersoltransfersafeownership-use-storage-for-_sdata)
    - [ODSafeManager.sol.modifySAFECollateralization(): Use storage for `_sData` and cache fields being called repeatedly](#odsafemanagersolmodifysafecollateralization-use-storage-for-_sdata-and-cache-fields-being-called-repeatedly)
    - [ODSafeManager.sol.transferCollateral(): Use storage for `_sData`](#odsafemanagersoltransfercollateral-use-storage-for-_sdata)
    - [ODSafeManager.sol.quitSystem(): Use storage for `_sData` and cache repeatedly read fields](#odsafemanagersolquitsystem-use-storage-for-_sdata-and-cache-repeatedly-read-fields)
    - [ODSafeManager.sol.enterSystem(): Use storage for `_sData` and cache repeatedly read fields](#odsafemanagersolentersystem-use-storage-for-_sdata-and-cache-repeatedly-read-fields)
    - [ODSafeManager.sol.moveSAFE(): Use storage for `_srcData,_dstData` and cache repeatedly read fields](#odsafemanagersolmovesafe-use-storage-for-_srcdata_dstdata-and-cache-repeatedly-read-fields)
    - [ODSafeManager.sol.removeSAFE(): Use storage for `_sData` and cache repeatedly read fields](#odsafemanagersolremovesafe-use-storage-for-_sdata-and-cache-repeatedly-read-fields)
    - [ODSafeManager.sol.protectSAFE(): Use storage for `_sData` and cache repeatedly read fields](#odsafemanagersolprotectsafe-use-storage-for-_sdata-and-cache-repeatedly-read-fields)
  - [Leverage mapping and dot notation for struct assignment](#leverage-mapping-and-dot-notation-for-struct-assignment)
  - [Don't cache if used only once](#dont-cache-if-used-only-once)


## Summary

The findings from the bot should be reviewed as they contain some solid issues
The following findings were missed by the bot(in some cases,bot has identified some instances but missed others, I try to explain whenever this happens)

Gas estimates are done using the opcodes involved since most of this functions were not covered by tests 

## Using immutable on variables that are only set in the constructor and never after (Save 14.7K Gas)

Use immutable if you want to assign a permanent value at construction. Use constants if you already know the permanent value. Both get directly embedded in bytecode, saving SLOAD.
**Note: This were not found by the bot**

**Total Instances: 7**
**Gas Per Instance: 2.1 K**
**Total Gas Saved: 14.7K gas**


https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L58
```solidity
File: /src/contracts/AccountingEngine.sol
58:  AccountingEngineParams public _params;
```


https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/oracles/UniV3Relayer.sol#L23
```solidity
File: /src/contracts/oracles/UniV3Relayer.sol
23:  address public baseToken;

25:  address public quoteToken;

33:  uint128 public baseAmount;

35:  uint256 public multiplier;

37:  uint32 public quotePeriod;
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L26
```solidity
File: /src/contracts/proxies/ODSafeManager.sol
26:  address public safeEngine;
```

## Avoid making external calls if we don't have to

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L175-L181
```solidity
File: /src/contracts/AccountingEngine.sol
175:  function auctionDebt() external returns (uint256 _id) {
176:    if (_params.debtAuctionBidSize == 0) revert AccEng_DebtAuctionDisabled();

178:    uint256 _coinBalance = safeEngine.coinBalance(address(this));
179:    uint256 _debtBalance = safeEngine.debtBalance(address(this));

181:    if (_params.debtAuctionBidSize > _unqueuedUnauctionedDebt(_debtBalance)) revert AccEng_InsufficientDebt();
```
The if statement on the line 182 only depends on the variable `_debtBalance` and therefore we do not need to load `_coinBalance` before performing the check. 
In case of a revert on the if check, the gas used to do the external call `safeEngine.coinBalance(address(this));` would be wasted. We can minimize this cost by performing the check earlier as shown below

```diff
diff --git a/src/contracts/AccountingEngine.sol b/src/contracts/AccountingEngine.sol
index 0f96326..1735f93 100644
--- a/src/contracts/AccountingEngine.sol
+++ b/src/contracts/AccountingEngine.sol
@@ -175,10 +175,10 @@ contract AccountingEngine is Authorizable, Modifiable, Disableable, IAccountingE
   function auctionDebt() external returns (uint256 _id) {
     if (_params.debtAuctionBidSize == 0) revert AccEng_DebtAuctionDisabled();

-    uint256 _coinBalance = safeEngine.coinBalance(address(this));
     uint256 _debtBalance = safeEngine.debtBalance(address(this));
-
     if (_params.debtAuctionBidSize > _unqueuedUnauctionedDebt(_debtBalance)) revert AccEng_InsufficientDebt();
+
+    uint256 _coinBalance = safeEngine.coinBalance(address(this));

     (_coinBalance, _debtBalance) = _settleDebt(_coinBalance, _debtBalance, _coinBalance);
     totalOnAuctionDebt += _params.debtAuctionBidSize;
```


## Avoid Making any state reads if we can revert early(Save 2100 Gas )

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L136-L152
```solidity
File: /src/contracts/proxies/ODSafeManager.sol
136:  function transferSAFEOwnership(uint256 _safe, address _dst) external {
137:    require(msg.sender == address(vault721), 'SafeMngr: Only Vault721');

139:    if (_dst == address(0)) revert ZeroAddress();
140:    SAFEData memory _sData = _safeData[_safe];
141:    if (_dst == _sData.owner) revert AlreadySafeOwner();
```
Avoid reading from storage if we might revert early. The first check involves making a state read, `vault721` 
The second check, the if statement, checks if  a function parameter `_dst`  is equal to `address(0)` and reverts if it happens to be zero.
As it is, if `_dst` is zero address, we would consume gas in the require statement to make the state read(2100 Gas for the state read) and then revert on the second check
we can save some gas in the case of such a revert by moving the cheap check to the top and only reading state variable after validating the function parameter

```diff
diff --git a/src/contracts/proxies/ODSafeManager.sol b/src/contracts/proxies/ODS
afeManager.sol
index b3ab2f5..33cc7db 100644
--- a/src/contracts/proxies/ODSafeManager.sol
+++ b/src/contracts/proxies/ODSafeManager.sol
@@ -134,9 +134,10 @@ contract ODSafeManager is IODSafeManager {

   // Give the safe ownership to a dst address.
   function transferSAFEOwnership(uint256 _safe, address _dst) external {
+    if (_dst == address(0)) revert ZeroAddress();
+
     require(msg.sender == address(vault721), 'SafeMngr: Only Vault721');

-    if (_dst == address(0)) revert ZeroAddress();
     SAFEData memory _sData = _safeData[_safe];
     if (_dst == _sData.owner) revert AlreadySafeOwner();

```

## Variable already cached but in the wrong order - refactor to save 1 SLOAD(100 Gas)

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L94-L99
```solidity
File: /src/contracts/proxies/Vault721.sol
94:  function mint(address _proxy, uint256 _safeId) external {
95:    require(msg.sender == address(safeManager), 'V721: only safeManager');
96:    require(_proxyRegistry[_proxy] != address(0), 'V721: non-native proxy');
97:    address _user = _proxyRegistry[_proxy];
98:    _safeMint(_user, _safeId);
99:  }
```

```diff
   function mint(address _proxy, uint256 _safeId) external {
     require(msg.sender == address(safeManager), 'V721: only safeManager');
-    require(_proxyRegistry[_proxy] != address(0), 'V721: non-native proxy');
-    address _user = _proxyRegistry[_proxy];
+     address _user = _proxyRegistry[_proxy];
+    require(_user != address(0), 'V721: non-native proxy');
+
     _safeMint(_user, _safeId);
   }
```

## Modifier execution order can help save an entire SLOAD(Save 2100 Gas)

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L104-L114
**Note: This would only make sense if `governor` remains a state variable, bot has suggested making it immutable**
```solidity
File: /src/contracts/proxies/Vault721.sol
104:  function updateNftRenderer(
105:    address _nftRenderer,
106:    address _oracleRelayer,
107:    address _taxCollector,
108:    address _collateralJoinFactory
109:  ) external onlyGovernor nonZero(_oracleRelayer) nonZero(_taxCollector) nonZero(_collateralJoinFactory) {
110:    address _safeManager = address(safeManager);
111:    require(_safeManager != address(0));
112:    _setNftRenderer(_nftRenderer);
113:    nftRenderer.setImplementation(_safeManager, _oracleRelayer, _taxCollector, _collateralJoinFactory);
114:  }
```

Our function makes use of 2 modifers `onlyGovernor` and `nonZero`. The modifiers are defined as follows
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L40-L43
```solidity
40:  modifier onlyGovernor() {
41:    if (msg.sender != governor) revert NotGovernor();
42:    _;
43:  }
```
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L48-L51
```solidity
48:  modifier nonZero(address _addr) {
49:    if (_addr == address(0)) revert ZeroAddress();
50:    _;
51:  }
```

The first modifier `onlyGovernor` which is also the first one to be executed in our function involves reading a state variable `governor`, Since this is the first SLOAD we consume 2100 Gas for the SLOAD.
The second modifier `nonZero` does not read any state variable, only parameters being passed to it, in our case , some function parameters

According to the docs,multiple modifiers are evaluated in the order presented

Due to how they are order in our calling function `updateNftRenderer` ie 
`  ) external onlyGovernor nonZero(_oracleRelayer) nonZero(_taxCollector) nonZero(_collateralJoinFactory) {`
The `onlyGovernor` is checked first and if `msg.sender` == `governor` we proceed to check the second modifier `nonZero` passing our function parameter `_oracleRelayer` and again proceed to the next or revert if the passed parameter `==` `address(0)`


In case we do revert on the check `nonZero` it means, the gas cost used to do the SLOAD in the `onlyGovernor` check, would be wasted.
We can save some gas (2100 Gas) for the SLOAD if we reorder the modifier execution to first check for `nonZero` and only do `onlyGovernor` if all our parameters are not `zero`

```diff
diff --git a/src/contracts/proxies/Vault721.sol b/src/contracts/proxies/Vault721.sol
index 54dc8f7..eca21a1 100644
--- a/src/contracts/proxies/Vault721.sol
+++ b/src/contracts/proxies/Vault721.sol
@@ -106,7 +106,7 @@ contract Vault721 is ERC721Enumerable {
     address _oracleRelayer,
     address _taxCollector,
     address _collateralJoinFactory
-  ) external onlyGovernor nonZero(_oracleRelayer) nonZero(_taxCollector) nonZero(_collateralJoinFactory) {
+  ) external  nonZero(_oracleRelayer) nonZero(_taxCollector) nonZero(_collateralJoinFactory) onlyGovernor {
     address _safeManager = address(safeManager);
     require(_safeManager != address(0));
     _setNftRenderer(_nftRenderer);

```

## Using storage instead of memory for structs/arrays saves gas(**Not found by the bot**)

### ODSafeManager.sol.transferSAFEOwnership(): Use storage for `_sData`

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L136-L152
```solidity
File: /src/contracts/proxies/ODSafeManager.sol
136:  function transferSAFEOwnership(uint256 _safe, address _dst) external {

140:    SAFEData memory _sData = _safeData[_safe];
141:    if (_dst == _sData.owner) revert AlreadySafeOwner();

143:    _usrSafes[_sData.owner].remove(_safe);
144:    _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);

146:    _usrSafes[_dst].add(_safe);
147:    _usrSafesPerCollat[_dst][_sData.collateralType].add(_safe);

149:    _safeData[_safe].owner = _dst;

151:    emit TransferSAFEOwnership(msg.sender, _safe, _dst);
152:  }
```
After changing to storage, we cache the fields that are being read several times
```diff
diff --git a/src/contracts/proxies/ODSafeManager.sol b/src/contracts/proxies/ODS
afeManager.sol
index b3ab2f5..0b2346b 100644
--- a/src/contracts/proxies/ODSafeManager.sol
+++ b/src/contracts/proxies/ODSafeManager.sol
@@ -137,14 +137,16 @@ contract ODSafeManager is IODSafeManager {
     require(msg.sender == address(vault721), 'SafeMngr: Only Vault721');

     if (_dst == address(0)) revert ZeroAddress();
-    SAFEData memory _sData = _safeData[_safe];
-    if (_dst == _sData.owner) revert AlreadySafeOwner();
+    SAFEData storage _sData = _safeData[_safe];
+    address _owner = _sData.owner;
+    if (_dst == _owner) revert AlreadySafeOwner();

-    _usrSafes[_sData.owner].remove(_safe);
-    _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);
+    _usrSafes[_owner].remove(_safe);
+    bytes32 _collateralType = _sData.collateralType;
+    _usrSafesPerCollat[_owner][_collateralType].remove(_safe);

     _usrSafes[_dst].add(_safe);
-    _usrSafesPerCollat[_dst][_sData.collateralType].add(_safe);
+    _usrSafesPerCollat[_dst][_collateralType].add(_safe);

+     _sData.owner = _dst;

```

### ODSafeManager.sol.modifySAFECollateralization(): Use storage for `_sData` and cache fields being called repeatedly

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L155-L165
```solidity
File: /src/contracts/proxies/ODSafeManager.sol
155:  function modifySAFECollateralization(
156:    uint256 _safe,
157:    int256 _deltaCollateral,
158:    int256 _deltaDebt
159:  ) external safeAllowed(_safe) {
160:    SAFEData memory _sData = _safeData[_safe];
161:    ISAFEEngine(safeEngine).modifySAFECollateralization(
162:      _sData.collateralType, _sData.safeHandler, _sData.safeHandler, _sData.safeHandler, _deltaCollateral, _deltaDebt
163:    );
164:    emit ModifySAFECollateralization(msg.sender, _safe, _deltaCollateral, _deltaDebt);
165: }
```

```diff
diff --git a/src/contracts/proxies/ODSafeManager.sol b/src/contracts/proxies/ODS
afeManager.sol
index b3ab2f5..4d6e7ab 100644
--- a/src/contracts/proxies/ODSafeManager.sol
+++ b/src/contracts/proxies/ODSafeManager.sol
@@ -157,9 +157,10 @@ contract ODSafeManager is IODSafeManager {
     int256 _deltaCollateral,
     int256 _deltaDebt
   ) external safeAllowed(_safe) {
-    SAFEData memory _sData = _safeData[_safe];
+    SAFEData storage _sData = _safeData[_safe];
+    address _safeHandler = _sData.safeHandler;
     ISAFEEngine(safeEngine).modifySAFECollateralization(
-      _sData.collateralType, _sData.safeHandler, _sData.safeHandler, _sData.safeHandler, _deltaCollateral, _deltaDebt
+      _sData.collateralType, _safeHandler, _safeHandler, _safeHandler, _deltaCollateral, _deltaDebt
     );
     emit ModifySAFECollateralization(msg.sender, _safe, _deltaCollateral, _deltaDebt);
   }
```

### ODSafeManager.sol.transferCollateral(): Use storage for `_sData`

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L168-L172
```solidity
File: /src/contracts/proxies/ODSafeManager.sol
168:  function transferCollateral(uint256 _safe, address _dst, uint256 _wad) external safeAllowed(_safe) {
169:    SAFEData memory _sData = _safeData[_safe];
170:    ISAFEEngine(safeEngine).transferCollateral(_sData.collateralType, _sData.safeHandler, _dst, _wad);
171:    emit TransferCollateral(msg.sender, _safe, _dst, _wad);
172:  }
```

```diff
   function transferCollateral(uint256 _safe, address _dst, uint256 _wad) external safeAllowed(_safe) {
-    SAFEData memory _sData = _safeData[_safe];
+    SAFEData storage _sData = _safeData[_safe];
     ISAFEEngine(safeEngine).transferCollateral(_sData.collateralType, _sData.safeHandler, _dst, _wad);
     emit TransferCollateral(msg.sender, _safe, _dst, _wad);
   }
```

### ODSafeManager.sol.quitSystem(): Use storage for `_sData` and cache repeatedly read fields

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L189-L202
**The bot only suggested changing `_safeInfo`** - `We can actually optimize more` 

```solidity
File: /src/contracts/proxies/ODSafeManager.sol
189:  function quitSystem(uint256 _safe, address _dst) external safeAllowed(_safe) handlerAllowed(_dst) {
190:    SAFEData memory _sData = _safeData[_safe];
191:    ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_sData.collateralType, _sData.safeHandler);
192:    int256 _deltaCollateral = _safeInfo.lockedCollateral.toInt();
193:    int256 _deltaDebt = _safeInfo.generatedDebt.toInt();
194:    ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(
195:      _sData.collateralType, _sData.safeHandler, _dst, _deltaCollateral, _deltaDebt
196:    );

198:    // Remove safe from owner's list (notice it doesn't erase safe ownership)
199:    _usrSafes[_sData.owner].remove(_safe);
200:    _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);
201:    emit QuitSystem(msg.sender, _safe, _dst);
202:  }
```

```diff
   function quitSystem(uint256 _safe, address _dst) external safeAllowed(_safe) handlerAllowed(_dst) {
-    SAFEData memory _sData = _safeData[_safe];
-    ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_sData.collateralType, _sData.safeHandler);
+    SAFEData storage _sData = _safeData[_safe];
+    bytes32 _collateralType= _sData.collateralType;
+    address _safeHandler = _sData.safeHandler;
+    ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_collateralType, _safeHandler);
     int256 _deltaCollateral = _safeInfo.lockedCollateral.toInt();
     int256 _deltaDebt = _safeInfo.generatedDebt.toInt();
     ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(
-      _sData.collateralType, _sData.safeHandler, _dst, _deltaCollateral, _deltaDebt
+      _collateralType, _safeHandler, _dst, _deltaCollateral, _deltaDebt
     );
+    address _owner = _sData.owner;

     // Remove safe from owner's list (notice it doesn't erase safe ownership)
-    _usrSafes[_sData.owner].remove(_safe);
-    _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);
+    _usrSafes[_owner].remove(_safe);
+    _usrSafesPerCollat[_owner][_collateralType].remove(_safe);
     emit QuitSystem(msg.sender, _safe, _dst);
   }

```

### ODSafeManager.sol.enterSystem(): Use storage for `_sData` and cache repeatedly read fields

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L205-L214
**The bot only suggested changing `_safeInfo`** - `We can actually optimize more` 
```solidity
File: /src/contracts/proxies/ODSafeManager.sol
205:  function enterSystem(address _src, uint256 _safe) external handlerAllowed(_src) safeAllowed(_safe) {
206:    SAFEData memory _sData = _safeData[_safe];
207:    ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_sData.collateralType, _sData.safeHandler);
208:    int256 _deltaCollateral = _safeInfo.lockedCollateral.toInt();
209:    int256 _deltaDebt = _safeInfo.generatedDebt.toInt();
210:    ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(
211:      _sData.collateralType, _src, _sData.safeHandler, _deltaCollateral, _deltaDebt
212:    );
213:    emit EnterSystem(msg.sender, _src, _safe);
214:  }
```

```diff
   function enterSystem(address _src, uint256 _safe) external handlerAllowed(_src) safeAllowed(_safe) {
-    SAFEData memory _sData = _safeData[_safe];
-    ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_sData.collateralType, _sData.safeHandler);
+    SAFEData storage _sData = _safeData[_safe];
+    bytes32 _collateralType = _sData.collateralType;
+    address _safeHandler = _sData.safeHandler;
+    ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_collateralType, _safeHandler);
     int256 _deltaCollateral = _safeInfo.lockedCollateral.toInt();
     int256 _deltaDebt = _safeInfo.generatedDebt.toInt();
     ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(
-      _sData.collateralType, _src, _sData.safeHandler, _deltaCollateral, _deltaDebt
+      _collateralType, _src, _safeHandler, _deltaCollateral, _deltaDebt
     );
     emit EnterSystem(msg.sender, _src, _safe);
   }
```

### ODSafeManager.sol.moveSAFE(): Use storage for `_srcData,_dstData` and cache repeatedly read fields

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L217-L232
**The bot only suggested changing `_safeInfo`** - `We can actually optimize more` 
```solidity
File: /src/contracts/proxies/ODSafeManager.sol
217:  function moveSAFE(uint256 _safeSrc, uint256 _safeDst) external safeAllowed(_safeSrc) safeAllowed(_safeDst) {
218:    SAFEData memory _srcData = _safeData[_safeSrc];
219:    SAFEData memory _dstData = _safeData[_safeDst];
220:    if (_srcData.collateralType != _dstData.collateralType) revert CollateralTypesMismatch();
221:    ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_srcData.collateralType, _srcData.safeHandler);
222:    int256 _deltaCollateral = _safeInfo.lockedCollateral.toInt();
223:    int256 _deltaDebt = _safeInfo.generatedDebt.toInt();
224:    ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(
225:      _srcData.collateralType, _srcData.safeHandler, _dstData.safeHandler, _deltaCollateral, _deltaDebt
226:    );

228:    // Remove safe from owner's list (notice it doesn't erase safe ownership)
229:    _usrSafes[_srcData.owner].remove(_safeSrc);
230:    _usrSafesPerCollat[_srcData.owner][_srcData.collateralType].remove(_safeSrc);
231:    emit MoveSAFE(msg.sender, _safeSrc, _safeDst);
232:  }
```

```diff
   function moveSAFE(uint256 _safeSrc, uint256 _safeDst) external safeAllowed(_safeSrc) safeAllowed(_safeDst) {
-    SAFEData memory _srcData = _safeData[_safeSrc];
-    SAFEData memory _dstData = _safeData[_safeDst];
-    if (_srcData.collateralType != _dstData.collateralType) revert CollateralTypesMismatch();
-    ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_srcData.collateralType, _srcData.safeHandler);
+    SAFEData storage _srcData = _safeData[_safeSrc];
+    SAFEData storage _dstData = _safeData[_safeDst];
+
+    bytes32 _srcDataCollateralType= _srcData.collateralType;
+
+    if (_srcDataCollateralType != _dstData.collateralType) revert CollateralTypesMismatch();
+    address _srcDataOwner = _srcData.owner;
+    address _srcDataSafeHandler = _srcData.safeHandler;
+
+    ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_srcDataCollateralType, _srcDataSafeHandler);
     int256 _deltaCollateral = _safeInfo.lockedCollateral.toInt();
     int256 _deltaDebt = _safeInfo.generatedDebt.toInt();
     ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(
-      _srcData.collateralType, _srcData.safeHandler, _dstData.safeHandler, _deltaCollateral, _deltaDebt
+      _srcDataCollateralType, _srcDataSafeHandler, _dstData.safeHandler, _deltaCollateral, _deltaDebt
     );

     // Remove safe from owner's list (notice it doesn't erase safe ownership)
-    _usrSafes[_srcData.owner].remove(_safeSrc);
-    _usrSafesPerCollat[_srcData.owner][_srcData.collateralType].remove(_safeSrc);
+    _usrSafes[_srcDataOwner].remove(_safeSrc);
+    _usrSafesPerCollat[_srcDataOwner][_srcDataCollateralType].remove(_safeSrc);
     emit MoveSAFE(msg.sender, _safeSrc, _safeDst);
   }

```

### ODSafeManager.sol.removeSAFE(): Use storage for `_sData` and cache repeatedly read fields

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L242-L246
```solidity
File: /src/contracts/proxies/ODSafeManager.sol
242:  function removeSAFE(uint256 _safe) external safeAllowed(_safe) {
243:    SAFEData memory _sData = _safeData[_safe];
244:    _usrSafes[_sData.owner].remove(_safe);
245:    _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);
246:  }
```

```diff
   function removeSAFE(uint256 _safe) external safeAllowed(_safe) {
-    SAFEData memory _sData = _safeData[_safe];
-    _usrSafes[_sData.owner].remove(_safe);
-    _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);
+    SAFEData storage _sData = _safeData[_safe];
+    address _owner = _sData.owner;
+    _usrSafes[_owner].remove(_safe);
+    _usrSafesPerCollat[_owner][_sData.collateralType].remove(_safe);
   }
```

### ODSafeManager.sol.protectSAFE(): Use storage for `_sData` and cache repeatedly read fields

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L249-L253
```solidity
File: /src/contracts/proxies/ODSafeManager.sol
249:  function protectSAFE(uint256 _safe, address _liquidationEngine, address _saviour) external safeAllowed(_safe) {
250:    SAFEData memory _sData = _safeData[_safe];
251:    ILiquidationEngine(_liquidationEngine).protectSAFE(_sData.collateralType, _sData.safeHandler, _saviour);
252:    emit ProtectSAFE(msg.sender, _safe, _liquidationEngine, _saviour);
253:  }
```

```diff
   function protectSAFE(uint256 _safe, address _liquidationEngine, address _saviour) external safeAllowed(_safe) {
-    SAFEData memory _sData = _safeData[_safe];
+    SAFEData storage _sData = _safeData[_safe];
     ILiquidationEngine(_liquidationEngine).protectSAFE(_sData.collateralType, _sData.safeHandler, _saviour);
     emit ProtectSAFE(msg.sender, _safe, _liquidationEngine, _saviour);
   }
```


## Leverage mapping and dot notation for struct assignment

In dot notation, values are directly written to storage variable, When we use the current method in the code the compiler will allocate some memory to store the struct instance first before writing it to storage

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L118-L124
```solidity
File: /src/contracts/proxies/ODSafeManager.sol
118:  function openSAFE(bytes32 _cType, address _usr) external returns (uint256 _id) {

124:    _safeData[_safeId] = SAFEData({owner: _usr, safeHandler: _safeHandler, collateralType: _cType});
```

```diff
diff --git a/src/contracts/proxies/ODSafeManager.sol b/src/contracts/proxies/ODSafeManager.sol
index b3ab2f5..ed1a730 100644
--- a/src/contracts/proxies/ODSafeManager.sol
+++ b/src/contracts/proxies/ODSafeManager.sol
@@ -120,8 +120,11 @@ contract ODSafeManager is IODSafeManager {

     ++_safeId;
     address _safeHandler = address(new SAFEHandler(safeEngine));
+    SAFEData storage _SAFEData = _safeData[_safeId];

-    _safeData[_safeId] = SAFEData({owner: _usr, safeHandler: _safeHandler, collateralType: _cType});
+    _SAFEData.owner = _usr;
+    _SAFEData.safeHandler = _safeHandler;
+    _SAFEData.collateralType = _cType;

     _usrSafes[_usr].add(_safeId);
     _usrSafesPerCollat[_usr][_cType].add(_safeId);
```

## Don't cache if used only once

Caching here will simply increase gas cost, since it doesn't affect readability, we should not cache since we only reference the cached variables only once

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L101-L111
```solidity
File: /src/contracts/proxies/actions/BasicActions.sol
101:    address _safeEngine = ODSafeManager(_manager).safeEngine();

105:    // Generates debt in the SAFE
106:    _modifySAFECollateralization(
107:      _manager,
108:      _safeId,
109:      0,
110:      _getGeneratedDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
111:    );
```

```diff
diff --git a/src/contracts/proxies/actions/BasicActions.sol b/src/contracts/proxies/actions/BasicActions.sol
index e45fd41..8ad98f9 100644
--- a/src/contracts/proxies/actions/BasicActions.sol
+++ b/src/contracts/proxies/actions/BasicActions.sol
@@ -98,7 +98,6 @@ contract BasicActions is CommonActions, IBasicActions {
     uint256 _safeId,
     uint256 _deltaWad
   ) internal {
-    address _safeEngine = ODSafeManager(_manager).safeEngine();
     ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
     ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

@@ -107,7 +106,7 @@ contract BasicActions is CommonActions, IBasicActions {
       _manager,
       _safeId,
       0,
-      _getGeneratedDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
+      _getGeneratedDeltaDebt(ODSafeManager(_manager).safeEngine(), _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
     );

```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L128-L139
```solidity
File: /src/contracts/proxies/actions/BasicActions.sol
128:    address _safeEngine = ODSafeManager(_manager).safeEngine();


135:	// Paybacks debt to the SAFE
136:    _modifySAFECollateralization(
137:      _manager, _safeId, 0, _getRepaidDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler)
138:    );
139:  }
```

```diff
-    address _safeEngine = ODSafeManager(_manager).safeEngine();
     ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
     ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

@@ -134,7 +133,7 @@ contract BasicActions is CommonActions, IBasicActions {

     // Paybacks debt to the SAFE
     _modifySAFECollateralization(
-      _manager, _safeId, 0, _getRepaidDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler)
+      _manager, _safeId, 0, _getRepaidDeltaDebt(ODSafeManager(_manager).safeEngine(), _safeInfo.collateralType, _safeInfo.safeHandler)
     );
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L179-L192
```solidity
File: /src/contracts/proxies/actions/BasicActions.sol
179:    address _safeEngine = ODSafeManager(_manager).safeEngine();


186:    // Locks token amount into the SAFE and generates debt
187:    _modifySAFECollateralization(
188:      _manager,
189:      _safeId,
190:      _collateralAmount.toInt(),
191:      _getGeneratedDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
192:    );
```

```diff
-    address _safeEngine = ODSafeManager(_manager).safeEngine();
     ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
     ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

@@ -188,7 +187,7 @@ contract BasicActions is CommonActions, IBasicActions {
       _manager,
       _safeId,
       _collateralAmount.toInt(),
-      _getGeneratedDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
+      _getGeneratedDeltaDebt(ODSafeManager(_manager).safeEngine(), _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
     );
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L354-L367
```solidity
File: /src/contracts/proxies/actions/BasicActions.sol
354:    address _safeEngine = ODSafeManager(_manager).safeEngine();

361:    // Paybacks debt to the SAFE and unlocks token amount from it
362:    _modifySAFECollateralization(
363:      _manager,
364:      _safeId,
365:      -_collateralWad.toInt(),
366:      _getRepaidDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler)
367:    );
```

```diff
-    address _safeEngine = ODSafeManager(_manager).safeEngine();
     ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
     ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

@@ -363,7 +362,7 @@ contract BasicActions is CommonActions, IBasicActions {
       _manager,
       _safeId,
       -_collateralWad.toInt(),
-      _getRepaidDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler)
+      _getRepaidDeltaDebt(ODSafeManager(_manager).safeEngine(), _safeInfo.collateralType, _safeInfo.safeHandler)
     );
```
