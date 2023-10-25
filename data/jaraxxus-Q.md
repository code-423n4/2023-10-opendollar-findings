### [L-01] Consider using upgrade contracts for Governor contract

```
  constructor(
    address _token,
    TimelockController _timelock
  )
    Governor('ODGovernor')
    GovernorSettings(1, 15, 0)
    GovernorVotes(IVotes(_token))
    GovernorVotesQuorumFraction(3)
    GovernorTimelockControl(_timelock)
  {}
```

The current governor contracts are not using its upgradeable counterparts. Consider using upgradeable contracts like this governor contract below:

https://etherscan.io/address/0x737c32545cf7b68baabedafd4325cd0527e29c5a#code
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/gov/ODGovernor.sol

### [L-02] Use safetransfer instead of transfer in commonActions.sol

Some tokens do not revert on failure, but instead return false (e.g. ZRX). https://github.com/d-xo/weird-erc20/#no-revert-on-failure tranfser/transferfrom is directly used to send tokens in many places in the contract and the return value is not checked. If the token send fails, it will cause a lot of serious problems.

Use safetransfer like in the collateralJoin contract

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/CommonActions.sol#L90-L92

### [L-03] Fee-on-transfer tokens will break accounting error
The tokenCollateral mapping in SAFEEngine carefully adds and subtracts the exact amount of collateral tokens. For example, if depositing collateral, the SAFEEngine will increase the amount of tokenCollateral[_cType][_account] for the safe first before decreasing the exact amount later through _modifySAFECollateralization. If the collateral is a fee-on-transfer, then all accounting in SafeEngine will break

Take note of collateral involving fee on transfers. It is best to whitelist collateral tokens to prevent such a situation from happening.

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L261-L266