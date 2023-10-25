# Quality Assurance Report

| QA issues | Issues                                                                                                                                 | Instances |
|-----------|----------------------------------------------------------------------------------------------------------------------------------------|-----------|
| [N-01]    | Public variable not used in external contracts can be marked private/internal                                                          | 3         |
| [N-02]    | Missing event emission for critical state changes                                                                                      | 13        |
| [N-03]    | Cache variable early to prevent redundant caching and an extra SLOAD                                                                   | 1         |
| [N-04]    | Remove redundant condition in _isNotProxy() function                                                                                   | 1         |
| [N-05]    | Remove `if (_dst == address(0)) revert ZeroAddress();` redundant check since it is already checked in `_afterTokenTransfer()` function | 1         |
| [N-06]    | Function read() does not revert with "OLD!" as mentioned in comments                                                                   | 1         |
| [N-07]    | Remove `if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver();` redundant check                                  | 1         |
| [L-01]    | Image field does not point to an image but the testnet website                                                                         | 1         |
| [L-02]    | Missing check in constructor to see if quotePeriod is not zero                                                                         | 1         |
| [L-03]    | Missing cardinality check in function read()                                                                                           | 1         |
| [L-04]    | Rounding down in `_exitCollateral()` function can cause loss of precision leading to loss of funds for users                              | 1         |
| [R-01]    | Consider modifying the build() function which allows anyone to create an ODProxy for any user                                          | 1         |
| [R-02]    | Use `user` instead of `usr` in mappings to improve readability                                                                         | 2         |
| [R-03]    | Use `safeAllow` and `handlerAllow` instead of `safeCan` and `handlerCan` to better match the intention of the mappings                 | 2         |
| [R-04]    | Add brackets around `10 ** multiplier` to improve code readability and provide clarity in which operation takes precedence first       | 1         |

**Note: N = Non-Critical, L = Low-severity, R = Recommendation**

### Total Non-Critical issues: 21 instances across 7 issues
### Total Low-severity issues: 4 instances across 4 issues
### Total Recommendations: 6 instances across 4 issues
### Total QA issues: 31 instances across 15 issues

## [N-01] Public variable not used in external contracts can be marked private/internal

There are 3 instances of this:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L18C1-L20C34

```solidity
File: src/contracts/proxies/Vault721.sol
18:   address public governor;
19:   IODSafeManager public safeManager;
20:   NFTRenderer public nftRenderer;
```

## [N-02] Missing event emission for critical state changes

There are 13 instances of this:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L33C1-L35C4

```solidity
File: Vault721.sol
34:   constructor(address _governor) ERC721('OpenDollar Vault', 'ODV') {
35:     governor = _governor;
36:     //@audit NC - missing event emission
37:   }
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L104C1-L114C4

```solidity
File: Vault721.sol
110:   function updateNftRenderer(
111:     address _nftRenderer,
112:     address _oracleRelayer,
113:     address _taxCollector,
114:     address _collateralJoinFactory
115:   ) external onlyGovernor nonZero(_oracleRelayer) nonZero(_taxCollector) nonZero(_collateralJoinFactory) {
116:     address _safeManager = address(safeManager);
117:     require(_safeManager != address(0));
118:     _setNftRenderer(_nftRenderer);
119:     nftRenderer.setImplementation(_safeManager, _oracleRelayer, _taxCollector, _collateralJoinFactory);
120:     //@audit NC - missing event emission
121:   }
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L119C1-L121C4

```solidity
File: Vault721.sol
127:   function updateContractURI(string memory _metaData) external onlyGovernor {
128:     contractMetaData = _metaData;
129:     //@audit NC - missing event emission
130:   }
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L172C1-L175C1

```solidity
File: Vault721.sol
183:   function _setSafeManager(address _safeManager) internal nonZero(_safeManager) {
184:     safeManager = IODSafeManager(_safeManager);
185:     //@audit NC - missing event emission
186:   }
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L179C1-L181C4

```solidity
File: Vault721.sol
191:   function _setNftRenderer(address _nftRenderer) internal nonZero(_nftRenderer) {
192:     nftRenderer = NFTRenderer(_nftRenderer);
193:     //@audit NC - missing event emission
194:   }
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L64C1-L68C4

```solidity
File: ODSafeManager.sol
65:   constructor(address _safeEngine, address _vault721) {
66:     safeEngine = _safeEngine.assertNonNull(); 
67:     vault721 = IVault721(_vault721);
68:     vault721.initializeManager();
69:     //@audit NC - missing event emission
70:   }
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L235

```solidity
File: ODSafeManager.sol
265:   function addSAFE(uint256 _safe) external {
266:     SAFEData memory _sData = _safeData[_safe];
267:     _usrSafes[msg.sender].add(_safe);
268:     _usrSafesPerCollat[msg.sender][_sData.collateralType].add(_safe);
269:     //@audit NC - missing event emission
270:   }
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L242

```solidity
File: ODSafeManager.sol
273:   function removeSAFE(uint256 _safe) external safeAllowed(_safe) {
274:     SAFEData memory _sData = _safeData[_safe];
275:     _usrSafes[_sData.owner].remove(_safe);
276:     _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);
277:     //@audit NC - missing event emission
278:   }
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODProxy.sol#L14C1-L17C1

```solidity
File: ODProxy.sol
14:   constructor(address _owner) {
15:     OWNER = _owner;
16:     //@audit NC - missing event emission
17:   }
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/oracles/CamelotRelayer.sol#L40C1-L62C4

```solidity
File: CamelotRelayer.sol
40:   constructor(address _baseToken, address _quoteToken, uint32 _quotePeriod) {
41:     // camelotPair = ICamelotFactory(_CAMELOT_FACTORY).getPair(_baseToken, _quoteToken);
42:     camelotPair = IAlgebraFactory(_CAMELOT_FACTORY).poolByPair(_baseToken, _quoteToken);
43:     if (camelotPair == address(0)) revert CamelotRelayer_InvalidPool();
44: 
45:     address _token0 = ICamelotPair(camelotPair).token0();
46:     address _token1 = ICamelotPair(camelotPair).token1();
47: 
48:     // The factory validates that both token0 and token1 are desired baseToken and quoteTokens
49:     if (_token0 == _baseToken) {
50:       baseToken = _token0;
51:       quoteToken = _token1;
52:     } else {
53:       baseToken = _token1;
54:       quoteToken = _token0;
55:     }
56: 
57:     baseAmount = uint128(10 ** IERC20Metadata(_baseToken).decimals());
58:     multiplier = 18 - IERC20Metadata(_quoteToken).decimals(); 
59:     quotePeriod = _quotePeriod; 
60: 
61:     symbol = string(abi.encodePacked(IERC20Metadata(_baseToken).symbol(), ' / ', IERC20Metadata(_quoteToken).symbol()));
62:     //@audit NC - missing event emission
63:   }
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L86

```solidity
File: AccountingEngine.sol
086:   constructor(
087:     address _safeEngine,
088:     address _surplusAuctionHouse,
089:     address _debtAuctionHouse,
090:     AccountingEngineParams memory _accEngineParams
091:   ) Authorizable(msg.sender) validParams {
092:     safeEngine = ISAFEEngine(_safeEngine.assertNonNull());
093:     _setSurplusAuctionHouse(_surplusAuctionHouse);
094:     debtAuctionHouse = IDebtAuctionHouse(_debtAuctionHouse);
095: 
096:     lastSurplusTime = block.timestamp;
097: 
098:     _params = _accEngineParams;
099:     //@audit NC - missing event emission
100:   }
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L247

```solidity
File: AccountingEngine.sol
265:   function _onContractDisable() internal override {
266:     totalQueuedDebt = 0; 
267:     totalOnAuctionDebt = 0;
268:     disableTimestamp = block.timestamp;
269: 
270:     surplusAuctionHouse.disableContract();
271:     debtAuctionHouse.disableContract();
272: 
273:     uint256 _debtToSettle = Math.min(safeEngine.coinBalance(address(this)), safeEngine.debtBalance(address(this)));
274:     safeEngine.settleDebt(_debtToSettle);
275:     //@audit NC - missing event emission
276:   }
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L305

```solidity
File: AccountingEngine.sol
326:   function _setSurplusAuctionHouse(address _surplusAuctionHouse) internal {
327:     if (address(surplusAuctionHouse) != address(0)) {
328:       safeEngine.denySAFEModification(address(surplusAuctionHouse));
329:     }
330:     surplusAuctionHouse = ISurplusAuctionHouse(_surplusAuctionHouse);
331:     safeEngine.approveSAFEModification(_surplusAuctionHouse);
332:     //@audit NC - Missing event emission
333:   }
```

## [N-03] Cache variable early to prevent redundant caching and an extra SLOAD

There is 1 instance of this:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L94C1-L99C4

On Line 97, we can see `_proxyRegistry[_proxy]` being cached to `_user`. This caching is redundant since it could anyways be inlined on Line 98 directly. But we also observe `_proxyRegistry[_proxy]` on Line 96 in the require check. This is an extra SLOAD that could've been prevented if we perform the caching done on Line 97 before the require check on Line 96.
```solidity
File: src/contracts/proxies/Vault721.sol
94:   function mint(address _proxy, uint256 _safeId) external {
95:     require(msg.sender == address(safeManager), 'V721: only safeManager');
96:     require(_proxyRegistry[_proxy] != address(0), 'V721: non-native proxy');
97:     address _user = _proxyRegistry[_proxy];
98:     _safeMint(_user, _safeId);
99:   }
```
**Solution:**
```solidity
File: src/contracts/proxies/Vault721.sol
94:   function mint(address _proxy, uint256 _safeId) external {
95:     address _user = _proxyRegistry[_proxy];
96:     require(msg.sender == address(safeManager), 'V721: only safeManager');
97:     require(_user != address(0), 'V721: non-native proxy');
98:     _safeMint(_user, _safeId);
99:   }
```

## [N-04] Remove redundant condition in _isNotProxy() function

There is 1 instance of this:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L154C1-L156C4

The condition `ODProxy(_userRegistry[_user]).OWNER() != _user` is redundant and the first condition itself is sufficient to determine whether a user has an ODProxy or not. There's never a situation where the second condition could evaluate to true. Look at the table below:

| First condition | Second condition                |
|-----------------|---------------------------------|
| True            | Not checked since first is true |
| False           | False                           |

```solidity
File: Vault721.sol
163:   function _isNotProxy(address _user) internal view returns (bool) {
164:     return _userRegistry[_user] == address(0) || ODProxy(_userRegistry[_user]).OWNER() != _user;
165:   }
```

## [N-05] Remove `if (_dst == address(0)) revert ZeroAddress();` redundant check since it is already checked in `_afterTokenTransfer()` function

There is 1 instance of this issue:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L139

Remove below check:
```solidity
File: src/contracts/proxies/ODSafeManager.sol
139: if (_dst == address(0)) revert ZeroAddress();
```
Check already done before in _afterTokenTransfer() function:
```solidity
File: Vault721.sol
206:       if (_isNotProxy(to)) {
207:         proxy = _build(to);
208:       } else {
209:         proxy = payable(_userRegistry[to]);
210:       }
```

## [N-06] Function read() does not revert with "OLD!" as mentioned in comments

There is 1 instance of this:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/oracles/CamelotRelayer.sol#L91C1-L101C4

Function does not revert with "OLD!" since there is no such revert message in the consult function.
```solidity
File: CamelotRelayer.sol
092:   function read() external view returns (uint256 _result) {
093:     // This call may revert with 'OLD!' if the pool doesn't have enough cardinality or initialized history 
094:     (int24 _arithmeticMeanTick,) = OracleLibrary.consult(camelotPair, quotePeriod);
095:     uint256 _quoteAmount = OracleLibrary.getQuoteAtTick({
096:       tick: _arithmeticMeanTick,
097:       baseAmount: baseAmount,
098:       baseToken: baseToken,
099:       quoteToken: quoteToken
100:     });
101:     _result = _parseResult(_quoteAmount);
102:   }
```

## [N-07] Remove `if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver();` redundant check

There is 1 instance of this:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L225

Remove check below:
```solidity
225: if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver();
```
Check already [done before in the same function](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L201):
```solidity
201: if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver();
```

## [L-01] Image field does not point to an image but the testnet website

There is 1 instance of this issue:

In the below string variable contractMetaData, we can observe the image field points as such `"image": "https://app.opendollar.com/collectionImage.png"`. If we follow the link it leads us to the testnet website and not an image.
```solidity
File: Vault721.sol
22:   string public contractMetaData =
23:     '{"name": "Open Dollar Vaults","description": "Tradable Vaults for the Open Dollar stablecoin protocol. Caution! Trading this NFT means trading the ownership of your Vault in the Open Dollar protocol and all of the assets/collateral inside each Vault.","image": "https://app.opendollar.com/collectionImage.png","external_link": "https://opendollar.com"}';
```

## [L-02] Missing check in constructor to see if quotePeriod is not zero

There is 1 instance of this:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/oracles/CamelotRelayer.sol#L59

```solidity
File: CamelotRelayer.sol
59:     quotePeriod = _quotePeriod;
```

## [L-03] Missing cardinality check in function read()

There is 1 instance of this issue:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/oracles/CamelotRelayer.sol#L91C1-L101C4

On Line 93, the comment mentions that the read() function should revert with "OLD!" if the pool does not have enough cardinality or initialized history. But there is no check done for the cardinality, which can return an incorrect quote.
```solidity
File: CamelotRelayer.sol
092:   function read() external view returns (uint256 _result) {
093:     // This call may revert with 'OLD!' if the pool doesn't have enough cardinality or initialized history
094:     (int24 _arithmeticMeanTick,) = OracleLibrary.consult(camelotPair, quotePeriod);
095:     uint256 _quoteAmount = OracleLibrary.getQuoteAtTick({
096:       tick: _arithmeticMeanTick,
097:       baseAmount: baseAmount,
098:       baseToken: baseToken,
099:       quoteToken: quoteToken
100:     });
101:     _result = _parseResult(_quoteAmount);
102:   }
```
Solution:
```solidity
File: CamelotRelayer.sol
69:   function read() external view returns (uint256 _result) {
70:     // If the pool doesn't have enough history return false
71:     if (OracleLibrary.getOldestObservationSecondsAgo(camelotPair) < quotePeriod) {
72:       return (0, false);
73:     }
74:     // Consult the query with a TWAP period of quotePeriod
75:     (int24 _arithmeticMeanTick,) = OracleLibrary.consult(camelotPair, quotePeriod);
76:     // Calculate the quote amount
77:     uint256 _quoteAmount = OracleLibrary.getQuoteAtTick({
78:       tick: _arithmeticMeanTick,
79:       baseAmount: baseAmount,
80:       baseToken: baseToken,
81:       quoteToken: quoteToken
82:     });
83:     // Process the quote result to 18 decimal quote
84:     _result = _parseResult(_quoteAmount);
85:   }
```

## [L-04] Rounding down in _exitCollateral() function can cause loss of precision leading to loss of funds for users

There is 1 instance of this issue:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/CommonActions.sol#L118

In the function [_exitCollateral()](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/CommonActions.sol#L118), users can experience loss of funds if the wad amount is smaller than the decimals representation in the denominator.

The below code snippet is from the CommonActions.sol contract is inherited in the BasicActions.sol contract. This _exitCollateral() function is called when the freeTokenCollateral() function in the BasicActions.sol contract is called. This function does the operation below based on the decimals of the ERC20 token being used. In case the numerator i.e. the _wad amount is smaller than the denominator, the final _weiAmount rounds down to zero. This can lead to loss of funds for the user who tries to exit with his collateral.
```solidity
File: CommonActions.sol
118:     uint256 _weiAmount = _wad / 10 ** (18 - _decimals);
119:     __collateralJoin.exit(msg.sender, _weiAmount);
```

## [R-01] Consider modifying the build() function which allows anyone to create an ODProxy for any user

There is 1 instance of this:

The build() function on Line 90 below allows anyone to create an ODProxy for any user. Although this is a convenient method, it should still implement some sort of approval mechanism where the caller can deploy an ODProxy only on the approval of the user (for whom the proxy is being created). That way the method is less prone to frontrunning attacks that could DOS the user's call (since the Proxy would already be created but user call fails when trying to build one).
```solidity
File: Vault721.sol
81:   function build() external returns (address payable _proxy) {
82:     if (!_isNotProxy(msg.sender)) revert ProxyAlreadyExist();
83:     _proxy = _build(msg.sender);
84:   }
85: 
86:   /**
87:    * @dev allows user without an ODProxy to deploy a new ODProxy
88:    */
89:   //@audit Low - allows anyone to build an ODProxy for any user
90:   function build(address _user) external returns (address payable _proxy) {
91:     if (!_isNotProxy(_user)) revert ProxyAlreadyExist();
92:     _proxy = _build(_user);
93:   }
```

## [R-02] Use `user` instead of `usr` in mappings to improve readability

There are 2 instances of this:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L32C2-L34C110

Instead of `_usrSafes` use `_userSafes` and `_userSafesPerCollat` instead of `_usrSafesPerCollat`
```solidity
File: ODSafeManager.sol
32:   mapping(address _safeOwner => EnumerableSet.UintSet) private _usrSafes;
33:   /// @notice Mapping of user addresses to their enumerable set of safes per collateral type
34:   mapping(address _safeOwner => mapping(bytes32 _cType => EnumerableSet.UintSet)) private _usrSafesPerCollat; 
```

## [R-03] Use `safeAllow` and `handlerAllow` instead of `safeCan` and `handlerCan` to better match the intention of the mappings

There are 2 instances of this:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L39C1-L42C1

These mappings represent the allowances the `_owner` gives to other addresses. To suit this intention of allowance, it would be better to rename the mappings to `safeAllow` and `handlerAllow`.
```solidity
File: ODSafeManager.sol
40:   mapping(address _owner => mapping(uint256 _safeId => mapping(address _caller => uint256 _ok))) public safeCan;
41:   /// @inheritdoc IODSafeManager
42:   mapping(address _safeHandler => mapping(address _caller => uint256 _ok)) public handlerCan;
```

## [R-04] Add brackets around `10 ** multiplier` to improve code readability and provide clarity in which operation takes precedence first

There is 1 instance of this:

Instead of this:
```solidity
File: CamelotRelayer.sol
104:   function _parseResult(uint256 _quoteResult) internal view returns (uint256 _result) {
105:     return _quoteResult * 10 ** multiplier;
106:   }
```
Use this:
```solidity
File: CamelotRelayer.sol
104:   function _parseResult(uint256 _quoteResult) internal view returns (uint256 _result) {
105:     return _quoteResult * (10 ** multiplier);
106:   }
```