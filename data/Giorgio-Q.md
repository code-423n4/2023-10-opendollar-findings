##  The `AccountingEngine.sol` contract can be disabled but cannot be enabled 

## Description

The `AccountingEngine.sol` contract imports a contract that allows the `AccountingEngine::addAuthorization` to be disabled but cannot be enabled after that, any authorized address can trigger this state. 

## Links

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L21-L23

## Fix

Consider adding an enable functionality or at least make the disable function a two step process as it is definitive.


## Missing functions in the `BasicActions` to reach `ODSafeManager`
## Description
Both functions allowing other users and handlers to manage the safe are restricted with access control. Only callable by the owner of the safe which is the ODproxy, the protocol has already set up a catalogue of actions in the BasicActions.sol contract however this one is missing the functionalities to reach`allowSAFE`, `allowHandler` and `protectSAFE` in the `ODSafeManager` contract. To perform these  actions the user will have to deploy a contract that call the `ODSafeManager` contract and fire calls to the `allowSAFE`, `allowHandler` and `protectSAFE` functions so that the `ODproxy` can make delegate calls through them. 

## Links

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L249-L253
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L112-L115
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L105-L109


## Fix
For the user's sake it would be more convenient for him if there are functions in `BasicActions` that call directly these functions in `ODSafeManager`.