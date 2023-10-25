### [Low-01] When a User-1 sell/transfer a safe to User-2, during transfer allowance is not clear in case of User-1
`safeCan` is a mapping which set allowance for other addresses, by which they can perform action on behalf of owner.

```solidity
mapping(address _owner => mapping(uint256 _safeId => mapping(address _caller => uint256 _ok))) public safeCan;
```

So basically when owner(user-1) sell/transfer his safe to other(user-2) these allowed address never clear. Problem occurs when same Owner(user-1) again buy that safe from User-2 again after some time period, Now previously allowed addresses now back in form(active) which may be the Owner(User-1) not aware off.

For a instance consider a senario
Bob has a Safe with id 99
Bob give allowance via `safeCan` to Sam
Bob sell safe to Alice
Years passed
Alice sell safe(id-99) to Bob
Now Sam has access to Safe, which may be Bob not aware of. Now he can preform action behalf of Bob
### Code
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L49-L53
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L39

### Mitigation
Re-think on current senario


### [Low-02] A mallicious allowed address can front run owner of a safe everytime and can give allowance for safe to other addresses.
`safeAllowed` is a modifier which ensure Owner of a perticular safe or allowed addresses by owner can perform perticular actions on safe.
```solidty
  modifier safeAllowed(uint256 _safe) {
    address _owner = _safeData[_safe].owner;
    if (msg.sender != _owner && safeCan[_owner][_safe][msg.sender] == 0) revert SafeNotAllowed(); 
    _;
  }
```
And these allowance set via `allowSAFE()` which called by Owner or allowed addresses only
```solidity
  function allowSAFE(uint256 _safe, address _usr, uint256 _ok) external safeAllowed(_safe) { 
    address _owner = _safeData[_safe].owner;
    safeCan[_owner][_safe][_usr] = _ok; 
    emit AllowSAFE(msg.sender, _safe, _usr, _ok); 
  }
```

Consider senario 
A mallicious address get allownce for any xyz reson, and try to do some fishy activity
Owner notice that and try to revoke allowance for that malicious address
Hacker notice that, and frontrun Owner Tx and give allowance to a other address controlled by him
Now he can continue doing phisy activity with new address
Same thing happens when owner try to revoke other
More Attacker can give allowance to so many milicious addresses

### Code
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L105-L109

### Mitigation
```allowSAFE()``` should only a `onlyOwner` function. Owner should be superior


### [Low-03] `allowSAFE` using wrong parameters during event emission.
While setting allowance for address for a safe function emits event `AllowSAFE` which may be used in off-chain tracking/activity

While emiting it uses 4 parameters `msg.sender, _safe, _usr, _ok` which i believe stands for following
*msg.sender* = Owner of safe
*_safe*      = Safe-id
*_user*      = Address to which allowance/revoke allowance occue
*ok*         = Allowance status of that address

But This `allowSAFE` function can callable by Owner as well as previoully allowed User 
```solidity
  function allowSAFE(uint256 _safe, address _usr, uint256 _ok) external safeAllowed(_safe) { 
    address _owner = _safeData[_safe].owner;
    safeCan[_owner][_safe][_usr] = _ok; 
    emit AllowSAFE(msg.sender, _safe, _usr, _ok); 
  }
```

So When allowed User call this function, in that case msg.sender != Owner

So event always should emited as following
```solidity
  function allowSAFE(uint256 _safe, address _usr, uint256 _ok) external safeAllowed(_safe) { 
    address _owner = _safeData[_safe].owner;
    safeCan[_owner][_safe][_usr] = _ok; 
-   emit AllowSAFE(msg.sender, _safe, _usr, _ok); 

+   emit AllowSAFE(_owner, _safe, _usr, _ok);
  }
```

### Code
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L105-L109


### [Low-4] `initializeRenderer()` and `initializeManager()` Both function has no access controls and they set critical state variable `nftRender` and `safeManager` respectively. So these could front-runnable

```solidity
  function initializeManager() external { // @audit front-runnable
    if (address(safeManager) == address(0)) _setSafeManager(msg.sender);
  }

  /**
   * @dev initializes NFTRenderer contract
   */
  function initializeRenderer() external { // @audit
    if (address(nftRenderer) == address(0)) _setNftRenderer(msg.sender);
  }
```
No access control present
Attacker can front-run and set those variable to own address
So It will cause to deploy whole contract again.

### Code
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L56-L58
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L63-L65

### Mitigation
Apply some owner access control on these function, or made those private or internal and call them only in `constructor` 