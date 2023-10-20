[L2 Bot]: https://github.com/code-423n4/2023-10-opendollar/blob/main/bot-report.md#l2-missing-checks-for-address-0-when-assigning-values-to-address-state-variables
[L1 Snippet]: https://github.com/open-dollar/od-contracts/blob/dev/src/contracts/AccountingEngine.sol#L37-L39
[L2 Snippet]: https://github.com/open-dollar/od-contracts/blob/dev/src/contracts/AccountingEngine.sol#L175-L193


## Low Issues
### Low Issue 1
This is another instance of bot reported L2 issue; as shown [here][L2 Bot] 

### Summary
There should be a zero address check when adding new authorized address via ```AccountingEngine.sol::addAuthorization(address _account)``` see affected code [here][L1 Snippet]

### Recommendation
Update function to check address validity using assertNonNull():

```diff
  function addAuthorization(address _account) external override(Authorizable, IAuthorizable) isAuthorized whenEnabled {
-               _addAuthorization(_account);
+               _addAuthorization(_account.assertNonNull());
          }
```

### Low Issue 2
### Summary
In ```AccountingEngine.sol::auctionDebt()``` after Line 183, which calls ```_settleDebt```, there should be a check to ensure there is enough debt to warrant creating a new auction via ```startAuction``` on Line 186.

### Vulnerability Details
Imagine the scenario where ```auctionDebt()``` is called and the total unauctioned debt, ```_unqueuedUnauctionedDebt(_debtBalance)```, is reduced to 0 via execution of ```_settleDebt()```
```
    (_coinBalance, _debtBalance) = _settleDebt(_coinBalance, _debtBalance, _coinBalance);
```
Subsequent to execution of ```_settleDebt()```, the system does not check again if ```_params.debtAuctionBidSize > _unqueuedUnauctionedDebt(_debtBalance)``` and then:
1. increases the value of the state variable ```totalOnAuctionDebt``` by _params.debtAuctionBidSize
2. creates an auction with debt that doesn't exist (importantly, there is no check either in ```DebtAuctionHouse.sol::startAuction()```

```
    (_coinBalance, _debtBalance) = _settleDebt(_coinBalance, _debtBalance, _coinBalance);
    totalOnAuctionDebt += _params.debtAuctionBidSize;

    _id = debtAuctionHouse.startAuction({
      _incomeReceiver: address(this),
      _amountToSell: _params.debtAuctionMintedTokens,
      _initialBid: _params.debtAuctionBidSize
    });
```

### Recommendation
Update the check used before ```_settleDebt``` to calculate based off the estimated unauctioned debt after settlement of ```_settleDebt```:

```diff
  function auctionDebt() external returns (uint256 _id) {
    if (_params.debtAuctionBidSize == 0) revert AccEng_DebtAuctionDisabled();

    uint256 _coinBalance = safeEngine.coinBalance(address(this));
    uint256 _debtBalance = safeEngine.debtBalance(address(this));

-   if (_params.debtAuctionBidSize > _unqueuedUnauctionedDebt(_debtBalance)) revert AccEng_InsufficientDebt();

+   uint256 expectedSettledDebt = (_coinBalance < _debtBalance) ? _coinBalance : _debtBalance;
+   uint256 expectedDebtBalanceAfterSettlement = _debtBalance - expectedSettledDebt;

+   if (_params.debtAuctionBidSize > _unqueuedUnauctionedDebt(expectedDebtBalanceAfterSettlement)) revert AccEng_InsufficientDebt();

    (_coinBalance, _debtBalance) = _settleDebt(_coinBalance, _debtBalance, _coinBalance);

    totalOnAuctionDebt += _params.debtAuctionBidSize;

    _id = debtAuctionHouse.startAuction({
      _incomeReceiver: address(this),
      _amountToSell: _params.debtAuctionMintedTokens,
      _initialBid: _params.debtAuctionBidSize
    });

    emit AuctionDebt(_id, _params.debtAuctionMintedTokens, _params.debtAuctionBidSize);
  }
```
