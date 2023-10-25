## Vault721.sol
#### Functions vulnerable to frontrunning
- `initializeManager()`
- `initializeRenderer()`

The above functions are vulnerable to front-running in Arbitrum by spamming the RPC. An example would be sending a large number of requests to the blockchain's RPC interface, disrupting its normal operations. This could lead to the Manager and Renderer being set to a malicious address rather than the intended.

[Link to functions](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L56C1-L66C1)

#### Not using the zero address check modifier
- `mint()`
- `updateNftRenderer()`

Both functions use a `require()` statement to check for zero address inputs, however, for consistency throughout the protocol these can be changed to the modifier available [here](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L45C1-L51C4).

