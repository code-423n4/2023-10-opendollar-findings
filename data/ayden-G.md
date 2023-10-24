1.Swapping the positions of baseAmount and quotePeriod can save one slot
baseAmount and quotePeriod  can be packed into 1 slot 
https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L33#L37.

