## 1. No need to validate the `_dst` in `ODSafeManager.sol/transferSAFEOwnership()` which appears in the `calldata`
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L139
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L187-L199

The variable `_dst` is already validated by the `Vault721.sol` which is the `msg.sender` of this function call. 
`_dst` is actually the address of the proxy in `Vault721.sol` and is validated in the function `_afterTokenTransfer()` and created using `_build()` function.


## 2. Any address should not be able to add safe in their mapping in `ODSafeManager.sol`
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L235-L239


## 3. The functions `initializeManager` and `initializeNFTRenderer` are prone to front-run attack as can be called by any address.
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L56-L65


The mappings in `ODSafeManager.sol` are meant to store the owners which are the addresses of `ODProxy` contract. But the function `ODSafeManager.sol/addSAFE()` allows any contract and even an EOA to be able to update themselves in mapping to contain any SAFE.