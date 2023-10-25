## Gas Optimizations

## Sumary

|   No | ISSUE  | INSTANCE  |
|------|--------|-----------|
|[G-01]|Use uint256(1)/uint256(2) instead for true and false boolean states|4|
|[G-02]|+= costs more gas than = + for state variables|1|
|[G-03]|ot using the named return variable when a function returns, wastes deployment gas|2|
|[G-04]|Use nested if statements instead of &&|4|
|[G-05]|Use != 0 instead of > 0 for unsigned integer comparison|2|
|[G-06]|Tightly pack storage variables/optimize the order of variable declaration|2|
|[G-07]|Using a positive conditional flow to save a NOT opcode|4|
|[G-08]|Use assembly to perform efficient back-to-back calls|5|
|[G-09]|Caching global variables is more expensive than using the actual variable (use msg.sender instead of caching it)|5|
|[G-10]|Use hardcode address instead address(this)|11|
|[G-11]|Unnecessary look up in if condition|1|
|[G-12]| Avoid emitting constants.|14|

## [G-01]  Use uint256(1)/uint256(2) instead for true and false boolean states
If you don’t use boolean for storage you will avoid Gwarmaccess 100 gas. In addition, state changes of boolean from true to false can cost up to ~20000 gas rather than uint256(2) to uint256(1) that would cost significantly less.

```solidity

file: contracts/oracles/CamelotRelayer.sol

71      return (0, false);

84   _validity = true;

90   _validity = true;

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L71
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L84

```solidity

77  return (0, false);

90_validity = true;

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L77
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L90

## [G-02]  += costs more gas than = + for state variables

```solidity

file: contracts/AccountingEngine.sol

184  totalOnAuctionDebt += _params.debtAuctionBidSize;
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L184

## [G-03] Not using the named return variable when a function returns, wastes deployment gas

```solidity

file: contracts/AccountingEngine.sol

62  return _params;
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L62

```solidity

file: contracts/proxies/ODSafeManager.sol

132   return _safeId;

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L132

## [G-04] Use nested if statements instead of &&
If the if statement has a logical AND and is not followed by an else statement, it can be replaced with 2 if statements.

```solidity

file: contracts/proxies/ODSafeManager.sol

51  if (msg.sender != _owner && safeCan[_owner][_safe][msg.sender] == 0) revert SafeNotAllowed();

60 if (msg.sender != _owner && safeCan[_owner][_safe][msg.sender] == 0) revert SafeNotAllowed();
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L51
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L60

## [G-05] Use != 0 instead of > 0 for unsigned integer comparison
it's generally more gas-efficient to use != 0 instead of > 0 when
comparing unsigned integer types.

```solidity

file: contracts/AccountingEngine.sol

224  if (_params.surplusTransferPercentage > 0) 

269   if (_coinBalance > 0) 
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L224
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L269

## [G-06] Tightly pack storage variables/optimize the order of variable declaration
The EVM works with 32 byte words. Variables less than 32 bytes can be declared next to eachother in storage and this will pack the values together into a single 32 byte storage slot (if the values combined are <= 32 bytes). If the variables packed together are retrieved together in functions

```solidity

file: ontracts/oracles/CamelotRelayer.sol

34   uint128 public baseAmount;
  /// @inheritdoc ICamelotRelayer
  uint256 public multiplier;
  /// @inheritdoc ICamelotRelayer
38 uint32 public quotePeriod;

```Unnecessary look up in if condition
```solidity

file: contracts/oracles/UniV3Relayer.sol

33   uint128 public baseAmount;
  /// @inheritdoc IUniV3Relayer
  uint256 public multiplier;
  /// @inheritdoc IUniV3Relayer
37  uint32 public quotePeriod;
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L33-L37

## [G-07] Using a positive conditional flow to save a NOT opcode
Estimated savings: 3 gas

```solidity

file: contracts/proxies/ODProxy.sol

32  if (!_succeeded) 

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODProxy.sol#L32


```solidit


file: contracts/proxies/Vault721.sol

78   if (!_isNotProxy(msg.sender)) revert ProxyAlreadyExist();

86     if (!_isNotProxy(_user)) revert ProxyAlreadyExist();

189   if (from != address(0)) 
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L78
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L86
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L189

## [G-08] Use assembly to perform efficient back-to-back calls
If a similar external call is performed back-to-back, we can use assembly to reuse any function signatures and function parameters that stay the same. In addition, we can also reuse the same memory space for each function call (scratch space + free memory pointer + zero slot), which can potentially allow us to avoid memory expansion costs.

```solidity

file: contracts/gov/ODGovernor.sol

52  function votingDelay() public view override(IGovernor, GovernorSettings) returns (uint256) {
    return super.votingDelay();
  }

  /**
   * inherit: GovernorSettings
   */
  function votingPeriod() public view override(IGovernor, GovernorSettings) returns (uint256) {
    return super.votingPeriod();
  }

  /**
   * inherit: GovernorVotesQuorumFraction
   */
  function quorum(uint256 blockNumber) public view override(IGovernor, GovernorVotesQuorumFraction) returns (uint256) {
    return super.quorum(blockNumber);
  }

  /**
   * inherit: Governor, GovernorTimelockControl
   */
  function state(uint256 proposalId)
    public
    view
    override(Governor, IGovernor, GovernorTimelockControl)
    returns (ProposalState)
  {
    return super.state(proposalId);
  }

  /**
   * inherit: Governor, GovernorCompatibilityBravo
   */
  function propose(
    address[] memory targets,
    uint256[] memory values,
    bytes[] memory calldatas,
    string memory description
  ) public override(Governor, GovernorCompatibilityBravo, IGovernor) returns (uint256) {
    return super.propose(targets, values, calldatas, description);
  }

  /**
   * inherit: Governor, GovernorSettings
   */
  function proposalThreshold() public view override(Governor, GovernorSettings) returns (uint256) {
    return super.proposalThreshold();
  }

  /**
   * inherit: Governor, GovernorTimelockControl
   */
  function _execute(
    uint256 proposalId,
    address[] memory targets,
    uint256[] memory values,
    bytes[] memory calldatas,
    bytes32 descriptionHash
  ) internal override(Governor, GovernorTimelockControl) {
    super._execute(proposalId, targets, values, calldatas, descriptionHash);
  }

  /**
   * inherit: Governor, GovernorTimelockControl
   */
  function _cancel(
    address[] memory targets,
    uint256[] memory values,
    bytes[] memory calldatas,
    bytes32 descriptionHash
  ) internal override(Governor, GovernorTimelockControl) returns (uint256) {
    return super._cancel(targets, values, calldatas, descriptionHash);
  }

  /**
   * inherit: Governor, GovernorTimelockControl
   */
  function _executor() internal view override(Governor, GovernorTimelockControl) returns (address) {
    return super._executor();
  }

  /**
   * inherit: Governor, GovernorTimelockControl
   */
  function supportsInterface(bytes4 interfaceId)
    public
    view
    override(Governor, IERC165, GovernorTimelockControl)
    returns (bool)
  {
    return super.supportsInterface(interfaceId);
143  }

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/gov/ODGovernor.sol#L52-L143

```solidit

file: contracts/proxies/Vault721.sol

56  function initializeManager() external {
    if (address(safeManager) == address(0)) _setSafeManager(msg.sender);
  }

  /**
   * @dev initializes NFTRenderer contract
   */
  function initializeRenderer() external {
    if (address(nftRenderer) == address(0)) _setNftRenderer(msg.sender);
  }

  /**
   * @dev get proxy by user address
   */
  function getProxy(address _user) external view returns (address _proxy) {
    _proxy = _userRegistry[_user];
  }

  /**
   * @dev allows msg.sender without an ODProxy to deploy a new ODProxy
   */
  function build() external returns (address payable _proxy) {
    if (!_isNotProxy(msg.sender)) revert ProxyAlreadyExist();
    _proxy = _build(msg.sender);
  }

  /**
   * @dev allows user without an ODProxy to deploy a new ODProxy
   */
  function build(address _user) external returns (address payable _proxy) {
    if (!_isNotProxy(_user)) revert ProxyAlreadyExist();
    _proxy = _build(_user);
88  }

119   function updateContractURI(string memory _metaData) external onlyGovernor {
    contractMetaData = _metaData;
  }

  /**
   * @dev allows DAO to update protocol implementation of SafeManager
   */
  function setSafeManager(address _safeManager) external onlyGovernor {
    _setSafeManager(_safeManager);
  }

  /**
   * @dev allows DAO to update protocol implementation of NFTRenderer
   */
  function setNftRenderer(address _nftRenderer) external onlyGovernor {
    _setNftRenderer(_nftRenderer);
  }

  /**
   * @dev generate URI with updated vault information
   */
  function tokenURI(uint256 _safeId) public view override returns (string memory uri) {
    uri = nftRenderer.render(_safeId);
  }

  /**
   * @dev contract level meta data
   */
  function contractURI() public view returns (string memory uri) {
    uri = string.concat('data:application/json;utf8,', contractMetaData);
  }

  /**
   * @dev check that proxy does not exist OR that the user does not own proxy
   */
  function _isNotProxy(address _user) internal view returns (bool) {
    return _userRegistry[_user] == address(0) || ODProxy(_userRegistry[_user]).OWNER() != _user;
  }

  /**
   * @dev deploys ODProxy for user to interact with protocol
   * updates _proxyRegistry and _userRegistry mappings for new ODProxy
   */
  function _build(address _user) internal returns (address payable _proxy) {
    _proxy = payable(address(new ODProxy(_user)));
    _proxyRegistry[_proxy] = _user;
    _userRegistry[_user] = _proxy;
    emit CreateProxy(_user, address(_proxy));
  }

  /**
   * @dev allows DAO to update protocol implementation of SafeManager
   */
  function _setSafeManager(address _safeManager) internal nonZero(_safeManager) {
    safeManager = IODSafeManager(_safeManager);
  }

  /**
   * @dev allows DAO to update protocol implementation of NFTRenderer
   */
  function _setNftRenderer(address _nftRenderer) internal nonZero(_nftRenderer) {
    nftRenderer = NFTRenderer(_nftRenderer);
  }

  /**
   * @dev _transfer calls `transferSAFEOwnership` on SafeManager
   * enforces that ODProxy exists for transfer or it deploys a new ODProxy for receiver of vault/nft
   */
  function _afterTokenTransfer(address from, address to, uint256 firstTokenId, uint256 batchSize) internal override {
188    require(to != address(0), 'V721: no burn');
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L56-L88
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L119-L188 

```solidit

file: contracts/proxies/ODSafeManager.sol

73   function getSafes(address _usr) external view returns (uint256[] memory _safes) {
    _safes = _usrSafes[_usr].values();
  }

  /// @inheritdoc IODSafeManager
  function getSafes(address _usr, bytes32 _cType) external view returns (uint256[] memory _safes) {
    _safes = _usrSafesPerCollat[_usr][_cType].values();
  }

  /// @inheritdoc IODSafeManager
  function getSafesData(address _usr)
    external
    view
    returns (uint256[] memory _safes, address[] memory _safeHandlers, bytes32[] memory _cTypes)
  {
    _safes = _usrSafes[_usr].values();
    _safeHandlers = new address[](_safes.length);
    _cTypes = new bytes32[](_safes.length);
    for (uint256 _i; _i < _safes.length; _i++) {
      _safeHandlers[_i] = _safeData[_safes[_i]].safeHandler;
      _cTypes[_i] = _safeData[_safes[_i]].collateralType;
    }
  }

  /// @inheritdoc IODSafeManager
  function safeData(uint256 _safe) external view returns (SAFEData memory _sData) {
    _sData = _safeData[_safe];
  }

  // --- Methods ---

  /// @inheritdoc IODSafeManager
  function allowSAFE(uint256 _safe, address _usr, uint256 _ok) external safeAllowed(_safe) {
    address _owner = _safeData[_safe].owner;
    safeCan[_owner][_safe][_usr] = _ok;
    emit AllowSAFE(msg.sender, _safe, _usr, _ok);
  }

  /// @inheritdoc IODSafeManager
  function allowHandler(address _usr, uint256 _ok) external {
    handlerCan[msg.sender][_usr] = _ok;
    emit AllowHandler(msg.sender, _usr, _ok);
  }

  /// @inheritdoc IODSafeManager
  function openSAFE(bytes32 _cType, address _usr) external returns (uint256 _id) {
119    if (_usr == address(0)) revert ZeroAddress();

142   function _openSAFE(address _manager, bytes32 _cType, address _usr) internal returns (uint256 _safeId) {
    _safeId = ODSafeManager(_manager).openSAFE(_cType, _usr);
  }

  /// @notice Routes the transferCollateral call to the ODSafeManager contract
  function _transferCollateral(address _manager, uint256 _safeId, address _dst, uint256 _deltaWad) internal {
    if (_deltaWad == 0) return;
    ODSafeManager(_manager).transferCollateral(_safeId, _dst, _deltaWad);
  }

  /// @notice Routes the transferInternalCoins call to the ODSafeManager contract
  function _transferInternalCoins(address _manager, uint256 _safeId, address _dst, uint256 _rad) internal {
    ODSafeManager(_manager).transferInternalCoins(_safeId, _dst, _rad);
155  }
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L73-L119
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L142-L155

## [G-09] Caching global variables is more expensive than using the actual variable (use msg.sender instead of caching it)

```solidity

file: contracts/AccountingEngine.sol

91 Authorizable(msg.sender) validParams 

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L91

```solidit

file: contracts/proxies/Vault721.sol

57  if (address(safeManager) == address(0)) _setSafeManager(msg.sender);

64  if (address(nftRenderer) == address(0)) _setNftRenderer(msg.sender);

78   if (!_isNotProxy(msg.sender)) revert ProxyAlreadyExist();
79    _proxy = _build(msg.sender)
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L57
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L64
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L78-L79

```solidit

file: contracts/proxies/SAFEHandler.sol

17   ISAFEEngine(_safeEngine).approveSAFEModification(msg.sender);
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/SAFEHandler.sol#L17

## [G-10] Use hardcode address instead address(this)
Instead of using address(this), it is more gas-efficient to pre-calculate and use the hardcoded address.

```solidity

file: contracts/AccountingEngine.sol
Use hardcode address instead address(this)s)), safeEngine.debtBalance(address(this)), _rad);

162  uint256 _coinBalance = safeEngine.coinBalance(address(this));

169  emit CancelDebt(_rad, _coinBalance - _rad, safeEngine.debtBalance(address(this)));
  }

178   uint256 _coinBalance = safeEngine.coinBalance(address(this));
179    uint256 _debtBalance = safeEngine.debtBalance(address(this)); 

187    _incomeReceiver: address(this),

204   uint256 _coinBalance = safeEngine.coinBalance(address(this));
205    uint256 _debtBalance = safeEngine.debtBalance(address(this));

228    _source: address(this),

255  uint256 _debtToSettle = Math.min(safeEngine.coinBalance(address(this)), safeEngine.debtBalance(address(this)));

264   uint256 _coinBalance = safeEngine.coinBalance(address(this));
265    uint256 _debtBalance = safeEngine.debtBalance(address(this));

271   _source: address(this),
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L105
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L140
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L162
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L169
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L178-179
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L187
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L204-L205
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L228
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L255
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L264-265
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L271

## [G-11] Unnecessary look up in if condition
If the || condition isn’t required, the second condition will have been looked up unnecessarily.
  
155   return _userRegistry[_user] == address(0) || ODProxy(_userRegistry[_user]).OWNER() != _user;  
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L155

## [G-12] Avoid emitting constants.
A log topic (declared with indexed) has a gas cost of Glogtopic (375 gas).

```solidity



file: contracts/proxies/Vault721.sol#

166 emit CreateProxy(_user, address(_proxy));

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L166

```solidity

file: factories/CamelotRelayerFactory.sol

30  emit NewCamelotRelayer(address(_camelotRelayer), _baseToken, _quoteToken, _quotePeriod);

```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/factories/CamelotRelayerFactory.sol#L30

```solidit

file: contracts/proxies/ODSafeManager.sol

108  emit AllowSAFE(msg.sender, _safe, _usr, _ok);

114 emit AllowHandler(msg.sender, _usr, _ok);

131  emit OpenSAFE(msg.sender, _usr, _safeId);

151   emit TransferSAFEOwnership(msg.sender, _safe, _dst);

164  emit ModifySAFECollateralization(msg.sender, _safe, _deltaCollateral, _deltaDebt);

171 emit TransferCollateral(msg.sender, _safe, _dst, _wad);

178  emit TransferCollateral(msg.sender, _cType, _safe, _dst, _wad);

185  emit TransferInternalCoins(msg.sender, _safe, _dst, _rad);

201 emit QuitSystem(msg.sender, _safe, _dst);

213 emit EnterSystem(msg.sender, _src, _safe);

231   emit MoveSAFE(msg.sender, _safeSrc, _safeDst);

252  emit ProtectSAFE(msg.sender, _safe, _liquidationEngine, _saviour);
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L108
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L114
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L131
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L151
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L164
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L171
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L178
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L185
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L201
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L213
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L231
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L252

## [G-12] Modifiers are redundant if used only once or not used at all. 

```solidit

file: contracts/proxies/Vault721.sol

40  modifier onlyGovernor() {
    if (msg.sender != governor) revert NotGovernor();
    _;
  }

  /**
   * @dev enforce non-zero address params
   */
  modifier nonZero(address _addr) {
    if (_addr == address(0)) revert ZeroAddress
    _;
51  }
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L40-L51

```solidit

file: contracts/proxies/ODSafeManager.sol

49   modifier safeAllowed(uint256 _safe) {
    address _owner = _safeData[_safe].owner;
    if (msg.sender != _owner && safeCan[_owner][_safe][msg.sender] == 0) revert SafeNotAllowed();
    _;
  }

  /**
   * @notice Checks if the sender is the safe handler has permissions to call the function
   * @param  _handler Address of the handler to check if msg.sender has permissions for
   */
  modifier handlerAllowed(address _handler) {
    if (msg.sender != _handler && handlerCan[_handler][msg.sender] == 0) revert HandlerNotAllowed();
    _;
62  }
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L49-L62

### [G-10] Use function instead of modifiers 
Deployment. Gas Saved: 115 926
Minimum Method Call. Gas Saved: 162
Average Method Call. Gas Saved: -264
Maximum Method Call. Gas Saved: -481
Overall gas change: 734 (2.459%)


40  modifier onlyGovernor() {
    if (msg.sender != governor) revert NotGovernor();
    _;
  }

  /**
   * @dev enforce non-zero address params
   */
  modifier nonZero(address _addr) {
    if (_addr == address(0)) revert ZeroAddress
    _;
51  }
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L40-L51

```solidit

file: contracts/proxies/ODSafeManager.sol

49   modifier safeAllowed(uint256 _safe) {
    address _owner = _safeData[_safe].owner;
    if (msg.sender != _owner && safeCan[_owner][_safe][msg.sender] == 0) revert SafeNotAllowed();
    _;
  }

  /**
   * @notice Checks if the sender is the safe handler has permissions to call the function
   * @param  _handler Address of the handler to check if msg.sender has permissions for
   */
  modifier handlerAllowed(address _handler) {
    if (msg.sender != _handler && handlerCan[_handler][msg.sender] == 0) revert HandlerNotAllowed();
    _;
62  }
```
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L49-L62