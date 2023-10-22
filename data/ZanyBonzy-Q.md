1. The `mint` function in Vault721.sol calls the ERC721 `_safeMint` which has a callback to the "to" address argument. THe function should be made non-reentrant to prevent the a malicious user from minting multiple tokens when they call the `OpenSafe` function. This ensures that one of the main invariants (1 token to 1 safe) is not broken. 

2. Consider using OpenZeppelin’s SafeCast libraries to prevent unexpected overflows when casting from uint256.
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L192C1-L193C57
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L208C1-L209C57
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L222C1-L223C57
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L63
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L43
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L190
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L265
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L276
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L396

3. Consider removing lines of codes that are no longer needed instead of commenting them out.
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/oracles/CamelotRelayer.sol#L7
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/oracles/CamelotRelayer.sol#L41

