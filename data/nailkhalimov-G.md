[src/contracts/AccountEngine.sol:L#175](https://github.com/open-dollar/od-contracts/blob/v1.5.5/src/contracts/AccountingEngine.sol#L175):
Function could save around 324 gas if reading  `_params.debtAuctionBidSize` and `_params.debtAuctionMintedTokens` moved to a variables and read from them.

Before:
Avg   | Med 
14845 | 15740

After:
Avg   | Med
11722 | 15327

[src/contracts/AccountEngine.sol:L#200](https://github.com/open-dollar/od-contracts/blob/v1.5.5/src/contracts/AccountingEngine.sol#L200):
Variables `_params.surplusTransferPercentage` and `_params.surplusAmount` could be moved to 2 uint256 variables and save around 284 gas for every transaction.

Before | After
21768 | 21484

[src/contracts/AccountEngine.sol:L#115](https://github.com/open-dollar/od-contracts/blob/v1.5.5/src/contracts/AccountingEngine.sol#L115):
Use `totalQueuedDebt += _debtBlock` instead of `totalQueuedDebt = totalQueuedDebt + _debtBlock`. Will save gas.

Before:
Avg   | Med
31943 | 36645

After:
29116 | 25712