# 1) Use nested if and, avoid multiple check combinations

Using nested is cheaper than using && multiple check combinations. There are more advantages, such as easier to read code and better coverage reports.

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol?plain=1#L51
```
    if (msg.sender != _owner && safeCan[_owner][_safe][msg.sender] == 0) revert SafeNotAllowed();
```


https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol?plain=1#L60
```
    if (msg.sender != _handler && handlerCan[_handler][msg.sender] == 0) revert HandlerNotAllowed();
```

# Recommended Mitigation Steps

```
if (msg.sender != _owner) {
  if (safeCan[_owner][_safe][msg.sender] == 0) {
      ...
  }
}
```

# 2) x += y (x -= y) costs more gas than x = x + y (x = x - y) for state variables
x += y (x -= y) costs more gas than x = x + y (x = x - y) for state variables.

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol?plain=1#L167
```
    totalOnAuctionDebt -= _rad;
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol?plain=1#L184
```
    totalOnAuctionDebt += _params.debtAuctionBidSize;
```

# Recommended Mitigation Steps
```
    totalOnAuctionDebt = totalOnAuctionDebt + _params.debtAuctionBidSize;
```
