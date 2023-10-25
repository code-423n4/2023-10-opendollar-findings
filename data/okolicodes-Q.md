## [L-01] Low level call returns true if the Target address doesn't exist
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODProxy.sol?plain=1#L26-L36

# Impact
As written in the solidity documentation, the low-level functions `call`, `delegatecall` and `staticcall` return true as their first return value if the account called is non-existent, as part of the design of the EVM. Account existence must be checked prior to calling if needed. 

## Proof of Concept
The low-level function `delegatecall` is used in the `execute` function in the `OD proxy` and it can be problematic. The function makes proper check by making sure the address is not a zero address but does not check if it is a contract address that can be able to execute code and if the target doesn't exist, success will be equal to true and the function will return true and the code execution will be continued like the call was successful.
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODProxy.sol#L26

```
    bool _succeeded;
    (_succeeded, _response) = _target.delegatecall(_data);
```

Also the return value of the data should be checked as well...


## Tools Used
Manual Review
## Recommended Mitigation Steps
Check before any low-level call that the target address actually exists, for example before the delegate call in the OD proxy function you can check that the address is a contract by checking its code size.
Another way in which you can mitiagte this is by checking for the returndata. 
A simple check like this will actually be fine: 
```
if (data.length <= 4) {
revert(); //delegatecall failed 
}
```