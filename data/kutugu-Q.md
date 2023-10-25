# Findings Summary

| ID     | Title                                                                | Severity |
| ------ | -------------------------------------------------------------------- | -------- |
| [L-01] | Use the factory constant address of the testnet                      | Low      |
| [L-02] | Single point oracle prices remain vulnerable to manipulation         | Low      |
| [L-03] | The tokenURI is not compatible with the ERC721 standard              | Low      |

# Detailed Findings

# [L-01] Use the factory constant address of the testnet

## Description

```solidity
import {UNISWAP_V3_FACTORY, GOERLI_UNISWAP_V3_FACTORY} from '@script/Registry.s.sol';

contract UniV3Relayer is IBaseOracle, IUniV3Relayer {
  // --- Registry ---
  address internal constant _UNI_V3_FACTORY = GOERLI_UNISWAP_V3_FACTORY;
}


import {CAMELOT_V3_FACTORY, GOERLI_CAMELOT_V3_FACTORY} from '@script/Registry.s.sol';

contract CamelotRelayer is IBaseOracle, ICamelotRelayer {
  // --- Registry ---
  address internal constant _CAMELOT_FACTORY = GOERLI_CAMELOT_V3_FACTORY;
}
```

`CamelotRelayer` and `UniV3Relayer` use the testnet's factory constant address, which should be used for testing purposes.    
But considering that the contract will be deployed to the production environment, I mark this here.    

## Recommendations

Pass in the addresses of different environments through parameters instead of using hard-coded addresses.   

# [L-02] Single point oracle prices remain vulnerable to manipulation

## Description

```solidity
  function getResultWithValidity() external view returns (uint256 _result, bool _validity) {
    // If the pool doesn't have enough history return false
    if (OracleLibrary.getOldestObservationSecondsAgo(uniV3Pool) < quotePeriod) {
      return (0, false);
    }
    // Consult the query with a TWAP period of quotePeriod
    (int24 _arithmeticMeanTick,) = OracleLibrary.consult(uniV3Pool, quotePeriod);
    // Calculate the quote amount
    uint256 _quoteAmount = OracleLibrary.getQuoteAtTick({
      tick: _arithmeticMeanTick,
      baseAmount: baseAmount,
      baseToken: baseToken,
      quoteToken: quoteToken
    });
    // Process the quote result to 18 decimal quote
    _result = _parseResult(_quoteAmount);
    _validity = true;
  }
```

`UniV3Relayer` and `CamelotRelayer` will read the pool price at `quotePeriod` time point, and `quotePeriod` is immutable.    
If an attacker wants to take risks manipulating the pool price, the attack is still possible.     
The attacker only needs to manipulate the pool price at any time and perform arbitrage after the `quotePeriod` time.     
Of course, the precise time difference is difficult to control, but the possibility of attack still exists.     

## Recommendations

- Use prices at multiple points in time rather than at a single point in time
- Compare whether the difference between the current price and the previous price exceeds the threshold

# [L-03] The tokenURI is not compatible with the ERC721 standard

## Description

```solidity
  function tokenURI(uint256 _safeId) public view override returns (string memory uri) {
    uri = nftRenderer.render(_safeId);
  }
```

`tokenURI` will call `nftRenderer` directly to render the data, and it will not detect whether the ID has been minted, which violates the ERC721 standard.     
Malicious attackers can forge NFTs to deceive users, causing users to think that the corresponding NFTs are real.    

## Recommendations

Check the id is real before return tokenURI