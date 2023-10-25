## Clone Deployment

When it comes to deploying contracts utilizing Clone contracts for deployment is a recommended approach. This method significantly lowers gas usage, as demonstrated in this tutorial from OpenZeppelin: https://www.youtube.com/watch?v=3Mw-pMmJ7TA&ab_channel=OpenZeppelin
https://docs.openzeppelin.com/contracts/4.x/api/proxy#Clones

Porter Finance adopted this strategy and reported a `10x reduction` in deployment costs, as documented in their GitHub issue #15 and pull request #34.
https://github.com/porter-finance/v1-core/issues/15#issuecomment-1035639516
https://github.com/porter-finance/v1-core/pull/34

I suggest implementing a similar pattern here, using a `cloneDeterministic` method to mimic the current `create2` function, thereby achieving comparable gas savings.

## Replacements create2 -> cloneDeterministic
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L122
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/factories/CamelotRelayerFactory.sol#L28
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/factories/UniV3RelayerFactory.sol#L39
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L163
