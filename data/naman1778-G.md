## [G-01] Stack variable used as a cheaper cache for a state variable is only used once

If the variable is only accessed once, it’s cheaper to use the state variable directly that one time, and save the 3 gas the extra stack assignment would spend.

There are 5 instances of this issue in 2 files:

```
File: contracts/proxies/Vault721. sol	

97: address _user = _proxyRegistry[_proxy];
```

    diff --git a/src/contracts/proxies/Vault721.sol b/src/contracts/proxies/Vault721.sol
    index 76343f0..f69bc12 100644
    --- a/src/contracts/proxies/Vault721.sol
    +++ b/src/contracts/proxies/Vault721.sol
    @@ -94,8 +94,7 @@ contract Vault721 is ERC721Enumerable {
       function mint(address _proxy, uint256 _safeId) external {
         require(msg.sender == address(safeManager), 'V721: only safeManager');
         require(_proxyRegistry[_proxy] != address(0), 'V721: non-native proxy');
    -    address _user = _proxyRegistry[_proxy];
    -    _safeMint(_user, _safeId);
    +    _safeMint(_proxyRegistry[_proxy], _safeId);
       }

       /**

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol

```
File: contracts/proxies/ODSafeManager.sol	
    
106: address _owner = _safeData[_safe].owner;

176: SAFEData memory _sData = _safeData[_safe];

183: SAFEData memory _sData = _safeData[_safe];

236: SAFEData memory _sData = _safeData[_safe];
```

    diff --git a/src/contracts/proxies/ODSafeManager.sol b/src/contracts/proxies/ODSafeManager.sol
    index b3ab2f5..dcfe6b4 100644
    --- a/src/contracts/proxies/ODSafeManager.sol
    +++ b/src/contracts/proxies/ODSafeManager.sol
    @@ -103,8 +103,7 @@ contract ODSafeManager is IODSafeManager {

       /// @inheritdoc IODSafeManager
       function allowSAFE(uint256 _safe, address _usr, uint256 _ok) external safeAllowed(_safe) {
    -    address _owner = _safeData[_safe].owner;
    -    safeCan[_owner][_safe][_usr] = _ok;
    +    safeCan[_safeData[_safe].owner][_safe][_usr] = _ok;
         emit AllowSAFE(msg.sender, _safe, _usr, _ok);
       }

    @@ -173,15 +172,14 @@ contract ODSafeManager is IODSafeManager {

       /// @inheritdoc IODSafeManager
       function transferCollateral(bytes32 _cType, uint256 _safe, address _dst, uint256 _wad) external safeAllowed(_safe) {
    -    SAFEData memory _sData = _safeData[_safe];
    -    ISAFEEngine(safeEngine).transferCollateral(_cType, _sData.safeHandler, _dst, _wad);
    +    ISAFEEngine(safeEngine).transferCollateral(_cType, _safeData[_safe].safeHandler, _dst, _wad);
         emit TransferCollateral(msg.sender, _cType, _safe, _dst, _wad);
       }

       /// @inheritdoc IODSafeManager
       function transferInternalCoins(uint256 _safe, address _dst, uint256 _rad) external safeAllowed(_safe) {
    -    SAFEData memory _sData = _safeData[_safe];
    -    ISAFEEngine(safeEngine).transferInternalCoins(_sData.safeHandler, _dst, _rad);
    +    SAFEData memory _sData = ;
    +    ISAFEEngine(safeEngine).transferInternalCoins(_safeData[_safe].safeHandler, _dst, _rad);
         emit TransferInternalCoins(msg.sender, _safe, _dst, _rad);
       }

    @@ -233,9 +231,8 @@ contract ODSafeManager is IODSafeManager {

       /// @inheritdoc IODSafeManager
       function addSAFE(uint256 _safe) external {
    -    SAFEData memory _sData = _safeData[_safe];
         _usrSafes[msg.sender].add(_safe);
    -    _usrSafesPerCollat[msg.sender][_sData.collateralType].add(_safe);
    +    _usrSafesPerCollat[msg.sender][_safeData[_safe].collateralType].add(_safe);
       }

       /// @inheritdoc IODSafeManager

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol

#### Test Code

    contract GasTest is DSTest {
        Contract0 c0;
        Contract1 c1;
        function setUp() public {
            c0 = new Contract0();
            c1 = new Contract1();
        }
        function testGas() public {
            c0.not_optimized();
            c1.optimized();
        }
    }
    
    contract Contract0 {
        uint256 num = 5;
        uint256 sum;
        function not_optimized() public returns(bool){
            uint256 num1 = num;
            sum = num1 + 2;
        }
    }

    contract Contract1 {
        uint256 num = 5;
        uint256 sum;
        function optimized() public returns(bool){
            sum = num + 2;
        }
    }

#### Gas Report

| Contract0 contract                        |                 |       |        |       |         |
|-------------------------------------------|-----------------|-------|--------|-------|---------|
| Deployment Cost                           | Deployment Size |       |        |       |         |
| 63799                                     | 244             |       |        |       |         |
| Function Name                             | min             | avg   | median | max   | # calls |
| not_optimized                             | 24462           | 24462 | 24462  | 24462 | 1       |


| Contract1 contract                        |                 |       |        |       |         |
|-------------------------------------------|-----------------|-------|--------|-------|---------|
| Deployment Cost                           | Deployment Size |       |        |       |         |
| 63599                                     | 243             |       |        |       |         |
| Function Name                             | min             | avg   | median | max   | # calls |
| optimized                                 | 24460           | 24460 | 24460  | 24460 | 1       |

## [G-02] Instead of counting down in *for* statements, count up

Counting down is more gas efficient than counting up because neither we are making zero variable to non-zero variable and also we will get gas refund in the last transaction when making non-zero to zero variable.

There is 1 instance of this issue in 1 file:

```
File: contracts/proxies/ODSafeManager.sol	

91: for (uint256 _i; _i < _safes.length; _i++) {
```

    diff --git a/src/contracts/proxies/ODSafeManager.sol b/src/contracts/proxies/ODSafeManager.sol
    index b3ab2f5..38cb289 100644
    --- a/src/contracts/proxies/ODSafeManager.sol
    +++ b/src/contracts/proxies/ODSafeManager.sol
    @@ -88,7 +88,7 @@ contract ODSafeManager is IODSafeManager {
         _safes = _usrSafes[_usr].values();
         _safeHandlers = new address[](_safes.length);
         _cTypes = new bytes32[](_safes.length);
    -    for (uint256 _i; _i < _safes.length; _i++) {
    +    for (uint256 _i = _safes.length-1 ; _i >= 0 ; _i--) {
           _safeHandlers[_i] = _safeData[_safes[_i]].safeHandler;
           _cTypes[_i] = _safeData[_safes[_i]].collateralType;
         }

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol

#### Test Code

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

#### Gas Report

| Contract0 contract                        |                 |      |        |      |         |
|-------------------------------------------|-----------------|------|--------|------|---------|
| Deployment Cost                           | Deployment Size |      |        |      |         |
| 77011                                     | 311             |      |        |      |         |
| Function Name                             | min             | avg  | median | max  | # calls |
| AddNum                                    | 7040            | 7040 | 7040   | 7040 | 1       |


| Contract1 contract                        |                 |      |        |      |         |
|-------------------------------------------|-----------------|------|--------|------|---------|
| Deployment Cost                           | Deployment Size |      |        |      |         |
| 73811                                     | 295             |      |        |      |         |
| Function Name                             | min             | avg  | median | max  | # calls |
| AddNum                                    | 3819            | 3819 | 3819   | 3819 | 1       |

