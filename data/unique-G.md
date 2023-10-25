## \[G-01\] Use hardcode address instead `address(this)`

Instead of using `address(this)`, it is more gas-efficient to pre-calculate and use the hardcoded `address`. Foundry’s script.sol and solmate’s `LibRlp.sol` contracts can help achieve this.

References: <ins>https://book.getfoundry.sh/reference/forge-std/compute-create-address</ins>

<ins>https://twitter.com/transmissions11/status/1518507047943245824</ins>

```
105:     return _unqueuedUnauctionedDebt(safeEngine.debtBalance(address(this)));
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L105

```
104:     _settleDebt(safeEngine.coinBalance(address(this)), safeEngine.debtBalance(address(this)), _rad);
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L140

```
162: uint256 _coinBalance = safeEngine.coinBalance(address(this));
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L162

```
169: emit CancelDebt(_rad, _coinBalance - _rad, safeEngine.debtBalance(address(this)));
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L169

```
178:  uint256 _coinBalance = safeEngine.coinBalance(address(this));
179:  uint256 _debtBalance = safeEngine.debtBalance(address(this));
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L178C5-L179C66

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L187

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L204C5-L205C66

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L271

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L203

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L218

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L305C6-L306C39

## \[G-02\] The result of a function call should be cached rather than re-calling the function

* External calls are expensive. Results of external function calls should be cached rather than call them multiple times. Consider caching the following*

```
194: function unqueuedUnauctionedDebt() external view returns (uint256 __unqueuedUnauctionedDebt) {
    return _unqueuedUnauctionedDebt(safeEngine.debtBalance(address(this)));
  }
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L105

```
34: function camelotRelayersList() external view returns (address[] memory _camelotRelayersList) {
    return _camelotRelayers.values();
  }
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/factories/CamelotRelayerFactory.sol#L34C2-L36C4

## \[G-03\] For `uint` use `!= 0` instead of `> 0`

```
224: if (_params.surplusTransferPercentage > 0) {
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L224

```
269: if (_coinBalance > 0) {
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L269

## \[G-04\] Use bytes32 rather than string/bytes.

If you can fit your data in 32 bytes, then you should use bytes32 datatype rather than bytes or strings as it is much cheaper in solidity. Basically, Any fixed size variable in solidity is cheaper than variable size.

```
31:   string public symbol;
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L31

```
30:   string public symbol;
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L30

## \[G-05\] Use assembly to perform efficient back-to-back calls

If similar external calls are performed back-to-back, we can use assembly to reuse any function signatures and function parameters that stay the same. In addition, we can also reuse the same memory space for each function call (scratch space + free memory pointer), which can potentially allow us to avoid memory expansion costs. In this case, we are also able to efficiently store the function signatures together in memory as one word, saving multiple MLOADs in the process.

Note: In order to do this optimization safely we will cache the free memory pointer value and restore it once we are done with our function calls. We will also set the zero slot back to 0 if neccessary.

```
45: address _token0 = ICamelotPair(camelotPair).token0();
46: address _token1 = ICamelotPair(camelotPair).token1();
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L45C5-L46C58

```
57:  baseAmount = uint128(10 ** IERC20Metadata(_baseToken).decimals());
58:  multiplier = 18 - IERC20Metadata(_quoteToken).decimals();
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L57C5-L58C62

```
address _token0 = IUniswapV3Pool(uniV3Pool).token0();
address _token1 = IUniswapV3Pool(uniV3Pool).token1();
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L51C5-L52C58

## \[G-06\] Use assembly to validate `msg.sender`

We can use assembly to efficiently validate msg.sender for the didPay and uniswapV3SwapCallback functions with the least amount of opcodes necessary. Additionally, we can use xor() instead of iszero(eq()), saving 3 gas. We can also potentially save gas on the unhappy path by using scratch space to store the error selector, potentially avoiding memory expansion costs.

```
95: require(msg.sender == address(safeManager), 'V721: only safeManager');
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L95

## \[G‑07\] Using `bool`s for storage incurs overhead

```
// Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.
```

Use `uint256(1)` and `uint256(2)` for true/false to avoid a Gwarmaccess (**<ins>100 gas</ins>**) for the extra SLOAD, and to avoid Gsset (**20000 gas**) when changing from `false` to `true`, after having been `true` in the past.

```
29: bool _succeeded;
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODProxy.sol#L29

## \[G-08\] Use a temporary variable to cache repetitive complex calculation

```
if (_coinAmount < _deltaWad * RAY) {
      // Calculates the needed deltaDebt so together with the existing coins in the safeEngine is enough to exit wad amount of COIN tokens
      _deltaDebt = ((_deltaWad * RAY - _coinAmount) / _rate).toInt();
      // This is neeeded due lack of precision. It might need to sum an extra deltaDebt wei (for the given COIN wad amount)
      _deltaDebt = uint256(_deltaDebt) * _rate < _deltaWad * RAY ? _deltaDebt + 1 : _deltaDebt;
    }
```

`_deltaWad * RAY ` should be cached.

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L41C4-L46C6

## \[G-09\] Make fewer external calls.

Every call to an external contract costs a decent amount of gas. For optimization of gas usage, It’s better to call one function and have it return all the data you need rather than calling a separate function for every piece of data. This might go against the best coding practices for other languages, but solidity is special.

## CONCLUSION

As you move forward with implementing the suggested optimizations, we urge you to exercise caution and conduct meticulous testing. It is essential to verify that these changes do not introduce any new vulnerabilities and effectively deliver the desired performance improvements. Carefully review the code modifications and perform rigorous testing to validate the security and effectiveness of the refactored code.