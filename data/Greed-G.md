# Unecessary check for ERC721 `to` address

The function `_afterTokenTransfer()` is triggered when the `Vault721` contract transfers an NFT.

Based upon `ERC721.sol` contract, an NFT is transfered using the functions `safeTransferFrom()` which already checks if the receiver is `address(0)`

Therefore, the `require(to != address(0), 'V721: no burn');` check is not required

Gas saved : 70

[https://github.com/open-dollar/od-contracts/blob/dev/src/contracts/proxies/Vault721.sol#L187-L188](https://github.com/open-dollar/od-contracts/blob/dev/src/contracts/proxies/Vault721.sol#L187-L188)

```sol
function _afterTokenTransfer(address from, address to, uint256 firstTokenId, uint256 batchSize) internal override {
  require(to != address(0), 'V721: no burn');
  ...
}
```
