## [N-01] Typos

There is 1 instance of this issue in 1 file:

    File: contracts/proxies/actions/BasicActions.sol	

    //@audit usr's 
    * @dev    The rate adjusted SAFE's debt minus COIN balance available in usr's address

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol

## [N-02] Lack of address(0) checks in the constructor

Zero-address check should be used in the constructors, to avoid the risk of setting smth as address(0) at deploying time.

There are 5 instances of this issue in 5 files:

    File: contracts/AccountingEngine.sol	

    86: constructor(

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol

    File: contracts/proxies/ODProxy.sol	

    14: constructor(address _owner) {

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODProxy.sol

    File: contracts/proxies/Vault721.sol	

    33: constructor(address _governor) ERC721('OpenDollar Vault', 'ODV') {

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol

    File: contracts/proxies/SAFEHandler.sol	

    16: constructor(address _safeEngine) {

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/SAFEHandler.sol

    File: contracts/proxies/ODSafeManager.sol	

    64: constructor(address _safeEngine, address _vault721) {

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol

## [N-03] According to the syntax rules, use *=> mapping (* instead of *=> mapping(* using spaces as keyword

There are 3 instances of this issue in 1 file:

    File: contracts/proxies/ODSafeManager.sol	

    34: mapping(address _safeOwner => mapping(bytes32 _cType => EnumerableSet.UintSet)) private _usrSafesPerCollat;

    39: mapping(address _owner => mapping(uint256 _safeId => mapping(address _caller => uint256 _ok))) public safeCan;

    41: mapping(address _safeHandler => mapping(address _caller => uint256 _ok)) public handlerCan;

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol

## [N-04] Remove commented out code

There are 2 instances of this issue in 1 file:

    File: contracts/oracles/CamelotRelayer.sol	

    7: import {ICamelotFactory} from '@camelot/interfaces/ICamelotFactory.sol';

    41: camelotPair = ICamelotFactory(_CAMELOT_FACTORY).getPair(_baseToken, _quoteToken);

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol

## [N-05] Using Vulnerable Version of OpenZeppelin

The package.json configuration file says that the project is using 4.8.2 of OpenZeppelin which has a vulnerability.

Although there is no security vulnerability covering the project, it is recommended to use the latest version 5.0.0.

There is 1 instance of this issue in 1 file: 

    File: od-contracts/package.json

    47: "@openzeppelin/contracts": "4.8.2",

https://github.com/open-dollar/od-contracts/blob/dev/package.json

