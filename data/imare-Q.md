## **QA-01**: Stability fee rendered with NFTRender is not correctly calculated

Inside the `NFTRenderer` the stability fee is displayed as a percent value. Its calculation is done like:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/NFTRenderer.sol#L144

Its value is taken from the [TaxCollector](https://goerli.arbiscan.io/address/0x1A906C363FBB28999bFE798b037094cF55561f39#readContract) contract and the `nextStabilityFee` is always divided by RAY (1e27) value.

The problem is that the `nextStabilitFee` is in range `RAY +/- maxStabilityFeeRange`

Taken from the TaxCollector contract: https://goerli.arbiscan.io/address/0x1A906C363FBB28999bFE798b037094cF55561f39#code#F1#L231

```solidity
 function _getNextStabilityFee(bytes32 _cType) internal view returns (uint256 _nextStabilityFee) {
    _nextStabilityFee = _params.globalStabilityFee.rmul(_cParams[_cType].stabilityFee);
    if (_nextStabilityFee < RAY - _params.maxStabilityFeeRange) return RAY - _params.maxStabilityFeeRange;
    if (_nextStabilityFee > RAY + _params.maxStabilityFeeRange) return RAY + _params.maxStabilityFeeRange;
  }
```

where `maxStabilitRange` is defined as always in range > 0 and < 1 RAY

https://goerli.arbiscan.io/address/0x1A906C363FBB28999bFE798b037094cF55561f39#code#F1#L321

```solidity
  function _validateParameters() internal view override {
    _params.primaryTaxReceiver.assertNonNull();
    _params.maxStabilityFeeRange.assertGt(0).assertLt(RAY);
    _params.globalStabilityFee.assertGtEq(RAY - _params.maxStabilityFeeRange).assertLtEq(
      RAY + _params.maxStabilityFeeRange
    );
  }
```

This is also confirmed by the `_validateCParameters` function

https://goerli.arbiscan.io/address/0x1A906C363FBB28999bFE798b037094cF55561f39#code#F1#L328

```solidity
function _validateCParameters(bytes32 _cType) internal view override {
    _cParams[_cType].stabilityFee.assertGtEq(RAY - _params.maxStabilityFeeRange).assertLtEq(
      RAY + _params.maxStabilityFeeRange
    );
  }
```

By always dividing [with 1 RAY](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/NFTRenderer.sol#L144) the calculation will always fall in two numbers:

- **1** when nextStability is > RAY
- or **0** when extStabilityFee < RAY 

and because so it will be always **displayed as 1% or 0%** inside the svg image.
