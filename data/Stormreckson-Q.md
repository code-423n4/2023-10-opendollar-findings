1-
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L94-L95

`mint()` in `vault721` isn't checking if the `safeId` already exist before minting, it is recommended to include a check for duplicate `safeId` even if there's a nonce increment it adds additional safe guard.

2- https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L105-L106

Permission Level Validation Issue in `SafeCan` Mapping

The mapping is designed to store the  `_ok`  value, which represents permission levels (0 for not allowed and 1 for allowed).

•The `allowSAFE()` does not check if the user had been added to the `safeCan` mapping. It is advised to include a check if the `_usr` being added already exist in the mapping so that a `_usr` won't be added twice causing issues when trying to change `_ok` to 0.

•Also there's no explicit validation or checks in the `allowSAFE` function, allowing the possibility of assigning values outside the valid range (0 or 1). This could lead to unexpected behavior and potential security risks.
Assigning a value of 2 or other invalid values to  `_ok` without proper handling or validation could lead to unpredictable behavior, potentially compromising the security and functionality of the system.

##FIX 
Include proper checks and validation in the relevant function to enforce the valid range of the  `_ok`  parameter (0 or 1). Reject any values outside this range to ensure consistent and expected behavior. Also check if the user had already been added.