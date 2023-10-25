##  The `AccountingEngine.sol` contract can be disabled but cannot be enabled 

## Description

The `AccountingEngine.sol` contract imports a contract that allows the AccountingEngine.sol to be disabled but cannot be enabled, any authorized address can trigger this state. Consider adding a enable function for more flexibility. 

## Links

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L21-L23

## Fix

Consider adding an enable functionality or at least make the disable function a two step process as it is definitive.


## 
## Description
## Links
## Fix