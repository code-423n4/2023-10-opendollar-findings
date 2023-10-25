Gas optimization: https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol

_unqueuedUnauctionedDebt Function can be more optimized

function _unqueuedUnauctionedDebt(uint256 _debtBalance) internal view returns (uint256 __unqueuedUnauctionedDebt) {
    return (_debtBalance - totalQueuedDebt) - totalOnAuctionDebt;
}

_unqueuedUnauctionedDebt performs multiple storage reads. To optimize, consider storing frequently accessed values in local variables. For example, in the _unqueuedUnauctionedDebt function, you can reduce storage reads:

function _unqueuedUnauctionedDebt(uint256 _debtBalance) internal view returns (uint256 __unqueuedUnauctionedDebt) {
    uint256 _totalQueuedDebt = totalQueuedDebt;
    uint256 _totalOnAuctionDebt = totalOnAuctionDebt;
    return (_debtBalance - _totalQueuedDebt) - _totalOnAuctionDebt;
}
