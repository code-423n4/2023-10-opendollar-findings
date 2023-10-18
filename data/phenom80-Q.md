[NC-1] Contract should expose an interface
All external/public functions should extend an interface to ensure the whole API is extracted.

Instances (92):

File: od-contracts-dev-SCOPE/AccountingEngine.sol

37:   function addAuthorization(address _account) external override(Authorizable, IAuthorizable) isAuthorized whenEnabled {

61:   function params() external view returns (AccountingEngineParams memory _accEngineParams) {

104:   function unqueuedUnauctionedDebt() external view returns (uint256 __unqueuedUnauctionedDebt) {

108:   function _unqueuedUnauctionedDebt(uint256 _debtBalance) internal view returns (uint256 __unqueuedUnauctionedDebt) {

115:   function pushDebtToQueue(uint256 _debtBlock) external isAuthorized {

123:   function popDebtFromQueue(uint256 _debtBlockTimestamp) external {

139:   function settleDebt(uint256 _rad) external {

143:   function _settleDebt(

159:   function cancelAuctionedDebtWithSurplus(uint256 _rad) external {

175:   function auctionDebt() external returns (uint256 _id) {

198:   function auctionSurplus() external returns (uint256 _id) {

247:   function _onContractDisable() internal override {

260:   function transferPostSettlementSurplus() external whenDisabled {

283:   function _modifyParameters(bytes32 _param, bytes memory _data) internal override {

305:   function _setSurplusAuctionHouse(address _surplusAuctionHouse) internal {

314:   function _validateParameters() internal view override {

File: od-contracts-dev-SCOPE/BasicActions.sol

31:   function _getGeneratedDeltaDebt(

53:   function _getRepaidDeltaDebt(

72:   function _getRepaidDebt(

94:   function _generateDebt(

121:   function _repayDebt(

142:   function _openSAFE(address _manager, bytes32 _cType, address _usr) internal returns (uint256 _safeId) {

147:   function _transferCollateral(address _manager, uint256 _safeId, address _dst, uint256 _deltaWad) internal {

153:   function _transferInternalCoins(address _manager, uint256 _safeId, address _dst, uint256 _rad) internal {

158:   function _modifySAFECollateralization(

170:   function _lockTokenCollateralAndGenerateDebt(

201:   function _collectAndExitCoins(address _manager, address _coinJoin, uint256 _safeId, uint256 _deltaWad) internal {

211:   function _collectAndExitCollateral(

226:   function openSAFE(address _manager, bytes32 _cType, address _usr) external delegateCall returns (uint256 _safeId) {

231:   function generateDebt(

242:   function repayDebt(

253:   function lockTokenCollateral(

269:   function freeTokenCollateral(

282:   function repayAllDebt(

313:   function lockTokenCollateralAndGenerateDebt(

328:   function openLockTokenCollateralAndGenerateDebt(

345:   function repayDebtAndFreeTokenCollateral(

374:   function repayAllDebtAndFreeTokenCollateral(

File: od-contracts-dev-SCOPE/CamelotRelayer.sol

68:   function getResultWithValidity() external view returns (uint256 _result, bool _validity) {

91:   function read() external view returns (uint256 _result) {

103:   function _parseResult(uint256 _quoteResult) internal view returns (uint256 _result) {

File: od-contracts-dev-SCOPE/CamelotRelayerFactory.sol

23:   function deployCamelotRelayer(

34:   function camelotRelayersList() external view returns (address[] memory _camelotRelayersList) {

File: od-contracts-dev-SCOPE/ODGovernor.sol

52:   function votingDelay() public view override(IGovernor, GovernorSettings) returns (uint256) {

59:   function votingPeriod() public view override(IGovernor, GovernorSettings) returns (uint256) {

66:   function quorum(uint256 blockNumber) public view override(IGovernor, GovernorVotesQuorumFraction) returns (uint256) {

73:   function state(uint256 proposalId)

85:   function propose(

97:   function proposalThreshold() public view override(Governor, GovernorSettings) returns (uint256) {

104:   function _execute(

117:   function _cancel(

129:   function _executor() internal view override(Governor, GovernorTimelockControl) returns (address) {

136:   function supportsInterface(bytes4 interfaceId)

File: od-contracts-dev-SCOPE/ODProxy.sol

26:   function execute(address _target, bytes memory _data) external payable onlyOwner returns (bytes memory _response) {

File: od-contracts-dev-SCOPE/ODSafeManager.sol

73:   function getSafes(address _usr) external view returns (uint256[] memory _safes) {

78:   function getSafes(address _usr, bytes32 _cType) external view returns (uint256[] memory _safes) {

83:   function getSafesData(address _usr)

98:   function safeData(uint256 _safe) external view returns (SAFEData memory _sData) {

105:   function allowSAFE(uint256 _safe, address _usr, uint256 _ok) external safeAllowed(_safe) {

112:   function allowHandler(address _usr, uint256 _ok) external {

118:   function openSAFE(bytes32 _cType, address _usr) external returns (uint256 _id) {

136:   function transferSAFEOwnership(uint256 _safe, address _dst) external {

155:   function modifySAFECollateralization(

168:   function transferCollateral(uint256 _safe, address _dst, uint256 _wad) external safeAllowed(_safe) {

175:   function transferCollateral(bytes32 _cType, uint256 _safe, address _dst, uint256 _wad) external safeAllowed(_safe) {

182:   function transferInternalCoins(uint256 _safe, address _dst, uint256 _rad) external safeAllowed(_safe) {

189:   function quitSystem(uint256 _safe, address _dst) external safeAllowed(_safe) handlerAllowed(_dst) {

205:   function enterSystem(address _src, uint256 _safe) external handlerAllowed(_src) safeAllowed(_safe) {

217:   function moveSAFE(uint256 _safeSrc, uint256 _safeDst) external safeAllowed(_safeSrc) safeAllowed(_safeDst) {

235:   function addSAFE(uint256 _safe) external {

242:   function removeSAFE(uint256 _safe) external safeAllowed(_safe) {

249:   function protectSAFE(uint256 _safe, address _liquidationEngine, address _saviour) external safeAllowed(_safe) {

File: od-contracts-dev-SCOPE/UniV3Relayer.sol

74:   function getResultWithValidity() external view returns (uint256 _result, bool _validity) {

97:   function read() external view returns (uint256 _result) {

110:   function _parseResult(uint256 _quoteResult) internal view returns (uint256 _result) {

File: od-contracts-dev-SCOPE/Vault721.sol

56:   function initializeManager() external {

63:   function initializeRenderer() external {

70:   function getProxy(address _user) external view returns (address _proxy) {

77:   function build() external returns (address payable _proxy) {

85:   function build(address _user) external returns (address payable _proxy) {

94:   function mint(address _proxy, uint256 _safeId) external {

104:   function updateNftRenderer(

119:   function updateContractURI(string memory _metaData) external onlyGovernor {

126:   function setSafeManager(address _safeManager) external onlyGovernor {

133:   function setNftRenderer(address _nftRenderer) external onlyGovernor {

140:   function tokenURI(uint256 _safeId) public view override returns (string memory uri) {

147:   function contractURI() public view returns (string memory uri) {

154:   function _isNotProxy(address _user) internal view returns (bool) {

162:   function _build(address _user) internal returns (address payable _proxy) {

172:   function _setSafeManager(address _safeManager) internal nonZero(_safeManager) {

179:   function _setNftRenderer(address _nftRenderer) internal nonZero(_nftRenderer) {

187:   function _afterTokenTransfer(address from, address to, uint256 firstTokenId, uint256 batchSize) internal override {
