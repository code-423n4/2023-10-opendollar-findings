# [L] Logical conditional if-else issue

The calcRisk contains a logical issue here

```js
File: NFTRenderer.sol
249:   function _calcRisk(uint256 ratio) internal pure returns (string memory, string memory) {
250:     if (ratio < 120) return ('LIQUIDATION', '#E45200');
251:     else if (ratio > 119 && ratio < 136) return ('HIGH', '#E45200');
252:     else if (ratio > 135 && ratio < 150) return ('ELEVATED', '#FCBF3B');
253:     else return ('LOW', '#459d00');
254:   }
```

The `_calcRisk` function in the `NFTRenderer.sol` file is responsible for determining the risk level based on a given `ratio`. The risk level is represented by a string and a corresponding color code.

In the original code, there is a logical issue on lines 251 and 252. The `else if` statements are using two conditional checks with `&&`, which is unnecessary. Since there is a preceding `if (ratio < 120)` check, the condition `ratio > 119` will always be true when execution reaches line 251. The same logic applies to line 252 with the condition `ratio > 135`.

The improved code simplifies this by removing the redundant conditional checks. It restructures the `else if` statements to only include a single conditional check. This makes the code more concise and efficient without changing the logic or behavior of the function. Here is the improved code:

```js
function _calcRisk(uint256 ratio) internal pure returns (string memory, string memory) {
  if (ratio < 120) return ('LIQUIDATION', '#E45200');
  else if (ratio < 136) return ('HIGH', '#E45200');
  else if (ratio < 150) return ('ELEVATED', '#FCBF3B');
  else return ('LOW', '#459d00');
}
```

# [L] Redundant check

There is a redundant check of checking `if (extraSurplusReceiver == address(0))`, we can remove the second one.

```js
File: AccountingEngine.sol
197:   /// @inheritdoc IAccountingEngine
198:   function auctionSurplus() external returns (uint256 _id) {
199:     if(_params.surplusTransferPercentage > WAD) revert AccEng_surplusTransferPercentOverLimit();
200:     if (_params.surplusAmount == 0) revert AccEng_NullAmount();
201:     if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver();
...
222: 
223:     // transfer surplus percentage
224:     if (_params.surplusTransferPercentage > 0) {
225:       if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver();
...
235:     }
236:   }
```



