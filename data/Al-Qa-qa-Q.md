# Reused code in `CamelotRelayer.sol` and `UniV3Relayer.sol` can be refactored in a single function

In `CamelotRelayer.sol` contract, function `read()` and function `getResultWithValidity()` have the same code to get the quoteAmount, so we can make a function `getQuoteAmount()` for example, and use it in both of them.

- **contracts/oracles/CamelotRelayer.sol**
  - [#L74-L81](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L74-L81)
  - [#L93-L99](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L93-L99)

The same refactor can be made in `UniV3Relayer.sol` contract, as it implements the same logic of `CamelotRelayer.sol`

- **contracts/oracles/UniV3Relayer.sol**
  - [#L80-L87](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L80-L87)
  - [#L99-L105](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L99-L105)


# Reused code in `ODSafeManager.sol` can be refactored in a single function

In `ODSafeManager.sol` contract, functions `quitSystem` and `enterSystem` have the same code to get the deltaCollateral and deltaDebt values. so we can make a function `getSafeDeltaInfo()` for example, and use it in both of them.

- **contracts/proxies/ODSafeManager.sol**
  - [#L190-L193](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L190-L193)
  - [#L206-L209](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L206-L209)


# Unused parameter in `Vault721::_afterTokenTransfer` function

The fourth parameter which is `batchSize` is not used in the `Vailt721` contract, but it must be set since the function has this parameter in `ERC721` contract. It is better to not path the parameter and just add the type instead, like this.

```solidity
// Vault721::_afterTokenTransfer
function _afterTokenTransfer(
    address from,
    address to,
    uint256 firstTokenId,
    uint256 /*batchSize*/ // @audit [removing the unused parameter]
) 
```
 
- **contracts/proxies/Vault721.sol**
  - [#L187](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L187)